---
title: "CDN Browser SDK Installation Guide"
date: "2023-11-07"
---

As part of our [Real User Monitoring](https://coralogixstg.wpengine.com/docs/real-user-monitoring/) (RUM) toolkit, Coralogix offers multi-faceted Error Tracking, enabled by our **CDN RUM Browser SDK**.

## Overview

Perfect for developers seeking client-side SDKs in web development and faster load times, our **CDN Browser SDK** detects and captures errors that arise within users’ browsers, unhandled exceptions, network errors, and application (custom logic) errors. The SDK collects essential error information and additional contextual data, such as browser details and URLs, and securely sends it to our platform through logs for further analysis.

For information on our **NPM Browser SDK**, view [this page](https://coralogixstg.wpengine.com/docs/browser-sdk-installation-guide/).

## Configuration

Add the CDN script to your application.

- \[**Recommended**\] The [](https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/latest/coralogix-browser-sdk.js)latest browser version: [https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/latest/coralogix-browser-sdk.js](https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/latest/coralogix-browser-sdk.js)

- A specific browser version: [](https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/latest/coralogix-browser-sdk.js)[https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/\[version\]/coralogix-browser-sdk.js](https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/**%5Bversion%5D**/coralogix-browser-sdk.js). Replace \[version\] with a version from our [Releases page](https://www.npmjs.com/package/@coralogix/browser?activeTab=versions).

```
<head>
  ...
<script src="<https://cdn.rum-ingress-coralogixstg.wpengine.com/coralogix/browser/latest/coralogix-browser-sdk.js>"></script>
</head>

```

## Initialization

Initialize the SDK using a JS or TS file.

### JS File

```
window.CoralogixRum.init({
  application: 'app-name',
  environment: 'production',
  public_key: 'abc-123-456',
  coralogixDomain: 'EU2',
  version: 'v1.0.3',
  labels: {
    payment: 'visa',
  },
  ignoreErrors: ['some error message to ignore'],
});

```

### TS File

```
window.CoralogixRum.init({
  application: 'app-name',
  environment: 'production',
  public_key: 'abc-123-456',
  coralogixDomain: 'EU2',
  version: 'v1.0.3',
  labels: {
    payment: 'visa',
  },
  ignoreErrors: ['some error message to ignore'],
});

// In case of warning from TSC
declare global {
  interface Window {
    CoralogixRum:any;
  }
}

```

## **Next Steps**

[Get started](https://coralogixstg.wpengine.com/docs/error-tracking/) with **Error Tracking**. Use our dedicated [user manual](https://coralogixstg.wpengine.com/docs/error-tracking-user-manual/) for support.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/real-user-monitoring/">Real User Monitoring</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/rum-integration-package/">RUM Integration Package</a></strong><br><a href="https://coralogixstg.wpengine.com/docs/error-tracking/"><strong>Error Tracking</strong></a><br><strong><a href="https://coralogixstg.wpengine.com/docs/error-tracking-user-manual/">Error Tracking: User Manual</a></strong></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
