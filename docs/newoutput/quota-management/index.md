---
title: "Quota Management"
date: "2021-06-29"
---

Manage team quotas in your Coralogix account using the following tutorial.

## Overview

The quota management API allows team administrators to **view** and **distribute** quotas across teams they manage. Allocate and move quotas across teams for any period to:

- Contend with unexpected spikes in logs shipping to one team

- Better handle resources when a team is underutilizing its quota

- Meet new or changing capacity requirements of your teams 

- [Save on cost](https://coralogixstg.wpengine.com/pricing/) by more efficiently using quotas

## Prerequisites

- The latest version of the [Coralogix CLI](https://coralogixstg.wpengine.com/tutorials/coralogix-cli/)

- Teams API key (Access this in **Account settings** > API Access)

- Team ID (Access this in **Account settings** > Send Your Logs)

- [Team administrator (admin) role](https://coralogixstg.wpengine.com/docs/user-team-management/) granted to the person moving quotas between teams (He / she must be team admin in both the team receiving and giving the quota)

## Limitations & Constraints

- The minimum quota that can be transferred is 0.001 GB.

- A quota cannot be moved to a [trial account](https://signup.coralogixstg.wpengine.com/#/).

- Quotas can only be moved between teams with the **same retention** period.

- Quotas cannot be transferred **from** a team if the remaining daily quota is **less** **than** the quota being moved. In this case, transfer will be possible once the daily quota resets at 00:00 UTC

- You need to be admin on the teams you are trying to move quota between.

- Quota allocations will take a maximum of 15 minutes to take effect.

## Environment Variables

The following table displays environment variables supported by commands.

\[table id=72 /\]

## Get Quota

### Command

This command returns the daily quota (GB) for the teamId being queried:

`cxctl account get-quota`

### Example

\[table id=70 /\]

**Options**

\[table id=68 /\]

\*Use eu if account top level domain is .com, in, if it is .in, and us if it is .us.

## Move Quota

### Command

This command moves quota (GB) from one team to another:

`cxctl account move-quota`

You may use the [Alerts, Rules & Tags API key](https://coralogix.com/docs/alerts-rules-tags-api-key/) for any of the teams between whom you are moving quotas.

### Example

\[table id=71 /\]

**Options**

\[table id=69 /\]

\*Use eu if account top level domain is .com, in, if it is .in, and us if it is .us.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
