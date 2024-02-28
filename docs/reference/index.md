# Reference Documentation

Optimize Coralogix’s observability monitoring and unlock its most powerful features by using our wide range of APIs. Use them to send data to Coralogix, build visualizations, manage your data, and query it. 

## Quick Links
|Data Ingestion|Data Query|Data Management|
|-|-|-|
|Send us your data.|Analyze data inside the platform.|Access features to control data use.|

## Data Ingestion

Data is ingested seamlessly and reliably into the Coralogix platform using our Data Ingestion APIs.

|<div style="width:150px">API</div>| |Repository Link|
|-|-|-|
|Coralogix REST API /singles| Send us your logs using either our /singles endpoint.|GitHub|
|Coralogix REST API /bulk| Send us your logs using either our /bulk endpoint.|GitHub|
|OpenTelemetry Custom Traces|Send your custom traces to Coralogix using our OpenTelemetry-compatible endpoint.|GitHub|
|OpenTelemetry Custom Metrics|Employ our OpenTelemetry-compatible custom metric endpoint, including serverless computing and quick cURL-like calls, to send counters, gauges, and histograms to Coralogix.|GitHub|
|OpenTelemetry Custom Logs|Send your custom logs to Coralogix using our OpenTelemetry-compatible endpoint.|GitHub|

## Data Query

Access and query your data using our Data Query APIs.

|<div style="width:150px">API</div>| Description| Repository Link|
|-|-|-|
|Direct Archive Query HTTP API|Direct Query API. Run DataPrime or Lucene queries of your indexed and archived logs without the need to access your Coralogix UI.|GitHub|

## Data Management

The Data Management APIs enable you to configure the Coralogix platform, customize your user interface, and optimize it for your observability requirements.

|<div style="width:150px">API</div>|Description|Repository Link|
|-|-|-|
|SLO Management API||GitHub|
|Service Removal API||GitHub|
|TCO Tracing Policy API|Define, query, and manage your TCO tracing policy criteria.|GitHub|
|Archive Setup API|Many of our customers send us their telemetry data via their Amazon S3 bucket. Our S3 Archive Setup gRPC API allows you to view bucket definitions, set a target bucket and to define archive retentions.|GitHub|
|Data Usage API|Coralogix provides a Data Usage Service API in support of our Detailed Data Usage Report, which presents you with the data you’ve sent to Coralogix, per policy, for either the current month or retroactively 30 or 90 days. The API allows you to query your data consumption in a given time period.|GitHub|
|Send-Your-Data Management API|Coralogix offers its customers the option of creating multiple Send-Your-Data API keys with advanced security settings, allowing you to minimize security vulnerabilities and utilize different keys across different systems, deployment methods, and teams.|GitHub|
|Recording Rules API|Coralogix recording rules allow you to pre-process and derive new time series from existing ones. The Recording Rules API allows you to manage these rules, which are executed in the background at regular intervals.|GitHub|
|Hosted Grafana API|Visualize your logs, metrics, and traces using our Grafana-hosted view without the need for any plugins. We provide a secure Grafana API to manage your Grafana-hosted dashboard, allowing you to create, edit, export, import, and query your data in that platform.|GitHub|
|Webhooks API|Define, query, and manage your webhooks.|GitHub|
|TCO Optimizer HTTP API|Define, query, and manage your TCO log policy overrides.|GitHub|
|Custom Data Enrichment API|Easily enrich your log data with business, operations, or security information using our Enrichment API. Automatically add fields to your JSON logs based on specific matches in your log data, using a predefined custom data source of your choice.|GitHub|
|Insights API|Manage our Insights Detection service, automatically detecting possible threats and security related anomalies in your traffic.|GitHub|
|Parsing Rules API|Use our log parsing rules to process, parse, and restructure log data for monitoring and analysis in the Coralogix platform. Create, read, update, or delete these rules and rule groups for your data.|GitHub|
|Alerts API|Coralogix gives you the ability to create monitors that actively check system performance and notify you when there are changes to your data. Our Alerts API allows you to define, query, and manage your alerts.|GitHub|