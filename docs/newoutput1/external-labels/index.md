---
title: "External Labels"
date: "2023-02-09"
---

Send Coralogix your metrics using **external labels**, maximizing query performance and adding an extra layer of granularity in your data indexing.

## Feature

When you send us your data using [Prometheus](https://coralogixstg.wpengine.com/docs/prometheus/) or another agent, you now have the option of using **external labels** with your metrics.

**External labels** mark label names that already exist in your data as a partition key. Using these labels, they differentiate metrics from different environments, allowing the creation of a **separate index for each data source producing metrics**. When you query your data from a particular environment, our system will only load the index associated with that environment, **maximizing** **query performance**.

### Example

If using, for instance, `external_labels=cluster` , Coralogix will extract the `cluster` label from your ingested metrics and then put all metrics with a particular timeseries value `cluster=cluster_eu` in one index group and another timeseries value `cluster=cluster_us` in another index group.

## Configuration

The following is an example configuration for one **external label** using the [Prometheus remote\_write URL](https://coralogixstg.wpengine.com/docs/prometheus/).

```
remote_write:
- url:<https://ingress.coralogixstg.wpengine.com/prometheus/v1?external_labels=cluster>
  name: '<customer_name>'
  remote_timeout: 120s
  bearer_token: '<Send_Your_Data_private_key>'
```

The following is an example configuration for two external labels using the [Prometheus remote\_write URL](https://coralogixstg.wpengine.com/docs/prometheus/).

```
remote_write:
- url:<https://ingress.coralogixstg.wpengine.com/prometheus/v1?external_labels=region&external_labels=cluster>
  name: '<customer_name>'
  remote_timeout: 120s
  bearer_token: '<Send_Your_Data_private_key>'
```

**Notes**:

- **External labels** are attached to the URL.

- The configuration requires that you input your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/).

## Best Practices

Follow these guidelines as best practices for using **external labels**:

- **Use external labels to refer to the name of a data source, rather than a particular value.**

For example, if `external_labels=cluster`, label values found in the timeseries may be `{cluster: prod}` and `{cluster: dev}`. The Coralogix pipeline will partition data by the `cluster` value.

- **Refrain from using external labels with many values**.

For every value of your **external label**, a new index is created. We suggest using a **maximum of 20 values per label** to ensure optimal query performance.

## Additional Resources

<table><tbody><tr><td><strong>Docs</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/prometheus/" target="_blank" rel="noreferrer noopener">Prometheus</a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
