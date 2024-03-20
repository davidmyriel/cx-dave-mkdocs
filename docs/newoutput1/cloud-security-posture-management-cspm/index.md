---
title: "Cloud Security Posture Management (CSPM)"
date: "2023-09-07"
---

![](images/CSPM-updated.png)

Reduce your attack surface and prevent risk of cloud data security breaches by assessing the overall security posture of your entire cloud environment against best practices and compliance standards using our **Cloud** **Security Posture Management (CSMP)**.

## What is CSPM?

Cloud Security Posture Management (CSPM) is a cybersecurity approach that focuses on continuously monitoring and improving the security of cloud resources and configurations. CSPM tools automate the process of assessing cloud environments, verifying that cloud configurations follow security best practices and compliance standards such as ISO, CIS AWS, SOC 2, PCI, and HIPAA frameworks. This provides organizations with real-time visibility, proactive risk mitigation, and the ability to maintain a strong security posture across their cloud deployments. As companies increasingly move to the cloud, CSPM has become a necessary aspect of security insights.

## How It Works

Our CSPM solution is based on an agent that runs in the customer’s cloud environment. The CSPM agent performs tests to assess the security posture - checking if the current cloud setting is aligned with the recommended best practice or not. The test results are then sent to Coralogix to be displayed in the Coralogix Security UI.

In order to set up CSPM, you will be required to take the following steps:

1. [Create roles](#CreateRoles) necessary for resource scanning;

3. Install the CSPM agent using a [virtual machine](#InstallationusingaVirtualMachine) or [orchestration tool](#InstallationusinganOrchestrationTool) such as Kubernetes;

5. [Configure settings](#Configuration), including scanning and environment settings, as well as those designed to send scanning results to Coralogix for visualization in your dashboard.

## Supported Services

Below you can find a list of the services that we currently support.

### CSPM Testing Availability

- **AWS (named: "aws"):** WAF, KMS, Lambda, S3, RDS, EC2, IAM, CloudTrail, CloudFront, VPC, DocumentDB, FSx, DynamoDB/Dax, SQS, Api Gateway, Security Hub, ECR, ELB (v1/v2), EBS, Backup, EMR, AutoScaling, EKS, CloudWatch, Redshift, OpenSearch, NetworkFirewall, Elasticache

- **GCP (named: "gcp"):** CloudDNS, CloudSQL, API Keys, GCE, GKE, VPC, Resource manager, Cloud Storage, Cloud Logging, Cloud Load Balancer, PubSub, Dataproc, KMS, IAM

### SSPM Testing Availability

- **GitHub (named: "github")**

## Create Roles

Create roles to grant relevant permissions to scan resources necessary for the CSPM agent.

### AWS

**STEP 1.** In each AWS account containing resources to scan, go to AWS IAM and create a role with these permissions:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CSPM",
            "Effect": "Allow",
            "Action": [
                "access-analyzer:Get*",
                "access-analyzer:List*",
                "acm:Describe*",
                "apigateway:Get*",
                "application-autoscaling:Describe*",
                "autoscaling-plans:Describe*",
                "autoscaling-plans:GetScalingPlanResourceForecastData",
                "autoscaling:Describe*",
                "autoscaling:GetPredictiveScalingForecast",
                "cloudformation:BatchDescribeTypeConfigurations",
                "cloudformation:Describe*",
                "cloudformation:DetectStack*",
                "cloudformation:EstimateTemplateCost",
                "cloudformation:Get*",
                "cloudformation:List*",
                "cloudformation:ValidateTemplate",
                "cloudfront:DescribeFunction",
                "cloudfront:Get*",
                "cloudfront:List*",
                "cloudtrail:Describe*",
                "cloudtrail:Get*",
                "cloudtrail:List*",
                "cloudtrail:LookupEvents",
                "cloudwatch:Describe*",
                "cloudwatch:Get*",
                "cloudwatch:List*",
                "dms:Describe*",
                "ec2:Describe*",
                "ec2:ExportClientVpn*",
                "ec2:Get*",
                "ec2:List*",
                "ec2:Search*",
                "ec2messages:Get*",
                "eks:Describe*",
                "eks:List*",
                "elasticache:Describe*",
                "elasticache:List*",
                "elasticloadbalancing:Describe*",
                "elasticmapreduce:Describe*",
                "elasticmapreduce:Get*",
                "elasticmapreduce:List*",
                "elasticmapreduce:ViewEventsFromAllClustersInConsole",
                "emr-containers:Describe*",
                "emr-containers:List*",
                "emr-serverless:Get*",
                "emr-serverless:List*",
                "es:Describe*",
                "es:Get*",
                "es:List*",
                "iam:Generate*",
                "iam:Get*",
                "iam:List*",
                "iam:Simulate*",
                "imagebuilder:Get*",
                "imagebuilder:List*",
                "kms:Describe*",
                "kms:Get*",
                "kms:List*",
                "lambda:Get*",
                "lambda:List*",
                "network-firewall:Describe*",
                "network-firewall:List*",
                "organizations:Describe*",
                "organizations:List*",
                "rds:Describe*",
                "redshift:Describe*",
                "redshift:List*",
                "redshift:ViewQueries*",
                "rolesanywhere:Get*",
                "rolesanywhere:list*",
                "route53:Get*",
                "route53:List*",
                "route53:TestDNSAnswer",
                "route53domains:CheckDomain*",
                "route53domains:Get*",
                "route53domains:List*",
                "route53domains:ViewBilling",
                "s3:Describe*",
                "s3:List*",
                "s3:GetBucketPublicAccessBlock",
                "s3:GetBucketPolicyStatus",
                "s3:GetEncryptionConfiguration",
                "s3:GetAccountPublicAccessBlock",
                "s3:GetBucketLogging",
                "s3:GetBucketVersioning",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation",
                "s3:GetBucketPolicy",
                "servicequotas:Get*",
                "servicequotas:List*",
                "ses:Describe*",
                "ses:Get*",
                "ses:List*",
                "sqs:Get*",
                "sqs:List*",
                "ssm:Describe*",
                "ssm:Get*",
                "ssm:List*",
                "sts:Get*",
                "tag:Get*",
                "waf-regional:Get*",
                "waf-regional:List*",
                "waf:Get*",
                "waf:List*",
                "wafv2:Describe*",
                "wafv2:Get*",
                "wafv2:List*",
                "elasticfilesystem:List*",
                "elasticfilesystem:Get*",
                "backup:List*",
                "backup:Get*",
                "redshift:Describe*",
                "redshift:ViewQueriesInConsole"
            ],
            "Resource": "*"
        }
    ]
}

```

**STEP 2.** If the CSPM agent will be deployed in a **different** AWS account than your resources, follow this step.

1\. In the AWS account where the CSPM agent is deployed:

a. Go to AWS IAM and create a role with this permission. In the “Resource” field, specify the Role ARN of all roles created in **STEP 1** above.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CSPM",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
						"Resource": [
						    "arn:aws:iam::111222333444:role/MyRole1",
						    "arn:aws:iam::111222333455:role/MyRole2"
						]
        }
    ]
}

```

b. Copy the Role ARN to be used below.

2\. In each of the AWS accounts where resources to scan exist (including the account where the CSPM agent will be deployed, if it contains resources to be scanned):

a. Go to AWS IAM and select the role created in **STEP 1** above.

b. In the Trust Relationships tab, add this section while pasting the Role ARN copied from **STEP 2.1.b** above into the “AWS” field:

```
{	
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111222333444:role/MyRole"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

c. Copy the Role ARN of this role to be used in the [CSPM](https://coralogixstg.wpengine.com/docs/cloud-security-posture-cspm-gcp/) agent configuration file below.

### GCP

**STEP 1.** Create a [service account](https://cloud.google.com/iam/docs/service-accounts-create) with the following permissions in the Google organization, folder, or project in which you have resources to scan.

```
apikeys.keys.list
bigquery.datasets.get
cloudkms.cryptoKeys.getIamPolicy
cloudkms.cryptoKeys.list
cloudkms.keyRings.list
cloudsql.instances.get
cloudsql.instances.list
compute.firewalls.list
compute.instances.list
compute.networks.list
compute.projects.get
compute.regions.get
compute.regions.list
compute.sslPolicies.get
compute.subnetworks.list
compute.targetHttpsProxies.list
container.clusters.list
dns.managedZones.get
dns.managedZones.list
iam.serviceAccountKeys.list
iam.serviceAccounts.list
logging.logMetrics.list
monitoring.alertPolicies.list
monitoring.notificationChannels.get
monitoring.notificationChannels.list
resourcemanager.projects.get
resourcemanager.projects.getIamPolicy
storage.buckets.get
storage.buckets.getIamPolicy
storage.buckets.list
compute.zones.list

```

**STEP 2.** Copy the email addresses of the service accounts you created, to be used in the CSPM agent configuration file.

### GitHub

**STEP 1.** Define a user as the owner for all of the organizations you wish to scan, while following [this procedure](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/maintaining-ownership-continuity-for-your-organization#appointing-an-organization-owner).

**STEP 2.** [Create a personal access token (classic)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic) for this user. Grant the the following scopes to this new token:

```
`read:audit_log`
`read:enterprise`
`read:gpg_key`
`read:org`
`read:project`
`read:public_key`
`read:repo_hook`
`read:ssh_signing_key`
`read:user`
`repo`
`user:email`

```

## Installation using a Virtual Machine

### **Prerequisites**

The following are minimum hardware requirements:

- 4 GiB RAM (To scan large cloud environments with 1000+ resources, use 8 GiB RAM or more.)

- 2+ CPU cores

### Installation

**STEP 1.** Access a virtual machine (such as an AWS EC2 instance) sized according to the guidelines in the prerequisites. Obtain terminal access (for example via SSH).

**STEP 2.** Install [Docker](https://docs.docker.com/engine/install/ubuntu/).

**STEP 3.** This step will vary depending on whether you are using AWS or GCP.

**For AWS:**

- Select the EC2 instance. Click **Actions** > **Security** > **Modify IAM role.**
    - To scan resources in the same account as the EC2 instance, select the role created in **STEP 1** in [Create Roles > AWS](#AWS) section above.
    
    - To only scan resources in an account other than where the EC2 instance is deployed, select the role created in **STEP 2.2** in the [Create Roles > AWS](#AWS) section above.

- After the role selection, click **Update IAM role**.

**For GCP:**

- Open the [Google Cloud Console](https://console.cloud.google.com/).

- Navigate to the **Compute Engine** section.

- Select the desired GCE instance.

- Click **Edit** to modify the instance.

- In the **Service account** section, click **Add item**.

- Select the service account created in the [Create Roles > GCP](#GCP) section above.

- Click **Save**.

**STEP 4.** Save the [default configuration file](https://coralogixstg.wpengine.com/wp-content/uploads/2023/10/defaults.yaml) as `local.yaml` in a local directory in the EC2.

- Configure the relevant sections for the resources you wish to scan:
    - Configuration File > AWS configuration file
    
    - Configuration File > GCP configuration file
    
    - Configuration File> GitHub configuration file

- Configure the Coralogix section following in the steps in [Configuration File > Coralogix Configuration](#CoralogixConfiguration).

**STEP 5.** Run the following command in a terminal from the folder within which the `local.yaml` file is located:

```
docker run -d -v ./local.yaml:/cxa/config/local.yaml -e RUST_LOG=info coralogixrepo/cspm-agent:latest

```

**Notes:**

- For AWS, if the same CSPM agent is used for scanning resources in multiple AWS accounts, repeat **STEP 4** and **STEP 5** for each AWS account you wish to scan.

- Use a different name for the configuration file for each AWS account (e.g. `local-myaccount.yaml`, `local-myaccount1.yaml`, `local-myaccount2.yaml`, etc.)

## Installation using an Orchestration Tool

**STEP 1.** Install [kubectl](https://kubernetes.io/docs/reference/kubectl/).

**STEP 2.** Save this file as `cspm-agent.yaml` in the current directory:

- [File for AWS](https://coralogixstg.wpengine.com/wp-content/uploads/2023/10/cxa-deployment.yaml)

- [File for GCP](https://coralogixstg.wpengine.com/wp-content/uploads/2023/10/cxa-deployment-nodepool-gce.yaml)

**STEP 3.** This step will vary depending on whether you are using AWS or GCP.

**For** **AWS:**

Attach the role to the cluster by following these steps.

- Open the Amazon EKS console and select your cluster.

- Select **Configuration** in the left-hand navigation pane.

- Select **Security** in the cluster details page.

- Under **Service account settings**, select **Add or remove IAM roles from service accounts**.

- Select the relevant namespace and service account.

- Choose **Add IAM role** and select this role:
    - To scan resources in the same account as the cluster, select the role created in **STEP 1** in the [Create Roles > AWS](#AWS) section above.
    
    - To only scan resources in an account other than where the cluster is deployed, select the role created in **STEP 2.2** in the [Create Roles > AWS](#AWS) section above.

- Save the changes.

**For GCP:**

Create a GKE node pool and attach the service account by following these steps.

- Open the [Google Cloud Console](https://console.cloud.google.com/).

- Navigate to the **GKE** section.

- Select the desired GKE cluster.

- Click on the **Node Pools** tab.

- Click on the **Add Node Pool** button.

- In the **Node pool details** section, provide the name **CSPM-POOL** for the node pool.

- In the **Service account** section, click **Add item**.

- Select the service account created in [Create Roles > GCP](#GCP) section above.

- Configure other settings for the node pool as per your requirements.

- Click **Create** to create the node pool.

**STEP 4.** Save the [default configuration file](https://coralogixstg.wpengine.com/wp-content/uploads/2023/10/defaults.yaml) as `local.yaml` in the same directory in which the `cspm-agent.yaml` file is saved. Configure the `local.yaml` file as follows:

- Configure the relevant sections for the resources you wish to scan:
    - [Configuration File > AWS Configuration](#AWSConfiguration)
    
    - [Configuration File > GCP Configuration](#GCPConfiguration)
    
    - [Configuration File> GitHub Configuration](#GitHubConfiguration)

- Configure the Coralogix section following in the steps in [Configuration File > Coralogix Configuration](#CoralogixConfiguration).

**STEP 5.** Run the following commands from the directory where the `cspm-agent.yaml` and `local.yaml` files are saved:

```
kubectl create configmap cspm-config --from-file=./local.yaml
kubectl apply -f cspm-agent.yaml

```

**STEP 6.** To stay up to date with the latest agent version, use a script to automatically rerun these commands periodically. It will automatically update the running agent when a newer version is released.

- We **recommend** verifying each new version in a staging environment before updating it in production.

- Run the above script daily as a [cron job](https://en.wikipedia.org/wiki/Cron) or use your favorite CI/CD platform to only update minor versions.

- Here are some example links:
    - [](https://github.com/coralogix/dspm-agent/blob/DSPM-14-As-a-Customer-I-want-CSPM-agent-to-automatically-upgraded-when-a-new-version-released)[https://argocd-image-updater.readthedocs.io/en/stable/](https://argocd-image-updater.readthedocs.io/en/stable/)
    
    - [](https://github.com/coralogix/dspm-agent/blob/DSPM-14-As-a-Customer-I-want-CSPM-agent-to-automatically-upgraded-when-a-new-version-released)[https://fluxcd.io/flux/guides/image-update/](https://fluxcd.io/flux/guides/image-update/)

**Notes:**

- For AWS, if the same CSPM agent is used for scanning resources in multiple AWS accounts, repeat **STEP 4** and **STEP 5** for each AWS account you wish to scan.

- Use a different name for the configuration file for each AWS account (e.g. `local-myaccount.yaml`).

## Configuration

A configuration file is used to define scanning and environment settings essential for the CSPM agent. Download the [default configuration file](https://coralogixstg.wpengine.com/wp-content/uploads/2023/10/defaults.yaml) and save it locally as instructed above. The configuration will vary depending on whether AWS, GCP, or GitHub is used. Once complete, configure the Coralogix section to send the scanning results to be displayed in your Coralogix dashboard.

### AWS Configuration

Unmark the AWS section and configure the relevant keys:

```
# AWS
aws:
# Default region
region: "eu-west-1"
# Retries in case the AWS API fails
retries: 100
# Optional role to use for querying the data
iam_role: "my-role"
# Regions to include for testers
include_regions: [ "sa-east-1", "us-east-2", "us-west-1", "us-west-2" ]
# In cross account access settings, test results from this master account are going to be skipped
master_account_id: 123456789012

```

| Key | Description |
| --- | --- |
| region | Fill in the AWS region where the CSPM agent is deployed (even if the agent is not expected to scan the AWS account where it is deployed) |
| retries | Select the number of scan retries in case of failure. |
| iam\_role | Optional: for scanning resources in a different AWS account, fill in the Role ARN copied in \[**STEP 2.2.c** in the [**Create Roles > AWS**](#AWS) section above\]. |
| include\_regions | \[**Optional**\] List all AWS regions of resources to scan, in addition to the “region” specified above, if any. |
| master\_account\_id | \[**Optional**\] For scanning resources in a different AWS account, fill in the AWS account ID where the CSPM agent is deployed. |

### GCP Configuration

Unmark the GCP section and configure the relevant keys:

```
# GCP
gcp:
  organization_configs:
  - organization_id: my-org-id
    organization_number: 123456789123
    client_email: "***"
  folder_configs: 
  - folder_id: my-folder-id
    folder_number: 123456789123
    client_email: "***"
	project_configs:
  - project_id: my-project-id
		project_number: 123456789123
    client_email: "***"
  postgres_max_connections: "100"
  sql_server_user_connections: "100"
# Your organization's email domain
organization_email_domain: "coralogixstg.wpengine.com"
# Duration in seconds 
storage_bucket_retention_period: 86400

```

- For each service account created in [Create Roles > GCP](#GCP) section above, fill in the relevant section according to the level where the service account was created (Google organization, folder or project):
    - `organization_id` / `folder_id` / `project_id`
    
    - `organization_number` / `folder_number` / `project_number`
    
    - `client_email` : the email identifier of the service account

- For multiple service accounts in the same level (Google organization, folder or project), duplicate the section starting with “-”. Make sure to keep the same structure and spaces.

- Remove the redundant sections with no service accounts in that level.

- `postgres_max_connections` : For scanning Postgres DB resource, set the maximum number of connections that the CSPM agent would initiate to a Postgres DB in parallel.

- `sql_server_user_connections` : For scanning SQL Server resource, set the maximum number of connections that the CSPM agent would initiate to an SQL Server in parallel.

- `organization_email_domain` : Fill in the organization’s email domain. The CSPM tests will use this to identify external users.

- `storage_bucket_retention_period` : Fill in the expected retention period in seconds for storage buckets. The CSPM tests will verify that this retention period is enforced.

### GitHub Configuration

Unmark the GitHub section and configure the relevant keys:

```
# GitHub
github:
# GitHub (classic) private access token. The required permissions are: 
# read:audit_log, read:enterprise, read:gpg_key, read:org, read:project, read:public_key, read:repo_hook, read:ssh_signing_key, read:user, repo, user:email
token: "ghp_99eiK0RNNRgYwLK8UT34PZybr1nDZoR"
# Which (GitHub) organizations to include in the testing. Note that the account has to have "owner" permissions for some testers. 
orgs: ["my-org"]

```

| Key | Description |
| --- | --- |
| token | Fill in the token created in STEP 2 in the [**Create Roles** > **GitHub**](#GitHub) section above. |
| orgs | Fill in all the organization names you wish to scan. |

### Coralogix Configuration

Configure the Coralogix section to send the scanning results to be displayed in Coralogix:

```
### CORALOGIX 
# Coralogix connection configuration
###

coralogix:
  # gRPC endpoint of the Coralogix cluster. Check the docs <https://coralogixstg.wpengine.com/docs/cloud-security-posture-cspm/>
  grpc_endpoint: https://ng-api-grpc.<coralogix-domain>
  # logs endpoint of the Coralogix cluster. Check the docs <https://coralogixstg.wpengine.com/docs/coralogix-domain/>
  logs_endpoint: https://ingress.<coralogix-domain>
  # Your Coralogix private key
  api_key: 877E1EB0-EBE2-4EEF-9170-9B418F98F654
  # Meta data application name for Coralogix
  application_name: MyApplication
  # Meta data subsystem name for Coralogix
  subsystem_name: MySubsystem
  # If true doesn't actually send the data
  dry_run: true

```

| Key | Description |
| --- | --- |
| `grpc_endpoint` | Update the <coralogix-domain> in the URL using your [Coralogix domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/). |
| `logs_endpoint` | Update the <coralogix-domain> in the URL using your [Coralogix domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).  
The CSPM agent sends the scanning results to this URL. |
| `api_key` | Your Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) |
| `application_name` / `subsystem_name` | [Metadata fields](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) that will be added to the results displayed in your Coralogix dashboard and will allow you to filter. |
| `dry_run` | false: Results sent to Coralogix  
true: For debugging mode, scan the resources without sending the logs to Coralogix. |

## Scanning

Coralogix triggers a scan, referred to as a run, by default every 24 hours. You can change this setting by navigating to **Security** in your Coralogix toobar, after logging into your Coralogix instance. Click on **SCAN SETTINGS** in the upper right-hand corner.

![](images/CSPM-image-2.png)

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/downloading-your-security-report/"><strong>Downloading Your Security Report</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
