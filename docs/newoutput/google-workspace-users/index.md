---
title: "Google Workspace Users"
date: "2024-02-20"
---

## Overview

This integration will let you collect all user details from your Google Workspace Admin Console along with their metadata. This provides your Coralogix features with additional user details to get better context. The integration will normalize user identifiers from relevant product logs into a `cx_security.user` key.

### Benefits

- **Monitor User Activities:** By ingesting user metadata, you can monitor actions such as user logins, network browsing activities, email communications, and more.

- **Investigate Incidents:** Since each user entry is enriched with additional metadata, security administrators can easily get full user context within Coralogix.

- **Maintain Compliance:** Additional user metadata will help you meet compliance requirements. You can generate a user activity report for auditing purposes, which is often used to maintain proof of compliance efforts.

- **Detect Anomalies:** Establish a baseline for a role behavior. As you import additional context, you can set Alert Rules based on metadata, such as user department or role.

- **Visualize Results:** Track your org’s activity via Custom Dashboards. As you ingest user metadata, you can visualize statistics of total activities by department, role or any other user group.

### Prerequisites

- Super admin permissions in Google Cloud.

- An existing project within your Google Cloud.

## Setup

**1\.** [Configure a Service Account and API Key](https://coralogix.com/docs/gcp-getting-started) to facilitate automated intermediation.

**2.** Set up [Domain Wide Delegation](https://developers.google.com/identity/protocols/oauth2/service-account#delegatingauthority) to authorize your Service Account to read user data and send it to Coralogix. The OAuth Scope permission required is [`https://www.googleapis.com/auth/admin.directory.user.readonly`](https://www.googleapis.com/auth/admin.directory.user.readonly).

**3.** Navigate to **API & Services** > **Library screen**. Select **Admin SDK API** and ensure it’s enabled.

**4.** From your Coralogix toolbar, navigate to **Data Flow** > **Integrations**. Select **Google Workspace Users**.

**5.** Click **\+ ADD NEW**.

**6.** If you haven’t already done so in step 1, click **GO TO GCP ACCOUNT** and create a key file. Then, click **NEXT**.

**7.** Click **SELECT FILE** and upload the key file **you previously created**.

**8.** A confirmation will appear when the file is uploaded successfully. Click **NEXT**.

**9.** Fill in the settings:

- **Integration Name:** Enter a name for your integration. This field is automatically populated, but can be changed if you want.

- **Organization Name:** Enter the organization name in Google Cloud where the service account was created. You can find this by navigating to **IAM & Admin** > **Settings** on Google Cloud.

- **Organization ID:** Enter the organization ID of Google Cloud where the service account was created. You can find this by going to **IAM & Admin** > **Settings** on Google Cloud.

- **Impersonated Email:** Enter a valid email address to be [impersonated](https://cloud.google.com/iam/docs/service-account-impersonation) when connecting to Google Workspace using the service account created above.

**10.** Click **COMPLETE** and finish the setup. Please wait a few minutes before the integration takes effect and user data is available.

**11.** Verify that the integration pipeline occurred. Go to **Data Flow** > **Data Enrichment >** **Custom Enrichment. T**he following two files should have been created: `users_normalization` and `users_metadata`

**12.** Check that user data was properly ingested. Go to the **Explore** screen, and run these DataPrime queries to verify that the enrichment files were uploaded with the user details:

```
source users_normalization

source users_metadata

```

If you are not getting a response, please wait a few minutes and try again.

## Work with Enriched User Data

Once you have activated this integration, the service will automatically connect to your Google Workspace environment and read all relevant user details. User data will be stored under two files in **Custom Enrichment**: `users_normalization`and `users_metadata`. This sync will run once a day, to keep an updated status of user details stored in Coralogix.

### Automatic normalization of a user key across product logs

All logs containing a user email in the `cx_security.email` key are automatically enriched into the `cx_security.email_enriched` key, which will show the user’s display name and the email in brackets. This is done by the `users_normalization` CSV file created automatically in **Custom Enrichments**.

To enrich other keys containing the email address during log ingestion, select the key in **Data Flow** > **Data Enrichment**\> **Custom Enrichments** > `users_normalization`. To view the contents of this file, run the DataPrime query:

```
source users_normalization

```

### View additional details about a user

The `users_metadata` CSV file in **Custom Enrichment** augments the user with additional metadata, such as department, title and manager’s email. To view all user metadata and further filter results by the user, run the DataPrime query:

```
source users_metadata

```

You can also use DataPrime enrich commands to augment your logs with additional user context. Do this automatically during log ingestion by defining the `cx_security.email_enriched` key in **Data Flow** > **Data Enrichment** > **Custom Enrichments**. This will allow you to define alert rules based on user context (e.g. department, role) and visualize these statistics in custom dashboards.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by emailing [support@coralogix.com](mailto:support@coralogix.com).
