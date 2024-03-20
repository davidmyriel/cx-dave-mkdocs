---
title: "Getting Started With Coralogix APIs"
date: "2023-10-30"
---

Optimize Coralogix's observability monitoring and unlock its [most powerful features](https://coralogixstg.wpengine.com/docs/coralogix-features-tour/) by using our wide range of APIs. Use them to send data to Coralogix, build visualizations, manage your data, and query it. Read on to learn about our **Data Ingestion**, **Data Management**, and **Data Query APIs** and how to use them.

## Send Your Data to Coralogix

### Data Ingestion APIs

Data is ingested seamlessly and reliably into the Coralogix platform using our **Data Ingestion APIs.**

- **HTTP**
    - [REST API](https://coralogixstg.wpengine.com/docs/coralogix-rest-api/). Send us your logs using either our `/logs` or `/singles` endpoint.

- **gRPC**
    - [Custom logs](https://coralogixstg.wpengine.com/docs/opentelemetry-custom-logs/). Send your **custom logs** to Coralogix using our OpenTelemetry-compatible endpoint.
    
    - [Custom metrics](https://coralogixstg.wpengine.com/docs/opentelemetry-custom-metrics/). Employ our OpenTelemetry-compatible **custom metric endpoint**, including serverless computing and quick cURL-like calls, to send counters, gauges, and histograms to Coralogix.
    
    - [Custom traces](https://coralogixstg.wpengine.com/docs/opentelemetry-custom-traces/). Send your **custom traces** to Coralogix using our OpenTelemetry-compatible endpoint.

## Manage Your Data

The **Data Management APIs** enable you to configure the Coralogix platform, customize your user interface, and optimize it for your observability requirements.

- [Alerts API](https://coralogixstg.wpengine.com/docs/alerts-api/). Coralogix gives you the ability to create monitors that actively check system performance and notify you when there are changes to your data. Our Alerts API allows you to define, query, and manage your alerts.

- [Parsing Rules API](https://coralogixstg.wpengine.com/tutorials/rules-api/). Use our [log parsing rules](https://coralogixstg.wpengine.com/docs/log-parsing-rules/) to process, parse, and restructure log data for monitoring and analysis in the Coralogix platform. Create, read, update, or delete these rules and rule groups for your data.

- [Enrichment API](https://coralogixstg.wpengine.com/docs/custom-log-enrichment/#api-support). Easily enrich your log data with business, operations, or security information using our Enrichment API. Automatically add fields to your JSON logs based on specific matches in your log data, using a predefined custom data source of your choice.

- [Hosted Grafana API](https://coralogixstg.wpengine.com/docs/grafana-api/). Visualize your logs, metrics, and traces using our Grafana-hosted view without the need for any plugins. We provide a secure Grafana API to manage your Grafana-hosted dashboard, allowing you to create, edit, export, import, and query your data in that platform.

- [Data Usage Service API](https://coralogixstg.wpengine.com/docs/data-usage-service-api/). Coralogix provides a Data Usage Service API in support of our [Detailed Data Usage Report](https://coralogixstg.wpengine.com/docs/data-usage/), which presents you with the data you’ve sent to Coralogix, per policy, for either the current month or retroactively 30 or 90 days. The API allows you to query your data consumption in a given time period.

- [TCO Optimizer HTTP API](https://coralogixstg.wpengine.com/docs/tco-optimizer-api/). Define, query, and manage your [TCO log policy](https://coralogixstg.wpengine.com/docs/optimize-log-management-costs/) overrides.

- [TCO Tracing Policy gRPC API](https://coralogixstg.wpengine.com/docs/tco-tracing-policy-grpc-api/). Define, query, and manage your [TCO tracing policy](https://coralogixstg.wpengine.com/docs/tracing-tco-optimizer/) criteria.

- [Insights API](https://coralogixstg.wpengine.com/docs/insights-api/). Manage our [Insights Detection service](https://coralogixstg.wpengine.com/docs/insights-detection/), automatically detecting possible threats and security related anomalies in your traffic.

- [Webhooks API](https://coralogixstg.wpengine.com/docs/webhooks-api/). Define, query, and manage your webhooks.

- [S3 Archive Setup API](https://coralogixstg.wpengine.com/docs/archive-setup-grpc-api/). Many of our customers send us their telemetry data via their [Amazon S3 bucket](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/). Our S3 Archive Setup gRPC API allows you to view bucket definitions, set a target bucket and to define archive retentions.

- [Send-Your-Data Management API](https://coralogixstg.wpengine.com/docs/private-keys-management-api/)**.** Coralogix offers its customers the option of creating multiple [Send-Your-Data API keys](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) with advanced security settings, allowing you to minimize security vulnerabilities and utilize different keys across different systems, deployment methods, and teams. Our recommended best practice when sending us data is to create multiple keys for your organization with all keys enjoying our advanced security settings. This feature is supported by our Send-Your-Data Management API.

- [Recording Rules API](https://coralogixstg.wpengine.com/docs/recording-rules-api/). Coralogix recording rules allow you to pre-process and derive new time series from existing ones. The Recording Rules API allows you to manage these rules, which are executed in the background at regular intervals.

- [SLO API](https://coralogixstg.wpengine.com/docs/slo-management-api/). Efficiently manage your Service Level Objectives (SLOs) programmatically. This API will let you retrieve, create, update and delete your SLOs.

- [Service Removal API](https://coralogixstg.wpengine.com/docs/service-removal-api/). Even if services no longer exist on your side, the catalog in Coralogix lists all previously imported services indefinitely. With the Service Removal API, you can manually remove one or more unused services from your Coralogix subscription.

- [Incidents Management API](https://coralogixstg.wpengine.com/docs/incidents-api/). Manage your reported incidents by listing individual or batch metadata, aggregating incidents, as well as assigning/unassigning incidents or resolving chosen events. 

## Query Your Data

Access and query your data using our **Data Query APIs**.

- [Direct Query API](https://coralogixstg.wpengine.com/docs/direct-query-http-api/). Run [DataPrime](https://coralogixstg.wpengine.com/docs/dataprime-query-language/) or [Lucene](https://coralogixstg.wpengine.com/docs/log-query-simply-retrieve-data/#lucene-query-syntax-reference) queries of your indexed and [archived](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/) logs without the need to access your Coralogix UI.

## Platform as a Service (PaaS)

Avoid implementing our APIs on your own by taking advantage of our own automated options, without having to write your own code:

- [Coralogix Terraform Provider](https://registry.terraform.io/providers/coralogix/coralogix/latest/docs)

- [Coralogix Kubernetes Operator](https://coralogixstg.wpengine.com/docs/coralogix-kubernetes-operator-cx-operator/)

## Support

**Need help?**

Our world-class customer success team is available 24/7 to answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
