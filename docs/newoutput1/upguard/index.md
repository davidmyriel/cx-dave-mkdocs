---
title: "UpGuard"
date: "2023-01-23"
---

The following tutorial demonstrates how to send your logs to Coralogix using [UpGuard](https://www.upguard.com/). Follow this five-step guide for each notification that you would like to send us.

UpGuard uses webhooks to send notifications when an event happens in your UpGuard account. This could be when an identity breach or data leak is detected, the score of a watched vendor drops below a certain threshold, or when a user requests access to your shared profile.

## Configuration

**STEP 1**. **Create Integration.**

- Login to your Upguard account.

- Select **Settings** in your left-hand sidebar.

![Settings UpGuard Coralogix](https://lh3.googleusercontent.com/gqGpo_jrjSV0phV1Wqs37nuB93gWRqzzuPV3b7ObkZb_Xw2sZ1uGV-6VFsq4rEPIpXnZoH6EqXXk_2bcsKxWNCcSRZHoq5WZHEGvPvA0ppVfuY287ibSS9IOgEvn0iLdrrJfkbKvlGNfx10PqSjai_NEHnjKzofXDnggz1rTQQuNyvr80mBXCSUOwnc3ng)

- Click on the **Integrations** tab.

![Integrations UpGuard Coralogix](https://lh3.googleusercontent.com/Y7DDOOS_qtZKDXsTZVIRFgZxlHJIOtmQIfgW37GQ_6DDonaJVQt2vqenyTZGITb598Z71zU1s00E1xrx1MFlWZgTYSvC2SHDAZbVGh5aKDFqPehmwqH2Jo2fZ1yFv8eBJTFtdIrg3zMx06AOz6uH4r6Rzl21nagktOqDzbOtQHBcRs89MokW9dovB1twxw)

- Click **\+ New Integration**.

![New Integration UpGuard Coralogix](https://lh6.googleusercontent.com/_xFBKGFTQ-WpGqByu_lcGge5o6ZC9gulOWOc9WglqoqGH9lYpYQMGyxKRGJUE5jV7ySnVv5oHh15wy2LcPlD9-KIb57t9-vcxPILKfx1_V38oGL9f8LX67CS8ymOSTi5OaO6KIcQvzEdgpYdlNWRAm1oCYqXjPYZsAgYCJKxG2N__GErXBEwZGfQuxtKgA)

- Select **Webhook**.

![Webhook UpGuard Coralogix](https://lh3.googleusercontent.com/_GqpM7lD2alEBPz9k2pHrveOVC6b9NVh0F7ZiKhowhOk6nB9OKAjncG3x0Ymz8e5mCnU3GZ37x0UyUcgGs9EpGSEhzscjYBVo1H16tfCUgFdn1l8IL5nJQBzM1fYVRKAqpsyNmUiVfnVq_IGiuw7B1ypjkx51nSyWzkm-OgA2wnFnFX_Xi082g5V7Pge2A)

**STEP 2. Select Triggers**

- Select from a wide-range of pre-defined triggers to use as part of this integration. Examples include:
    - 'When my company's score drops below 600'
    
    - 'When a domain or IP's score drops below 600'
    
    - 'When a new identity breach is detected'
    
    - 'When a new identity breach for a VIP email is detected'

- Enable a trigger by clicking on the associated pill, which slides to the right.

![Triggers UpGuard Triggers](https://lh6.googleusercontent.com/kscB2jgi6XVGPyOPrU6iTjp43cn9DbC2uEEsYY8zBbd4ydxbCXdLDYRd5CYLVikkFgmh3b4RsLSFuh3ZWgrNYxIn_jgseE0-7Dpyh7D7BnRcY_uO4bufUfR1Vyy8QAsmJq2X7epvVWKS541Fv0bQgs3_FqnxKwsYEbMe0w4A8Rvy62twNfyCjSbCnM3QRw)

- Click **Confirm and next**.

**STEP 3. Name and Destination**

- Provide the webhook URL corresponding to the Coralogix cluster URL associated with the [domain and region](https://coralogixstg.wpengine.com/docs/coralogix-domain/) where your data is stored.

<table><tbody><tr><td><strong>Coralogix</strong> <strong>Cluster URL</strong></td><td><strong>API</strong> <strong>Endpoint</strong></td></tr><tr><td>.com</td><td>https://api.coralogixstg.wpengine.com</td></tr><tr><td>.us</td><td>https://api.coralogix.us</td></tr><tr><td>.in</td><td>https://api.app.coralogix.in</td></tr><tr><td>.app.eu2.coralogixstg.wpengine.com</td><td>https://api.eu2.coralogixstg.wpengine.com</td></tr><tr><td>.app.coralogixsg.com</td><td>https://api.coralogixsg.com</td></tr></tbody></table>

<table><tbody><tr><td><strong>Schema</strong><br><strong>Endpoint Details</strong></td><td></td></tr><tr><td><strong>Webhook URL</strong></td><td>https://api.&lt;clusterURL&gt;/api/v1/logs</td></tr><tr><td><strong>Content-Type&nbsp;</strong></td><td>application/json</td></tr></tbody></table>

For **example**, if your Coralogix data is hosted in India, your webhook URL should appear as **https://api.app.coralogix.in/api/v1/logs**.

- Configure the **HTTP Header** values by inputting **Content-Type: application/json**. As webhook by default uses POST method to send requests, there is no need to define the method.

![Name and destination UpGuard Coralogix](https://lh4.googleusercontent.com/hpOy34RmcwTjAGe14SSvbSoECZq8kPBvlLDmrjMRZpvW6WEGCxlAFpuoLoDDU970OnWXMfAfA9xmxHV1AKgXsZqeqnZns8uPKaqlSzdxoVoISetq1LRFAyyFLeEtju4tULBTEBaA6mnF6rfyQ1PYLFCoD2pt6OT7P5s628jHDDdlqDk4BTPjSJf2dn2izw)

  
Example:

![](images/image-4-1024x702.png)

- Click **Confirm and next**.

**STEP 4. Define Payload Structure**

- For each trigger, UpGuard provides a default payload template as in the example below.

![Example payload data UpGuard Coralogix](https://lh4.googleusercontent.com/DrrpV6Q2Xl0rWYvjYEOHpZeoc4bJb4pNfgWgdIeXVojg5OCmqrVHppRGs5sH8jaEWJz2oMJw9q6syv945v3gSHCin1UZTzgUFTJ0JDoiqTV6hnpUpYJH_YNEXPgZymYtxcZrEzPIlkTVApDUE6KpRIMw_fPdGxaLwdk4dZL7ZRdJLSxZrw_3bVKxdlwfPQ)

- Modify the payload template to comply with the Coralogix structure.

**POST Body**

<table><tbody><tr><td><strong>Required</strong></td><td><strong>Property Name</strong></td><td><strong>Property Type</strong></td><td><strong>Note</strong></td></tr><tr><td>Yes</td><td>privateKey</td><td>UUID</td><td></td></tr><tr><td>Yes</td><td>applicationName</td><td>string</td><td>usually used to separate environments</td></tr><tr><td>Yes</td><td>subsystemName</td><td>string</td><td>usually used to separate components</td></tr><tr><td></td><td>computerName</td><td>string</td><td></td></tr><tr><td>Yes</td><td>logEntries</td><td>array of logs</td><td></td></tr></tbody></table>

**Log**

<table><tbody><tr><td><strong>Required</strong></td><td><strong>Property Name</strong></td><td><strong>Property Type</strong></td><td><strong>Notes</strong></td></tr><tr><td>Yes</td><td>timestamp</td><td>number</td><td>UTC milliseconds since 1970 (supports sub millisecond via a floating point)</td></tr><tr><td>Yes</td><td>severity</td><td>number</td><td>1 – Debug, 2 – Verbose, 3 – Info, 4 – Warn, 5 – Error, 6 – Critical</td></tr><tr><td>Yes</td><td>text</td><td>string</td><td></td></tr></tbody></table>

- Wrap the payload template in JSON as follows. You will need to input your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/), [application and subsystem](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) names, and computer name.

```
{
   "privateKey": "<Coralogix send your data api-key>",
   "applicationName": "<application name>",
   "subsystemName": "<subsytem name>",
   "computerName": "<computer name>",
   "logEntries": [
     {
       "severity": <default severity of event 1-6>,
       "text": {
   "notification": {
   }
 }
     }
   ]
 }
```

- The following is an **example** of a Coralogix-compatible payload template.

```
{ 
"privateKey": "xxxxxxx-xxxxxx-xxxxxx-xxxxxxx",
   "applicationName": "upguard",
   "subsystemName": "upguard",
   "computerName": "upguard01",
   "logEntries": [
     {
       "severity": 4,
       "text": {
   "notification": {
     "id": {{ notification.id }},
     "type": "{{ notification.type }}",
     "description": "{{ notification.description }}",
     "occurredAt": "{{ notification.occurredAt }}",
     "context":     {
       "PrevScore": {{ notification.context.PrevScore }},
       "PrevScoreOn": "{{ notification.context.PrevScoreOn }}",
       "Threshold": {{ notification.context.Threshold }},
       "LatestScore": {{ notification.context.LatestScore }},
       "LatestScoreOn": "{{ notification.context.LatestScoreOn }}"
     }
   }
 }
     }
   ]
 }
```

- Validate that your webhook is working properly by clicking **Send test message**. The value '200 OK' should appear as the **Response**.

![Payload template response UpGuard Coralogix](https://lh4.googleusercontent.com/JCTLFfvwULkmLjK2tqEbwB2DqYnj4tQHvYR0iGx-2mx17grgVbULcjCvRMmoeYQsGlAaXMrWD9esDVL15EYSCFpBAYtGa0hASPSsViOV34_p-MdSTnjj7iSc-GYkOZpkrKws7V3jrHUDL8ITrr7MA-wGTA0KQ5JbdHHIP5r5zwofQnvJL-rtf_i08jTp1g)

Example:  

![](images/image-5-1024x878.png)

- Validate that Coralogix has received the test notification by searching the logs in your Coralogix dashboard.

![](images/image-8-1024x248.png)

- Click **Confirm and next**.

![](images/image-7-1024x316.png)

  
  

**STEP 5. Enable the Integration**

- Click the toggle to enable the integration and click **Finish**.

![Enable integration UpGuard Coralogix](https://lh3.googleusercontent.com/UKZ1qJa0JNjITHX0Jrbz58bMD7WEOndq2PKDFFl3DlC0XsKyyWyGbmnapEJ37uEm4nXAq6ga96m_k9Q5urIaZ6b11-fQzmuL_qNUhN4BarEkvWnqVZ0dbN79JA7kBDeUKWnICsVME4NuH_kDQLYADDxIh1i5IF1DJJ69OdJXqozuBG22T-cJ_yEjSfKL3A)

## A**dditional Resources**

<table><tbody><tr><td><strong>UpGuard</strong></td><td><a href="https://help.upguard.com/en/articles/4205928-how-to-integrate-upguard-with-other-services-using-webhooks">Webhook documentation for advanced modification of Webhook Payload</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
