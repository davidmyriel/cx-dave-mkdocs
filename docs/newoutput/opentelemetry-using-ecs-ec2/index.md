---
title: "AWS ECS-EC2 using OpenTelemetry"
date: "2022-12-27"
---

This tutorial demonstrates how to deploy OpenTelemetry to AWS ECS-EC2 to facilitate the collection of logs, metrics, and traces.

Telemetry is sent to Coralogix via the Coralogix Exporter, which allows for the use of enrichments such as dynamic `application` or `subsystem` name, defined using `application_name_attributes` and `subsystem_name_attributes`, respectively. Find out more [here](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/coralogixexporter).

## [](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#required)Prerequisites

- [Coralogix account](https://signup.coralogixstg.wpengine.com/#/)

- [AWS account](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-creating.html) with [AWS credentials configured](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)

- [ecs-cli](https://github.com/aws/amazon-ecs-cli#installing)

- [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- Coralogix distribution for OpenTelemetry (”CDOT”) ECS Service daemon, deployed using [CloudFormation template](https://github.com/coralogix/cloudformation-corlaogix-aws/tree/master/opentelemetry/ecs-ec2) or [Terraform module](https://github.com/coralogix/terraform-coralogix-aws/tree/master/modules/ecs-ec2)

## [](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#open-telemetry-configuration)Image

This implementation utilises the **Coralogix Opentelemetry Collector** image [coralogixrepo/coralogix-otel-collector](https://hub.docker.com/r/coralogixrepo/coralogix-otel-collector/tags). This image is an enhanced version of the official OpenTelemetry Contrib distribution, featuring custom components and extended configuration loading options, including environment variable support.

Note the latest tag on this image is not updated, you must explicitly select a version when pulling. Tags can be found [here](https://hub.docker.com/r/coralogixrepo/coralogix-otel-collector/tags).

The image configuration utilizes the otlp receiver for both HTTP (on 4318) and GRPC (on 4317). Data can be sent using either endpoint.

## OpenTelemetry Configuration

The OpenTelemetry configuration for the agent is stored in a Base64 encoded environment variable and applied at runtime. This allows you to dynamically pass any configuration values you choose as a parameter to CloudFormation.

The following configuration files work directly with the **coralogixrepo/coralogix-otel-collector** docker image for ECS:

- [logs](https://github.com/coralogix/terraform-coralogix-aws/blob/master/modules/ecs-ec2/otel_config.tftpl.yaml)

- [metrics & traces](https://github.com/coralogix/terraform-coralogix-aws/blob/master/modules/ecs-ec2/otel_config_metrics.tftpl.yaml)

Create other configurations by combining logs, metrics and/or traces.

## [](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#ecs-cluster)Deploy a New ECS Cluster

If you already have an existing ECS cluster, skip this step.

Deploy a new cluster:

```
ecs-cli up --region <region> --keypair <your-key-pair> --cluster <cluster-name> --size <no. of instances> --capability-iam 
```

**Notes**:

- The `--keypair` flag is not mandatory. However, if not supplied, you cannot connect to any of the instances in the cluster via SSH. Create a key pair using the command below:

```
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```

- The `ecs-cli up` command will leverage CloudFormation to create an ECS cluster.

- Default values will be used to create and configure a VPC and Subnets. These and other values can be controlled from `ecs-cli` using the following command:

```
ecs-cli up --help
```

## Deploy OTEL Agent ECS Task Definition & Service

Once an ECS cluster has been deployed, deploy a task definition to be used by ECS to create an ECS service to run OpenTelemetry.

- An AWS **ECS** **task definition** serves as template defining a container configuration.

- An **ECS service** is a configuration item that defines and orchestrates how a task definition should be run.

**Option 1: CloudFormation template**

- Deploy this [CloudFormation template](https://github.com/coralogix/cloudformation-corlaogix-aws/tree/main/opentelemetry/ecs-ec2), with the necessary parameters provided:

- **CDOTImageVersion**: The Coralogix Open Telemetry Distribution Image Version/Tag. Take the most recent tag from [here](https://hub.docker.com/r/coralogixrepo/coralogix-otel-collector/tags) if this is the first deployment.

- **ClusterName**: Name of an **existing** ECS Cluster

- **CoralogixRegion**: [Coralogix Region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) associated with your Coralogix account

- **ApplicationName** & **SubsystemName**: [Application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as they will appear in your Coralogix dashboard

- **PrivateKey**: Your [Coralogix Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

Once the template is deployed successfully, verify that the container is running:

```
ecs-cli ps --region <region> -c <cluster name>
```

**Option 2: Terraform module**

- This [ECS EC2 Open Telemetry Agent Terraform module](https://github.com/coralogix/terraform-coralogix-aws/blob/master/modules/ecs-ec2/README.md) sets up the OTEL Agent as an ECS Daemon Service on a specified ECS cluster. See readme for usage instructions.

## Configure the Application Container with the Location of the OTEL Agent

The Coralogix OTEL Collector is typically deployed as a Daemon Service [Agent](https://opentelemetry.io/docs/collector/deployment/agent/) on each ECS container instance / EC2 instance. The OTEL Agent Task is connected using host networking, so it is accessible via the primary private IP address of the EC2 instance.

**Note**: The steps in this section will **not** apply to Fargate Tasks. These will connect to an OTEL Collector Gateway service deployed in a [Gateway architecture](https://opentelemetry.io/docs/collector/deployment/gateway/), or an OTEL “sidecar” container. Daemon deployments are not supported in Fargate. Use a suitable OTEL service discovery approach.

For ECS Tasks running on EC2, Tasks that are OTEL-instrumented will need to discover the IP address of an OTEL Agent to connect with. The discovery approach will differ based on which [AWS ECS Task networking modes](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking.html) your ECS Tasks utilize. In each case, set the `[OTEL_EXPORTER_OTLP_ENDPOINT` environment variable\]([https://opentelemetry.io/docs/concepts/sdk-configuration/otlp-exporter-configuration/](https://opentelemetry.io/docs/concepts/sdk-configuration/otlp-exporter-configuration/)) to the address of the OTEL Agent.

Your ECS Task should be configured to connect to the OTEL Collector daemon task listening at the primary IP of the EC2 host.

- **Method 1: from EC2 instance metadata v1.** One straightforward, simplest, and recommended way is to extract the primary IP of the EC2 host from the [EC2 instance metadata v1](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html) endpoint. Here we specify http protocol and gRPC protocol at port 4317. Adjust if required.

```
export OTEL_EXPORTER_OTLP_ENDPOINT="http://"$( curl -s http://169.254.169.254/latest/meta-data/local-ipv4 )":4317"
```

- **Method 2: from EC2 instance metadata v2.** As a variation, secure the same call with the [EC2 instance metadata v2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html) security token.

```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/local-ipv4
```

- **Method 3: from ECS container metadata file.** Alternately, if you have the [ECS container metadata file](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-metadata.html#metadata-file-format) enabled, the host private IP can be parsed from the file:

```
cat $ECS_CONTAINER_METADATA_FILE | jq -r .HostPrivateIPv4Address
```

**Set variables during the application container entryPoint**

Update the application container to set `OTEL_EXPORTER_OTLP_ENDPOINT` at startup.

One approach is to override the [ECS Task Definition’s entryPoint](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/example_task_definitions.html#example_task_definition-ping). Prepend setting variables prior to calling the original application command. E.g.:

```
"entryPoint": [
  "sh",
  "-c",
  "export OTEL_EXPORTER_OTLP_ENDPOINT='http://'$( curl -s http://169.254.169.254/latest/meta-data/local-ipv4 )':4317'; <Actual Startup Command>"
]
```

Other approaches could include embedding commands in the Entrypoint script file itself, calling out to a helper script, calling an intermediate entry point wrapper script, etc. Given the wide variety of container configuration options, it is left to the customer to decide on the optimal ways to script and set environment variables.

## Configure the Application Container to Send Identifying Resource Attributes

Applications that are instrumented for OpenTelemetry, can mark their telemetry by specifying attribute name/value pairs in the [`OTEL_RESOURCE_ATTRIBUTES` environment variable](https://opentelemetry.io/docs/concepts/sdk-configuration/general-sdk-configuration/#otel_resource_attributes).

If you decide on Container ID as the resource attribute identifier for telemetry, obtain container ID from ECS Container Metadata endpoint.

```
#!/bin/bash

# Must run within an ECS Docker container
# Requires jq cli tool

# get container ID
containerID=$(curl ${ECS_CONTAINER_METADATA_URI_V4} | jq '.DockerId' -r)

# set env variable for resource attributes
export OTEL_RESOURCE_ATTRIBUTES="containerID=${containerID},$OTEL_RESOURCE_ATTRIBUTES"

```

Like `OTEL_EXPORTER_OTLP_ENDPOINT`, the `OTEL_RESOURCE_ATTRIBUTES` environment variable should be set upon container startup.

## Additional Resources

<table><tbody><tr><td><strong>External</strong></td><td><strong><a href="https://github.com/coralogix/cloudformation-coralogix-aws/blob/master/opentelemetry/ecs-ec2/template.yaml#L376|https://github.com/coralogix/cloudformation-coralogix-aws/blob/master/opentelemetry/ecs-ec2/template.yaml#L376">GitHub Repo</a></strong></td></tr><tr><td><strong>Features</strong></td><td><strong><a href="https://coralogixstg.wpengine.com/docs/apm/" target="_blank" rel="noreferrer noopener">Coralogix APM Features</a><br><a href="https://coralogixstg.wpengine.com/docs/apm-amazon-ec2/">APM using Amazon EC2</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
