# Google Alerts Center

## Overview

Google Workspace Admin Alert Center offers real-time security alerts and insights that help you protect your organization from the latest threats, including phishing, malware, and other suspicious activity.

You can receive these alerts directly to Coralogix, enabling you to further expand your org’s security administration. The following tutorial will show you how to directly integrate Alert Center with Coralogix.

### Benefits

Centralized view of all security incidents.
Broad context to improve the investigation with other incidents/activities done in other products/systems.
Dashboard visualization of overall security issues, across products.

### Prerequisites

- Super admin permissions in Google Cloud.
- An existing project within Google Cloud.
- **Configure a Service Account** and API Key to facilitate automated intermediation.
- [**Domain Wide Delegation**](https://developers.google.com/identity/protocols/oauth2/service-account#delegatingauthority), to authorise your Service Account to read user data and send it to Coralogix.

## Setup Instructions

**1.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**.

**2.** From the **Integrations** section, select **Google Alerts Center**.

**3.** Click **+ ADD NEW**.

**4.** If you haven’t already done so, click **GO TO GCP ACCOUNT** and create a key file.  Then, click **NEXT**.

**5.** Click **SELECT FILE** and upload the key file **you previously created**.

**6.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**7.** Fill in the settings: 

- **Integration Name:** Enter a name for your integration. This field is automatically populated, but can be changed if you want.
- **Organization Name:** Enter the name of the organization on Google Workspaces to be monitored. You can find this by navigating to **IAM & Admin** > **Settings** on Google Cloud.
- **Organization ID:** Enter the ID of the organization to be monitored. You can find this by going to **IAM & Admin** > **Settings** on Google Cloud.
- **Impersonated Email:** Enter a valid email address to be [impersonated](https://cloud.google.com/iam/docs/service-account-impersonation) when connecting to Google Workspace using the service account created above.

**8.** Click **COMPLETE** and finish the setup. 

<aside>
💡 Please wait a few minutes before the integration takes effect and your data is available on the platform.

</aside>

## Support

**Need help?** 

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up. 

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogix.com](mailto:support@coralogix.com).