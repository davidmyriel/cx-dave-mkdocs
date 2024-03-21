---
title: "Nagios"
date: "2022-08-16"
---

Nagios is **an open-source** monitoring system for computer systems. It was designed to run on the Linux operating system and can monitor devices running Linux, Windows, and Unix operating systems. Nagios software runs periodic status checks on critical parameters of application, network, and server resources.

Both **Nagios XI** and **Nagios Core** have the ability to generate status checks, which arrive in the form of logs and metrics.

Nagios supports forwarding status checks to another Nagios instances using NRDP protocol; Coralogix uses this protocol to collect logs and metrics from each status check that is sent.

This tutorial demonstrates how to send your logs and metrics to Coralogix using Nagios.

## Prerequisites

- Active Coralogix [account](https://dashboard.eu2.coralogixstg.wpengine.com/#/signup) with [metric bucket](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/) and a working Grafana dashboard for metrics

- Nagios Core [installed](https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/quickstart.html)

- Nagios XI [installed](https://www.nagios.com/downloads/nagios-xi/?utm_campaign=cross-site&utm_source=exchange&utm_medium=banner&utm_content=scott&__hstc=118811158.a5b31732ab87aeaa21ef6552433f4375.1660212701135.1660630783080.1660632623911.6&__hssc=118811158.1.1660632623911&__hsfp=251130574)

## Configuration

### Nagios Core

**STEP 1**. Update the main configuration file **nagios.cfg**, usually located in: /usr/local/nagios/etc/, by adding the following:

```
obsess_over_hosts=1
obsess_over_services=1
ochp_command=send_nrdp_host
ocsp_command=send_nrdp_service
```

**STEP 2**. Create (or update) the file **commands.cfg**, usually located in: /usr/local/nagios/etc/, and add it to  
**nagios.cfg**:

```
command_name send_nrdp_host
command_line $USER1$/send_nrdp.php --url=<clusterURL>/nrdp/api/v1/<appname>/<subsystem> --token=<privatekey> --host="$HOSTNAME$" --state=$HOSTSTATEID$ --output="$HOSTOUTPUT$|$HOSTPERFDATA$"
}
define command{
command_name send_nrdp_service
command_line $USER1$/send_nrdp.php --url=<clusterURL>nrdp/v1/:application/:subsystem/nrdp/ --token=<privatekey> --host="$HOSTNAME$" --service="$SERVICEDESC$" --state=$SERVICESTATEID$ --output="$SERVICEOUTPUT$|$SERVICEPERFDATA$"
}

```

You are required to input 4 mandatory fields:

- **clusterURL**: Coralogix [ingress endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix domain with path changed to one of the following:
    - `nrdp/v1/:application/:subsystem/nrdp/`
    
    - `nrdp/v1/:application/:subsystem/`

- **appName**: [Application name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) added to your metric attributes

- **subsystemName**: [Subsystem name](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) added to your metric attributes

- **privateKey**: Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

### Nagios XI

Once Nagios Core is configured, configure Nagios XI.

**STEP 1**. Select the admin tab in your navigation pane.

**STEP 2**. In the left-hand side bar, click **Outbound Transfers** and input the following parameters:

- **Outbound check transfer**: Enable.

- **Global options**: Change filter mode to include and filter /^.\*/.

- **NRDP**: Enable.

- **IP**: (without HTTPS): Coralogix [ingress endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) associated with your Coralogix domain with path changed to one of the following:
    
    - `nrdp/v1/:application/:subsystem/nrdp/`
    
    - `nrdp/v1/:application/:subsystem/`

- **Type**: HTTPS

- **Token**: Input Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

- **Settings**: Update as necessary.

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
