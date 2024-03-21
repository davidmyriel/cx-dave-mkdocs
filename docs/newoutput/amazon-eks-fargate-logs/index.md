---
title: "AWS EKS Fargate Logs"
date: "2022-08-09"
---

Seamleslly collect and send your `EKS Fargate` cluster logs straight to Coralogix.

Amazon EKS on Fargate offers a built-in log router based on Fluent Bit. Using the side-car pattern, all logs captured by the container will be sent to Fluent Bit and Coralogix.

## General

**Private Key** – Your [Send Your Data - API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) is a unique token that represents your company

**Application Name** – The name of your main application or the environment of the application

**SubSystem Name** – Your application probably has multiple subsystems. For example: Backend servers, Middleware, Frontend servers, etc. in order to help you examine the data you need, inserting the subsystem parameter is vital.

## Configuration

To enable the built-in log router AWS provides, create a kubernetes manifest file named coralogix-logger.yaml with the FluentBit configMap named `aws-logging` in the `aws-observability` namespace.

```yaml
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
        region <AWS_Region>
        delivery_stream <Delivery_Stream_Name>
  parsers.conf: |
    [PARSER]
        Name crio
        Format Regex
        Regex ^(?<time>[^ ]+) (?<stream>stdout|stderr) (?<logtag>P|F) (?<log>.*)$
        Time_Key    time
        Time_Format %Y-%m-%dT%H:%M:%S.%L%z
        Time_Keep true
```

Note: change the <Delivery\_stream\_Name> to your kinesis delivery stream name and <AWS\_Region> to the delivery stream region.

Then apply the manifest file to your EKS cluster

```bash
kubectl apply -f coralogix-logger.yaml
```

Now we need to add to the pod execution role a policy that allows pods to send record to firehose.

Go to AWS EKS -> Clusters -> 'Your-cluster' -> Compute -> 'Your-Fargate-Profile' -> Pod execution role

```json
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

Note: Each fargate profile has a different execution role so apply this step to all your profiles.

To apply all these changes the pods have to restart.

That's it all logs will be sent to your firehose delivery stream.

Note: don't forget to follow our guide on [Configuring firehose](https://coralogixstg.wpengine.com/docs/aws-firehose/) to make sure you have your firehose configured correctly.
