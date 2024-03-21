---
title: "Alcide kAudit"
date: "2021-01-07"
---

Stream Alcide kAudit findings to Coralogix, enabling you to view, analyze and monitor your logs using cutting-edge tools.

## Overview

Coralogix complements Alcide kAudit by creating one pane of glass through which DevSecOps as well as other teams like engineering and CS can view logs from different parts of the infrastructure in a consolidated way. Beyond visualizing the data, Coralogix gives all these teams powerful analysis tools and helps identify correlations between applications and components events using ML-techniques.

Coralogix can also put logs in the context of the application’s lifecycle in the CI/CD process – allowing you to assess the impact of every change to your infrastructure. The end result is faster problem identification and time to resolution.

kAudit can send two log categories to Coralogix:

- Policy Findings: The K8s audit log entries that show violations of the policy which are configured by the Sec and DevOps team.

- Detections: Suspicious behavior with potential security risks to the K8s cluster which is automatically detected by kAudit.

## Configuration

Alcide kAudit findings are streamed to Coralogix, enabling you to view, analyze and monitor your data.

Exporting kAudit findings is available via the kaudit-integrationConfigMap.

URL: https://api.**<domain>**/logs/v1/bulk. Input your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).  
Private Key: Input your Coralogix [](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)[Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/)  
Reference: https://coralogixstg.wpengine.com/integrations/coralogix-rest-api/

```
body fields:
applicationName: application
subsystemName: subsystem
logEntries: each log has timestamp, severity, category
```

Note: the ConfigMap should already exist where kAudit has been deployed.  
Edit kaudit-integration-<your cluster name> ConfigMap and add the integrations that you wish.

Example configuration:

```
apiVersion: v1
kind: ConfigMap
metadata:
name: kaudit-integration-<your cluster name>
namespace: alcide-kaudit
labels:
app: kaudit
app-name: kaudit # kAudit instance
data:
audit-integration: |

- type: detections
target:
target-type: http-api
http-api-uri: 'https://api.<domain>/logs/v1/bulk'
http-api-token: 'Private-key'
stopped: true
- type: selections
target:
target-type: http-api
http-api-uri: 'https://api.<domain>/logs/v1/bulk'
rate-limit: 10
data-filter:
entity-no-match: ^system:|^admin$
rules-match: ^exec|unsafe$
report: details
```

UI:

In Alcide, select the **Integrations** tab and go to the **Detections Integrations Configuration** section, which is used to configure integrations for threat intel logs.

Select **HTTP API** as your target.

In the URL box, enter https://api.**<domain>**/logs/v1/bulk.

Under Entities Types, select the types that you want to forward threat intel about.

Under Detection Categories, select the categories you wish to forward.

Under Detection Confidence, select your desired levels of confidence. Coralogix recommends selecting at least high and medium.

Optionally, you can create whitelist and blacklist filters on entities using the Entities Matching and Entities Not Matching boxes.

Then, go to the Selected Audit Entries Integration Configuration section, located underneath the previous section. This section is used to configure integrations for audit logs.

Select HTTP API as your target.

In the URL box, enter https://api.**<domain>**/logs/v1/bulk.

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
