---
title: "Google Workspace Alert Center"
date: "2024-03-13"
---

## Overview

**Google Workspace Alert Center** offers real-time security alerts and insights that help you protect your organization from the latest threats, including phishing, malware, and other suspicious activity.

The following tutorial will show you how to integrate Google Workspace Alert Center with Coralogix directly.

## Benefits

- Develop a centralized view of all security incidents in the organization.

- Contextualize and cross-reference the investigation with other activities.

- Set up custom dashboards to visualize security issues across products.

## Prerequisites

- Super admin permissions in Google Cloud

- An existing project within Google Cloud

## Setup

**1\.** [Configure a Service Account and API Key](https://coralogix.com/docs/gcp-getting-started/) to facilitate automated intermediation.

**2.** Set up [Domain Wide Delegation](https://developers.google.com/identity/protocols/oauth2/service-account#delegatingauthority), to authorize your Service Account to read user data and send it to Coralogix. The OAuth Scope permission required is [`https://www.googleapis.com/auth/apps.alerts`](https://www.googleapis.com/auth/apps.alerts).

**3.** Navigate to **API & Services** > **Library screen**. Select **Google Workspace Alert Center API** and ensure it’s enabled.

**4.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**. Select **Google Workspace Alert Center**.

**5.** Click **\+ ADD NEW**.

**6.** If you haven’t already done so, click **GO TO GCP ACCOUNT** and create a key file. Then, click **NEXT**.

**7.** Click **SELECT FILE** and upload the key file **you previously created**.

**8.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**9.** Fill in the settings:

- **Integration Name:** Enter a name for your integration. This field is automatically populated, but can be changed if you want.

- **Impersonated Email:** Enter a valid email address to be [impersonated](https://cloud.google.com/iam/docs/service-account-impersonation) when connecting to Google Workspace using the service account created above.

- **Application Name**. Input your [application](https://coralogix.com/docs/application-and-subsystem-names/) name.

- **Subsystem Name**. The subsystem name, Google Workspace Alert Center, is read-only and cannot be modified.

**10**. Click **COMPLETE** and finish the setup.

Please wait a few minutes before the integration takes effect, and your data is available on the platform.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by sending us an email to [support@coralogix.com](mailto:support@coralogix.com).
