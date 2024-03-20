---
title: "Data Enrichment in Coralogix STA"
date: "2021-12-30"
---

The raw packet data that is passed through Coralogix is enriched with the following fields, in addition to protocol specific fields, to facilitate the creation of effective and powerful alerting rules that will accurately detect security related threats:

**Connection state (security.connection\_state, security.connection\_state\_description)** - Allows you to filter, in alerts as well as in searches, only for successful or unsuccessful connections or connections at a certain stage. For example, by using this field it is possible to detect SYN flooding or SYN based reconnaissance.

**Whether the connection is established (security.established)** - Allows you to filter, in alerts as well as in searches, only for connections that have succeeded or failed. For example, you might want to measure, in a dashboard, the ratio between established and not established connections to your front end servers to detect connectivity issues to them that may result from or be an indication of DDoS attack that is in progress.

**Connection duration (security.duration)** - Indicates the connection’s duration. Each protocol has a typical connection length, for example HTTP connections are relatively short since it’s a connectionless protocol while SSH connections can be much longer.  
By identifying the correct threshold for your organization for common protocols you can detect attacks such as slow post attacks which are essentially non-volumetric denial of service attacks.

**Private/Public Source/Destination IP indications (security.local\_orig, security.local\_respond)** - Makes it easier to create rules that apply only to outbound or inbound connections.

Bytes transferred from each connected party (security.original\_ip\_bytes, security.respond\_ip\_bytes) - Makes it easier to detect large data leakage issues, unusual high number of stale connections and many other important issues.  
**Source & Destination geographic location details (security.geo. city\_name, security.\_geo.continent\_code, security.\_geo.country\_code2, security.\_geo.country\_code3, security.\_geo.country\_name, security.\_geo.location)** - Allows you to create whitelist or blacklist based alerts that will fire when a connection from/to an unexpected geographic location is detected.

**NLP based score for domain names (security.frequency\_scores,  
security.highest\_registered\_domain\_frequency\_score)** - Many types of attacks nowadays rely on a technique known as Domain Generation Algorithm, Coralogix automatically calculates a score for each domain, domain part certificate names and much more. This score is based on the frequency of letter combinations in the text and the expected letter combinations in English. By using this score, in alert rules it would be possible to detect DGA usage.

**Domain creation date (security.creation\_date)** - Coralogix will enrich domain names with the date at which they were created. This is since that younger domains are often involved in malicious activity such as DGAs and phishing attempts. By using this field you can easily create an alert that alerts you if a computer is trying to access young  
domains.

**Domain parts (security.highest\_registered\_domain, security.parent\_domain, security.subdomain, security.tld.subdomain, security.top\_level\_domain)** - Allows you to filter for specific types of domains without having to rely on complex regular expressions.

**Domain parts lengths (security.parent\_domain\_length, security.subdomain\_length)** - Since DGA tends to be quite long, the length of parts of the domain can be used to create alerts that will help in the detection of such cases.

**Source & Destination ASN** (Autonomous System Number) details - allows you to detect any suspicious activity originating from certain IP addresses associated with malicious intent and create alerts based on it.

```
{
    "security": {
		
		...

        "source_ip_as_number": {
            "as_number": 15169,
            "as_number_country_code": "US",
            "as_number_description": "GOOGLE"
        }

		...

        "destination_ip_as_number": {
            "as_number": 396982,
            "as_number_country_code": "US",
            "as_number_description": "GOOGLE-CLOUD-PLATFORM"
        }

		...

	}
}
```
