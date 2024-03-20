---
title: "Custom Metrics"
date: "2022-07-06"
---

Coralogix provides a scalable Prometheus-compatible managed service for time-series data.

## Overview

Coralogix supports ingesting metrics in multiple ways. Our most common integrations are [Prometheus](https://coralogixstg.wpengine.com/docs/prometheus/) & [OpenTelemetry](https://coralogixstg.wpengine.com/blog/tag/opentelemetry/), and we also have metrics integrations such as [Cloudwatch metrics](https://coralogixstg.wpengine.com/docs/cloudwatch-metrics/) and [AWS Kinesis firehose](https://coralogixstg.wpengine.com/docs/aws-firehose/).

This tutorial presents a series of use cases employing our custom metric endpoint, referred to [here](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) as the **Otel endpoint**, including serverless computing and quick cURL-like calls to send counters and gauges to Coralogix.

[View this GitHub repo](https://github.com/coralogix/custom-metrics-examples) for SDK examples of the custom metrics endpoint for grpcurl, Java, and Go.

## Data Model

Coralogix metrics follow the Prometheus data model, and metrics can be one of the following: Counter, Gauge, and Histogram (Read more about [Prometheus data model](https://prometheus.io/docs/concepts/data_model/)).

The Custom Metric API implementation is based on the stable OpenTelemetry Metric [Spec](https://github.com/open-telemetry/opentelemetry-proto/blob/main/opentelemetry/proto/metrics/v1/metrics.proto).

Here's a sample of both a counter and a gauge:

```
{
  "resource_metrics": {
    "scope_metrics": {
      "metrics": [{
        "name": "grpc_sample_gauge1",
        "gauge": {
          "data_points": [{
            "as_double": 0.8,
            "attributes": [{
              "key": "service.name",
              "value": {
                "string_value": "test-service"
              }
              }],
              "start_time_unix_nano": 1657079957000000000,
              "time_unix_nano": 1657079957000000000
          }]
        }
      },{
        "name": "grpc_sample_counter1",
        "gauge": {
          "data_points": [{
            "as_int": 100,
            "attributes": [{
              "key": "service.name",
              "value": {
                "string_value": "test-service"
              }
              }],
              "start_time_unix_nano": 1657079957000000000,
              "time_unix_nano": 1657079957000000000
          }]
        }
      }]
    }
  }
}
```

\* Currently both timestamps as well as `service.name` are mandatory.

## Sending Data with grpcurl

gRPC is a modern way of calling APIs on top of HTTP/2. Similar to cURL, [grpcurl](https://github.com/fullstorydev/grpcurl) is a command-line tool used to communicate with gRPC services.

Coralogix currently supports gRPC for its custom metrics endpoint.

Assuming the example in the data model is saved as "sample.json", the following command will send it to Coralogix:

```
grpcurl -v -d @ -H 'Authorization: Bearer e0cxxxx-xxxx-xxxx-xxxx-xxxa08b' <custom-metrics-endpoint> opentelemetry.proto.collector.metrics.v1.MetricsService/Export <sample.json
```

- For `<custom-metrics-endpoint>`, input the Coralogix **[OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/)** associated with your Coralogix domain.

- For the Authorization key, input your Coralogix [](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)**[Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)**.

## Using Java

While there are many OpenTelemetry SDKs in this tutorial we will be using Java.

In order to get started quickly with OpenTelemetry SDK and Coralogix, follow this sample project.

Add to your maven pom.xml the following libraries:

```
<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-sdk-metrics</artifactId>
</dependency>
<dependency>
  <groupId>io.opentelemetry</groupId>
  <artifactId>opentelemetry-exporter-otlp</artifactId>
</dependency>
```

Code snippet to generate a counter and a gauge:

```
SdkMeterProvider meterProvider = 
      SdkMeterProvider.builder()
        .registerMetricReader(
          PeriodicMetricReader.builder(
            OtlpGrpcMetricExporter.builder()
              .setEndpoint("<custom-metrics-endpoint>")
              .addHeader("Authorization", "Bearer e0cxxxx-xxxx-xxxx-xxxx-xxxa08b")
          .build())
        .build())
      .build();
                
    Meter meter = meterProvider.meterBuilder("test").build();
    
    LongCounter counter = meter
      .counterBuilder("otlp_test_counter1")
      .setDescription("Processed jobs")
      .build();

    counter.add(
      100l, 
      Attributes.of(AttributeKey.stringKey("service.name"), "my-test-service")
    );
    
    meter
      .gaugeBuilder("otlp_test_gauge1")
      .buildWithCallback(measurement -> {
        measurement.record(0.8, Attributes.of(AttributeKey.stringKey("service.name"), "my-test-service"));
      });

    meterProvider.forceFlush();
```

\* Currently the `service.name` attribute is mandatory on each metric.

## Limits & Quotas

Coralogix places the following limits on endpoints:

- A **hard limit of 10MB** of data to our **OpenTelemetry** **endpoint**, with a **recommendation of 2MB**

- A **hard limit of 2411724 bytes** of data to our **Prometheus RemoteWrite** **endpoint**, with a **recommendation** for any amount less than this limit

Limits apply to single requests, regardless of timespan.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/coralogix-endpoints/"><strong>Coralogix Endpoints</strong></a></td></tr><tr><td>GitHub</td><td><a href="https://github.com/coralogix/custom-metrics-examples" target="_blank" rel="noreferrer noopener"><strong>GitHub Repo</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
