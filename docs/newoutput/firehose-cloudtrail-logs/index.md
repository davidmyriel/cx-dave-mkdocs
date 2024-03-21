---
title: "AWS CloudTrail Logs via Firehose"
date: "2023-11-27"
---

Streamline the process of ingesting and analyzing logs from your AWS resources using our automated **CloudTrail Logs using AWS Kinesis Data Firehose** integration package.

## Overview

AWS CloudTrail logs are comprehensive records of actions performed within your Amazon Web Services (AWS) account, encompassing details like API calls, configuration changes, and user activities. These logs play a pivotal role in fortifying security, ensuring compliance, and enhancing operational transparency. By sending CloudTrail logs to AWS Kinesis Data Firehose, you streamline the process of ingesting and transforming this data, setting the stage for its efficient delivery to external services like Coralogix.

## Benefits

CloudTrail logs, when processed through Kinesis Data Firehose and sent to Coralogix, enable real-time monitoring, advanced analytics, and proactive alerting. This combined approach equips you with the tools to seamlessly analyze and visualize your AWS audit data, thereby strengthening security, simplifying compliance endeavors, identifying anomalies, and refining operational efficiency. By harnessing the capabilities of both AWS Kinesis Data Firehose and Coralogix, your CloudTrail logs become actionable insights that safeguard your AWS environment and streamline compliance reporting, thus optimizing the overall value of your CloudTrail data.

## **Configuration**

**STEP 1**. From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**.

**STEP 2.** In the Integrations section, select **AWS Cloudtrail Logs via Firehose**.

![](images/AWS-Cloudtrail-Logs-Via-Firehose-Integration-Selection.png)

**STEP 3.** Click **ADD NEW**.

**STEP 4.** Input your integration details.

![](images/AWS-Cloudtrail-Logs-Via-Firehose-Integration-Details.png)

- **Integration Name.** Enter a name for your integration. This will be used as a stack name in CloudFormation.

- **API Key**. Enter your [Send-Your-Data API key](https://www.notion.so/d6f178687d464c58b9988fe223c719cc?pvs=21) or click **CREATE A NEW KEY** to create a new API key for the integration.

- **Application Name.** Enter an [application name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/). The default name is AWS.

- **Subsystem Name.** Enter a [subsystem name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/). The default name is Firehose.

- **Enable Dynamic Metadata**. When enabled, this sets the `applicationName` and `subsystemName` dynamically. When enabled, this feature dynamically sets the `applicationName` based on the name of the CloudWatch log group.

- **Kinesis Stream ARN**. \[**Optional**\] Enter the ARN of the Kinesis stream if using Amazon Kinesis Data Streams as a source for logs.

- **AWS Region.** Select your AWS region from the dropdown menu.

- **AWS PrivateLink (Advanced Settings)**. Enabling the use of AWS PrivateLink is **recommended** in order to ensure a secure and private connection between your VPCs and AWS services. Find out more [here](https://coralogixstg.wpengine.com/docs/coralogix-amazon-web-services-aws-privatelink-endpoints/).

**STEP 6.** Click **NEXT**.

**STEP 7.** Review the instructions for your integration. Click **CREATE CLOUDFORMATION**.

![](images/AWS-Cloudtrail-Logs-Via-Firehose-Create-Cloudformation.png)

**STEP 8.** You will be rerouted to the AWS website. Verify that all of the auto pre-populated values are correct. Click **Create Stack**.

**STEP 9.** Return to the Coralogix application where you will be presented with instructions on how to create a subscription filter inside your CloudWatch log group .

![](images/AWS-Cloudtrail-Logs-Via-Firehose-Final-Steps.png)

**Notes**:

If you provide a Kinesis Stream ARN, Coralogix assumes that the data is in the stream and does not provide any additional instructions. It is the user’s responsibility to deliver data to the stream. Instead of the instructions you will see a message that prompts the user to confirm the integration.

**STEP 10**. Click **COMPLETE** to close the module.

**STEP 11.** \[**Optional**\] Deploy the [extension package](https://coralogixstg.wpengine.com/docs/extension-packages/) of your choice to complement your integration needs. We offer the following extensions for data originating from CloudTrail and WAF:

- AWS CloudTrail

- AWS WAF

**STEP 12.** View the logs by navigating to **Explore** > **Logs** in your Coralogix toolbar. Find out more [here](https://coralogixstg.wpengine.com/docs/logs-screen/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/guide-first-steps-coralogix/"><strong>Getting Started with Coralogix</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
