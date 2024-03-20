---
title: "GCP Metrics"
date: "2023-08-17"
---

Google Cloud Platform provides built-in monitoring and observability tools that allow users to collect and analyze metrics and traces from their GCP resources. Send **Google Cloud metrics** seamlessly to Coralogix. Search, analyze, and visualize your data, gaining insights into application behavior, identifying errors, and troubleshooting problems.

## Overview

Google Cloud Platform (GCP) logs encompass all types of log entries generated across various Google Cloud Platform services and resources. These logs capture a wide range of information, including activities, events, and system operations related to your entire GCP environment. This can include logs from compute instances, databases, networking, security, access, and more.

The integration collects logs that are published to a GCP Pub/Sub topic to which one has a subscription, referenced by a subscription name.

### Benefits

By integrating Coralogix seamlessly with your GCP environment, you can gain a comprehensive understanding of your system’s performance, user experiences, and security. This enables you to proactively detect issues, troubleshoot effectively, and optimize your resources, ultimately improving the reliability and efficiency of your applications.

### Prerequisites

- Super admin permissions in Google Workspace.

- An existing project within your Workspace.

## Setup Instructions

**1.** **[Configure a Service Account](https://coralogix.com/docs/gcp-getting-started/)** and API Key to facilitate automated intermediation. You will need to set proper account Roles to collect metrics.

**2.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**.

**3.** From the **Integrations** section, select **GCP Metrics**.

**4.** Click **\+ ADD NEW**.

**5.** If you haven’t already done so, click **GO TO GCP ACCOUNT** and create a key file. Then, click **NEXT**.

**6.** Click **SELECT FILE** and upload the key file **you previously created**.

**7.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**8.** Choose the metric prefixes you would like to pull into Coralogix by selecting them from the dropdown menu. To limit unnecessary API calls that fetch no data but count towards the quota limit, specify only the GCP prefixes of the metrics that actually exist and you want to pull into Coralogix.

- Coralogix queries only the metrics under the selected prefixes.

- Metrics for which no data is available for more than 15 minutes will reduce the scraping interval from 15 minutes to 1 hour.

- Querying all of your metrics may result in GCP quota limits and less frequent metric updates.

**9**. Click **NEXT**. On the next screen, fill in the settings:

- **Integration Name:** Name your integration. This field is automatically populated, but can be changed if you want.

- **Application Name:** (optional) **Naming conventions** for production, development and / or staging environments.

- **Subsystem Name:** (optional) N**aming conventions** for production, development and / or staging environments.

- **Subscription Name:** This is the name of your subscription you previously created in Prerequisites.

**10.** Click **COMPLET**E and finish the setup. Please wait a few minutes before the integration takes effect and your data is available on the platform.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogix.com](mailto:support@coralogix.com).
