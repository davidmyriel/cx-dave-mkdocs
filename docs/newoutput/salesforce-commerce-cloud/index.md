---
title: "Salesforce Commerce Cloud"
date: "2022-06-27"
---

Coralogix provides an easy way to collect your Salesforce logs. The preferred and easiest integration method will be to use our app in the [AWS Serverless Application Repository](https://eu-central-1.console.aws.amazon.com/serverlessrepo/home?region=eu-central-1#/available-applications).

## Requirements

- Your AWS user should have permissions to create lambdas and IAM roles.

## Installation

- Navigate to [Application Page](https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-sfcc-webdav).

- Fill in the required parameters.

- Click Deploy.

Once the deployment is done the lambda will execute every X minutes based on the 'lambda schedule' parameter.

## Parameters and Descriptions

| Variable | Description |
| --- | --- |
| Application Name | The stack name of this application created via AWS CloudFormation |
| NotificationEmail | Failure notification email address |
| ApplicationName | Application Name in Coralogix |
| CoralogixRegion | The region associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) |
| FunctionArchitecture | Our Function supports x86\_64 or arm64 |
| FunctionMemorySize | Max Memory for the function itself. |
| FunctionSchedule | The lambda function schedule invoke in minutes |
| FunctionTimeout | Lambda function timeout |
| PrivateKey | Your Coralogix account [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) |
| SfccEndpoints | Salesforce commerce cloud endpoints to check logs in, in a DOMAIN/PATH format. Multiple can be inserted with a comma in between. |
| SfccPassword | Salesforce commerce cloud password used for authentication |
| SfccUsername | Salesforce commerce cloud username used for authentication |
| SubsystemName | Subsystem name in Coralogix |
