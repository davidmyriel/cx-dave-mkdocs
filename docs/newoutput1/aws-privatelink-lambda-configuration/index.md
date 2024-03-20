---
title: "AWS PrivateLink: Lambda Configuration"
date: "2023-11-28"
---

Once you have configured our [AWS PrivateLink](https://coralogixstg.wpengine.com/docs/coralogix-amazon-web-services-aws-privatelink-endpoints/), align the VPC to your Lambda.

## Lambda Configuration

### Permissions

Grant the Lambda the permissions necessary to add virtual interfaces, as follows:

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"ec2:DescribeNetworkInterfaces",
				"ec2:CreateNetworkInterface",
				"ec2:DeleteNetworkInterface"
			],
			"Resource": "*"
		}
	]
}

```

Full instructions can be found [here](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html#vpc-permissions).

### Align the VPC to the Lambda

**STEP 1**. Follow [these instructions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html#vpc-configuring) to align the VPC to the Lambda.

**STEP 2**. Update the `CORALOGIX_URL` environment variable to match the FQDN [endpoint](https://coralogixstg.wpengine.com/docs/coralogix-amazon-web-services-aws-privatelink-endpoints/) for your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/).

## AWS Secrets Manager

If you intend to use [AWS Secrets Manager](https://aws.amazon.com/blogs/security/how-to-connect-to-aws-secrets-manager-service-within-a-virtual-private-cloud/) with your Lambda, you must create another VPC endpoint for the `com.amazonaws.<AWS Region>.secretsmanager` service. Detailed instructions can be found [here](https://aws.amazon.com/blogs/security/how-to-connect-to-aws-secrets-manager-service-within-a-virtual-private-cloud/).

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
