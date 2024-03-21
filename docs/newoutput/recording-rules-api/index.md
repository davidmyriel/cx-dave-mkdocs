---
title: "Recording Rules API"
date: "2023-01-26"
---

Recording rules allow you to pre-process and derive new time series from existing ones. The rules are defined in a [configuration file](https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/#recording-rules) and are executed in the background at regular intervals.

Coralogix provides an API that allows you to manage your recording rules.

## Overview

A recording rule is defined by:

1. a name

3. a query expression

5. an optional set of labels to add to the resulting time series.

The query expression is used to select the time series that the rule will operate on, and can include aggregation functions such as sum, min, max, etc. When the rule is executed, it will evaluate the query expression over a range of time, specified in the configuration file as evaluation\_interval, and create a new time series for each unique combination of label values resulting from the evaluation.

**Example**:

Suppose you have a metric called _request\_duration\_seconds_ that represents the duration of HTTP requests made to your web server, and you want to track the average request duration over the past minute. You could define a recording rule like this:

```
groups:
  - name: 
      rules:
        - record: service:request_duration_seconds:avg1m
          expr: avg(request_duration_seconds{namespace="frontend"}[1m]) by (service)

```

* * *

This rule would create a new time series called _service:request\_duration\_seconds:avg1_m for each unique value of the service label, with the value of the time series being the average of the _request\_duration\_seconds_ time series over the past minute.

## Recording Rules API

Coralogix provides an API that allows you to manage your recording rules. In order to use the Recording Rules API, you will need:

1. An authentication key from _Data Flow_ –> _API Keys_ -> _Alerts, Rules, and Tags API key_

3. The API endpoint associated with your Coralogix [domain](http://coralogixstg.wpengine.com/docs/coralogix-domain/).

| Coralogix Region | Endpoint |
| --- | --- |
| US1 | https://ng-api-grpc.coralogix.us:443/ |
| US2 | https://ng-api-grpc.cx498.com:443 |
| EU1 | https://ng-api-grpc.coralogixstg.wpengine.com:443/ |
| EU2 | https://ng-api-grpc.eu2.coralogixstg.wpengine.com:443/ |
| AP1 (IN) | https://ng-api-grpc.app.coralogix.in:443/ |
| AP2 (SG) | https://ng-api-grpc.coralogixsg.com:443/ |

**NOTE**:

1. The Alerts, Rules, and Tags API keys are visible only for admin users.

3. Rules that depend on other recording-rules are not supported.

5. Importing a new configuration file will **replace** the existing recording-rules. Therefore, new configuration must contain **all** your rules, not just updates/changes.

7. Alerting rules are not supported.

The actions supported by the API are listed below. These examples require that you have the [grpcurl](https://github.com/fullstorydev/grpcurl) tool installed in your environment. Also note that these examples use the EU1 endpoint

### Save RuleGroups

This action will Create a new rule-group. In case the rule-group exists, it will be overwritten.

```
grpcurl -H "Authorization: Bearer <ALERTS_RULES_TAGS_API_KEY>" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 rule_manager.groups.RuleGroups.Save <<EOF
{
  "name": "FrontendRuleGroup",
  "interval": 60,
  "limit": "0",
  "rules": [
    {
      "record": "service:request_duration_seconds:avg1h",
      "expr": "avg(request_duration_seconds{namespace=\"frontend\"}[1h]) by (service)",
      "labels": {}
    }
  ]
}
EOF

```

An empty response is returned on success.

```
{}

```

### Fetch rule-groups

This action will retrieve the specified rule-group.

```
grpcurl -H "Authorization: Bearer <ALERTS_RULES_TAGS_API_KEY>" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 rule_manager.groups.RuleGroups.Fetch <<EOF
{
  "name": "HTTPRuleGroup"
}
EOF

```

A single rule-group is returned on success

```
{
  "ruleGroup": {
    "name": "HTTPRuleGroup",
    "interval": 60,
    "limit": "0",
    "rules": [
      {
        "record": "job:http_requests_total:sum",
        "expr": "sum(rate(http_requests_total{namespace=\"frontend\"}[5m])) by (job)"
      }
    ]
  }
}

```

### List rule-groups

This action will list all existing rule-groups.

```
grpcurl -H "Authorization: Bearer <ALERTS_RULES_TAGS_API_KEY>" ng-api-grpc.coralogixstg.wpengine.com:443 rule_manager.groups.RuleGroups.List

```

A list of rule-groups is returned on success.

```
{
  "ruleGroups": [{
  "name": "FrontendRuleGroup",
  "interval": 60,
  "limit": "0",
  "rules": [
    {
      "record": "service:request_duration_seconds:avg1m",
      "expr": "avg(request_duration_seconds{namespace=\"frontend\"}[1m]) by (service)"
    }]
  }, {
  "name": "HTTPRuleGroup",
  "interval": 60,
  "limit": "0",
  "rules": [{
    "labels": {},
    "record": "job:http_requests_total:sum",
    "expr": "sum(rate(http_requests_total{}[5m])) by (job)"
    }]		
  }]
}

```

### Delete rule-groups

This action will delete the specified rule-groups.

```
grpcurl -H "Authorization: Bearer <ALERTS_RULES_TAGS_API_KEY>" -d @ ng-api-grpc.coralogixstg.wpengine.com:443 rule_manager.groups.RuleGroups.Delete <<EOF
{
  "name": "HTTPRuleGroup"
}
EOF

```

An empty response is returned on success.

```
{}

```

## Additional Resources

[Coralogix Terraform Provider - Recording Rules](https://registry.terraform.io/providers/coralogix/coralogix/latest/docs/resources/recording_rules_group)

[Prometheus Recording Rules](https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/)

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
