---
title: "Open Commerce API"
date: "2022-08-01"
---

Coralogix provides an easy way to collect your Open Commerce OrderSearch API logs.  
The preferred and easiest integration method will be to use our app in the [AWS Serverless Application Repository](https://eu-central-1.console.aws.amazon.com/serverlessrepo/home?region=eu-central-1#/available-applications).

## Requirements

- Your AWS user should have permissions to create lambdas and IAM roles.

## Installation

- Navigate to the [Application Page](https://eu-central-1.console.aws.amazon.com/lambda/home?region=eu-central-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-ocapi).

- Fill in the required parameters.

- Click Deploy.

Once the deployment is done the lambda will execute every X minutes based on the ‘lambda schedule’ parameter.

## Parameters and Descriptions

<table><tbody><tr><td>Variable</td><td><strong>Description</strong></td></tr><tr><td>Application name</td><td>The stack name of this application created</td></tr><tr><td>NotificationEmail</td><td>Failure notification email address</td></tr><tr><td>ApplicationName</td><td>Application Name in Coralogix</td></tr><tr><td>CoralogixRegion</td><td>The Coralogix region [EU1, EU2, US1, US2, AP1 (India), AP2 (Singapore)] associated with your Coralogix <a href="https://coralogixstg.wpengine.com/docs/coralogix-domain/">domain</a></td></tr><tr><td>FunctionArchitecture</td><td>Lambda function architecture [x86_64, arm64]</td></tr><tr><td>FunctionMemorySize</td><td>Lambda function memory limit</td></tr><tr><td>FunctionSchedule</td><td>Lambda function schedule in minutes, the function will be invoked each X minutes. After deploy, first invocation will be after X minutes.</td></tr><tr><td>FunctionTimeout</td><td>Lambda function timeout limit</td></tr><tr><td>LogsToStdout</td><td>Send logs to stdout/cloudwatch. Possible values are&nbsp;<code>True</code>,&nbsp;<code>False</code></td></tr><tr><td>OcapiEndpoint</td><td>The full endpoint to the orderSearch API</td></tr><tr><td>OcapiPassword</td><td>The OCAPI password used for authentification</td></tr><tr><td>OcapiUsername</td><td>OCAPI username. used to get authenticated.</td></tr><tr><td>PrivateKey</td><td>Coralogix <a href="https://coralogixstg.wpengine.com/docs/send-your-data-api-key/" target="_blank" rel="noreferrer noopener">Send-Your-Data API key</a></td></tr><tr><td>SelectStatement</td><td>The select statement to be used in the query. Default to&nbsp;<code>(*)</code></td></tr><tr><td>SubsystemName</td><td>Subsystem name in Coralogix</td></tr></tbody></table>
