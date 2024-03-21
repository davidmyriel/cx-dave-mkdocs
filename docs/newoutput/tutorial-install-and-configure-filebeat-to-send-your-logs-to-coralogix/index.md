---
title: "Tutorial: Install and Configure Filebeat to Send Your Logs to Coralogix"
date: "2022-12-22"
---

This tutorial provides a step-by-step guide on to how install and configure [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html) to send logs from a file to your Coralogix team over TLS. It does this using a deployment of Filebeat on a single Amazon Linux 2 instance.

**Note**! Filebeat can be used to ship logs from a variety of sources, including Syslog, Docker, and Windows Environments.

## Tutorial

Learn how to:

- Easily install Filebeat

- Familiarize yourself with the Filebeat environment

- Create a working example of a FileBeat configuration that ships logs to Coralogix

## **Supported Versions**

Coralogix supports these versions of Filebeat:

- Filebeat 7.x (v7.17 as of 12.2022)

- Filebeat 8.x (v8.5 as of 12.2022)

**Note!** To avoid breaking changes between these major versions, do **not** upgrade directly from v7 to v8.

## Set Up

This section demonstrates how to deploy Filebeat on a single Amazon Linux 2 instance. General instructions for installing and configuring Filebeat and sending your data to Coralogix can be found [here](https://coralogixstg.wpengine.com/docs/filebeat/).

### **Installation**

[Install and configure](https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-installation-configuration.html) Filebeat v7.17 on your Linux distribution.

```
curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.17.8-linux-x86_64.tar.gz
tar xzvf filebeat-7.17.8-linux-x86_64.tar.gz
cd filebeat-7.17.8-linux-x86_64/

```

### Configuration

To [configure](https://www.elastic.co/guide/en/beats/filebeat/7.17/configuring-howto-filebeat.html) Filebeat, modify the main parts of the configuration file `filebeat.yml`: modules, inputs, fields, outputs.

### Modules

Configure the [modules](https://www.elastic.co/guide/en/beats/filebeat/7.17/configuration-filebeat-modules.html). These are Filebeat inputs enabling the input and parser.

The example below configures the Fortinet / Firewall module, enabling Filebeat to ingest Syslog data from FortiGate Firewall on port 9004/UDP and parse Syslog messages in JSON format.

```
#==========================  Modules configuration =============================
filebeat.modules:
- module: fortinet
  firewall:
    enabled: true
    var.input: udp
    var.syslog_host: 0.0.0.0
    var.syslog_port: 9004
```

**Note**! Modules change dramatically between different versions of Filebeat. Previous versions of Filebeat do not have all modules available.

### **Inputs**

To configure Filebeat manually (rather than using modules), specify a list of inputs in the `filebeat.inputs` section of the `filebeat.yml`. Inputs specify how Filebeat locates and processes input data.

The log input in the example below enables Filebeat to ingest data from the log file. It then points Filebeat to the logs folder and uses a wildcard `*.log` to collect all files ending with `.log` .

```
#=========================== Filebeat inputs =============================

#------------------------------ Log input --------------------------------
- type: log

  # Change to true to enable this input configuration.
  enabled: false

  # Paths that should be crawled and fetched. Glob based paths.
  # To fetch all ".log" files from a specific level of subdirectories
  # /var/log/*/*.log can be used.
  # For each file found under this path, a harvester is started.
  # Make sure not file is defined twice as this can lead to unexpected behaviour.
  paths:
	- /var/log/*.log
	#- c:\programdata\elasticsearch\logs\*
```

### **Fields**

Apply additional configuration settings (such as `fields`, `include_lines`, `exclude_lines`, `multiline`) to the lines harvested from logs. The options that you specify are applied to all of the files harvested by a single input.

To apply different configuration settings to different files, define multiple input sections.

**Note!** Ensure a file is not defined more than once across all inputs because this can lead to unexpected behavior.

```
filebeat.inputs:
- type: log 
  paths:
    - /var/log/*.log
  fields:
    PRIVATE_KEY: '<coralogix_send-your-data-api-key>'
    COMPANY_ID: <companyID>
    APP_NAME: '<application_name>'
    SUB_SYSTEM: '<subsystem_name>'
  fields_under_root: true

```

### **Outputs**

1\. Configure Filebeat to write specific outputs by setting options in the `output` section of the configuration file.

The `logstash` output in the example below enables Filebeat to ship data to Logstash. It points Filebeat to the Coralogix logstash in the `coralogixstg.wpengine.com` [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) and points Filebeat to the TLS and SSL certificates (same certificate) that are required to ship data securely to Coralogix.

**Note**! Only a single output may be defined.

```
# ================================= Logstash output =============================

	output.logstash:
  	enabled: true
  	hosts: ['logstashserver.coralogixstg.wpengine.com:5015']
  	tls.certificate_authorities: ['/usr/share/Coralogix-EU.crt']
  	ssl.certificate_authorities: ['/usr/share/Coralogix-EU.crt']
```

2\. [Download](https://coralogix.atlassian.net/wiki/spaces/SE/pages/2756476929/Integrations+Filebeat#%5BhardBreak%5DURLs-and-Certificates) and store the certificate in a location accessible by Filebeat.

## **Example Configuration**

```
filebeat.yml: |-
# ============================== Filebeat Inputs ===============================
#-------------------------------- logs input ---------------------------------	
filebeat.inputs:
	
- type: log
  	  paths:
     - "/var/log/your_app/your_app.log"
  	 	line_delimiter: "\n"
  	 	max_message_size: 10MiB
        timeout: 300s
  	 	enable_metric: true
#------------------------ Coralogix fields configuration --------------------
  	  fields:
    		PRIVATE_KEY: '<coralogix_privatekey>'
    		COMPANY_ID: <companyID>
    		APP_NAME: '<application_name>'
    		SUB_SYSTEM: '<subsystem_name>'
  	  fields_under_root: true
#==========================  Modules configuration =============================
Filebeat.modules:
- module: fortinet
         Firewall:
	     enabled: true
	     var.input: udp
	     var.syslog_host: 0.0.0.0
         var.syslog_port: 9004
         fields:
    		PRIVATE_KEY: '<coralogix_send-your-data-api-key>'
    		COMPANY_ID: <companyID>
    		APP_NAME: '<application_name>'
    		SUB_SYSTEM: '<subsystem_name>'
  	     fields_under_root: true

# ================================= Logstash output =============================
#------------------------- Coralogix Logstash output —-----------------------

	output.logstash:
  	enabled: true
  	hosts: ['logstashserver.coralogixstg.wpengine.com:5015']
  	tls.certificate_authorities: ['/usr/share/Coralogix-EU.crt']
  	ssl.certificate_authorities: ['/usr/share/Coralogix-EU.crt']
```

## **URLs and Certificates**

<table><tbody><tr><td></td><td><strong>EU</strong></td><td><strong>IN</strong></td><td><strong>US</strong></td></tr><tr><td><strong>Cluster Domain</strong></td><td>coralogixstg.wpengine.com</td><td>app.coralogix.in</td><td>coralogix.us</td></tr><tr><td><strong>SSL Certificates</strong></td><td>https://coralogix-public.s3-eu-west-1.amazonaws.com/certificate/Coralogix-EU.crt</td><td>https://coralogix-public.s3-eu-west-1.amazonaws.com/certificate/Coralogix-IN.pem</td><td>https://www.amazontrust.com/repository/AmazonRootCA1.pem</td></tr><tr><td><strong>Logstash Server URL</strong></td><td>logstashserver.coralogixstg.wpengine.com</td><td>logstash.app.coralogix.in</td><td>logstashserver.coralogix.us</td></tr></tbody></table>

## Validation

Test Filebeat by running it and monitoring the logs.

1\. Modify the user credentials in `filebeat.yml` and specify a user who is authorized to publish events.

```
sudo chown root filebeat.yml 

```

2\. By default, Filebeat sends all of its output to Syslog. When you run Filebeat in the foreground, you can use the `-e` command line flag to redirect the output to standard error instead, as in the example below.

```
sudo ./filebeat -e

```

### **Debugging**

To increase the verbosity of debug messages, use the `-d` command line flag to debug selectors.

```
./filebeat -e -d "*"

```

### **Common Messages**

You may encounter certain common messages as follows:

- No new logs were received by Filebeat:

```
INFO    [monitoring]    log/log.go:145  Non-zero metrics in the last 30s

```

- Filebeat received logs and was able to establish a connection to Coralogix Logstash:

```
2022-12-19T19:35:41.758Z	INFO	[publisher_pipeline_output]	pipeline/output.go:101	Connecting to backoff(async(tcp://logstashserver.coralogixstg.wpengine.com:5015))
2022-12-19T19:35:41.886Z	INFO	[publisher_pipeline_output]	pipeline/output.go:111	Connection to backoff(async(tcp://logstashserver.coralogixstg.wpengine.com:5015)) established
```

# **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
