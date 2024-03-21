---
title: "Ignore Errors"
date: "2023-11-07"
---

As part of [Error Tracking](https://coralogixstg.wpengine.com/docs/error-tracking/), we understand that not all errors are created equal. Some errors may be inconsequential, recurring, or known issues that do not warrant immediate attention. To enhance your [Real User Monitoring](https://coralogixstg.wpengine.com/docs/real-user-monitoring/) experience, we provide a feature that allows you to selectively **ignore errors**. This functionality empowers you to focus on the critical issues that truly impact user experience and streamline your error tracking process.

## Ignoring Errors: Benefits

Enjoy the following feature benefits:

- **Noise Reduction.** In a dynamic web environment, it's common for non-critical errors to occur. Ignoring these errors helps reduce noise in your error tracking system, making it easier to pinpoint and address the most impactful issues.

- **Customized Monitoring.** With the ability to ignore specific error types, you can tailor your RUM setup to align with your priorities and goals. This ensures that you receive alerts and insights on errors that matter most to your business.

- **Efficient Resource Utilization.** By filtering out low-priority errors, you can optimize your resources and reduce the time and effort spent on investigating and resolving issues that have minimal impact on user satisfaction and business operations.

## Getting Started

Refer to our [Browser SDK Installation Guide](https://coralogixstg.wpengine.com/docs/browser-sdk-installation-guide/#ignore-errors) for Ignore Error setup instructions or get started by following these steps:

- Using Regex, you can set up the SDK to reject all request with a full or partial URL, as in the following example.

```
ignoreUrls: [
    '<https://my-server.com/user/123>', // will ignore all requests match this url
    /.*\\.svg/, // will ignore all requests match this regex - all svg files
    /.*material-override.*/, // will ignore all requests match this regex - all urls with the string material-override
  ],

```

- You also have the option setup the SDK to reject all errors with a particular message or string, as in the following example.

```
ignoreErrors: [
    'This is a custom error', // will ignore all errors with this message
    /.*my-error.*/, // will ignore all errors match this regex - all errors with the string my-error
  ],

```

## Next Steps

[Get started](https://coralogixstg.wpengine.com/docs/error-tracking/) with **Error Tracking**. Use our dedicated [User Manual](https://coralogixstg.wpengine.com/docs/error-tracking-user-manual/) for support.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/browser-sdk-installation-guide/"><strong>RUM SDK Installation Guide</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
