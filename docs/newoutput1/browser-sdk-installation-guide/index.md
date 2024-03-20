---
title: "NPM Browser SDK Installation Guide"
date: "2023-11-07"
---

As part of our [Real User Monitoring](https://coralogixstg.wpengine.com/docs/real-user-monitoring/) (RUM) toolkit, Coralogix offers multi-faceted **Error Tracking,** enabled by our **NPM** **RUM** **Browser SDK**.

Integrated into the front end of web applications, this lightweight code tool detects and captures errors that arise within users' browsers, including JavaScript runtime errors, unhandled exceptions, network errors, and application (custom logic) errors. The SDK collects essential error information and additional contextual data, such as browser details and URLs, and securely sends it to our platform through logs for further analysis.

For information on our **CDN** **Browser SDK,** view [this page](https://coralogixstg.wpengine.com/docs/cdn-browser-sdk-installation-guide/).

## Prerequisites

Deploy our [RUM Integration Package](https://coralogixstg.wpengine.com/docs/rum-integration-package/). This includes creating your RUM API key, which is required for the Browser SDK setup.

## Setup

### Configuration

**STEP 1**. Install the Coralogix [NPM Browser SDK package](https://www.npmjs.com/package/@coralogix/browser).

```
npm install @coralogix/browser

```

**STEP 2**. Initialize the SDK.

```
    CoralogixRum.init({
      public_key: 'abc-123-456',
      application: 'natan-test',
      version: '1.0',
      coralogixdDomain: 'EU2',
      user_context: {
       user_email: 'test@test.com',
       user_name: 'aa',
       user_id: '123'
      },
      labels: {
        payment: 'visa'
      }
    });

```

Input the following basic parameters.

| Basic Parameters | Description |
| --- | --- |
| public\_key | The SDK uses your Coralogix RUM API Key to authenticate you. |
| application | Details for the specific user, including user\_email, user\_name, and user\_id. This can be sent later to Coralogix if you do not have these details during installation. |
| version | Input your application version. This allows Coralogix to match your code to your data. |
| coralogixDomain | Input your Coralogix [**domain**](https://coralogixstg.wpengine.com/docs/coralogix-domain/). |
| user\_context | Details for the specific user, including user\_email, user\_name, and user\_id. This can be sent later to Coralogix if you do not have these details during the installation process. |
| labels | Input labels of your choosing. |

You also have the options to add other instrumentation options with other advanced parameters.

```
export interface CoralogixOtelWebConfig {
  /** Publicly-visible `public_key` value */
  public_key: string;

  /** Sets a value for the `application` attribute */
  application: string;

  /** Coralogix account domain */
  coralogixdDomain: CoralogixDomain;

  /** Configuration for user context. */
  user_context?: UserContextConfig;

  /** Turns on/off internal debug logging */
  debug?: boolean;

  /** Sets a value for the 'app.version' attribute */
  version?: string;

  /** Sets labels added to every Span. */
  labels?: CoralogixRumLabels;

  /**
   * Applies for XHR and Fetch URLs. URLs that partially match any regex in ignoreUrls will not be traced.
   * In addition, URLs that are _exact matches_ of strings in ignoreUrls will also not be traced.
   * */
  ignoreUrls?: Array<string | RegExp>;

  /**
   A pattern for error messages which should not be sent to Coralogix. By default, all errors will be sent.
   * */
  ignoreErrors?: Array<string | RegExp>;

  /** Configuration for instrumentation modules. */
  instrumentations?: CoralogixOtelWebOptionsInstrumentations;

  /** Add Traceparent to headers in order to start a trace */
  traceParentInHeader?: TraceHeaderConfiguration;
}

```

**Note**:

- `traceParentInHeader`: If you are using [OpenTelemetry](https://coralogixstg.wpengine.com/docs/opentelemetry/) instrumentation in your backend services, use this flag to ensure your frontend creates spans.

### Ignore Errors

Coralogix provides a feature that allows you to selectively [ignore errors](https://www.notion.so/Ignore-Errors-50ed194e1fe441db8924676f3d072520?pvs=21). This functionality lets you focus on the critical issues impacting user experience and streamline your error-tracking process.

Using Regex, you can set up the SDK to reject all requests with a full or partial URL, as in the following example.

```
ignoreUrls: [
    '<https://my-server.com/user/123>', // will ignore all requests match this url
    /.*\\.svg/, // will ignore all requests match this regex - all svg files
    /.*material-override.*/, // will ignore all requests match this regex - all urls with the string material-override
  ],

```

You also have the option setup the SDK to reject all errors with a particular message or string, as in the following example.

```
ignoreErrors: [
    'This is a custom error', // will ignore all errors with this message
    /.*my-error.*/, // will ignore all errors match this regex - all errors with the string my-error
  ],

```

### Upload Your Source Maps

Install the RUM CLI tool and upload your source maps. Find out more [here](https://coralogixstg.wpengine.com/docs/rum-source-maps/).

## Next Steps

[Get started](https://coralogixstg.wpengine.com/docs/error-tracking/) with **Error Tracking**. Use our dedicated [User Manual](https://coralogixstg.wpengine.com/docs/error-tracking-user-manual/) for support.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><strong><a href="https://coralogixstg.wpengine.com/docs/real-user-monitoring/">Real User Monitoring</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/rum-integration-package/">RUM Integration Package</a></strong><br><a href="https://coralogixstg.wpengine.com/docs/error-tracking/"><strong>Error Tracking</strong></a><br><strong><a href="https://coralogixstg.wpengine.com/docs/error-tracking-user-manual/">Error Tracking: User Manual</a></strong></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
