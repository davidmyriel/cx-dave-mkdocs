---
title: "AWS S3 Log Collection via SNS Trigger"
date: "2023-03-05"
---

Like our [S3 Log collection integration](https://coralogixstg.wpengine.com/docs/data-collection-s3/), this integration will process logs from you S3 buckets, but is triggered by an SNS notification. For easy setup, use our app in the [AWS Serverless Application Repository](https://eu-central-1.console.aws.amazon.com/serverlessrepo/home?region=eu-central-1#/available-applications).

## Prerequisites

- Ready-made SNS Topic

- Ready-made S3 bucket with configured Event Notifications to above SNS Topic

- AWS permissions to create Lambdas and IAM roles

## Installation

**STEP 1**. Navigate to the [application page](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-S3-via-SNS) and search for Coralogix-S3-via-SNS.

**STEP 2**. Fill in the required parameters.

**STEP 3**. Click **Deploy**.

## Parameters & Descriptions

| Variable | Description |
| --- | --- |
| **Application Name** | Stack name of the application created via AWS CloudFormation.  
If your log is JSON format, use its dynamic value.  
Example: $.level1.level2.value |
| **NotificationEmail** | Failure notification email address |
| **ApplicationName** | [**Application name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as it appears in your Coralogix UI |
| **BlockingPattern** | If you wish to block some of the logs, adding a substring will act as a selector.  
Default is empty to send all logs. |
| **BufferSize** | Buffer size for logs in the Lambda function |
| **CoralogixRegion** | Coralogix region associated with your [**Coralogix domain**](https://coralogixstg.wpengine.com/docs/coralogix-domain/) |
| **CustomDomain** | Coralogix custom domain. Leave empty if you do not use a custom domain. |
| **Debug** | Coralogix logger debug mode |
| **FunctionArchitecture** | Function supports x86\_64 or arm64 |
| **FunctionMemorySize** | Max memory for the function itself |
| **FunctionTimeout** | Maximum time in seconds the function may be allowed to run |
| **NewlinePattern** | Pattern for lines splitting. Default is (?:\\r\\n|\\r|\\n). |
| **PrivateKey** | **[Coralogix Send-Your-Data API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)** |
| **S3BucketName** | Name of the S3 bucket to watch |
| **SNSTopicArn** | ARN of SNS topic to subscribe |
| **SamplingRate** | Sets the sampling rate  
The rate is set to 1 by default, meaning that it collects every log message from the S3 bucket. Increase it to change the sampling rate \[i.e. increase it to 2 to ship 1 of every 2 logs, etc\]. |
| **SubsystemName** | [**Subsystem name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as it appears in your Coralogix UI.  
If your log is JSON format, can use its dynamic value, for example: $.level1.level2.value. |

**Notes:**

- To perform filtering based on prefix or suffix of the S3 object, filters need to be placed on the S3 bucket’s Event Notification. Documentation for this process can be found [here](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-event-notifications.html).

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
