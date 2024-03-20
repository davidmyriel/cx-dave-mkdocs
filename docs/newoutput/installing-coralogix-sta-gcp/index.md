---
title: "Installing Coralogix STA - GCP"
date: "2024-01-31"
---

The [Coralogix Security Traffic Analyzer](https://coralogixstg.wpengine.com/docs/cloud-security-quick-start/) (STA) is a Coralogix tool designed for tasks such as deep packet inspection, packet capturing, cloud configuration vulnerability scanning, and other security-related functions.

In Google Cloud Platform (GCP), the implementation of STA is exclusively carried out through Terraform.

## Prerequisites

- Configuration saved using Cloud Storage

- The following permissions deployed: VM Compute Instance, Cloud Storage, Networks and Subnetworks, IAM roles

## Deployment

**STEP 1**. Connect to GCP using `gcloud` or any other method of authentication.

**STEP 2**. Download the Terraform template [here](https://snowbit-integrations.s3.eu-west-1.amazonaws.com/cloud-security/modules/gcp-latest.tgz).

**STEP 3**. Once the files are extracted, from the folder run `terraform init`.

**STEP 4**. Fill in the information in the `values.auto.tfvars` file:

- The STA must connect to the internet. When selecting `STA-Public-Access` to `false`, make sure the STA is located on a subnetwork that has an NAT gateway attached.

- `STA-Config-Cloud-Storage-Bucket` - Optional to manage independently, but mandatory to use. If not provided by the user, Terraform will create one for you.

- `STA-Subnetwork-Mgmt` and `STA-Subnetwork-VxLanSniffing` are optional. Terraform will create them when not provided.

- `STA-IP-CIDR-for-Mgmt-Nic` and `STA-IP-CIDR-for-VxSniffing-Nic` - If `STA-Subnetwork-Mgmt` and `STA-Subnetwork-VxLanSniffing` are not provided. Select which CIDR range will be used in the newly created subnetwork. Note that '**STA-Subnetwork-Mgmt**' - defaults to '172.30.0.0/24’ and '**STA-Subnetwork-Mgmt**' - defaults to '172.30.1.0/24’

- `STA-Subnetwork-RawSniffing` is mandatory. Select the subnetwork that holds the instances you wish to mirror to the STA.

- `STA-Ingress-SSH-Address` - The IP address that will be allowed to manage (SSH) the STA

- SSH key management in the STA:
    - `GCP-block-project-ssh-keys` - Set to false to block SSH keys that are defined on the GCP project level.
    
    - `GCP-SSH-Key-Required` -Set to true\\false if SSH key is required or not.
        - When true:
            - `GPC-Existing-SSH-Public-Key-full-path` - when used, the key content will be read and used to manage the STA.
            
            - If the `GPC-Existing-SSH-Public-Key-full-path` variable is left empty, `GPC-New-SSH-Public-Key-name` will create a new SSH key to manage the STA on the stack directory.
            
            - When both variables are left empty - a new key will be created with the name `STA_GCP_key`

**STEP 5**. Run `terraform plan` and review the deployment.

**STEP 6**. Run `terraform apply` and type **yes**.

## Additional Resources

<table><tbody><tr><td>Documentation</td><td><a href="https://coralogixstg.wpengine.com/docs/cloud-security-quick-start/"><strong>Coralogix Security Traffic Analyzer</strong></a></td></tr></tbody></table>

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Contact us **via our in-app chat** or by emailing [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
