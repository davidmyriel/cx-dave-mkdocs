---
title: "cURL Version Tags"
date: "2021-06-10"
---

**Note:** This document includes cluster-dependent URLs. Please refer to the following table to select the correct Coralogix Version Tags API endpoint for your Coralogix Portal domain’s extension (.com/.us/.in).  
  
The following examples have been built for the .com Coralogix Portal domain extension.  
  
Please use the corresponding API endpoint for your domain:

You can add version tags per Application and Subsystem using cURL:

**URL:** https://webapi.coralogixstg.wpengine.com/api/v1/external/tags

**Method:** POST

**Body Schema:**

```
{"iconUrl": string,"name": string,"timestamp": date type (JavaScript date string or epoch milliseconds)"application": string[]"subsystem": string[]}
```

**Application:** Your Application name.

**Subsystem:** Your Subsystem(s) name(s). You can input more than one subsystem name, use comma delimiter ‘,’ between Subsystem names.

**Name:** Your Version Tag name.

**Timestamp:** Tag Timestamp. (This is **OPTIONAL**: If a timestamp is not supplied, the current timestamp will be applied).

**IconUrl:** Tag's Picture. (This is **OPTIONAL**: Use an URL _(URL encoded)_ to a valid image file uploaded to a public repository)  
  
**Supported Image Formats:** png, jpeg, SVG.  
  
**Maximum File Size:** 50 Kilobytes.  
  
**Note:** If the URL to the image contains spaces, please use _%20_.  
  
For example:

https://www.myimagesite.com/Path%20To%20The%20Image.png.

**Example (GET):**

```
curl --location --request GET 'http://webapi.coralogixstg.wpengine.com/api/v1/external/tags/add?key=&application=Application_Name,Application_Name2&subsystem=Subsystem1,Subsystem2,Subsystem3&name=MyFirstTag&timestamp=2020-06-23:09:00:00&iconUrl=your_icon_url'
```

**Example (POST):**

```
curl --location --request POST 'https://webapi.coralogixstg.wpengine.com/api/v1/external/tags' \
--header 'Authorization: Bearer 12345678-abcd-efgh-ijk-123456789012' \
--header 'Content-Type: application/json' \
--data-raw '{
"timestamp": 1617793927675,
"name": "test tag",
"application": ["prd", "dev"],
"subsystem": ["app", "mobile"],
"iconUrl": "my-avatar.png"
}'
```

**Note:** application and subsystem should always be contained between square brackets, even if it is an array of one element, for example:  
  
`"application": ["prd"]`,  
`"subsystem": ["app"]`  
  
  
The API key should be taken from:  
  
**Data Flow --> API Keys --> "Alerts, Rules and Tags API Key"**

![Coralogix Alerts API key](images/Screenshot-2021-11-17-at-19.51.19.png)
