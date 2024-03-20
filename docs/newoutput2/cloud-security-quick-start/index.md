---
title: "Coralogix Cloud Security Quick Start"
date: "2022-01-06"
---

Coralogix Cloud Security helps you detect security threats across all of your network traffic with rapid setup and without the need for additional tooling. Once running, Cloud Security easily integrates all your security logs for a multidimensional view of your security posture and gives you the ability to perform deep and wide forensic investigations.

Cloud Security runs on your AWS account to provide real-time monitoring and analysis of your infrastructure.

### **With Coralogix STA you can do the following**

- Detect system intrusions

- Monitor your entire enterprise for unauthorized changes

- Centrally manage and analyze all security-related logs

## **Known Issues / Limitations**

1. The Cloud Security instance MUST be installed in a VPC that has access to the Internet in order to send the logs to Coralogix

3. The Cloud Security instance hasn’t been tested in private VPCs

5. The Cloud Security can be installed only in the following regions:
    - eu-west-1 (Ireland)
    
    - ap-south-1 (Mumbai)
    
    - us-east-1 (N. Virginia)
    
    - us-east-2 (Ohio)
    
    - us-west-1 (N. California)
    
    - us-west-2 (Oregon)

7. Currently, the Cloud Security solution doesn’t offer access to the actual packets that were captured

9. No alerts are created in Coralogix during the installation. Security alerts must be created manually

11. Mirroring or autoscaling instances are not supported

13. Mirroring of non-Nitro-based instances are not supported

15. Currently, you need to create individual Mirror Sessions for each Network Interface that you want to mirror

## Next Steps

The STA has several key concepts that you should understand to make the most of it.

Of-course you can jump straight to the Installation part, use the default configurations and later on do later on the desired drill down.

See the following links to navigate through the STA’s documentation.

[How to install Coralogix STA](https://coralogixstg.wpengine.com/docs/how-to-install-coralogix-sta/)
