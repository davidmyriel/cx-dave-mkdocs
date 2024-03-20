---
title: "JumpCloud SCIM Identity Management"
date: "2023-01-19"
---

Send your logs to Coralogix using the [JumpCloud SCIM Identity Management Integration](http://Custom SCIM Identity Management integration).

The Custom SCIM Identity Management integration allows you to provision, update, and deprovision users and groups from JumpCloud in applications that support SCIM. Leverage this integration to centralize user lifecycle management, sync user data, manage groups, and control access and authorization from the JumpCloud Admin Portal.

## Configuration

**STEP 1. Single Sign-On**

Login to JumpCloud portal. Select **SSO** \> **Coralogix SSO application**

![JumpCloud SCIM Identity Management Integration SSO](images/image-30.png)

**STEP 2. Service Provider Configuration**

- Navigate to the **Identity Management** tab.

![JumpCloud SCIM Identity Management Integration Base URL](images/image-31-1024x583.png)

- Input the **Base URL** as one of the following values for **SCIM Connector Base URL**. The URL should correspond to the Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/) where your data is stored.

<table><tbody><tr><td><strong>Region</strong></td><td><strong>Tenant URL</strong></td></tr><tr><td>US1</td><td><a href="https://ng-api-http.coralogix.us/scim">https://ng-api-http.coralogix.us/scim</a></td></tr><tr><td>EU1</td><td><a href="https://ng-api-http.coralogixstg.wpengine.com/scim">https://ng-api-http.coralogixstg.wpengine.com/scim</a></td></tr><tr><td>EU2</td><td><a href="https://ng-api-http.eu2.coralogixstg.wpengine.com/scim">https://ng-api-http.eu2.coralogixstg.wpengine.com/scim</a></td></tr><tr><td>AP1 (IN)</td><td><a href="https://ng-api-http.app.coralogix.in/scim">https://ng-api-http.app.coralogix.in/scim</a></td></tr><tr><td>AP2 (SG)</td><td><a href="https://ng-api-http.coralogixsg.com/scim">https://ng-api-http.coralogixsg.com/scim</a></td></tr></tbody></table>

- Add the **Token Key**. This can be found in your Coralogix team settings > **Configure SAML** > **Provisioning Token**

![JumpCloud SCIM Identity Management Integration Configure SAML](images/image-32-298x1024.png)

![JumpCloud SCIM Identity Management Integration SAML Configuration](images/image-33-886x1024.png)

- Input the email address of a user that belongs to one of the relevant groups in your SSO under **Test User Email**.

![JumpCloud SCIM Identity Management Integration User Email](images/image-35-1024x583.png)

**STEP 3. Test Connection**

- Click **Test Connection.**  You should see the following popup confirming the connection was successful:

![JumpCloud SCIM Identity Management Integration Successful Connection](images/image-36.png)

- Ensure **Group management** is **enabled**.

**STEP 4. Activation**

- Click **Activate.**

![JumpCloud SCIM Identity Management Integration Activation](images/image-37-1024x599.png)

- Click **Save.**

**Step 5. Assign your groups to the Coralogix SSO application**

- Go to JumpCloud "User Groups".

- For each group that should be managed by SCIM, attach it to the Coralogix SSO application as shown below.

![](images/image-10-1024x498.png)

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
