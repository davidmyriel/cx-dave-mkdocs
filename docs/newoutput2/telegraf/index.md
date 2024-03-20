---
title: "Telegraf"
date: "2022-08-14"
---

Telegraf is a server-based agent for **collecting and sending metrics for further processing**. It's a piece of software that you can install anywhere in your infrastructure and it will read metrics from specified sources – typically application logs, events, or data outputs.

This tutorial demonstrates how to send your metrics to Coralogix via Telegraf.

## Prerequisites

- Telegraf [installed](https://docs.influxdata.com/telegraf/v1.21/introduction/installation/)

- Active Coralogix [account](https://signup.coralogixstg.wpengine.com/#/) with [metric bucket](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/) and a working [Grafana dashboard](https://coralogixstg.wpengine.com/docs/hosted-grafana-view/) for metrics

## Configuration

If you are using Telegraf version `v1.23.4` or above, you can use the Coralogix dialect for a more straightforward configuration.

In order to send your data to Coralogix, you are required to declare the following variables in your configuration:

- `**service_address**`: Coralogix [OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix domain

- **`private_key`**: Access your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/). Your key is recorded in the override file as a secret in order to ensure that this sensitive information remains protected and unexposed.

- **`application`** & **`subsystem`**: Customize and organize your data in your Coralogix dashboard using [application](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) and [subsystem](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) names. Application name is available as a `__meta_applicationname` label for all metrics for this Telegraf instance. Subsystem name is available as a `__meta_subsystem` label for all metrics for this Telegraf instance.

The following example shows how to configure Telegraf using the Coralogix dialect:

```
[[outputs.opentelemetry]]
 service_address = "<coralogix-otel-endpoint>"
 compression = "gzip"
 [outputs.opentelemetry.coralogix]
 private_key = "<private_key>"
 application = "<application>"
 subsystem = "<subsystem>" 
```

For older versions of Telegraf (`v1.23.3` or earlier), you should use the following configuration to send telemetry data using the OpenTelemetry output plugin:

```
 [[outputs.opentelemetry]]
   service_address = "<coralogix-otel-endpoint>"
   insecure_skip_verify = true
   compression = "gzip"
   [outputs.opentelemetry.headers]
     Authorization = "Bearer <private_key>"
     ApplicationName = "<application>"
     ApiName = "<subsystem>"
   
```

## Monitoring Node

In this example, we set up a Telegraf agent to monitor Node, which runs a website. It's a development environment, so we set ApplicationName to "development" and subsystem to "website". Additionally, we provide more metadata in the `\\\\[global\\\\_tags\\\\]` section, such as `dc="us-east-1"` and `rack="1a"`. For input plugins, we use:

- cpu

- disk

- diskio

- kernel

- mem

- processes

- swap

- system

These plugins give us a good overview of the Node's metrics. The following example shows the complete Telegraf configuration:

```
# Telegraf Configuration
[global_tags]
  # will tag all metrics with dc=us-east-1 and rack = "1a"
  dc = "us-east-1"
  rack = "1a"

# Configuration for telegraf agent
[agent]
  interval = "10s"
  round_interval = true
  metric_batch_size = 1000
  metric_buffer_limit = 10000
  flush_interval = "10s"
  flush_jitter = "0s"
  precision = ""

 # Send OpenTelemetry metrics over gRPC
 [[outputs.opentelemetry]]
   service_address = "<coralogix-otel-endpoint>"
   insecure_skip_verify = true
   compression = "gzip"
   [outputs.opentelemetry.headers]
     Authorization = "Bearer <private_key>"
     ApplicationName = "development"
     ApiName = "website"

# Input plugins
# Read metrics about cpu usage
[[inputs.cpu]]
  percpu = true
  totalcpu = true
  collect_cpu_time = false
  report_active = false

# Read metrics about disk usage by mount point
[[inputs.disk]]
  ignore_fs = ["tmpfs", "devtmpfs", "devfs", "iso9660", "overlay", "aufs", "squashfs"]

[[inputs.diskio]]
[[inputs.kernel]]
[[inputs.mem]]
[[inputs.processes]]
[[inputs.swap]]
[[inputs.system]]


```

In Coralogix, you can query all the data using PromQL via Grafana. The following PromQL query shows us all the metrics exported by this Telegraf instance:

```
{__meta_applicationname="development",__meta_subsystem="website"}
```

Results in:

```
cpu_usage_guest{__meta_applicationname="development", __meta_subsystem="website", cpu="cpu-total", cx_tenant_id="32680", dc="us-east-1", host="website-host", hostname="$HOSTNAME", rack="1a"}
....
diskio_io_time_total{__meta_applicationname="development", __meta_subsystem="website", cx_tenant_id="32680", dc="us-east-1", host="website-host, hostname="$HOSTNAME", name="loop0", rack="1a"}


```

Note: application name, subsystem, and global tags are available as labels on the exported metrics.

Next, you can start building dashboards and configuring different input plugins.

## Troubleshooting

If you are having issues, you can enable debug logs via the Telegraf configuration:

```
[agent]
  debug = true
...


```

Telegraf will then start logging a detailed overview of what is happening. For example:

```
2022-10-19T11:12:34Z D! [outputs.opentelemetry] Wrote batch of 43 metrics in 87.062989ms
2022-10-19T11:12:34Z D! [outputs.opentelemetry] Buffer fullness: 0 / 10000 metrics
2022-10-19T11:13:20Z D! [inputs.disk] [SystemPS] -> using mountpoint "/run/user/1000/doc"...
2022-10-19T11:13:20Z D! [inputs.disk] [SystemPS] => dropped by disk usage ("/run/user/1000/doc"): operation not permitted


```

## Additional Resources

[Telegraf Docs by InfluxDB](https://docs.influxdata.com/telegraf/v1.21/introduction/installation/)

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
