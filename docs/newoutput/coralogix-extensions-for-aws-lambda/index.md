---
title: "Coralogix Extensions for Amazon Web Services (AWS) Lambda"
date: "2022-02-16"
---

Coralogix provides an easy way to collect your Cloud watch metrics.  
The preferred and easiest integration method will be to use our AWS [Serverless Application Repository](https://eu-central-1.console.aws.amazon.com/serverlessrepo/home?region=eu-central-1#/available-applications).  
Our code is open source and you can see it on [Github](https://github.com/coralogix/coralogix-aws-serverless/tree/master/src/lambda-extension)

## Requirements

- Your AWS user should have permissions to create lambdas and IAM roles.

## Installation - Lambda layer

- Navigate to the [Application Page](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/create/app?applicationId=arn:aws:serverlessrepo:eu-central-1:597078901540:applications/Coralogix-Lambda-Extension).

- Based on your lambda write the runtime in 'CompatibleRuntimes'

- Based on your lambda architecture put 'true' in either 'AMD64SupportEnabled' or 'ARM64SupportEnabled' and 'false' in the other

- Click Deploy.

## Installation - Container image lambda

In case you deploy your lambda as container image, to inject extension as part of your function just copy it to your image:

```dockerfile
FROM coralogixrepo/coralogix-lambda-extension:latest AS coralogix-extension
FROM public.ecr.aws/lambda/python:3.8
# Layer code
WORKDIR /opt
COPY --from=coralogix-extension /opt/ .
# Function code
WORKDIR /var/task
COPY app.py .
CMD ["app.lambda_handler"]
```

Note: in this example, python3.8 is being used as the runtime but all runtimes are supported with our lambda extension.

### Usage

Once the deployment is done the Layer is ready and you can add it to any Lambda you wish to collect logs from.  
_This is how_:

1. Go to the Lambda functions from which you want to send logs to Coralogix, choose the 'Layers' component, and click 'Add Layer'

3. Select Custom Layers, choose the coralogix-extension layer that matches your lambda architecture, select the latest version and click 'Add'

5. Go to the environment variables section in your Lambda function and add the following ENV variables:
    1. **CORALOGIX\_PRIVATE\_KEY -** The [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/), a unique ID that represents your company.
    
    3. **CORALOGIX\_APP\_NAME -** A mandatory metadata field that is sent with the logs. Helps identify the logs sent by this Lambda function.
    
    5. **CORALOGIX\_SUB\_SYSTEM -** A mandatory metadata field sent with the logs. Helps identify the logs sent by this Lambda function.
    
    7. **CORALOGIX\_LOG\_URL (Optional) -** use https://<cluster\_url>/api/v1/logs  
        Using the table below find the appropriate '_Cluster URL'_ .  
        

### Coralogix Endpoints <cluster\_url>

| Region | Logs Endpoint |
| --- | --- |
| EU | `api.coralogixstg.wpengine.com` |
| EU2 | `api.eu2.coralogixstg.wpengine.com` |
| US | `api.coralogix.us` |
| SG | `api.coralogixsg.com` |
| IN | `api.app.coralogix.in` |

You're all set! Data will begin streaming to Coralogix.

Need any help with your integration? ping us on our in-app chat or at support@coralogixstg.wpengine.com.
