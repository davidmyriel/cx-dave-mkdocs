---
title: "Role-Based Access Control: Logs"
date: "2023-06-26"
---

As part of our [team and user management](https://coralogixstg.wpengine.com/docs/user-team-management/) options, **role-based access control (RBAC)** allows account administrators to grant some or all team members specific [application and subsystem](https://coralogixstg.wpengine.com/docs/application-and-subsystem-names/) data permissions and action permissions. You can also assign team users into multiple groups with different action permissions to any subset of users.

User roles are determined when a user is initially invited to a Team on the [Invites page](https://coralogixstg.wpengine.com/tutorials/user-team-management/). You may change users' roles by either assigning them to a different group from the **Team Members** page.

![](images/RBAC-1.png)

Or add the user to the group by choosing him or her in the **Members** option. You can also change permissions to an entire group by changing the role in the **Select Role** option.

![](images/RBAC-2.png)

In **Manage application scope,** you may choose specific applications that will be visible to the group across Coralogix. For example, in the AWS people group, choosing the application "AWS" will give access to aws-related information only to the users who are members of the AWS people group (access to view aws logs in the Logs view, aws logs in the LiveTail view, aws-related alerts, AWS traces etc.). You can also choose a different **Filter type** to include several applications which start, end or include a search term.

![](images/RBAC-3.png)

The same goes for **Manage subsystem scope**.

Note: If Application and Subsystem are not specified then users have access to all applications and subsystems.

## Support

**Need help?**

Our world-class customer success team is available 24/7 to walk you through your setup and answer any questions that may come up.

Feel free to reach out to us **via our in-app chat** or by sending us an email at [support@coralogixstg.wpengine.com](mailto:support@coralogixstg.wpengine.com).
