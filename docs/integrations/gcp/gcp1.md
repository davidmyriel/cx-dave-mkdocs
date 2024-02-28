# Google Workspace Users

## Overview

This integration will let you collect all user details from your Google Workspace Admin Console along with their metadata. This integration normalizes user identifiers from relevant product logs into a `cx_security.user` key. This will let you enrich user data on d with additional context, which can be leveraged through many platform features.

### Benefits

- **Monitor User Activities:** By ingesting user metadata, you can track and oversee user activities across products. Monitor actions such as user logins, network browsing activities, email communications.
- **Investigate Incidents:** Since each user entry is enriched with additional metadata, security administrators can easily get full user context within Coralogix.
- **Maintain Compliance:** Additional user metadata will help you meet compliance requirements. You can generate a user activity report for auditing purposes, which is often used to maintain proof of compliance efforts.
- **Detect Anomalies:** By analyzing user metadata along with other logs, you can establish a baseline for user behavior. For example, as you import additional context, you can set Alert Rules based on metadata, such as user department or role. 
- **Visualize Results:** Track your organizational activity via Custom Dashboards. As you ingest user metadata, you can apply this information to your existing dashboards across all products or create new dashboards with added context. 

### Prerequisites

- Super admin permissions in Google Cloud.
- An existing project within your Google Cloud.
- **Configure a Service Account** and API Key to facilitate automated intermediation.
- [**Domain Wide Delegation**](https://developers.google.com/identity/protocols/oauth2/service-account#delegatingauthority), to authorise your Service Account to read user data and send it to Coralogix.

## Setup Instructions

**1.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**.

**2.** From the **Integrations** section, select **Google Workspace**.

**3.** Click **+ ADD NEW**.

**4.** If you haven’t already done so, click **GO TO GCP ACCOUNT** and create a key file. Then, click **NEXT**.

**5.** Click **SELECT FILE** and upload the key file **you previously created**.

**6.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**7.** Fill in the settings: 

- **Integration Name:** Enter a name for your integration. This field is automatically populated, but can be changed if you want.
- **Organization Name:** Enter the organization name in Google Cloud where the service account was created. You can find this by navigating to **IAM & Admin** > **Settings** on Google Cloud.
- **Organization ID:** Enter the organization ID of Google Cloud where the service account was created. You can find this by going to **IAM & Admin** > **Settings** on Google Cloud.
- **Impersonated Email:** Enter a valid email address to be [impersonated](https://cloud.google.com/iam/docs/service-account-impersonation) when connecting to Google Workspace using the service account created above.

**8.** Click **COMPLETE** and finish the setup. 

<aside>
💡 Please wait a few minutes before the integration takes effect and user data is available.

</aside>

**9.** Verify that the integration pipeline occurred. Go to **Data Flow** > **Data Enrichment**. Under **Custom Enrichment** verify that the following two files were created: `users_normalization` and `users_metadata`

**10.** Check that user data was properly ingested. Go to the **Explore** screen, and run these DataPrime queries to verify that the enrichment files were uploaded with the user details: 

```bash
source users_normalization

source users_metadata
```

 If you are not getting a response, please wait few minutes and try again.

## Work with Enriched User Data

Once you have activated this integration, you will want to start working with your ingested user data. For this, you will need to work with two auto generated files in DataPrime.

### Look up users in the users_normalization file

Before enrichment, all you see is the user’s email address. After this integration, the UI will reveal additional identity metadata. At least, you will see the user’s display name, which is automatically enriched via `users_normalization`.

Specifically, the `users_normalization` file lists enrichment details across users. In the example above, a log key containing the user’s email address `cx_security.email` is augmented with the user’s display name and email address in brackets under `cx_security.email_enriched`.

This basic enrichment update is automatic. To enrich other keys containing the email address during log ingestion, select the key in **Data Flow** > **Data Enrichment**  > **Custom Enrichments**  > `users_normalization`. To view the contents of this file, run the DataPrime query:

```bash
source users_normalization
```

### Enrich metadata via the users_metadata file

The `users_metadata` file augments the user with additional metadata, such as department, title and manager’s email. To view all user metadata and further filter results by user, run the DataPrime query:

```bash
source users_metadata
```

To look up the individual user, find the lookup value in the `users_normalization` file above. To enrich the individual user, run the DataPrime command:

```bash
cx_security.email_enriched
```

## Support

**Need help?** 

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up. 

Feel free to reach out to us **via our in-app chat** or by sending us an email to [support@coralogix.com](mailto:support@coralogix.com).