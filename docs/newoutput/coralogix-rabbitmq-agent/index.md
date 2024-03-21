---
title: "RabbitMQ Metrics"
date: "2022-06-14"
---

With tens of thousands of users, RabbitMQ is one of the most popular open source message brokers. RabbitMQ is used worldwide at small startups and large enterprises.

Getting Metrics from RabbitMQ Management API allow you to monitor and track your brokers performance.

To Achieve this we will use Telegraf to fetch, and ship RabbitMQ Metrics into Coralogix.

## Prerequisites

- Telegraf [installed](https://docs.influxdata.com/telegraf/v1.21/introduction/installation/)

- Coralogix account with [metric bucket](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/)

## Configuration

To verify you have a working Telegraf install please run:

```bash
# telegraf --version
Telegraf 1.23.4 (git: HEAD 5b48f5da)
```

Now let's modify the configuration to pickup RabbitMQ Metrics and send them to Coralogix.

Edit telegraf configuration file, normally located in `/etc/telegraf/telegraf.conf`

Here is a sample config:

```
[[inputs.rabbitmq]]
  ## Management Plugin url. (default: http://localhost:15672)
   url = ""
  ## Tag added to rabbitmq_overview series; deprecated: use tags
   name = ""
  ## Credentials
   username = ""
   password = ""

  ## Optional TLS Config
  # tls_ca = "/etc/telegraf/ca.pem"
  # tls_cert = "/etc/telegraf/cert.pem"
  # tls_key = "/etc/telegraf/key.pem"
  ## Use TLS but skip chain & host verification
  # insecure_skip_verify = false

  ## Optional request timeouts
  ##
  ## ResponseHeaderTimeout, if non-zero, specifies the amount of time to wait
  ## for a server's response headers after fully writing the request.
  # header_timeout = "3s"
  ##
  ## client_timeout specifies a time limit for requests made by this client.
  ## Includes connection time, any redirects, and reading the response body.
  # client_timeout = "4s"

  ## A list of nodes to gather as the rabbitmq_node measurement. If not
  ## specified, metrics for all nodes are gathered.
  # nodes = ["rabbit@node1", "rabbit@node2"]

  ## A list of queues to gather as the rabbitmq_queue measurement. If not
  ## specified, metrics for all queues are gathered.
  ## Deprecated in 1.6: Use queue_name_include instead.
  # queues = ["telegraf"]

  ## A list of exchanges to gather as the rabbitmq_exchange measurement. If not
  ## specified, metrics for all exchanges are gathered.
  # exchanges = ["telegraf"]

  ## Metrics to include and exclude. Globs accepted.
  ## Note that an empty array for both will include all metrics
  ## Currently the following metrics are supported: "exchange", "federation", "node", "overview", "queue"
  # metric_include = []
  # metric_exclude = []

  ## Queues to include and exclude. Globs accepted.
  ## Note that an empty array for both will include all queues
  # queue_name_include = []
  # queue_name_exclude = []

  ## Federation upstreams to include and exclude specified as an array of glob
  ## pattern strings.  Federation links can also be limited by the queue and
  ## exchange filters.
  # federation_upstream_include = []
  # federation_upstream_exclude = []

[[outputs.opentelemetry]]
   service_address = "<yourclusterURL>:443"
   insecure_skip_verify = true
   compression = "gzip"
   [outputs.opentelemetry.coralogix]
     private_key = "<private_key>"
     application = "<application>"
     subsystem = "<subsystem>"


```

The fields needed to be filled are:

- url - Coralogix [OpenTelemetry endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/)

- name - Tag added to rabbitmq\_overview series; deprecated: use tags

- username - RabbitMQ AdminUI username

- password - RabbitMQ AdminUI password

- private\_key- Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

- application- [Application name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/), which will be added to your metric attributes

- subsystem- [Subsystem name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/), which will be added to your metric attributes

- service\_address- Cluster endpoint location

## Additional Resources

<table><tbody><tr><td class="has-text-align-left" data-align="left"><strong>Documentation</strong></td><td class="has-text-align-left" data-align="left"><a href="https://github.com/influxdata/telegraf/blob/master/plugins/inputs/rabbitmq/README.md" target="_blank" rel="noreferrer noopener">RabbitMQ Plugin</a><br></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
