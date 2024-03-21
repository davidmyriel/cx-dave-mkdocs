---
title: "OpenTelemetry Custom Metrics"
date: "2023-06-29"
---

Coralogix provides a scalable Prometheus-compatible managed service for time-series data. Employ our **custom metric endpoint**, including serverless computing and quick cURL-like calls, to send counters, gauges, and histograms to Coralogix.

## Overview

Coralogix supports ingesting metrics in multiple ways. Our most common integrations are [Prometheus](https://coralogixstg.wpengine.com/docs/prometheus/) & [OpenTelemetry](https://coralogixstg.wpengine.com/blog/tag/opentelemetry/), as well as metric-specific integrations such as [CloudWatch metrics](https://coralogixstg.wpengine.com/docs/cloudwatch-metrics/) and [AWS Kinesis Firehose](https://coralogixstg.wpengine.com/docs/aws-firehose/). View a full list of available integrations [here](https://coralogixstg.wpengine.com/integrations/).

This tutorial presents a series of use cases employing our custom metric endpoint, referred to as the [OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/), to send your data to Coralogix. The examples below employ gRPCurl and OpenTelemetry Java SDK.

Coralogix metrics employs the [Prometheus data model](https://prometheus.io/docs/concepts/data_model/), wherein metrics sent can be in the form of counters, gauges, and histograms.

## Prerequisites

If you are sending us your data using gRPCurl, you are required to have [Git](https://git-scm.com/) and [gRPCurl](https://github.com/fullstorydev/grpcurl) installed.

## Data Model

The custom logs API implementation is based on the OpenTelemetry [metric specification](https://github.com/open-telemetry/opentelemetry-proto/blob/main/opentelemetry/proto/metrics/v1/metrics.proto). This ensures that our logging implementation adheres to industry best practices and can seamlessly integrate with other components and tools in the OpenTelemetry ecosystem.

### Counter & Gauge Example

```
{
  "resource_metrics": {
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
        },
        {
          "key": "service.name",
          "value": {
            "string_value": "my-test-service"
          }
        }
      ]
    },
    "scope_metrics": {
      "metrics": [{
        "name": "grpc_sample_gauge1",
        "gauge": {
          "data_points": [{
            "as_double": 0.8,
            "start_time_unix_nano": 1657079957000000000,
            "time_unix_nano": 1657079957000000000
          }]
        }
      },{
        "name": "grpc_sample_counter1",
        "gauge": {
          "data_points": [{
            "as_int": 100,
            "start_time_unix_nano": 1657079957000000000,
            "time_unix_nano": 1657079957000000000
          }]
        }
      }]
    }
  }
}

```

**Notes**:

- Currently both timestamps as well as `service.name` are mandatory.

## Sending Data With gRPCurl

gRPC is a modern way of calling APIs on top of HTTP/2. Similar to cURL, [gRPCurl](https://github.com/fullstorydev/grpcurl) is a command-line tool used to communicate with gRPC services.

Coralogix currently supports gRPC for its custom metrics endpoint.

Assuming the example in the data model is saved as `metrics.json`, use the following command to send your data to Coralogix:

```
# Clone OpenTelemetry protobuf definitions
git clone <https://github.com/open-telemetry/opentelemetry-proto.git>
# Send metrics to Coralogix 
grpcurl -v -d @ \\
  -rpc-header 'Authorization: Bearer <send-your-data-api-key>' \\
  -proto opentelemetry-proto/opentelemetry/proto/collector/metrics/v1/metrics_service.proto \\
  -import-path opentelemetry-proto \\
  <open-telemetry-endpoint> \\
  opentelemetry.proto.collector.metrics.v1.MetricsService/Export \\
  < metrics.json

```

**Notes**:

- For `<open-telemetry-endpoint>`, input the Coralogix [OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix domain.

- For the `<send-your-data-api-key>`, input your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

- Set the `start_time_unix_nano` and the `time_unix_nano` in the `metrics.json` to a timestamp that is within the last 24 hours.

## Sending Data Using the OpenTelemetry Java SDK

The example below guides you using OpenTelemetry Java SDK to send your custom traces to Coralogix. Others [SDKs](https://opentelemetry.io/docs/concepts/sdk-configuration/) may also be used.

**STEP 1**. Add to your maven pom.xml the following libraries:

```
<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-sdk-metrics</artifactId>
  <version><!-- put a recent version of opentelemetry sdk here --><version>
</dependency>
<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-exporter-otlp</artifactId>
  <version><!-- put a recent version of opentelemetry sdk here --><version>
</dependency>

```

**STEP 2**. Use this code snippet to generate a counter and a gauge:

```
SdkMeterProvider meterProvider =
      SdkMeterProvider.builder()
        .registerMetricReader(
          PeriodicMetricReader.builder(
            OtlpGrpcMetricExporter.builder()
              .setEndpoint("https://<open-telemetry-endpoint>")
              .addHeader("Authorization", "Bearer <send-your-data-api-key>")
              .build()
          ).build()
        )
        .setResource(Resource.create(Attributes.of(
          ResourceAttributes.SERVICE_NAME, "my-test-service",
          AttributeKey.stringKey("cx.application.name"), "my-test-application",
          AttributeKey.stringKey("cx.subsystem.name"), "my-test-subsystem")))
        .build();

    Meter meter = meterProvider.meterBuilder("test").build();

    LongCounter counter = meter
      .counterBuilder("otlp_test_counter1")
      .setDescription("Processed jobs")
      .build();
    counter.add(100);

    meter
      .gaugeBuilder("otlp_test_gauge1")
      .buildWithCallback(measurement -> {
        measurement.record(0.8);
      });
    meterProvider.forceFlush();

```

**Notes**:

- Currently the `service.name` attribute is mandatory on each metric.

## Limits & Quotas

- Coralogix places a **hard limit of 10MB** of data to our OpenTelemetry [endpoints](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/), with a **recommendation of 2MB**.

- Metric names must be a maximum of 255 characters.

- Attribute keys for metric data must be a maximum of 255 characters.

Limits apply to single requests, regardless of timespan.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/coralogix-endpoints/">Coralogix Endpoints</a></strong></td></tr><tr><td>External</td><td><a href="https://github.com/coralogix/custom-metrics-examples"><strong>GitHub</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
