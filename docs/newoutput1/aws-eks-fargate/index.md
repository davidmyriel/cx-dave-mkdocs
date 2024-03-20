---
title: "AWS EKS Fargate"
date: "2023-08-07"
---

Integrate Coralogix seamlessly with **Amazon EKS on AWS Fargate** to effortlessly collect, analyze, and visualize **logs, metrics, and traces** from your containerized applications, empowering you with comprehensive full-stack monitoring and insights.

## Overview

The **Coralogix AWS EKS Fargate** integration has two independent parts: metrics and traces via OpenTelemetry, and logs via the AWS log\_router framework. Each integration can be deployed separately from the other.

## Metrics & Traces

This integration leverages the OpenTelemetry Collector (Contrib) to collect your Fargate workload pod and container metrics. It does so by querying the Kubelet stats API running on each node. As Fargate hosts are managed by AWS, node metrics are not available.

### Prerequisites

- `cx-eks-fargate-otel` namespace declared in your EKS cluster \[**Note**: This namespace need not be hosted by a Fargate profile. If this is desired, create a profile.\]

- A secret containing your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) in the `cx-eks-fargate-otel` namespace

### Create a Secret

**STEP 1**. Export your API key to a local variable.

```
export PRIVATE_KEY=<Send-Your-Data API key>

```

**STEP 2.** Set your namespace variable.

```
export NAMESPACE=cx-eks-fargate-otel

```

**STEP 3**. Create the secret using kubectl.

```
kubectl create secret generic coralogix-keys -n $NAMESPACE --from-literal=PRIVATE_KEY=$PRIVATE_KEY

```

**STEP 4**. Validate the newly-created secret.

```
kubectl get secret coralogix-keys -o yaml -n $NAMESPACE

```

### Create a ServiceAccount

In order for the OpenTelemetry Collector to get full access to the Kubernetes API, it will need to bind to a ServiceAccount. Create the service account by running the following bash script. Set the CLUSTER\_NAME and REGION accordingly, with the remainder unchanged.

```
##!/bin/bash
CLUSTER_NAME=<EKS Cluster Name>
REGION=<EKS Cluster Region>
SERVICE_ACCOUNT_NAMESPACE=cx-eks-fargate-otel
SERVICE_ACCOUNT_NAME=cx-otel-collector
SERVICE_ACCOUNT_IAM_ROLE=EKS-Fargate-cx-OTEL-ServiceAccount-Role
SERVICE_ACCOUNT_IAM_POLICY=arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy

eksctl utils associate-iam-oidc-provider \\
--cluster=$CLUSTER_NAME \\
--approve

eksctl create iamserviceaccount \\
--cluster=$CLUSTER_NAME \\
--region=$REGION \\
--name=$SERVICE_ACCOUNT_NAME \\
--namespace=$SERVICE_ACCOUNT_NAMESPACE \\
--role-name=$SERVICE_ACCOUNT_IAM_ROLE \\
--attach-policy-arn=$SERVICE_ACCOUNT_IAM_POLICY \\
--approve

```

### Configure and Deploy OTEL Collector Service

The attached yaml manifest will deploy the following:

- an OpenTelemetry Collector,

- a clusterIP service for submission of application traces and metrics, and

- cluster permissions required to query the Kubernetes API.

**STEP 1**. Set the container environment variables detailed at the top of the [yaml file](https://coralogixstg.wpengine.com/wp-content/uploads/2023/08/cx-eks-fargate-otel-1.yaml).

**STEP 2**. Once you’ve adjusted the manifests appropriately, deploy using the kubectl apply command.

```
kubectl apply -f cx-eks-fargate-otel.yaml

```

**Notes**:

- This manifest is all that is required to collect metrics from your EKS Fargate cluster and process application metrics and traces from gRPC sources.

- The OTLP gRPC endpoint is: `http://cx-otel-collector-service.cx-eks-fargate-otel.svc.cluster.local:4317`.

### Configure and Deploy the Self Monitoring Pod

Since Fargate workloads are unable to directly communicate with their host due to networking restrictions, we cannot monitor the OTEL collector pod’s performance directly. Instead, we have constructed a secondary manifest that’ll deploy a second OTEL collector to collect just these missing pod metrics.

[This manifest](https://coralogixstg.wpengine.com/wp-content/uploads/2023/08/cx-eks-fargate-otel-self-monitoring-1.yaml) also has some required environmental variables that need to be set, which detailed at the top.

Again, after setting the environment variables, deploy using kubectl apply:

```
kubectl apply -f cx-eks-fargate-otel-self-monitoring.yaml

```

## Logs

For EKS Fargate logs, leverage the AWS log\_router built into the Fargate Kubelet to route your application logs to the Coralogix platform through a Kinesis Firehose.

### Prerequisites

- AWS Kinesis Firehose [configured](https://coralogixstg.wpengine.com/docs/aws-firehose/) \[**Note**: By following our documentation, you will have configured Kinesis Firehose to use with your log\_router. Though there are other exporters available for the log\_router (fluent-bit), it is restricted to ElasticSearch, Firehose and Cloudwatch. We **recommend** Kinesis Firehose as it limits added cost, while allowing direct submission to the Coralogix Platform.\]

- Proper permissions added to each of your Fargate profile pod execution roles

- Deployment of the log\_router configuration

### Fargate Profile Permissions

For the sidecar fluent-bit pod to submit messages to Kinesis Firehose, it requires put and put batch permissions. This is accomplished by adding the following permissions to every Fargate profile that you wish to monitor for application logs.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "firehose:PutRecord",
                "firehose:PutRecordBatch"
            ],
            "Resource": [
                "<firehose_ARN>"
            ]
        }
    ]
}

```

### log\_router Deployment

To deploy the log\_router, you’ll need a specific namespace with appropriate labels configured in your cluster. You won’t be deploying any workloads into this namespace, so you don’t need a Fargate Profile configured for it.

Though the configuration will look like a standard fluent-bit configuration, only specific sections can be modified and only certain modules can be used. Below is our **recommended** Kubernetes manifest which will deploy the namespace and the fluent-bit ConfigMap.

```
---
kind: Namespace
apiVersion: v1
metadata:
  name: aws-observability
  labels:
    aws-observability: enabled

---
kind: ConfigMap
apiVersion: v1
metadata:
  name: aws-logging
  namespace: aws-observability
data:
  filters.conf: |
    [FILTER]
        Name parser
        Match *
        Key_name log
        Parser crio

    [FILTER]
        Name             kubernetes
        Match            kube.*
        Merge_Log           On
        Buffer_Size         0
        Kube_Meta_Cache_TTL 300s
        Keep_Log Off
        Merge_Log_Key log_obj
        K8S-Logging.Parser On
        K8S-Logging.Exclude On
        Annotations Off

  output.conf: |
    [OUTPUT]
      Name  kinesis_firehose
      Match *
      region <AWS Region> (no quotes)
      delivery_stream <Data Firehose Delivery Stream Name> (no quotes)

  parsers.conf: |
    [PARSER]
        Name crio
        Format Regex
        Regex ^(?<time>[^ ]+) (?<stream>stdout|stderr) (?<logtag>P|F) (?<log>.*)$
        Time_Key    time
        Time_Format %Y-%m-%dT%H:%M:%S.%L%z
        Time_Keep true

```

**Notes**:

- The log\_router will only be attached to workloads started after the manifest has been applied. **You will need to restart your pods** in order for the log\_router to start forwarding the logs to your Firehose. Find out more about the AWS log router [here](https://docs.aws.amazon.com/eks/latest/userguide/fargate-logging.html).

- You can add additional filters, but they are limited to the following types: `grep, parser, record_modifier, rewrite_tag, throttle, nest, modify, kubernetes`.

- If you wish to add Cloudwatch output, you are required to add additional permissions to your Fargate profile. Find out more [here](https://docs.aws.amazon.com/eks/latest/userguide/fargate-logging.html).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/aws-firehose/"><strong>AWS Kinesis Data Firehose - Logs</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
