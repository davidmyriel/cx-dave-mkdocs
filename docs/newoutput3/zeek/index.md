---
title: "Zeek"
date: "2021-08-02"
---

In order to ship Zeek logs to Coralogix, we need to first install Filebeat.

If you haven't already, you can follow our documentation here: [https://coralogixstg.wpengine.com/integrations/filebeat/](https://coralogixstg.wpengine.com/integrations/filebeat/)

## Installation

First, enable the Filebeat module for Zeek:

```
filebeat modules enable zeek
```

## Configuration

You need to configure the Zeek module file **zeek.yml**. Usually this file is located in **_/etc/filebeat/modules.d/_**

In this configuration, you need to add the base directory where Zeek saves the logs usually, in this example replacing **/opt/zeek/logs/current** with the path of your Zeek scan results.

Here is an example of **zeek.yml**:

```
# Module: zeek
# Docs: /guide/en/beats/filebeat/7.6/filebeat-module-zeek.html
- module: zeek
 capture_loss:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/capture_loss.log"]
 connection:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/conn.log"]
 dce_rpc:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/dce_rpc.log"]
 dhcp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/dhcp.log"]
 dnp3:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/dnp3.log"]
 dns:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/dns.log"]
 dpd:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/dpd.log"]
 files:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/files.log"]
 ftp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/ftp.log"]
 http:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/http.log"]
 intel:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/intel.log"]
 irc:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/irc.log"]
 kerberos:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/kerberos.log"]
 modbus:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/modbus.log"]
 mysql:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/mysql.log"]
 notice:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/notice.log"]
 ntlm:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/ntlm.log"]
 ocsp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/ocsp.log"]
 pe:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/pe.log"]
 radius:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/radius.log"]
 rdp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/rdp.log"]
 rfb:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/rfb.log"]
 #  signatures:
 #    enabled: true
   #    var.paths: ["/opt/zeek/logs/current/signatures.log"]
 sip:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/sip.log"]
 smb_cmd:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/smb_cmd.log"]
 smb_files:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/smb_files.log"]
 smb_mapping:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/smb_mapping.log"]
 smtp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/smtp.log"]
 snmp:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/snmp.log"]
 socks:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/socks.log"]
 ssh:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/ssh.log"]
 ssl:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/ssl.log"]
 stats:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/stats.log"]
 syslog:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/syslog.log"]
 traceroute:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/traceroute.log"]
 tunnel:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/tunnel.log"]
 weird:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/weird.log"]
 x509:
   enabled: true
   var.paths: ["/opt/zeek/logs/current/x509.log"]
```

This way every time a Zeek scan is executed, Filebeat will ship logs to Coralogix.
