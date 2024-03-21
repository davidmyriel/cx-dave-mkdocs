---
title: "Suricata"
date: "2022-02-16"
---

To integrate Suricata into Coralogix, we will use Filebeat Module.

For this integration, we need Filebeat working ([https://coralogixstg.wpengine.com/integrations/filebeat/](https://coralogixstg.wpengine.com/integrations/filebeat/))

Here is an example of **filebeat.xml**:

```
filebeat.config:
  modules:
    path: ${path.config}/modules.d/*.yml
    reload.enabled: false
processors:
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_fields:
        target: ''
        fields:
          PRIVATE_KEY: "XXXXXX-XXXXX-XXXXX-XXXXXX"
          COMPANY_ID: XXXX
          APP_NAME: "APP-NAME"
          SUB_SYSTEM: "SUBSYSTEM"
output.logstash:
  enabled: true
  hosts: ["logstashserver.coralogixstg.wpengine.com:5015"]
  tls.certificate_authorities: ["<path to folder with certificates>/ca.crt"]
  ssl.certificate_authorities: ["<path to folder with certificates>/ca.crt"]
```

You need to enable the Filebeat Suricata module:

```
 filebeat modules enable suricata
```

After that we need to edit the Suricata module configuration file, normally located in **/etc/filebeat/modules.d/suricata.yml**

Here is an example:

```
# Module: suricata
# Docs: https://www.elastic.co/guide/en/beats/filebeat/7.13/filebeat-module-suricata.html

- module: suricata
  # All logs
  eve:
    enabled: true
    var.paths: ["/logs/eve.json"]
```

**Suricata is saving logs in /logs for this example.**

You will notice that you have duplicated items, as you have the original message in the key event, and then the items in JSON format.

We can avoid this by dropping that key.

```
processors:
 - drop_fields:
     fields: ["event"]
     ignore_missing: false
```

Be careful that you **don't** have any other key named “event” because this will drop all.
