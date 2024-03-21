---
title: "AWS CoudTrail Log Collection via SNS Trigger"
date: "2023-06-01"
---

Coralogix provides a predefined Lambda function to easily forward your CloudTrail logs through SNS to the Coralogix platform. For easy setup, use our app in the [AWS serverless application repository](https://serverlessrepo.aws.amazon.com/applications/eu-central-1/597078901540/Coralogix-CloudTrail-via-SNS).

## Prerequisites

- Active CloudTrail account

- Ready-made SNS topic with permissions `SNS:Publish` to the bucket

- Ready-made CloudTrail S3 bucket with configured event notifications to the above SNS topic

- AWS permissions to create Lambdas and IAM roles

## Installation

**STEP 1**. Navigate to the [application page](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-CloudTrail-via-SNS) and search for Coralogix-CloudTrail-via-SNS.

**STEP 2**. Fill in the required parameters.

**STEP 3**. Click **Deploy**.

## Parameters

<table><tbody><tr><td><strong>Parameter</strong></td><td><strong>Description</strong></td></tr><tr><td><strong>Application Name</strong></td><td>Stack name of the application created via AWS CloudFormation</td></tr><tr><td><strong>ApplicationName</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/"><strong>Application name</strong></a> as it will be seen in your Coralogix UI</td></tr><tr><td><strong>SubsystemName</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/"><strong>Subsystem name</strong></a> as it will appear in your Coralogix UI</td></tr><tr><td><strong>NotificationEmail</strong></td><td>A notification email will be sent to this address via SNS if the Lambda fails.<br>Requires you have a working SNS with a validated domain</td></tr><tr><td><strong>S3BucketName</strong></td><td>Name of the S3 bucket with CloudTrail logs to watch.<br>Must be in the same region as the stack that you create</td></tr><tr><td><strong>SNSTopicARN</strong></td><td>ARN of the SNS topic.<br>Must be in the same region as the S3 bucket</td></tr><tr><td><strong>CoralogixRegion</strong></td><td>Region associated with your Coralogix <a href="https://coralogixstg.wpengine.com/docs/coralogix-domain/"><strong>domain</strong></a></td></tr><tr><td><strong>FunctionArchitecture</strong></td><td>Lambda function architecture. Possible options: x86_64, arm64</td></tr><tr><td><strong>FunctionMemorySize</strong></td><td>Maximum allocated memory this Lambda may consume. Do <strong>not</strong> change default, which is set to 1024.</td></tr><tr><td><strong>FunctionTimeout</strong></td><td>Maximum time (seconds) that the function may be allowed to run. Do <strong>not</strong> change default, which is set to 300.</td></tr><tr><td><strong>PrivateKey</strong></td><td>Coralogix <a href="https://coralogixstg.wpengine.com/docs/send-your-data-api-key/"><strong>Send-Your-Data API Key</strong></a></td></tr></tbody></table>

**Notes**:

- Do not change the `**FunctionMemorySize**` and `**FunctionTimeout**` parameters.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/aws-cloudtrail/">AWS CloudTrail</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [**support@coralogixstg.wpengine.com**](mailto:support@coralogixstg.wpengine.com).
