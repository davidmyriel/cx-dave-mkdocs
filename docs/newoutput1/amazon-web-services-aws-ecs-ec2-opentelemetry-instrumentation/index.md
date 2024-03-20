---
title: "AWS ECS-EC2 OpenTelemetry Instrumentation"
date: "2023-03-21"
---

This tutorial demonstrates how to deploy [OpenTelemetry](https://coralogixstg.wpengine.com/docs/opentelemetry/) to ECS to facilitate the collection of logs, metrics, and traces, and send them to Coralogix.

## Image

This implementation utilizes the wrapper image `coralogixrepo/otel-coralogix-ecs-ec2`, based on the official OpenTelemetry contrib image.

**Notes**:

- The wrapper image is used to dynamically apply the OpenTelemetry configuration at runtime from an environment variable.

- The image configuration utilizes the otlp receiver for both HTTP (on 4318) and GRPC (on 4317). Data can be sent using either endpoint.

## Prerequisites[](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#image)

- Coralogix [account](https://signup.coralogixstg.wpengine.com/#/)

- AWS credentials [configured](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html)

- [ecs-cli](https://github.com/aws/amazon-ecs-cli#installing)

- [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- Coralogix Otel ECS Agent [CloudFormation template](https://github.com/coralogix/cloudformation-corlaogix-aws/tree/master/opentelemetry/ecs-ec2)

## Configuration

### OpenTelemetry

The OpenTelemetry configuration for the agent is stored in a Base64-encoded environment variable and is applied at runtime. This allows you to dynamically pass any configuration values you choose as a parameter to CloudFormation.

Use either of the following configuration examples or combine them to send Coralogix your logs, metrics, and traces.

- [logs](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/ecs-ec2/logging.yaml)

- [metrics and traces](https://github.com/coralogix/telemetry-shippers/blob/master/otel-agent/ecs-ec2/config.yaml)

**Notes**:

- The configuration examples work directly with the `coralogixrepo/otel-coralogix-ecs-wrapper` docker image for ECS.

- The [Coralogix Exporter](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/coralogixexporter) enables the use of enrichments such as dynamic `application` or `subsystem` name, defined using `application_name_attributes` and `subsystem_name_attributes`, respectively.

### [](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#ecs-cluster)ECS Cluster

Deploy a new ECS cluster. If you already have an existing ECS Cluster, skip this step.

**STEP 1**. Deploy a new ECS cluster:

```
ecs-cli up --region <region> --keypair <your-key-pair> --cluster <cluster-name> --size <no. of instances> --capability-iam 
```

**Note**: The `--keypair` flag and STEP 2 are not mandatory. However, if no Key Pair is supplied, you will be unable to connect to any of the EC2 instances in the cluster via SSH.

**STEP 2**. Create a key pair using the command below:

```
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem
```

**STEP 3**. Control default values.

The `ecs-cli up` command will leverage CloudFormation to create an ECS Cluster. Default values will be used to create and configure a VPC and Subnets. These values can also be controlled using:

```
ecs-cli up --help
```

### [](https://github.com/coralogix/telemetry-shippers/tree/master/otel-agent/ecs-ec2#ecs-task-definition--service)ECS Task Definition & Service

Deploy a [Task Definition](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html), used by ECS to create an [ECS Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) to run OpenTelemetry.

**STEP 1**. Deploy [this CloudFormation template](https://github.com/coralogix/cloudformation-corlaogix-aws/tree/main/opentelemetry/ecs-ec2), ensuring that all the necessary parameters are provided. You are required to input:

- Coralogix [Send Your Data - API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

- Coralogix region associated with your account [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/)

- Coralogix [application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/)

**STEP 2**. Once the template is deployed, verify that the container is running:

```
ecs-cli ps --region <region> -c <cluster name>
```

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
