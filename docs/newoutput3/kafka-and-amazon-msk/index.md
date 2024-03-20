---
title: "AWS MSK & Kafka"
date: "2021-04-14"
---

Use Coralogix Kafka lambdas to seamlessly send Kafka Topics data to Coralogix.

## Overview

Many organizations collect important operational and business data in Kafka topics and would like to aggregate, analyze, and correlate it with data collected from other sources, in order to enable a better view of the technology and business stack and improve its management.

Use Coralogix Kafka lambdas to seamlessly send Kafka Topics data to Coralogix with our [AWS serverless application repository](https://eu-central-1.console.aws.amazon.com/serverlessrepo/home?region=eu-central-1#/available-applications) to gain these desired insights into your data.

## Requirements

- AWS account with permissions to create lambdas and IAM roles

- A ready-made MSK cluster

- A ready-made Kafka cluster

## Installation

**STEP 1**. Navigate to this [application page](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-MSK).

**STEP 2**. Fill in the required parameters

**STEP 3**. Check this checkbox: "I acknowledge that this app creates custom IAM roles."

**STEP 4**. Click **Deploy**.

## Parameters and Descriptions

| Variable | Description |
| --- | --- |
| **Application Name** | Stack name of this application created via AWS CloudFormation |
| **ApplicationName** | [**Application name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as it appears in your Coralogix UI  
If your log is JSON format, use its dynamic value, for example: `$.level1.level2.value` |
| **CoralogixRegion** | Region \[Europe, Europe2, India, Singapore, or US\] associated with your Coralogix account [**domain**](https://coralogixstg.wpengine.com/docs/coralogix-domain/)  
In case that you want to use Custom domain, leave this as default and write the custom domain in the `CustomDomain` filed. |
| **CustomDomain** | Coralogix custom domain. Leave empty if you do not use a custom domain. |
| **FunctionArchitecture** | Function supports x86\_64 or arm64 |
| **FunctionMemorySize** | Max memory for the function itself |
| **FunctionTimeout** | Maximum time in seconds the function may be allowed to run |
| **MSKClusterArn** | ARN of the Amazon MSK Kafka cluster |
| **NotificationEmail** | Failure notification email address |
| **PrivateKey** | Coralogix [**Send-Your-Data API Key**](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) |
| **SsmEnabled** | **True**, if you want to store your coralogix private\_key as a secret  
**False**, if you do not |
| **SubsystemName** | [**Subsystem name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) as it appears in your Coralogix UI  
If your log is in JSON format, use its dynamic value, for example: `$.level1.level2.value`. |
| **Topic** | Name of the Kafka topic used to store records in your Kafka cluster |
| **LayerARN** | Coralogix SSM Layer ARN |

**Notes**:

- You can dynamically set the [application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) by setting the corresponding parameter above with a filter string with the following syntax: `$.first_key.additional_key`.

- The example `$.computedValues.functionName` uses the functionName of a computedValues array as your dynamic value.

## Automation

You can include SAM (Serverless Application Model) in your [automation frameworks](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-deploying.html). If you need access to the latest and greatest Lambda code go to [coralogix-aws-serverless/src at master · coralogix/coralogix-aws-serverless](https://github.com/coralogix/coralogix-aws-serverless/tree/master/src).

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
