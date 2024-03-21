---
title: "OpenTelemetry"
date: "2023-10-29"
---

[OpenTelemetry](https://opentelemetry.io/) is a vendor-neutral, open-source observability framework for instrumenting, generating, collecting, and exporting telemetry data such as traces, metrics, and logs. Use OpenTelemetry’s collection of APIs, SDKs, and tools to collect and export observability data from your environment to Coralogix.

## Instrumentation

Coralogix supports OpenTelemetry to get telemetry data (traces, logs, and metrics) from your application as requests travel through its many services and other infrastructure.

We assume you have already [instrumented your application](https://opentelemetry.io/docs/concepts/instrumenting/) with OTel SDKs and set up a [receiver](https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver) for your data.

If you’ve never instrumented for observability, we provide some examples of automatic instrumentation for your applications in the languages below.

- [Java](https://coralogixstg.wpengine.com/docs/java-opentelemetry-instrumentation/)

- [Python](https://coralogixstg.wpengine.com/docs/capture-opentelemetry-traces-from-your-python-applications/)

## Kubernetes Observability using OpenTelemetry

Coralogix offers [Kubernetes Observability using OpenTelemetry](https://coralogixstg.wpengine.com/docs/introduction-to-kubernetes-observability-using-opentelemetry/) for comprehensive Kubernetes and application observability. Using our OpenTelemetry Chart, the integration enables you to simplify the collection of logs, metrics, and traces from the running application in your pods to the cluster-level components of your Kubernetes cluster, while enabling our [Kubernetes Dashboard](https://coralogixstg.wpengine.com/docs/kubernetes-dashboard/).

## Configuration

We provide a few alternative scenarios for setting up OpenTelemetry and sending your data to Coralogix. Documentation related to the configuration and installation of the **v0.64.0** release of OpenTelemetry Collector for different deployments can be found below.

- [Kubernetes Observability using OpenTelemetry](https://coralogixstg.wpengine.com/docs/introduction-to-kubernetes-observability-using-opentelemetry/)

- [OpenTelemetry using ECS - EC2](https://coralogixstg.wpengine.com/docs/opentelemetry-using-ecs-ec2/)

- [OpenTelemetry using ECS - Fargate](https://coralogixstg.wpengine.com/docs/aws-ecs-fargate/)

- [OpenTelemetry using Docker](https://coralogixstg.wpengine.com/docs/opentelemetry-using-docker/)

## Otel Metrics: Best Practices

OpenTelemetry metrics are broadly compatible with Coralogix dimensional metrics. We currently support OpenTelemetry metrics **v1.x**. All of the supported metric types include an independent set of associated attributes that map directly to dimensions customers can use to filter metric data during your query. While Coralogix now supports Delta Temporality in addition to Cumulative Temporality, **we encourage the use of Cumulative Temporality as a best practice**.

### Temporality

The OpenTelemetry Metrics [Data Model](https://opentelemetry.io/docs/reference/specification/metrics/data-model/) and [SDK](https://opentelemetry.io/docs/reference/specification/metrics/sdk/) are designed to support both Cumulative and Delta [Temporality](https://opentelemetry.io/docs/reference/specification/metrics/data-model/#temporality). It is important to understand that temporality will impact how the SDK manages memory usage.

The use of Cumulative Temporality for monotonic sums is common, exemplified by Prometheus. The use of Delta Temporality for metric sums is also common, exemplified by Statsd.

### Delta Temporality

While Coralogix supports Delta Temporality in addition to Cumulative Temporality, we encourage using Cumulative Temporality as a best practice.

Those customers using Delta Temporality should take note of the following configuration parameters in our Delta Cumulative Converter:

- Coralogix supports monotonic [sum](https://github.com/open-telemetry/opentelemetry-specification/blob/87a5ed7f0d4c403e2b336f275ce3e7fd66a8041b/specification/metrics/datamodel.md#sums) and [histogram](https://github.com/open-telemetry/opentelemetry-specification/blob/87a5ed7f0d4c403e2b336f275ce3e7fd66a8041b/specification/metrics/datamodel.md#histogram) data points.

- Coralogix keeps all metrics in our data store for 24 hours. If values for the same metric arrive in intervals greater than 24 hours, they will be recorded, but treated as “resets.”

- Your data points may not always arrive consecutively. When a data point is missing, the time series with a missing data point will be delayed for 60 seconds. You can retry sending us missing data points during this window.

- If a second data point does not arrive after the first, your time series will restart from the third data point.

- Be cautious about properly setting up your retry or reset series, as the OTEL Exporter can potentially miss data points and may need to resend them.

## Batch Sizing

Coralogix recommends the default otel-integration chart settings for [batch processors](https://github.com/open-telemetry/opentelemetry-collector/tree/main/processor/batchprocessor) in all collectors:

```
  batch:
    send_batch_size: 1024
    send_batch_max_size: 2048
    timeout: "1s"
```

This sizing ensures that the telemetry sent to the Coralogix backend is batched into larger requests, reducing networking overhead and improving performance.

These settings impose a hard limit of 2048 units (spans, metrics, logs) on the batch size, balancing the recommended batch size and networking overhead.

While you can adjust these settings to suit your requirements, considering the size limits enforced by Coralogix endpoints, [currently set to a max of 10 MB after decompression](https://coralogix.com/docs/coralogix-endpoints/#limits--quotas), is essential.

## Limits & Quotas

- Coralogix places a **hard limit of 10MB** of data to our [Otel endpoints](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/), with a **recommendation of 2MB**.

- Metric names must be a maximum of 255 characters.

- Attribute keys for metric data must be a maximum of 255 characters.

Limits apply to single requests, regardless of timespan.

## **Additional Resources**

<table><tbody><tr><td>GitHub</td><td><a href="https://github.com/coralogix/telemetry-shippers/tree/master/otel-integration/k8s-helm#prerequisites"><strong>Official Repository</strong></a></td></tr><tr><td>Instructional Videos</td><td><strong><a href="https://www.youtube.com/watch?v=_YK-9z8cMgI&amp;ab_channel=Coralogix" target="_blank" rel="noreferrer noopener">Integrate metrics into Coralogix using OpenTelemetry, Kubernetes &amp; Helm</a><br><a href="https://www.youtube.com/watch?v=8w1KCiOGKWY&amp;ab_channel=Coralogix" target="_blank" rel="noreferrer noopener">Integrate logs into Coralogix using OpenTelemetry, Kubernetes &amp; Helm</a><br><a href="https://www.youtube.com/watch?v=GWggKh4ny30&amp;ab_channel=Coralogix" target="_blank" rel="noreferrer noopener">Capture Kubernetes logs, transform with Logs2Metrics and render with DataMap</a></strong></td></tr><tr><td>Blogs</td><td><strong><a href="https://coralogixstg.wpengine.com/blog/configure-otel-demo-send-telemetry-data-coralogix/">How to Configure the OTel Community Demo App to Send Telemetry Data to Coralogix</a><br><a href="https://coralogixstg.wpengine.com/blog/ship-opentelemetry-data-to-coralogix-via-reverse-proxy-caddy-2/">Ship OpenTelemetry Data to Coralogix via Reverse Proxy (Caddy 2)</a></strong></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
