# Coralogix REST API /singles

## Example REST API Request

``` bash

curl --location --request POST 'https://ingress.<domain>/logs/v1/singles' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer <Send-Your-Data API key>' \
  --data-raw '[
    {
      "applicationName": "*insert desired application name*",
      "subsystemName": "*insert desired subsystem name*",
      "computerName": "*insert computer name*",
      "severity": 3,
      "text": "this is a normal text message",
      "category": "cat-1",
      "className": "class-1",
      "methodName": "method-1",
      "threadId": "thread-1",
      "timestamp": 1675148539123.342
    },
    {
      "applicationName": "*insert desired application name*",
      "subsystemName": "*insert desired subsystem name*",
      "computerName": "*insert computer name*",
      "hiResTimestamp": "1675148539789123123",
      "severity": 5,
      "text": "{\"key1\":\"val1\",\"key2\":\"val2\",\"key3\":\"val3\",\"key4\":\"val4\"}",
      "category": "DAL",
      "className": "UserManager",
      "methodName": "RegisterUser",
      "threadId": "a-352"
    }
  ]'

```

