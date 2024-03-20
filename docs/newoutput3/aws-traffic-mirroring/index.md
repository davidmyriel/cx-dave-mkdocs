---
title: "AWS Traffic Mirroring Strategies"
date: "2021-12-30"
---

So, you’ve installed Coralogix’s STA and you would like to start analyzing your traffic and getting valuable insights but you’re not sure that you’re mirroring enough traffic or wondering if you might be mirroring too much data and could be getting more for less.

In order to be able to detect everything, you have to capture everything and in order to be able to investigate security issues thoroughly, you need to capture every network packet. 

More often than not, the data once labeled irrelevant and thrown away is found to be the missing piece in the puzzle when slicing and dicing the logs in an attempt to find a malicious attacker or the source of an information leak.

However, as ideal as this might be, in reality, capturing every packet from every workstation and every server in every branch office is usually impractical and too expensive, especially for larger organizations. Just like in any other field of security, there is no real right or wrong here, it’s more a matter of whether or not it is worth the trade-off in particular cases.

There are several strategies that can be taken to minimize the overall cost of the AWS traffic monitoring solution and still get acceptable results. Here are some of the most commonly used strategies:

## 1\. Mirror by Resource/Information Importance

1. **Guidelines:** After mapping out the most critical assets for the organization from a business perspective, configure the mirroring to mirror only traffic to and from the most critical servers and services to be mirrored and analysed. For example, a bank will probably include all SWIFT related servers, for a software company it will probably include all traffic from and to their code repository, release location, etc.

3. **Rationale:** The rationale behind this strategy is that mirroring the most critical infrastructures will still provide the ability to detect and investigate security issues that can harm the organization the most and will save money by not mirroring the entire infrastructure.

5. **Pros:** By following this strategy, you will improve the visibility around the organization’s critical assets and should be able to detect issues related to your organization’s “crown jewels” (if alerts are properly set) and to investigate such issues.

7. **Cons:** Since this strategy won’t mirror the traffic from non-crown jewels environments, you will probably fail to pinpoint the exact (or even approximate) path the attacker took in order to attack the organization’s “crown jewels”.

9. **Tips:** If your organization uses a jump-box to connect to the crown jewels servers and environments, either configure the logs of that jump-box server to be as verbose as possible and store them on Coralogix with a long retention or mirror the traffic to the jumpbox server.

## 2\. Mirror by Resource/Information Risk

1. **Guidelines:** After mapping out all the paths and services through which the most critical data of the organization is being transferred or manipulated, configure the mirroring to mirror only traffic to and from those services and routes. The main difference between this strategy and the one mentioned above is that it is focused on sensitive data rather than critical services as defined by the organization.

3. **Rationale:** The rationale behind this strategy is that mirroring all the servers and services that may handle critical information will still provide the ability to detect and investigate security issues that can harm the organization the most and will save money by not mirroring the entire infrastructure.

5. **Pros:** You will improve the visibility around the critical data across services and environments and you should be able to detect, by configuring the relevant alerts, attempts to modify or otherwise interfere with handling and transferring the organization’s sensitive data

7. **Cons:** Since this strategy won’t mirror traffic from endpoints connecting to the services and paths used for transmission and manipulation of sensitive data, it might be difficult or even impossible to detect the identity of the attacker and the exact or even approximate path taken by the attacker.

9. **Tips:** Collecting logs from firewalls and WAFs that control the connections from and to the Internet and sending the logs to Coralogix can help a great deal in creating valuable alerts and by correlating them with the logs from the STA can help identify the attacker (to some extent) and his/her chosen MO (Modus Operandi).

## 3\. Mirror by Junction Points

1. **Guidelines:** Mirror the data that passes through the critical “junction points” such as WAFs, NLBs or services that most of the communication to the organization and its services goes through.

3. **Rationale:** The idea behind this strategy is that in many organizations there are several “junction points” such as WAFs, NLBs, or services that most of the communication to the organization and its services goes through. Mirroring this traffic can cover large areas of the organization’s infrastructure by mirroring just a handful of ENIs.

5. **Pros:** You will save money on mirroring sessions and avoid mirroring some of the data while still keeping a lot of the relevant information.

7. **Cons:** Since some of the data (e.g. lateral connections between servers and services in the infrastructure) doesn’t necessarily traverse the mirrored junction points, it won’t be mirrored which will make it harder and sometimes even impossible to get enough information on the attack or even to be able to accurately detect it.

9. **Tips:** Currently, AWS cannot mirror an NLB directly but it is possible and easy to mirror the server(s) that are configured as target(s) for that NLB. Also, you can increase the logs’ verbosity on the non-monitored environments and services and forward them to Coralogix to compensate for the loss in traffic information.

## 4\. Mirror by Most Common Access Paths

1. **Guidelines:** Mirror traffic from every server is based on the expected and allowed set of network protocols that are most likely to be used to access it.

3. **Rationale:** The idea behind this strategy is that servers that expose a certain service are more likely to be attacked via that same service. For example, an HTTP/S server is more likely to be attacked via HTTP/S than via other ports (at least at the beginning of the attack). Therefore, it makes some sense to mirror the traffic from each server based on the expected traffic to it.

5. **Pros:** You will be able to save money by mirroring just part of the traffic that arrived or was sent from the organization’s servers. You will be able to detect, by configuring the relevant alerts, some of the indications of an attack on your servers.

7. **Cons:** Since you mirror only the expected traffic ports, you won’t see _unexpected_ traffic that is being sent or received to/from the server which can be of great value for a forensic investigation.

9. **Tips:** Depending on your exact infrastructure and the systems and services in use, it might be possible to cover some of the missing information by increasing the services' log verbosity and forwarding them to Coralogix.

## 5\. Mirror Some of Each

1. **Guidelines:** Randomly select a few instances of each role, region or subnet and mirror their traffic to the STA.

3. **Rationale:** The idea behind this strategy is that it would be reasonable to assume that the attacker would not know which instances are mirrored and which are not, and also, many tools that are used by hackers are generic and will try to propagate through the network without checking if the instance is mirrored or not, therefore, if the attacker tries to move laterally in the network (manually or automatically), or to scan for vulnerable servers and services, it is very likely that the attacker will hit at least one of the mirrored instances (depending on the percentage of instances you have selected in each network region) and if alerts were properly configured, it will raise an alert.

5. **Pros:** A high likelihood of detecting security issues throughout your infrastructure, especially the more generic types of malware and malicious activities.

7. **Cons:** Since this strategy will only increase the chances of detecting an issue, it is still possible that you will “run out of luck” and the attacker will penetrate the machines that were not mirrored. Also, when it comes to investigations it might be very difficult or even impossible to create a complete “story” based on the partial data that will be gathered.

9. **Tips:** Since this strategy is based on a random selection of instances, increasing the operating system and auditing logs as well as other services logs and forwarding them to Coralogix for monitoring and analysis can sometimes help in completing the picture in such cases.

In addition to every strategy, you’ll choose or develop, we would also recommend that you mirror the following. These will probably cost you near nothing but can be of great value when you’ll need to investigate an issue or detect security issues (manually and automatically):

1. **All DNS traffic** - It is usually the tiniest traffic in terms of bytes/sec and packets/sec but can compensate for most black spots that will result in such trade-offs.

3. **Mirror traffic that should never happen** - Suppose you have a publicly accessible HTTP server that is populated with new content only by scp from another server. In this case, mirroring should be done on the FTP access to the server, since that FTP is one of the most common methods to push new content to HTTP servers, mirroring FTP traffic to this server and defining an alert on such an issue will reveal attempts to replace the HTTP contents even before they have succeeded. This is just one example, there are many possible examples (ssh or nfs to Windows servers, RDP, SMB, NetBIOS and LDAP connections to Linux servers) you probably can come up with more based on your particular environment. The idea here is that since an attacker doesn’t have any knowledge of the organization’s infrastructure, the attacker will have to first scan hosts to see which operating systems are running and which services they are hosting, for example by trying to connect via SMB (a protocol mostly used by Windows computers) and if there is a response, the attacker would assume that it is Windows. Of course, the same applies to Linux.

## Cloud Access

In cloud infrastructures, instances and even the mirroring configuration are accessible via the Internet and therefore theoretically allows an attacker to find out whether an instance is mirrored and to act accordingly. Because of this, it is even more important to make sure that access to the cloud management console is properly secured and monitored.
