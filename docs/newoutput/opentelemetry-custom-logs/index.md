---
title: "OpenTelemetry Custom Logs"
date: "2023-06-29"
---

Send your **custom logs** to Coralogix using our OpenTelemetry-compatible endpoint.

## Overview

Custom logs can be delivered directly to the Coralogix OpenTelemetry-compatible [endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) using any gRPC client or [OpenTelemetry SDKs](https://opentelemetry.io/docs/concepts/sdk-configuration/).

The examples below guide you using gRPCurl and OpenTelemetry Java SDK.

## Prerequisites

If you are sending us your data using gRPCurl, you are required to have [Git](https://git-scm.com/) and [gRPCurl](https://github.com/fullstorydev/grpcurl) installed.

## Data Model

The custom logs API implementation is based on the OpenTelemetry [logging specification](https://github.com/open-telemetry/opentelemetry-proto/blob/main/opentelemetry/proto/logs/v1/logs.proto). This ensures that our logging implementation adheres to industry best practices and can seamlessly integrate with other components and tools in the OpenTelemetry ecosystem.

### Example Log

```
{
  "resource_logs": [
    {
      "resource": {
        "attributes": [
          {
            "key": "cx.application.name",
            "value": {
              "string_value": "my-test-application"
            }
          },
          {
            "key": "cx.subsystem.name",
            "value": {
              "string_value": "my-test-subsystem"
            }
          }
        ]
      },
      "scope_logs": [
        {
          "scope": {
            "name": "test"
          },
          "log_records": [
            {
              "time_unix_nano": "1665989944490035000",
              "severity_number": "SEVERITY_NUMBER_WARN",
              "severity_text": "WARN",
              "body": {
                "string_value": "Test log message"
              }
            }
          ]
        }
      ]
    }
  ]
}

```

## Sending Data With gRPCurl

gRPC is a modern way of calling APIs on top of HTTP/2. Similar to cURL, [gRPCurl](https://github.com/fullstorydev/grpcurl) is a command-line tool used to communicate with gRPC services.

Coralogix currently supports gRPC for its custom logs endpoint. REST APIs will be added in the future.

Assuming the example in the data model is saved as `logs.json`, use the following command to send your data to Coralogix:

```
# Clone OpenTelemetry protobuf definitions
git clone <https://github.com/open-telemetry/opentelemetry-proto.git>
# Send logs to Coralogix 
grpcurl -v -d @ \\
  -rpc-header 'Authorization: Bearer <send-your-data-api-key>' \\
  -proto opentelemetry-proto/opentelemetry/proto/collector/logs/v1/logs_service.proto \\
  -import-path opentelemetry-proto \\
  <open-telemetry-endpoint> \\
  opentelemetry.proto.collector.logs.v1.LogsService/Export \\
  < logs.json

```

**Notes**:

- For `<open-telemetry-endpoint>`, input the Coralogix [OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

- For the `<send-your-data-api-key>`, input your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

- Set the `time_unix_nano` in the `logs.json` to a timestamp that is within the last 24 hours.

## Sending Data Using the OpenTelemetry Java SDK

The example below guides you using OpenTelemetry Java SDK to send your custom logs to Coralogix. Others [SDKs](https://opentelemetry.io/docs/concepts/sdk-configuration/) may also be used.

**STEP 1**. Add to your maven pom.xml the following libraries:

```
<dependency>
	<groupId>io.opentelemetry</groupId>
	<artifactId>opentelemetry-sdk-logs</artifactId>
  <version><!-- put a recent version of opentelemetry sdk here --><version>
</dependency>
<dependency>
	<groupId>io.opentelemetry</groupId>
	<artifactId>opentelemetry-exporter-otlp-logs</artifactId>
  <version><!-- put a recent version of opentelemetry sdk here --><version>
</dependency>

```

**STEP 2**. Use the following code snippet to create and report a span:

```
SdkLoggerProvider loggerProvider =
      SdkLoggerProvider.builder()
        .addLogRecordProcessor(BatchLogRecordProcessor.builder(
          OtlpGrpcLogRecordExporter.builder()
            .setEndpoint("https://<open-telemetry-endpoint>")
            .addHeader("Authorization", "Bearer <send-your-data-api-key>")
            .build()
        ).build())
        .setResource(Resource.create(Attributes.of(
          AttributeKey.stringKey("cx.application.name"), "my-test-application",
          AttributeKey.stringKey("cx.subsystem.name"), "my-test-subsystem")))
        .build();

    Logger logger = loggerProvider.loggerBuilder("test").build();

    logger.logRecordBuilder()
      .setSeverity(Severity.WARN)
      .setSeverityText("WARN")
      .setBody("Test log message")
      .emit();

    loggerProvider.forceFlush();

```

## Limits & Quotas

Coralogix places a **hard limit of 10MB** of data to our OpenTelemetry [endpoints](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/), with a **recommendation of 2MB**.

Limits apply to single requests, regardless of timespan.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/coralogix-endpoints/">Coralogix Endpoints</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
