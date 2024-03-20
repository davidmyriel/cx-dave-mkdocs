---
title: "SAML Management (via CLI)"
date: "2021-06-30"
---

The Coralogix CLI tool allows management of [SAML SSO](https://coralogixstg.wpengine.com/tutorials/sso-with-saml/) configuration by admin users. Actions supported on the CLI include **viewing**, **initializing**, **activating,** and **deactivating** SAML configuration. This capability makes it possible for SAML integration to be automated using scripts or other provisioning tools.

This tutorial will guide you on how to manage the SAML integration using the CLI tool.

**Notes**:

- If you intend to follow this integration with our [SCIM integration](https://coralogixstg.wpengine.com/docs/scim/), delete any existing users before the SCIM integration is applied. If necessary, leave one admin user.

- Upon completion of the SCIM integration, recreate all users through SCIM.

## Getting started:

1. Install the latest version of the [Coralogix CLI](https://coralogixstg.wpengine.com/tutorials/coralogix-cli/)

3. Teams API key (Fetch this from Account -> Settings -> API access)

5. User must have an admin role.

### Environment variables:

\[table id=76 /\]**  
Note:** When the environment variable is set --api-key (-k) becomes an optional argument when using the tool.

## Commands:

### **details**

This command displays SAML configuration for:: your Team.

### **activate**

This command will activate SAML on your Team.

### **deactivate**

This command will deactivate SAML on your Team.

### **init**

This command will initialize SAML on Coralogix with metadata file from the IdP.

**Note:** Initializing SAML does not activate it. For SSO authentication to work, SAML needs to be activated (using activate command).

### **add-entity-id** 

This command adds your team-id to the SP Entity URL. 

This will help uniquely identify the Coralogix SP on the IdP (required when you are configuring SAML for multiple teams with the same Identity Provider).

### **remove-entity-id**

This command removes team-id from the SP Entity URL

### **generate-provisioning-token**

This command generates the provisioning token

### **remove-provisioning-token**

This command removes the provisioning token

## **Examples:**

**Note:** Examples below assume the api-key is provided as an environment variable.

\[table id=77 /\]

**Options**

\[table id=78 /\]
