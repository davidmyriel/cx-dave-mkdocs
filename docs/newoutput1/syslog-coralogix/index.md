---
title: "Syslog"
date: "2023-05-10"
---

Whether your system generates syslog messages in `rfc3164` or `rfc5424` format, or lets you transform them to a custom format, seamlessly send your syslogs to Coralogix.

## Overview

To accommodate many different systems, this tutorial provides configuration options in `rfc3164`, `rfc5424`, and custom format.

Regardless of format, each syslog message sent to Coralogix is labeled with a facility code, indicating the type of system generating the message, and is assigned a severity level.

## Parameters

You are required to input the following parameters in your configuration, determined by the system sending your syslogs.

| Parameter | Description |
| --- | --- |
| Private Key | Coralogix [**Send-Your-Data API Key**](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) |
| Application Name | [**Application name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) |
| SubSystem Name | [**Subsystem name**](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) |
| SyslogEndpoint | [**TLS/TCP syslog endpoint**](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix domain |

## Format

The platform running your applications will typically support only some set of syslog formats with some customization possibilities, making it impossible send logs in one predefined, fixed format. As such, Coralogix supports various formats and associated structures to pass the needed additional data with the syslog message.

| Format | Structure |
| --- | --- |
| rfc3164 | key-value pairs |
| rfc5424 | key-value pairs or structured data |
| JSON | JSON fields |

## Message Structure

### **`rfc5424` Structured Data**

Assuming the system sending your syslog messages supports `rfc5424` format, we **highly recommend** using it. If the configuration allows, add structured data in the format shown in the example.

**Example**

```
<134>1 2022-11-23T07:03:31.402569Z pg-454f526-1 postgres 
  285528 - [coralogix@1 application_name="..." private_key="..." subsystem_name="..."] pid=285528,user=postgres,db=defaultdb, 
  client=[local] LOG: disconnection: session time: 0:00:00.005 
  user=postgres database=defaultdb host=[local]

```

**Notes**:

- The first part of the message is the ID of the structured data followed by three key-value pairs. Because the structured data is named `coralogix@1`, the values inside are not prefixed with `cx_`.

- The `[coralogix@1 ...]` segment allows adding structured data to any message. This is important for sending syslog metadata to Coralogix.

### Key-Value Pairs

Depending on your system, it may be possible to add key-value pairs to the message body. In this case, add the values prefixed with `cx_`.

**Example**

```
cx_application_name="..." cx_private_key="..." cx_subsystem_name="..."

```

### JSON

If the system sending your syslog messages supports custom format, transform the message to JSON along with additional fields. The example below consists of the contents of the file `/etc/rsyslog`, instructing syslog how to send messages.

**Example**

```
$template CoralogixSyslogFormat,"{\\"fields\\": 
       {\\"private_key\\":\\"...\\", 
        \\"application_name\\":\\"...\\",
        \\"subsystem_name\\":\\"...\\"},
        \\"message\\": {\\"message\\":\\"%msg:::json%\\" }}\\n"
@@syslog.coralogixstg.wpengine.com:6514;CoralogixSyslogFormat

```

**Note**: This requires that rsyslog is configured to support TLS connections.

## Troubleshooting

To test your connection, send a message from the terminal with the command below.

```
echo '<14>1 2023-04-03T07:48:26.086Z hostname appname - msg_id - Message cx_private_key="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" cx_application_name="app" cx_subsystem_name="sub" Something happened' | openssl s_client -connect syslog.coralogixstg.wpengine.com:6514

```

**Note**: Be aware that one extra space in the command will render it invalid.

## Monitor Your Syslog Messages

View your syslog messages as rendered structured logs in your Coralogix dashboard. In your navigation bar, click on **Explore** > logs tab.

```
{
  appname:logforwarder
  msg: Failed password for user123 from 192.168.0.1 port 12345 ssh2
  facility:user
  hostname:stream-logfwd20
  msgid:panwlogs
}

```

As with all the logs ingested into Coralogix, you have the ability to modify their format using [parsing rules](https://coralogixstg.wpengine.com/docs/log-parsing-rules/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/syslog-using-fluentd/"><strong>Syslog using OpenTelemetry</strong></a></td></tr><tr><td>Blog</td><td><a href="https://coralogixstg.wpengine.com/blog/syslog-101-everything-you-need-to-know-to-get-started/"><strong>Syslog 101</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
