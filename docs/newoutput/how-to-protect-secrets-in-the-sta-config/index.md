---
title: "How to protect secrets in the STA config?"
date: "2022-03-27"
---

Several STA customers have asked us to provide a mechanism for securely storing secrets in the STA config. Some of them said that they would like to prevent users that use the STA from seeing the Coralogix [](https://coralogixstg.wpengine.com/docs/private-key/)[Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) for example. Other customers were wondering whether it would be possible to share several configuration values between multiple STA installations without having to share all of the configuration or manually synchronize the configuration between the instances. The following steps will help you address both concerns.

The STA supports the following two configuration storing methods:

1. Locally stored configuration - The configuration is stored and managed locally on the STA instance. This is the default method which is recommended for small POCs or test environments.

3. Storing configuration on an S3 bucket - The configuration is stored on an S3 bucket and periodically synchronized to the STA. This is the recommended method for production deployments.

In both methods the STA now supports the following mechanisms for mechanisms for storing configuration values securely:

1. As a reference to an AWS Secrets Manager secret - That allows you to store the secret value as an AWS secret, add a permission to the STA's role to access the new secret.

3. As a value encrypted by the STA - That allows you to encrypt the secret value by using STA internal mechanisms which use AES256 and a unique key to encrypt the value.

You can now mix and match these as needed. For example, you can store the STA configuration on an S3 bucket and have some of the values stored as secrets in AWS Secrets Manager and encrypt (some or all of them) by using the STA's encryption. Here is how to do it:

## Storing the configuration on S3

It is possible to provide an S3 bucket name at the CloudFormation/Terraform template, that way, the STA will start by copying the configuration to that bucket (if it is empty) or downloading the configuration from it and applying it automatically.

If you want to configure an existing STA that uses local configuration (the default) to use an S3 bucket to hold its configuration, here are the steps you need to take:

1. Login to the STA by using SSH

3. Run the command `sta-edit-config`

5. If this is the first time you are doing this, you'll probably be asked to choose a configuration editor. Choose one from the list presented by typing its corresponding number

7. Find the field "sync\_config\_from" and set its value to the bucket name with `s3://` as prefix. For example: `"sync_config_from": "s3://my-sta-config-bucket"`,

9. Exit the editor while saving your changes

11. You'll be presented with the following question. Just answer "y": `NOTE: The new config file includes a configuration to use an S3 bucket. If you'll choose to continue, any additional configuration from now on will have to be done using the configured bucket. Are you sure you want to continue?`

13. If all goes well, the following line should be prompted to indicate that the new configuration has been applied: `Configuration updated successfully.`

## Storing configuration value as an AWS secrets

To strengthen the security of the STA, it is now possible to configure the STA to store any of the values for the various configuration settings as secrets stored in AWS Secrets Manager. Here are the steps you need to take to get it done:

1. Install an STA as you normally would and verify it is working correctly

3. Login to the STA by using SSH

5. Run the command `sta-get-status-short` and verify that all services mentioned in the output are in any of the following statuses only: "OK", "NOT\_RUNNING\_NOW" or "RUNNING\_NOW"

7. Login to your AWS console and go to the AWS Secrets Manager

9. Create a secret for storing the configuration value or values that you want to store as a secret. The secret can either be a plaintext value that contains a single configuration value you want to store as a secret or a JSON key/value dictionary that contains multiple configuration values you want to store as a secret.

11. Modify the IAM role attached to the STA to allow access to this secret's ARN by either attaching an existing policy or creating an inline policy

13. Login to the STA by using SSH

15. Run the command `sta-edit-config`

17. Set the value for the configuration key you want to a value that matches the following pattern: `${{<aws_secret_region_name>;<aws_secret_arn>[;<secret_keyvalue_keyname>]}}`  
    For example, here is a value for a key-value type of secret that has a key named "private\_key" that we would like to use:  
    `${{eu-west-1;arn:aws:secretsmanager:eu-west-1:746543792062:secret:test-sta-od-encrypt-secrets-RfAkDs;private_key}}`  
    Another example, is a value for a plain-text type of secret that is expected to contain the entire configuration value:  
    ${{eu-west-1;arn:aws:secretsmanager:eu-west-1:746543792062:secret:test-sta-od-encrypt-secrets-RfAkDs}}

## Storing configuration values as encrypted values

To strengthen the security of the STA even more, it is now possible to configure the STA to store any of the values for the various configuration settings as encrypted values that can only be decrypted by the root user on the STA. It is possible to store these encrypted values anywhere one can store STA configuration: local, S3 and AWS Secrets Manager. Currently encrypted values are only supported on "on-demand" and on-prem STA installations. This is something that we plan to fix on future STA versions. Here are the steps you need to take to get it done:

1. Install an STA as you normally would and verify it is working correctly

3. Login to the STA by using SSH

5. Run the command `sta-get-status-short` and verify that all services mentioned in the output are in any of the following statuses only: "OK", "NOT\_RUNNING\_NOW" or "RUNNING\_NOW"

7. Run the command `sta-encrypt-config-value` and enter the value you want to encrypt and then hit Ctrl+D (If you need to abort just click Ctrl+C)

9. The result should be something similar to this: `${{b64:enc:ASSDDSFsdfdsfSDFDSDSFDSFsdfdsf12322DFFD==}}`

11. Copy this result and paste it as the value of an AWS secret in AWS Secrets Manager or as a value of the relevant configuration setting in the sta.conf that is stored on the S3 bucket or as a value of the relevant local configuration setting directly by using the command `sta-edit-config`

13. If you changed the value of a secret or the config that is stored on the S3 bucket it may take up to 3-4 minutes for it to be automatically applied.

This feature, in addition to being a security related feature can also be used to simplify the configuration of multiple STA instances. You can create an AWS secret for several configuration values and then use the same secret reference in multiple STA instances and set the permissions to the STA instances correctly. That way you are essentially sharing configuration values between multiple STA instances while storing them only once.

We hope you found this guide helpful. If you have any further questions, don't hesitate to contact us via the chat.

## **Support**

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at **[support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com)**.
