---
title: "Direct Archive Query HTTP API"
date: "2023-06-13"
---

Use our **Direct Archive Query HTTP API** to run [DataPrime](https://coralogixstg.wpengine.com/docs/dataprime-query-language/) or [Lucene](https://coralogixstg.wpengine.com/docs/log-query-simply-retrieve-data/#lucene-query-syntax-reference) queries of your **indexed and archived logs** without the need to access your Coralogix UI.

## **API Reference**

The Direct Archive Query HTTP API is of an HTTP-style method.

Our API has predictable resource-oriented URLs, accepts POST JSON-formatted request bodies, returns Newline Delimited or NDJSON-formatted responses, and uses standard HTTP response codes, authentication, and verbs.

### Prerequisites

Requests must be sent using TLS.

### Base URL

Use the [Management Endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) that matches your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

When sending an HTTPS POST, you are required to present your arguments as JSON.

<table><tbody><tr><td><strong>HTTP Method</strong></td><td>POST</td></tr><tr><td><strong>Content-Type</strong></td><td>JSON</td></tr></tbody></table>

## **Authentication**

### Authorization Header

To authenticate, add an `Authorization Bearer <key>` header to your API request. It contains a Bearer Token, which identifies a single user, bot user, or workspace-application relationship

for authentication.

<table><tbody><tr><td><strong>Key</strong></td><td>Authorization</td></tr><tr><td><strong>Value</strong></td><td>Bearer &lt;Your Coralogix Logs Query Key&gt;</td></tr></tbody></table>

### API Key

The Direct Archive Query HTTP API uses your Logs Query API Key to authenticate requests. To access this API key in your Coralogix navigation pane, click **Data Flow** > API Keys > Logs Query API Key.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

## **Requests**

### Host

Use the [Management Endpoint](https://coralogixstg.wpengine.com/docs/coralogix-endpoints/) that matches your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

**All requests must be made over HTTPS. The API does not support HTTP.**

### Authorization Header

You must provide an authorization header as described in Authentication.

## **Formatting Your Request**

For **complete guidance on how to structure requests and responses**, view [this swagger.md](https://coralogixstg.wpengine.com/sw_viewer.html?url=https://coralogixstg.wpengine.com/wp-content/uploads/2023/09/swagger-1.md), a human-readable document derived from a swagger.json file.

[Download swagger.json](https://coralogixstg.wpengine.com/wp-content/uploads/2024/01/direct-archive-querty-swagger.json)

We **recommend** using [OpenAPI Editor](https://editor.swagger.io/) or a similar technology for support.

### Request Body

When submitting data to a resource via `POST`, you must submit your payload in JSON.

You are required to include a `query` in the API body.

All other fields are optional and form the metadata object.

## **Body Examples**

### JSON Object

The following example consists of a JSON object that represents the request

**Minimal DataPrime Query**

```
{
	"query": "source logs | limit 100"
}

```

**Dataprime with Lucene Operator**

```
{
	"query": "source logs | filter log_obj.subsystem_name == 'foo' | lucene 'coralogix.metadata.applicationName:bar'"
}

```

**Lucene Query**

```
{
	"query": "coralogix.metadata.applicationName:bar'",
	"metadata": {
		"syntax": "QUERY_SYNTAX_LUCENE"
	}
}

```

**Full DataPrime Query**

```
{
	"query": "source logs | limit 100",
	"metadata": {
		"tier": "TIER_FREQUENT_SEARCH",
		"syntax": "QUERY_SYNTAX_DATAPRIME",
		"startDate": "2023-05-29T11:20:00.00Z",
		"endDate": "2023-05-29T11:30:00.00Z",
		"defaultSource": "logs"
	}
}

```

### cURL

**Basic Usage**

```
curl --location '<url>/api/v1/dataprime/query' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <api-key>' \
--data '{
    "query": "source logs | limit 10"
}'

```

**With Metadata**

```
curl --location '<url>/api/v1/dataprime/query' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <api-key>' \
--data '{
    "query": "source logs | limit 10",
    "metadata": {
        "tier": "TIER_FREQUENT_SEARCH",
        "syntax": "QUERY_SYNTAX_DATAPRIME",
        "startDate": "2023-05-29T11:20:00.00Z",
        "endDate": "2023-05-29T11:30:00.00Z"
    }
}'

```

**Notes**:

- `startDate` and `endDate` assume Coordinated Universal Time (**UTC**). If the time zone of your timestamp differs, convert it to UTC or offset the timestamp to your time zone. The timestamp in Coralogix is usually presented in you local time.
    - For example, to start the query at 11:20 local time:
        - In San Fransisco (Pacific Daylight Time): 2023-05-29T11:20:00.00**\-07:00**
        
        - In India (India Standard Time): 2023-05-29T11:20:00.00+**05:30**

- To view or amend your the time zone settings in your Coralogix account, navigate to **Account settings** > Preferences > Change Your Time Zone Settings

**With `defaultSource`**

```
curl --location '<url>/api/v1/dataprime/query' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <api-key>' \
--data '{
    "query": "limit 10",
    "metadata": {
        "tier": "TIER_FREQUENT_SEARCH",
        "syntax": "QUERY_SYNTAX_DATAPRIME",
				"defaultSource": "logs"
    }
}'

```

## **Responses**

The `Content-Type` representation header is used to indicate the original  media type of the resource (prior to any content encoding applied for sending).

In responses, a `Content-Type` header provides the client with the actual content type of the returned content.

Correct results are returned in batches. Each batch may contain multiple rows of results, which are returned as Newline Delimited JSON or [ndjson format](http://ndjson.org/).

### Example Response

The following is a generic example response. Responses may vary per query and user data.

```
{
    "result": {
        "results": [
            {
                "metadata": [
                    {
                        "key": "logid",
                        "value": "c3ca5343-88dc-4807-a8f3-82832274afb7"
                    }
                ],
                "labels": [
                    {
                        "key": "applicationname",
                        "value": "staging"
                    }
                ],
                "userData": "{ ... \\"log_obj\\":{\\"level\\":\\"INFO\\",\\"message\\":\\"some log message\\" ... }, ...}"
            }
        ]
    }
}

```

### Status Codes

Here are some of the most common status codes to expect.

| **Status Code** | **Description** |
| --- | --- |
| 200 | No Error |
| 400 | Bad Request |
| 403 | Forbidden |

### Failed Requests

The general format guidelines are displayed when the accompanying status code is returned.

## **Warnings**

The `Warning` response contains information about possible problems with the status of the message. More than one `Warning` header may appear in a response.

`Warning` responses can be applied to any message, before or after query results.

| **Warning Type** | **Description** |
| --- | --- |
| CompileWarning | Warning of potential compilation error in your query. In the event of a compilation failure, you will receive an error response. |
| TimeRangeWarning | When the time frame for your query has been built incorrectly or exceeds internal limits |
| NumberOfResultsLimitWarning | When the number of query results exceeds internal limits |
| BytesScannedLimitWarning | When the number of bytes returned in query results exceeds internal limits |
| DeprecationWarning | When a value in your query is changed or deprecated incorrectly |

## Limitations

Coralogix places certain limitations on query responses. Warnings are returned when a limit is breached.

### Results Returned

The number of **results returned in rows** is limited as follows:

- 12k for [S3 Archive queries](https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/)

- 12k for [OpenSearch](https://coralogixstg.wpengine.com/docs/elastic-api/) queries

### Bytes Scanned Limit

The number of **bytes scanned** (for high-tier data) is limited as follows:

- 100MB for for [OpenSearch](https://coralogixstg.wpengine.com/docs/elastic-api/) queries

This limitation is placed on fetching 100 MB of high-tier data. It does **not** limit the scanning within the database storage.

### Rate Limiting

The **number of requests per minute** is limited as follows:

- 10 requests per minute

## **Additional Resources**

<table><tbody><tr><td><strong>Documentation</strong></td><td><a href="https://coralogixstg.wpengine.com/sw_viewer.html?url=https://coralogixstg.wpengine.com/wp-content/uploads/2023/09/swagger-1.md"><strong>Complete Guidance on Structuring Requests &amp; Responses (Swagger)</strong></a><br><strong><a href="https://coralogixstg.wpengine.com/docs/dataprime-query-language/">DataPrime Syntax</a></strong><br><strong><a href="https://coralogixstg.wpengine.com/docs/log-query-simply-retrieve-data/#lucene-query-syntax-reference">Lucene Syntax</a></strong><br><a href="https://coralogixstg.wpengine.com/docs/archive-s3-bucket-forever/"><strong>S3 Archive</strong></a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
