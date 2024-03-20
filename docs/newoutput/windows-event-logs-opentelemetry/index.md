---
title: "Windows Event Logs & OpenTelemetry"
date: "2024-02-04"
---

Utilizing OpenTelemetry in conjunction with the Windows Event Log receiver is an excellent method for collecting Windows Event Logs. To implement this solution, it is essential to deploy an Opentelemetry Collector directly onto the Windows Server and configure it as a service to enable seamless integration with the Windows Event Log receiver.

## Supported OS

Windows

## Overview

Leveraging the Windows Event Log receiver facilitates monitoring specific Event Log sources known as Channels. These channel receivers are specified and configured in the Opentelemetry Collector configuration file. Typical channels to be monitored include:

- Application

- Security

- System

## Prerequisites

- Microsoft User account with permissions to access EventLog

- Microsoft User account with permissions to create Services

## Setup

### Installation

OpenTelemetry is released as a binary by the OpenTelemetry group [here](https://github.com/open-telemetry/opentelemetry-collector-releases/releases).

**STEP 1**. Download the latest `otelcol-contrib` binary, as the standard otelcol binary does not include the WindowsEventLog receiver or our Coralogix exporter. After downloading, extract the binary to C:\\cx-otel for documentation purposes and establish it as a service using the sc framework.

**STEP 2**. Create the service definition using the following command:

```
sc.exe create cx-otelcol displayname=cx-otelcol start=delayed-auto binPath="C:\\cx-otel\\otelcol-contrib.exe --config C:\\cx-otel\\config.yaml"

```

**Note**: After creating the configuration file, you must restart the parameters and start the service.

### Configuration

Configuring the Opentelemetry Collector is done through the use of a standard config.yaml file. The example file below demonstrates the configuration of two separate Channels, with the appropriate pipeline configurations. This file must be saved as config.yaml in the same folder as the OpenTelemetry Collector binary.

```
receivers:
    windowseventlog/application:
        channel: application
    windowseventlog/system:
        channel: system

processors:
  resourcedetection:
    detectors: [system]
    system:
      hostname_sources: ["os"]
  batch:
    send_batch_size: 1024
    send_batch_max_size: 2048
    timeout: "1s"

exporters:
  coralogix:
    domain: "coralogixstg.wpengine.com"
    private_key: "cxtp_<redacted>"
    application_name: "Windows"
    subsystem_name: "EventLogs"
    timeout: 30s

service:
  pipelines:
    logs:
      receivers: [ windowseventlog/application, windowseventlog/system ]
      processors: [ resourcedetection, batch ]
      exporters: [ coralogix ]

```

**Notes:**

- Replace `domain` with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/), and `private_key` with your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/). The `application_name` and `subsystem_name` [may be configured as desired](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/).

- For `batch`, Coralogix recommends the default otel-integration chart settings for batch processors in all collectors. Find out more [here](https://coralogix.com/docs/faqs-kubernetes-observability-using-opentelemetry/#how-do-i-optimize-batch-sizing?).

## Finalization

Once you’ve created your config.yaml file, configure the restart settings and start the service. To do so, use the following commands:

```
sc.exe failure cx-otelcol reset= 86400 actions= restart/5000/restart/5000/restart/5000
sc.exe start cx-otelcol

```

## Validation

Once you’ve configured the `cx-otelcol` service, you should start seeing messages for your configured `application_name` and `subsystem_name` arriving in the Coralogix UI. If your Windows server is very inactive, you can restart the cx-otelcol service from the service console to trigger a System channel event. This restart event should be captured in your Coralogix UI.

## Troubleshooting

If you don’t see messages arriving in your Coralogix UI, you can troubleshoot this integration by adding a “debug” exporter and viewing the output in the console after executing the collector manually.

**STEP 1**. Stop your defined service and make a copy of your config.yaml file.

**STEP 2**. In this new copy, edit the config to add a “debug:” exporter and add “debug” to the logs pipeline exporters array, as follows:

```
exporters:
  debug:
    verbosity: detailed
  coralogix: ..

service:
  pipelines:
    logs:
      receivers: ..
      processors: ..
      exporters: [ coralogix, debug ]

```

**STEP 3**. Execute the Opentelemetry Collector from the command line like so:

```
C:\\cx-otel\\otelcol-contrib.exe --config C:\\cx-otel\\config_debug.yaml

```

You should then see the events in your console and confirm they are processing correctly.

**STEP 4**. Once you’ve verified that they are being processed correctly, begin troubleshooting the network, starting with your local firewall and then your egress path to the internet.

If you’re using AWS PrivateLink, validate that the PrivateLink FQDN resolves from the host.

## Additional Resources

<table><tbody><tr><td>GitHub Documentation</td><td><a href="https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/windowseventlogreceiver/README.md"><strong>WindowsEventLogReceiver</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
