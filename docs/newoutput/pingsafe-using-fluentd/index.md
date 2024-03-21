---
title: "PingSafe"
date: "2023-03-01"
---

[PingSafe](https://www.pingsafe.com/) is an industry-leading cloud security platform, which scans your cloud infrastructure from an attacker's lens. Security lapses are identified, prioritized, and auto-remediated to eliminate unwanted business impacts.

This tutorial demonstrates how to undertake the PingSafe integration to Coralogix via webhook using [Fluentd](https://coralogixstg.wpengine.com/docs/fluentd/), allowing you to seamlessly send us your logs.

## Prerequisites

1\. Server to install [Fluentd](https://coralogixstg.wpengine.com/docs/fluentd/)

2\. Static public IP allocated to the server for initial configuration

3\. Active PingSafe account

The webhook is configured to send data to the instance where Fluentd is installed, using the particular port configured in the Fluentd configuration file.

## Deployment

**STEP 1**. [Install and configure](https://coralogixstg.wpengine.com/docs/fluentd/) Fluentd on your server. Additional information can be found [here](https://docs.fluentd.org/installation).

**STEP 2**. Under **/etc/td-agent/**, edit the configuration file called **td-agent.conf** and replace the content with the following configuration:

```
<system>
  log_level info
</system>

<source>
  @type http
  @label @CORALOGIX
  port 9880
  bind 0.0.0.0
  body_size_limit 32m
  keepalive_timeout 10s
</source>

<label @CORALOGIX>
  <filter **>
  @type record_transformer
  @log_level warn
  enable_ruby true
  auto_typecast true
  renew_record true
  <record>
    applicationName "application_name"
    subsystemName "subsystem_name"
    text ${record.to_json}
  </record>
  </filter>

<match **>
  @type http
  @id http_to_coralogix
  endpoint "https://api.<coralogix domain>/logs/rest/singles" 
  headers {"private_key":"Your Coralogix account private key"}
  retryable_response_codes 503
  error_response_as_unrecoverable false
  <buffer>
    @type memory
    chunk_limit_size 10MB
    compress gzip
    flush_interval 1s
    retry_max_times 5
    retry_type periodic
    retry_wait 2
  </buffer>
  <secondary>
    #If any messages fail to send they will be send to STDOUT for debug.
    @type stdout
  </secondary>
</match>
</label>
```

Replace the values for:

- applicationName & subsystemName: [Application and subsystem names](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) to be displayed in your Coralogix dashboard

- endpoint: Replace **<coralogix domain>** with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/)

- private\_key: Replace with your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)

Save and configure your **td-agent.conf** file.

**STEP 3**. Test the configuration:

```bash
td-agent --dry-run
```

**STEP 4**. Start the **td-agent** service:

```bash
sudo systemctl start td-agent.service
```

**STEP 5**. Check the status:

```bash
sudo systemctl status td-agent.service
```

**STEP 6**. To test the full flow, run this curl command:

```bash
curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"example_key":"example_value"}' \
 http://<ip of the server>:9880
```

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
