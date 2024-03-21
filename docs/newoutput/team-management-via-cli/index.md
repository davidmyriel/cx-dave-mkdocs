---
title: "Team Management (via CLI)"
date: "2021-06-30"
---

The Coralogix CLI tool supports the management of teams. Actions supported on the CLI are **creating teams**, **inviting members** to teams (with role assignment). Users to be invited can be provided in one of the following formats:

- Comma-separated command-line arguments

- New-line delimited text file  

This capability allows team management to be automated using scripts or other provisioning tools. 

This tutorial will guide you on managing teams using the CLI tool.

## Getting Started

Only Organization and Platform administrators have the predefined permissions to manage teams.

**STEP 1**. Install the latest version of the [Coralogix CLI](https://coralogixstg.wpengine.com/tutorials/coralogix-cli/).

**STEP 2**. Access your **Teams API key** by navigating to **Data Flow** > **API Keys** from your Coralogix toolbar. Input this as your CORALOGIX\_API\_KEY environment variable.

**Note**: When the environment variable is set, _\--api-key_ (-k) becomes an optional argument when using the tool.

**STEP 3**. Access your Team ID. (Account -> Settings -> Send Your Logs)

## Commands

### **create-team**

This command creates a new team.

### **invite**

This command will invite users to join the specified team-id (optionally assign roles)

**Note:** A default role of **user** is assigned if none is specified 

## Examples

**Note:** The Examples below assume the api-key is provided as an environment variable.

\[table id=74 /\]

## Options

\[table id=75 /\]
