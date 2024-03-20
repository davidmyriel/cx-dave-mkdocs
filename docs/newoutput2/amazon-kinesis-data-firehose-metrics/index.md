---
title: "AWS Kinesis Data Firehose - Metrics"
date: "2022-07-26"
---

[Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/) delivers real-time streaming data to destinations like Amazon Simple Storage Service (Amazon S3), Amazon Redshift, or Amazon OpenSearch Service (successor to Amazon Elasticsearch Service), and now supports delivering streaming data to Coralogix. There is no limit on the number of delivery streams, so it can be used for getting data from multiple AWS services.

Coralogix is an AWS Partner Network (APN) Advanced Technology Partner with AWS  Competencies in DevOps. The platform enables you to easily explore and analyze logs, metrics, and traces to gain deeper insights into the state of your applications and AWS infrastructure. You can analyze all your AWS service metrics to uncover trends in your AWS services.

Using Coralogix with Amazon Kinesis Data Firehose offers a few significant benefits compared with other solutions:

- It keeps monitoring simple.

- It integrates flawlessly.

- It's flexible with minimum maintenance.

- Scale, scale, scale.

## Prerequisites

- Metrics bucket configured in your Coralogix dashboard \[In your Coralogix toolbar, navigate to **Data Flow** > Setup Archive.\]

## Configuration

**STEP 1**. Go to the Kinesis Data Firehose console and choose 'Create delivery stream'.

**STEP 2**. Under 'Choose source and destination'.

- Source: Direct PUT

- Destination: Coralogix 

- Delivery stream name: Fill in the desired stream name.

**STEP 3**. Scroll down to 'Destination settings'.

- HTTP endpoint URL: Choose the [Firehose endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

- Private key: Enter your Coralogix [Send Your Data - API Key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

- Content encoding: Select GZIP.

- Retry duration: Choose 300 seconds.

**STEP 4**. Scroll down to 'Parameters':

By default, your delivery stream arn and name will be used as 'applicationName' and 'subsystemName'. To override the associated 'applicationName' or 'subsystemName', add a new parameter with the desired value.

- Key: 'applicationName' , value - 'new-app-name'

- Key: 'subsystemName' , value - 'new-subsystem-name'

The source of the data in firehose determines the 'integrationType' parameter value:

- Key: ‘integrationType' , value: ‘CloudWatch\_Metrics\_OpenTelemetry070'

**STEP 5**. Scroll down to 'Backup settings':

- Source record backup in Amazon S3: We suggest selecting '**Failed data only'**.

- S3 backup bucket: Choose an existing bucket or create a new one.

- Buffer hints, compression, encryption: Leave these fields as is.

**STEP 6**. Review your settings and choose 'Create delivery stream'.

Metrics subscribed to your delivery stream will be immediately sent and available for analysis within Coralogix.

## Data Source Configuration

### Cloudwatch Metrics

To start sending your metrics to coralogix you first need to create a metric stream.

**STEP 1**. Go to the Cloudwatch console and choose 'Streams' under the 'Metrics' side menu.

**STEP 2**. Click on 'Create metric stream'.

**STEP 3**. Under 'Metrics to be streamed':

- Choose what metrics to send

**STEP 4**. Scroll down to 'Configuration':

- For 'Select configuration option' choose 'Select an existing Firehose owned by your account'

- For 'Select your Kinesis Data Firehose stream' choose the delivery stream created above

**STEP 5**. Scroll down to 'Change output format'

- Make sure that 'OpenTelemetry 0.7' is selected

**STEP 6**. Scroll down to 'Custom metric stream name' and pick a name for the metrics stream.

**STEP 7**. Scroll down and click on 'Create metric stream'.

After a few minutes, the metrics will start streaming to coralogix and you will see them on the Grafana dashboard.

## Transformation Lambda \[optional\]

[](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation)[CloudWatch Metric Streams Lambda transformation](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation) function can be used as a Kinesis Firehose transformation function to enrich the metrics from CloudWatch Metric Streams with AWS resource tags.

This installation is **optional** and you can install the transformation Lambda if you’d like to take advantage of having AWS resource tags as labels in your metrics data. Find out more [here](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation/blob/main/README.md).

Take the following steps to install the transformation Lambda.

**STEP 1.** Create a new AWS Lambda function in your designated region with the following parameters:

- Runtime: Custom runtime on Amazon Linux 2

- Handler: bootstrap

- Architecture: arm64

**STEP 2.** Download the function ZIP file from the [releases](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation/releases) in the repository. Unless instructed otherwise, use the latest release. Upload the function.zip as the code source for Lambda.

**STEP 3.** Make sure to set the memory. We **recommend** starting with `128 MB`. Depending on the number of metrics you export and the speed of Lambda processing, you may consider increasing it.

**STEP 4.** Adjust the role of the Lambda function as described in the [necessary permissions](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation###necessary-permissions).

**STEP 5.** Optionally, add environment variables to configure the Lambda, as described in the [configuration](https://github.com/coralogix/cloudwatch-metric-streams-lambda-transformation#configuration).

**STEP 6.** The Lambda function is ready to be used in [Kinesis Data Firehose Data Transformation](https://docs.aws.amazon.com/firehose/latest/dev/data-transformation.html?icmpid=docs_console_unmapped). Please note the function ARN and provide it in the relevant section of the Kinesis Data Firehose configuration.

In order to prevent a delay in the delivery of your data, and depending on the size of your setup, we **recommend** adjusting your Lambda [buffer hint](https://docs.aws.amazon.com/firehose/latest/dev/data-transformation.html) and Kinesis Data Firehose [buffer size](https://docs.aws.amazon.com/firehose/latest/dev/basic-deliver.html#frequency) configuration accordingly. For optimal experience, we **recommend** setting the Lambda buffer hint to `0.2 MB` and the Kinesis Data Firehose buffer size to `1 MB`. **Note that this may cause more frequent Lambda runs, which may result in higher costs**.

You can check the staleness of your data in your Kinesis Data Firehose delivery stream in the Monitoring tab by looking at the ‘Delivery to HTTP endpoint data freshness’. If you see the staleness value grow, this may indicate the Lambda function runs are too slow. In such a case, you may increase the Lambda’s memory and set the buffering configuration as described above.

## Destination Errors

View these common destinations errors and their possible solutions.

<table><tbody><tr><td><strong>Message</strong></td><td><strong>Solution</strong></td></tr><tr><td>The delivery timed out before a response was received and will be retried. If this error persists, contact the AWS Firehose service team.</td><td>None needed - no data loss</td></tr><tr><td>Delivery to the endpoint was unsuccessful. See Troubleshooting HTTP Endpoints in the Firehose documentation for more information. Response received with status code 502.</td><td>Coralogix returned HTTP 502 error code, firehose will resend the data. None needed - no data loss</td></tr></tbody></table>

## Limitations

CloudWatch Metric Streams does not send metrics that are older than 2 hours. This means that some CloudWatch metrics are calculated at the end of a day and reported with the beginning timestamp of the same day. This includes S3 daily storage metrics and some billing metrics.

Should you need these metrics, we **recommend** using [Cloudwatch Exporter using Prometheus](https://github.com/prometheus/cloudwatch_exporter) alongside our new CloudWatch integration designed to retrieve those metrics. For updated information, contact **Coralogix Support**.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/tutorials/guide-first-steps-coralogix/"><strong>Getting Started with Coralogix</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
