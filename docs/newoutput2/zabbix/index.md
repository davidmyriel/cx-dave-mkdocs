---
title: "Zabbix"
date: "2022-08-29"
---

Zabbix is an open-source monitoring software tool for diverse IT components, including networks, servers, virtual machines (VMs), and cloud services. [Zabbix](https://www.zabbix.com/) provides monitoring metrics, such as network utilization, CPU load, and disk space consumption.

This tutorial demonstrates how to send your metrics to Coralogix via Zabbix.

## Prerequisites

- Active Coralogix [account](https://dashboard.eu2.coralogixstg.wpengine.com/#/signup) with [metric bucket](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/) and a working Grafana dashboard for metrics

- Zabbix [installed](https://www.zabbix.com/download)

- Fluent Bit [installed](https://docs.fluentbit.io/manual/installation/getting-started-with-fluent-bit)

## Configuration

To tail the logs with Fluentbit, we must first enable the Zabbix export directory.  
The steps differ if you're using Docker or VM, follow the one that suits your use case.

### VM

- The relevant config is located in: /etc/zabbix/zabbix\_server.conf

- Edit the config

- Uncomment and update the lines mentioned as follows (use a custom path to store the data from Zabbix, the path will later be used in Fluentbit):

```
ExportDir=<Path_To_Export_TheData>
ExportFileSize=1M
```

**Note:** If the service doesn't run, check the permissions on the path used for the export.

### Docker

- Enter Zabbix dir

- Paste the following commands:  
    This will export the data from Zabbix to this path: /var/lib/zabbix/export

```
echo "ZBX_EXPORTFILESIZE=1M" >> ./env_vars/.env_srv
echo "ZBX_EXPORTDIR=/var/lib/zabbix/export" >> ./env_vars/.env_srv
```

## Fluent Bit Configuration

Below will be the config needed for Fluentbit to tail the data from Zabbix and forward it to Coralogix.  
Paste the config to the fluent-bit.conf and fill in the mandatory fields:

<table><tbody><tr><td><strong>Field</strong></td><td><strong>Description</strong></td></tr><tr><td>Host</td><td>ingress.coralogix.in</td></tr><tr><td>Port</td><td>443</td></tr><tr><td>URI</td><td>/zabbix/api/v1/real-time-export</td></tr><tr><td>Path_To_Data:</td><td>Location of the exported data from Zabbix</td></tr><tr><td>AppName</td><td><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/">Application name</a> added to your metric attributes</td></tr><tr><td>SubsystemName</td><td><a href="https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/">Subsystem name</a> added to your metric attributes</td></tr><tr><td>PrivateKey</td><td>Coralogix <a href="https://coralogixstg.wpengine.com/docs/send-your-data-api-key/" target="_blank" rel="noreferrer noopener">Send-Your-Data API key</a></td></tr></tbody></table>

```
[INPUT]
    Name    tail
    Path    <Path_To_Data>/history-history-syncer-*.ndjson
    Tag     Item
[OUTPUT]
    Name                  http
    Match                 Item
    Host                  <Endpoint>
    Port                  443
    URI                   /zabbix/api/v1/real-time-export
    Format                json
    TLS                   On
    Json_date_key         false
    Header                Authorization Bearer <PrivateKey>
    Header                data-type Item
    Header                destination Metrics
    Header                application-name <AppName>
    Header                subsystem <SubsystemName>
    compress              gzip
    Retry_Limit           10

[INPUT]
    Name    tail
    Path    <Path_To_DATA>/trends-history-syncer-*.ndjson
    Tag     Trend
[OUTPUT]
    Name                  http
    Match                 Trend
    Host                  <Endpoint>
    Port                  443
    URI                   /zabbix/api/v1/real-time-export
    Format                json
    TLS                   On
    Json_date_key         false
    Header                Authorization Bearer <PrivateKey>
    Header                data-type Trend
    Header                destination Metrics
    Header                application-name <AppName>
    Header                subsystem <SubsystemName>
    compress              gzip
    Retry_Limit           10


[INPUT]
    Name    tail
    Path    <Path_To_DATA>/problems-history-syncer-*.ndjson
    Tag     Trigger
[OUTPUT]
    Name                  http
    Match                 Trigger
    Host                  <Endpoint>
    Port                  443
    URI                   /zabbix/api/v1/real-time-export
    Format                json
    TLS                   On
    Json_date_key         false
    Header                Authorization Bearer <PrivateKey>
    Header                data-type Trigger
    Header                destination Metrics
    Header                application-name <AppName>
    Header                subsystem <SubsystemName>
    compress              gzip
    Retry_Limit           10
```

**Notes:**

- The header "destination Metrics" will send Coralogix metrics only.

- To access data for both logs and metrics, delete the header. The default is "LogsAndMetrics"

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
