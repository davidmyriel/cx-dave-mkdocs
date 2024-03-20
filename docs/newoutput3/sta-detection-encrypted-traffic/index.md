---
title: "STA Detection for Encrypted Traffic"
date: "2021-12-30"
---

Today, one of the biggest challenges a Cyber security professional have to deal with is that, according to [Fortinet](https://www.fortinet.com/blog/industry-trends/keeping-up-with-performance-demands-of-encrypted-web-traffic#:~:text=Not%20only%20is%20global%20consumer,55%25%20in%20Q3%20of%202017.), for example, the total percentage of encrypted web traffic is now around 85%. More websites are encrypting their traffic by using the TLS protocol, servers are no longer managed in clear text telnet sessions but rather in secure SSH sessions, even emails are now being transmitted in SMTPS sessions. This trend is obviously good for keeping our private information private but it also allows attackers to hide their tracks and makes it harder for blue teams to detect threats in the traffic.

You can use many different services for handling SSL termination and then send decrypted traffic to Coralogix for analysis or even configure the Coralogix STA to decrypt the data by using your [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/). But even if you don’t, the Coralogix STA can detect lots of things about encrypted traffic without having to decrypt it first.

The most basic thing is, of course, the connection itself. There are servers and services that do not need any Internet access, for those, you can create a simple alert rule that will fire whenever any of them is trying to access the Internet.

Also, since most of the encrypted communications will first query the DNS for the correct IP address, forwarding the DNS traffic to Coralogix can greatly improve its effectiveness by creating dashboards and alerts based on the DNS traffic as well.

The following is a partial list of what Coralogix can detect in encrypted traffic.

| Protocol | Detectable from Encrypted Traffic |
| --- | --- |
| **DNS/TLS/SSL** | **NLP based score** - For every domain queried or found in a certificate, the Coralogix STA will calculate a score based on NLP to help determine if the domain was machine generated. |
|  | **Domain creation date** - For every domain queried or found in a certificate, the Coralogix STA will attempt to retrieve its creation date. Domains that are very young are often associated with attack campaigns |
|  | **New public DNS server used** - Normally, instances in the LAN should use a local DNS server unless they are a DNS server themselves. Based on that information it is possible to detect direct access from LAN instances to unauthorized DNS servers |
|  | **Non-DNS traffic on DNS ports** - A Suricata signature can detect cases in which non DNS traffic was sent over DNS port under the assumption that DNS ports are open in most organizations. |
| **TLS/SSL** (HTTPS,  
FTPS,  
SMTPS…) | **Trend & Rate** – The amount of connections and bytes that were transferred  
over time. This can be used to detect anomalies |
|  | **TLS/SSL Version** – Access to public services that were expected to be secure  
and are detected as using an old version of TLS can indicate that they were  
compromised or that an attacker is trying to cause the server and client to  
use a weaker type of encryption to allow him/her to eavesdrop the traffic. An  
attack known as SSL Stripping. |
|  | **Source & Destination Countries** – Based on IP to GeoLocation enrichment we  
can detect the geographic location of the source and destination servers |
|  | **Certificate Details** – Since the certificate that is being used by the server is  
not encrypted, the STA will analyze it and provide details such as Common  
Name, Issuer Common Name, Issuer Country, Server name, TLS/SSL version,  
certificate validation status, Certificate chain length, Certificate cipher details |
|  | **Client Fingerprinting** - We use JA3/JA3s algorithms to generate fingerprints that can accurately detect the software used to access or respond to SSL/TLS requests. |
| **SSH** | **Trend & Rate** – The amount of connections and bytes that were transferred  
over time. This can be used to detect anomalies |
|  | **Server and client software types** – In the case of outbound SSH connections  
from your organization to the internet, an attempt that should be considered  
suspicious, the client software can provide an indication of the malware and  
even of the attacking group. In the case of connections to your server, the  
client software type can indicate whether it’s a legitimate client or not. |
|  | **Source & Destination Countries** – Based on IP to GeoLocation enrichment we  
can detect the geographic location of the source and destination servers |
|  | **Cipher Algorithm** – The cipher algorithm used in this SSH session. This can  
indicate that one of your servers is using an unauthorized or non standard  
cipher and by that becoming vulnerable |
|  | **Number of observed authentication attempts** – The number of authentication  
attempts that were observed by the server. Not all of them necessarily indicate  
failures but if you see a large number here that probably means that someone  
is trying to guess your SSH password |
|  | **Client Fingerprinting** - We use HASSH algorithms to generate fingerprints that can accurately detect the software used to access or respond to SSH |
