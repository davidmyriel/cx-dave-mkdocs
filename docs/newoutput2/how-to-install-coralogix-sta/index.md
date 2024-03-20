---
title: "How to install Coralogix STA"
date: "2022-01-04"
---

The Coralogix STA (Security Traffic Analyzer) is a tool by Coralogix for deep packet inspection, packet capturing, cloud configuration vulnerability scanning, and more.

For additional information see the [introduction doc](https://coralogixstg.wpengine.com/docs/cloud-security-quick-start/).

### The STA can be installed using the following methods

1. CloudFormation Template
    - As this an AWS service, this installation only for AWS

3. Terraform Template
    - Available for AWS and Azure

5. OVA image

In addition, STA can be installed in a limited internet access environment.

## Pre-requisites

Before you install the STA please make sure the following requests are met:

### AWS

1. Configuration is saved using an AWS S3 bucket. It is recommended to use a dedicated bucket, if you won’t define one, the STA will generate a bucket.
    - Have an **empty** S3 bucket for holding the configuration.

3. You have permissions to deploy EC2 instances, spot fleets, load-balancers and security groups in the AWS account you plan to deploy the STA in.

5. Instances that you plan to mirror their traffic by using the VPC traffic mirroring feature belong to one of the following types:
    - `C4, D2, G3, G3s, H1, I3, M4, P2, P3, R4, X1, X1e, A1, C5, C5d, C5n, I3en, M5, M5a, M5ad, M5d, p3dn.24xlarge, R5, R5a, R5ad, R5d, T3, T3a, and z1d.`

7. If you are looking to monitor instances by using our Virtual Tap, make sure you can run privileged containers in that environment (for example in AWS FarGate you cannot do that) - to read more about the Virtual Tap, see [this doc](https://coralogixstg.wpengine.com/docs/coralogix-sta-virtual-tap/).

### Azure

1. Configuration is saved using an Azure’s Storage Accounts service. It is recommended to use a dedicated container, so the configuration will be saved outside of the STA and can be modified/restored later

3. Have an **empty** container for holding the configuration

5. You have permissions to deploy VM instances, and create resources, in the Azure’s account you plan to deploy the STA in

7. mirroring instances’ traffic can be performed only by using our Virtual Tap. To read more about the Virtual Tap, see [this doc](https://coralogixstg.wpengine.com/docs/coralogix-sta-virtual-tap/)

## Deployment

### CloudFormation Template

1. Connect to your AWS account and on another tab, login to your Coralogix account

3. From Coralogix UI, go to the Settings page and then to the Cloud Security tab

5. Click "Deploy Security Service"

7. From the top drop-down list named "Deployment method", choose the option "CloudFormation" (should already be selected)

9. Fill in the various fields on the form and click "Launch AWS CloudFormation":
    1. Set the CloudFormation's stack name (The default is "CoralogixSecurity")
    
    3. Optionally, fill in the name of an S3 bucket that will be used for storing the STA's configuration
    
    5. Optionally, configure the STA to use an encrypted disk
    
    7. Select the SSH key pair that will be used to connect to the STA
    
    9. Select the security group that will be assigned to the management network interface
    
    11. Optionally, fill in the name of an S3 bucket that will be used for storing the packets captured by the STA as compressed PCAP files
    
    13. If you chose to run the STA as a spot, you can set the maximum spot price here
    
    15. Select the subnet you'd like to run the STA in. Make sure that the security group you chose for the management interface belongs to this subnet. Otherwise the installation will fail
    
    17. Select the VPC you'd like to run the STA in. Make sure that the subnet you selected belongs to this VPC
    
    19. Tick the box below that says "I acknowledge that AWS CloudFormation might create IAM resources." and click "Create stack"

### Terraform Template

#### AWS - Prerequisites

1. Connect to your AWS account and on another tab, login to your Coralogix account

3. From Coralogix UI, go to the Settings page and to the Cloud Security tab

5. Click "Deploy Security Service"

7. From the top drop-down list named "Deployment method", choose the option "Terraform Template"

9. Click "Launch tutorial"

#### Deployment Steps

1. Create an empty folder somewhere on your computer

3. Download the terraform module files
    - AWS - download content from the [following link](https://snowbit-integrations.s3.amazonaws.com/cloud-security/modules/aws-latest.tgz)
    
    - Azure - download content from the [following link](https://github.com/coralogix/snowbit-modules/blob/master/STA/azure/main.tf)
    
    - For older versions, [follow this link](https://snowbit-integrations.s3.eu-west-1.amazonaws.com/cloud-security/html-module-share/list.html) and download the desired version

5. Create file `values.auto.tfvars`
    - Fill in the required variables in the values file
    
    - for the explanation of variable types and expected content see comments from downloaded content

7. Run the command `terraform init` from the same folder

9. Run the command `terraform plan` and examine the changes that are going to be applied to your environment

11. Run the command `terraform apply` from the same folder and approve the changes

### OVA File

1. You can download the OVA file from the following links based on the environment you would like to use them at:
    1. VirtualBox: [https://coralogix-integrations.s3-eu-west-1.amazonaws.com/cloud-security/sta-ng.virtualbox.ova](https://coralogix-integrations.s3-eu-west-1.amazonaws.com/cloud-security/sta-ng.virtualbox.ova)
    
    3. VMware ESXi: [https://coralogix-integrations.s3-eu-west-1.amazonaws.com/cloud-security/sta-ng.vmware.ova](https://coralogix-integrations.s3-eu-west-1.amazonaws.com/cloud-security/sta-ng.vmware.ova)

3. Once the file is downloaded, import the VM into the relevant environment and start it

5. After the VM has finished loading, login to the VM with the user 'ubuntu' and the password 'Coralogix-STA!'

7. Automatically, once the user is logged on, a series of questions will be presented. Please answer all of them with all the relevant information

9. Run the command `passwd` and change the default password of the ubuntu user

## STA Deployment In Limited Internet Access Environments

STA requires access to S3 for its config files. In some environments Internet outbound access is required to be limited to specific IPs, which means no access to public S3 will be available. In order to allow connectivity using amazon private network - Set a designated [VPC gateway endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-s3.html) that connects your VPC directly to Amazon S3.

\* Make sure your VPC's route table contains [Coralogix’s endpoints](https://coralogixstg.wpengine.com/docs/how-to-troubleshoot-your-log-shipment/#connectivity-issues).

\* In addition, in such environments the following enrichment services will not work: `dns-rbls, unshorten-url, nist-cpe`, also updates to `Suricata` service will fail.

## Next Steps

After installing the STA, you can move forward in one of the following ways (or all of them) to get the most out of your newly installed STA:

1. Configure VPC traffic mirroring to allow the STA to analyze raw traffic. For this use the following tutorials: [How to automate VPC Mirroring for Coralogix STA](https://coralogixstg.wpengine.com/blog/how-to-automate-vpc-mirroring-for-coralogix-sta/), [Guide: Smarter AWS Traffic Mirroring for Stronger Cloud Security](https://coralogixstg.wpengine.com/blog/aws-traffic-mirroring-and-better-cloud-security/)

3. Deploy Wazuh agents in selected instances to get insights into the processes running inside them. For this use the following tutorial: [How to connect a Wazuh agent to the STA](https://coralogixstg.wpengine.com/tutorials/how-to-connect-a-wazuh-agent-to-the-sta/)

5. Review alerts configured and modify them to be more accurate for your organization. You can find more about it in these tutorials: [Security Traffic Analyzer (STA) Alerts](https://coralogixstg.wpengine.com/tutorials/the-default-set-of-alerts-in-the-coralogix-security-traffic-analyzer-sta/), [Alerts API](https://coralogixstg.wpengine.com/tutorials/alerts-api/)

7. Run the command `sta-get-installation-id` and copy the uuid that is displayed on the screen and save it in a safe place. This key is required to login to the STA with administrative privileges which might be needed as part of a troubleshooting session.

9. Once the installation ID is safely stored and properly backed-up, run the command `sta-acknowledge-installation-id` and carefully follow the instructions on the screen to remove the installation ID from the STA

**If you have any questions or need any additional help, please contact our support team via our 24/7 in-app chat!**
