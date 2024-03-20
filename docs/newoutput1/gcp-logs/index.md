---
title: "GCP Logs"
date: "2023-11-08"
---

Google Cloud Platform offers integrated monitoring and observability tools that enable users to gather and analyze logs from their GCP resources. Send these **GCP logs** to Coralogix to search, analyze, and visualize your data. Gain insights into application behavior, identify errors, and troubleshoot problems effectively.

## Overview

Google Cloud Platform (GCP) logs encompass all types of log entries generated across various Google Cloud Platform services and resources. These logs capture a wide range of information, including activities, events, and system operations related to your entire GCP environment. This can include logs from compute instances, databases, networking, security, access, and more.

The integration collects logs that are published to a GCP Pub/Sub topic to which one has a subscription, referenced by a subscription name.

### Benefits

By integrating Coralogix seamlessly with your GCP environment, you can gain a comprehensive understanding of your system’s performance, user experiences, and security. This enables you to proactively detect issues, troubleshoot effectively, and optimize your resources, ultimately improving the reliability and efficiency of your applications.

### Prerequisites

- Super admin permissions in Google Workspace.

- An existing project within your Workspace.

- [Create a pull subscription](https://cloud.google.com/pubsub/docs/create-subscription) to a Google Cloud Pub/Sub topic within your Google Cloud project.
    - The following property settings are mandatory:
        - **Delivery type:** Pull
        
        - **Exactly once delivery**: Disabled
    
    - All other property settings below are recommended, but not mandatory.

## Setup

**1.** [Configure a Service Account](https://coralogix.com/docs/gcp-getting-started/) and API Key to facilitate automated intermediation.

**2.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**.

**3.** From the **Integrations** section, select **GCP Logs**.

**4.** Click **\+ ADD NEW**.

**5.** If you haven’t already done so, click **GO TO GCP ACCOUNT** and create a key file. Then, click **NEXT**.

**6.** Click **SELECT FILE** and upload the key file **you previously created**.

**7.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**8.** Fill in the settings:

- **Integration Name:** Name your integration. This field is automatically populated, but can be changed if you want.

- **Application Name:** (optional) **naming conventions** for production, development and / or staging environments.

- **Subsystem Name:** (optional) **naming conventions** for production, development and / or staging environments.

- **Subscription Name:** This is the subscription name you previously created in Prerequisites.

**9.** Click **COMPLET**E and finish the setup.

Please wait a few minutes before the integration takes effect and your data is available on the platform.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogix.com](mailto:support@coralogix.com).
