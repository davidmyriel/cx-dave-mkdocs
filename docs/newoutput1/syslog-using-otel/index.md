---
title: "Syslog using OpenTelemetry"
date: "2023-03-02"
---

This tutorial demonstrates how to use custom syslog to send your logs to Coralogix using [OpenTelemetry](https://coralogixstg.wpengine.com/docs/opentelemetry/).

## Overview

Syslog is a standard for message logging. It allows separation of the software that generates messages, the system that stores them, and the software that reports and analyzes them. Each message is labeled with a facility code, indicating the type of system generating the message, and is assigned a severity level.

When there is no support for custom syslog, an intermediate server is required in order to send the data to the Coralogix account.

## Prerequisites

- Server to install [OpenTelemetry](https://coralogixstg.wpengine.com/docs/opentelemetry/)

- Static public IP allocated to the server for initial configuration

## Deployment

**STEP 1**. **[Install](https://opentelemetry.io/docs/collector/getting-started/)** OpenTelemetry on your server.

**STEP 2**. Create a configure file.

```
receivers:
  syslog:
    tcp:
      listen_address: "0.0.0.0:514"
    protocol: rfc5424
    operators:
      - type: syslog_parser
        protocol: <**message_format>**
        parse_from: body
        parse_to: body
			- type: remove
        field: attributes
exporters:
  coralogix:
    domain: "<coralogix_domain>"
    private_key: "private_key"
    application_name: "applicationName"
    subsystem_name: "subsystemName"
    timeout: 30s
service:
  pipelines:
    logs:
      receivers: [ syslog ]
      exporters: [ coralogix ]

```

Replace the following values.

<table><tbody><tr><td><strong>Value</strong></td><td><strong>Description</strong></td></tr><tr><td>applicationName</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/">Application name</a></strong>&nbsp;to be displayed in your Coralogix dashboard</td></tr><tr><td>subsystemName</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/">Subsystem name</a></strong>&nbsp;to be displayed in your Coralogix dashboard</td></tr><tr><td>coralogix_domain</td><td>Your <strong><a href="https://coralogixstg.wpengine.com/docs/coralogix-domain/">Coralogix domain</a></strong></td></tr><tr><td>private_key</td><td>Your <strong><a href="https://coralogixstg.wpengine.com/docs/send-your-data-api-key/">Coralogix Send-Your-Data API key</a></strong></td></tr><tr><td>message_format</td><td>The syslog message format ( rfc3164/rfc5424 )</td></tr></tbody></table>

**Notes**:

- `port 514` is the default port for Syslog.

- To change to this port, modify the OpenTelemetry configuration should be changed accordingly.

**STEP 3**. Save the configure file.

## **Additional Resources**

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/syslog-coralogix/"><strong>Syslog</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
