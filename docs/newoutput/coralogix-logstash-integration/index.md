---
title: "Logstash"
date: "2022-02-20"
---

Coralogix provides seamless integration with Logstash, so you can send your logs from anywhere and parse them according to your needs.

## Prerequisites

- Logstash [installed](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

## Best Practices

We **recommend** using the generic http output plugin with this integration, given its high level of configurability and metric support for monitoring output.

## Installation

**STEP 1**. Share the Ruby code snippet depicting the event structure as it flows through Logstash.

- \[Optional\] Use this opportunity to set dynamic application and subsystem fields.

- The example below adopts a JSON structure and has these fields: application, subsystem and host.

```
filter {
  ruby {code => "
                event.set('[@metadata][application]', event.get('application'))
                event.set('[@metadata][subsystem]', event.get('subsystem'))
                event.set('[@metadata][event]', event.to_json)
                event.set('[@metadata][host]', event.get('host'))
                "}
}
```

- If you prefer that the fields application, subsystem and host remain static, replace the `event.get` with a plain string, as in the example below.

```
filter {
  ruby {code => "
                event.set('[@metadata][application]', MyApplicationName)
                event.set('[@metadata][subsystem]', MySubsystemName)
                event.set('[@metadata][event]', event.to_json)
                event.set('[@metadata][host]', event.get('host'))
                "}
}
```

**STEP 2**. Once the Event is ready, configure the output itself to send the logs.

- Input your [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

- Select the Coralogix [REST API Singles endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/#coralogix-rest-api-singles) associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

```
output {
	http {
        url => "<Coralogix REST API singles endpoint>"
        http_method => "post"
        headers => ["authorization", "Bearer <Coralogix Send-Your-Data API key>"]
        format => "json_batch"
        codec => "json"
        mapping => {
            "applicationName" => "%{[@metadata][application]}"
            "subsystemName" => "%{[@metadata][subsystem]}"
            "computerName" => "%{[@metadata][host]}"
            "text" => "%{[@metadata][event]}"
        }
        http_compression => true
        automatic_retries => 5
        retry_non_idempotent => true
        connect_timeout => 30
        keepalive => false
        }
}
```

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
