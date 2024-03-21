---
title: "Generic Outbound Webhooks (Alert Webhooks)"
date: "2023-10-25"
---

Enhance your observability workflows by sending real-time event notifications and log data to any endpoint that accepts HTTP requests. With this **generic** **outbound webhook**, you can easily integrate Coralogix with different endpoints, automate responses to critical events, and improve your organization's incident management and alerting processes.

## Create a Webhook

**STEP 1.** From the Coralogix toolbar, navigate to **Data Flow** > **Outbound Webhooks.**

**STEP 2.** In the **Outbound Webhooks** section, click **GENERIC WEBHOOK**.

![](images/Outgoing-Webhooks-Generic-Overview-1024x635.png)

**STEP 3.** Click **\+ ADD NEW**.

![](images/Outgoing-Webhooks-Generic-Settings-1024x635.png)

**STEP 4.** Enter a webhook name and the URL to which you want to send an event notification.

The UUID field is auto-populated.

**STEP 5.** Select an HTTP method for the webhook (GET, POST, or PUT).

**STEP 6.** Click **NEXT**.

![](images/Outgoing-Webhooks-Generic-Edit-Message-1024x635.png)

**STEP 7.** \[**Optional**\] Edit the message to customize the header and body of the messages that will be sent when the webhook is triggered.

### Placeholders

Here is a list of all available placeholders you may use and a description of each one.

**Alert Event Information**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$ALERT_NAME</td><td>Name of the alert</td></tr><tr><td>$ALERT_ACTION</td><td>Alert action, whether triggered or resolved</td></tr><tr><td>$ALERT_URL</td><td>URL used to access the alert in Coralogix</td></tr><tr><td>$ALERT_ID</td><td>Alert ID<br>This changes every time a significant alert parameter, such as query or condition, is changed.</td></tr><tr><td>$ALERT_DESCRIPTION</td><td>Description added in the alert</td></tr><tr><td>$ALERT_UNIQUE_IDENTIFIER</td><td>Persists even when significant alert parameters are changed</td></tr><tr><td>$ALERT_THRESHOLD</td><td>Threshold that was defined in the alert</td></tr><tr><td>$ALERT_TIMEWINDOW_MINUTES</td><td>The time frame in minutes for which the alert is defined</td></tr><tr><td>$ALERT_GROUPBY_LABELS</td><td>The group by labels defined in the alert</td></tr><tr><td>$ALERT_GROUP_BY_VALUES</td><td>The values for the group by labels defined in the alert</td></tr><tr><td>$EVENT_TIMESTAMP_ISO</td><td>The event timestamp in ISO format</td></tr><tr><td>$EVENT_SEVERITY</td><td>The significance chosen for the alert: Info, Warning, Error, or Critical.</td></tr><tr><td>$EVENT_SEVERITY_LOWERCASE</td><td>Acts like $EVENT_SEVERITY, but uses lowercase&nbsp;letters</td></tr><tr><td>$OPSGENIE_PRIORITY</td><td>OpsGenie severity mapped from this event’s severity (INFO - P5, WARNING - P3, ERROR - P2, CRITICAL - P1)</td></tr><tr><td>$META_LABELS<br><br>Meta labels are the&nbsp;<br><code>Labels</code>&nbsp;that you attach to an <a href="https://coralogixstg.wpengine.com/docs/getting-started-with-coralogix-alerts/">alert</a> when defining it. If you want your outbound webhooks to contain these labels, add them to your template when defining the custom webhook.</td><td>Labels of the alert as one string of key-value pairs, comma-separated.<br><br>Example:<br>"firstKey:firstValue, justThis, anotherKey:anotherValue"</td></tr><tr><td>$META_LABELS_JSON<br><br>Meta labels are the&nbsp;<br><code>Labels</code>&nbsp;that you attach to an <a href="https://coralogixstg.wpengine.com/docs/getting-started-with-coralogix-alerts/">alert</a> when defining it. If you want your outbound webhooks to contain these labels, add them in your template when defining the custom webhook.</td><td>Labels of the alert presented as a JSON-formatted string<br><br>Example:<br>"{\"firstKey\":\"firstValue\",\"justThis\":null,\"anotherKey\":\"anotherValue\"}"</td></tr><tr><td>$META_LABELS_LIST<br><br>Meta labels are the&nbsp;<br><code>Labels</code>&nbsp;that you attach to an <a href="https://coralogixstg.wpengine.com/docs/getting-started-with-coralogix-alerts/">alert</a> when defining it. If you want your outbound webhooks to contain these labels, add them in your template when defining the custom webhook.</td><td>Alert label defined<br>The set of labels is presented as an array of elements.<br><br>Example:<br>[<br>"firstKey:firstValue",<br>"justThis",<br>"anotherKey:anotherValue"<br>]</td></tr><tr><td>$EVENT_TIMESTAMP_MS</td><td>The time in milliseconds when the alert was triggered</td></tr><tr><td>$EVENT_TIMESTAMP</td><td>The time when the alert was triggered as a string with the date and time</td></tr><tr><td>$GROUP_BY_FIELD_1</td><td>Provides the first group-by field that triggers an alert.</td></tr><tr><td>$GROUP_BY_FIELD_2</td><td>Provides the second group-by field that triggers an alert.</td></tr><tr><td>$GROUP_BY_FIELD_#</td><td>Provides the X group-by field that triggers an alert. May be higher than 2 in some cases.</td></tr><tr><td>$GROUP_BY_VALUE_1</td><td>Provides the first group-by value for the field that triggers an alert.<br>When grouping by a given Group By field in your alert settings, you <strong>must</strong> group the metric by this field to allow the data to propagate to the $GROUP_BY_VALUE_1.</td></tr><tr><td>$GROUP_BY_VALUE_2</td><td>Provides the second group-by value for the field that triggers an alert.<br>When grouping by a given Group By field in your alert settings, you <strong>must</strong> group the metric by this field to allow the data to propagate to the $GROUP_BY_VALUE_2.</td></tr><tr><td>$GROUP_BY_VALUE_#</td><td>Provides the X group-by value that triggers an alert. May be higher than 2 in some cases.<br>When grouping by a given Group By field in your alert settings, you <strong>must</strong> group the metric by this field to allow the data to propagate to the $GROUP_BY_VALUE_X.</td></tr><tr><td>$HIT_COUNT</td><td>Hit count presents the hit count of logs that triggered the alert</td></tr><tr><td>$RELATIVE_HIT_COUNT</td><td>For ratio and time relative alerts, relative hit count presents the hit count of the second query logs</td></tr><tr><td>$QUERY_TEXT</td><td>Presents the alert's query</td></tr><tr><td>$RELATIVE_QUERY_TEXT</td><td>For Ratio and Time Relative alerts, relative query text presents the alert's second query</td></tr><tr><td>$DEFINED_RATIO_THRESHOLD</td><td>For Ratio and Time Relative alerts, the defined ratio threshold presents the ratio threshold defined in the alert</td></tr><tr><td>$ACTUAL_RATIO</td><td>For Ratio and Time Relative alerts, the actual ratio presents the resulted ratio for the alert</td></tr><tr><td>$METRIC_KEY</td><td>For Metric Lucene-based alerts, the metric key is the field on which you create the metric alert.<br>This alert type is deprecated and exists only for existing customers who previously defined this type of alert.</td></tr><tr><td>$METRIC_OPERATOR</td><td>For Metric Lucene-based alerts, the metric operator is the arithmetic function that is being applied when checking the alert<br>This alert type is deprecated and exists only for existing customers who previously defined this type of alert.</td></tr><tr><td>$TIMEFRAME</td><td>For Metric alerts, the timeframe over which the metric alert is checked</td></tr><tr><td>$TIMEFRAME_OVER_THRESHOLD</td><td>For Metric alerts, contains all of the following elements:<br>• The percentage of time over the threshold.<br>• Average of the values crossing the threshold.<br>• Max of the values crossing the threshold.<br>• Min of the values crossing the threshold.<br>(Irrelevant for sum and count arithmetic operators.)</td></tr><tr><td>$METRIC_CRITERIA</td><td>For Metric alerts, the condition that is checked in the alert (‘over’ or ‘under’)</td></tr><tr><td>$SERVICE</td><td>The service for which the span was triggered</td></tr><tr><td>$SPANS</td><td>The number of spans</td></tr><tr><td>$DURATION</td><td>Duration of the triggered span</td></tr></tbody></table>

**Ratio / Time Relative Alerts**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$RATIO_QUERY_ONE</td><td>Query one alias</td></tr><tr><td>$RATIO_QUERY_TWO</td><td>Query two aliases</td></tr><tr><td>$RATIO_TIMEFRAME</td><td>The timeframe over which the alert triggers</td></tr></tbody></table>

**Flow Alerts**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$FLOW_ALERT_RELATED_ALERTS</td><td>The data about the alerts that trigger this flow</td></tr></tbody></table>

**Unique Count Alerts**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$UNIQUE_COUNT_VALUES_LIST</td><td>The unique values for the triggered alert</td></tr></tbody></table>

**New Value Alerts**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$NEW_VALUE_TRACKED_KEY</td><td>The key defined to track new values from</td></tr></tbody></table>

**Log Information**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$LOG_URL</td><td>Link to the alert logs</td></tr><tr><td>$APPLICATION_NAME</td><td>The application name of the presented example log</td></tr><tr><td>$SUBSYSTEM_NAME</td><td>The subsystem name of the presented example log</td></tr><tr><td>$LOG_TEXT</td><td>The entire log payload, whether it is a textual log or JSON formatted log</td></tr><tr><td>$JSON_KEY</td><td>In case the logs are JSON formatted, you may include any key (JSON field) from the log itself</td></tr><tr><td>$JSON_KEY.numeric</td><td>If the chosen field possesses a number value and you wish to include it in its numeric form (use it in the custom webhook body without wrapping quotes), use it with the suffix .numeric. E.g. $status_code.numeric</td></tr><tr><td>$COMPUTER_NAME</td><td>The computer name (if it exists) of the presented example log</td></tr><tr><td>$CATEGORY</td><td>The category (if it exists) of the presented example log</td></tr><tr><td>$IP_ADDRESS</td><td>The IP address (if it exists) of the presented example log</td></tr><tr><td>$THREAD_ID</td><td>The thread ID (if it exists) of the presented example log</td></tr></tbody></table>

**General Information**

<table><tbody><tr><td><strong>Placeholder</strong></td><td><strong>Description</strong></td></tr><tr><td>$TEAM_NAME</td><td>The Coralogix account name from which the alert originates</td></tr><tr><td>$CORALOGIX_ICON_URL</td><td>The Coralogix icon</td></tr><tr><td>$COMPANY_ID</td><td>The company ID</td></tr><tr><td>$DEDUP_KEY</td><td>The key Coralogix uses to dedup when sending to different integrations</td></tr></tbody></table>

**STEP 8.** Click **TEST CONFIG**.

The system sends an HTTP call with the specified parameters to check that your configuration is valid. If the HTTP call is received successfully, a confirmation message is displayed.

![](images/Outgoing-Webhooks-Generic-Add-Alerts-1024x635.png)

**STEP 9.** Once the configuration is validated, [configure your alert notifications](https://coralogixstg.wpengine.com/docs/alert-notifications-outbound-webhooks/).

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/alert-notifications-outbound-webhooks/">Configure Alert Notifications for Outbound Webhooks</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
