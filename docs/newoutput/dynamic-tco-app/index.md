---
title: "Dynamic TCO App"
date: "2022-05-31"
---

[TCO](https://coralogixstg.wpengine.com/tutorials/optimize-log-management-costs/) pipelines are one of the key features in Coralogix. Customers are allowed to define in which way their logs are distributed across 3 distinct use cases.

One of our customers had a Coralogix quota alarm set up to send an email to them when the quota reached 90%. When the alert was triggered, they logged into Coralogix and changed the distribution of their logs using our TCO Optimizer. This way they could address the spike in their log volume without reaching or surpassing the account limits.

This is a great solution for ensuring that there are no billing overages or data loss, but ideally, the process would be automated. So we started thinking of a solution for them (and the rest of our customers).

We developed a Lambda function that will, upon a POST Request (API Gateway), set up the “Emergency” TCO Policy and Overrides to the account. At 00:00 UTC, when the daily quota resets, it will revert back to the “Original” TCO Policy and Overrides.

Now, you can automatically handle a volume increase without reaching your quota limit.

View the [full Dynamic TCO App installation instructions](https://github.com/coralogix/dynamic-tco).

To start, we wanted the user to be able to easily change their “Emergency TCO Policy and Overrides” when needed, so we followed the same syntax that the [TCO Optimizer API](https://coralogixstg.wpengine.com/tutorials/tco-optimizer-api/) uses for Creating Policy. This way the body of our request will be an array of “TCO Policy”.

This means that you can pick one of the items in the below body example and use it as the body of the TCO Optimizer API Call, and it will successfully create the corresponding Policy.

**The most important thing is to define which TCO Rules we will want to apply once we reach quota usage warning.**

Here is the structure of the Dynamic TCO POST:

Header:

```
{
  "Content-Type": "application/json",
  "Function-Key": "thisismysecretkey"
}
```

And Body:

```
[
  {
    "name": "All low",
    "priority": "low",
    "severities": [
      4,
      5,
      6
    ]
  },
  {
    "name": "Policy Creation test new",
    "priority": "medium",
    "severities": [
      1,
      2,
      3
    ]
  }
]
```

With this setup, we are only missing a way we can trigger this. For that, we are going to use Coralogix’s alerting mechanism and Audit Account.

When the quota warning threshold is surpassed a new log will be written in the Audit Account. We can then configure an alert on this event that will send a webhook to our Lambda Function with the body and headers requirements.

This way as soon as your quota reaches 90%, the alert will automatically trigger the function to change your TCO Policy and Overrides so you can cope with the high volume of logs.

At 00:00 UTC, a Cron event (A scheduled trigger) will trigger the Lambda to restore your original TCO Policy and Overrides.

For traceability, the Lambda function will log the changes in your Coralogix account and save them to the S3 bucket configured in your TCO Policy and Overrides configuration.

This way you can not only control how your logs are categorized on arrival with our normal TCO Pipelines but also have emergency TCO Pipelines setup for handling log volume bursts. 

View the [full Dynamic TCO App installation instructions](https://github.com/coralogix/dynamic-tco).

If you have any questions, feel free to reach out to our 24/7 support team via our in-app chat!
