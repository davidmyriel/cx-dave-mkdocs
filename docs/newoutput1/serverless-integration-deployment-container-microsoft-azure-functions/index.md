---
title: "Serverless Integration Deployment Container: Microsoft Azure Functions"
date: "2023-02-14"
---

Coralogix provides a seamless integration with `Azure` cloud so you can send your logs from anywhere and parse them according to your needs. The following tutorial demonstrates a Coralogix Azure serverless integration deployment using Docker.

## [](https://github.com/coralogix/coralogix-azure-deploy#prerequisites)Prerequisites

- An [Azure account](https://azure.microsoft.com/en-us/free/) with an active subscription

- [Docker](https://docs.docker.com/get-docker/)

## [](https://github.com/coralogix/coralogix-azure-deploy#installation)Installation

**STEP 1**. Clone this repo:

```
$ git clone https://github.com/coralogix/coralogix-azure-deploy.git
$ cd coralogix-azure-deploy
```

**STEP 2**. Modify the requisite .vars file for your intended integration deployment:

```
BlobStorage.vars
StorageQueue.vars
EventHub.vars
```

**STEP 3**. [Log in](https://docs.docker.com/engine/reference/commandline/login/) to a Dockerhub registry.

**STEP 4**. Run the following command:

```
$ docker run -it --rm --platform linux/amd64 --env-file <integration>.vars coralogixrepo/azure-functions-deploy
```

**STEP 5**. The Azure CLI will request authorization. You'll need to follow the prompt and authenticate through your browser with a provided code.

## Additional Resources

<table><tbody><tr><td><strong>Github</strong></td><td><a href="https://github.com/coralogix/coralogix-azure-deploy">Coralogix Azure serverless integration deployment container</a></td></tr><tr><td><strong>Microsoft Azure Functions</strong> <strong>Manual Integrations</strong></td><td><a href="https://coralogixstg.wpengine.com/docs/azure-eventhub-trigger-function/">Event Hub</a><br><a href="https://coralogixstg.wpengine.com/docs/blobstorage-microsoft-azure-functions/">Blob Storage</a><br><a href="https://coralogixstg.wpengine.com/docs/queue-storage-microsoft-azure-functions/">Queue Storage</a></td></tr></tbody></table>

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
