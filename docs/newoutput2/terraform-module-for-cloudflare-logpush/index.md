---
title: "Cloudflare Logpush Terraform Module"
date: "2022-07-14"
---

Terraform simplifies the way we deploy our infrastructure and allows us to maintain it as code.

Using our Terraform Modules, you can easily install and manage Cloudflare logpush integrations to Coralogix as modules in your infrastructure code.

Our modules are open source and available on our [Github](https://github.com/coralogix/terraform-coralogix-cloudflare) and in the [Terraform Registry](https://registry.terraform.io/modules/coralogix/cloudflare/coralogix/latest).

## Installation

This module will be creating a Logpush job which will send logs to your Coralogix account.

Note: This module requires `Terraform`  Version 1.20+ 

To use the module, first, add the provider configure block to your Terraform project:

```vhdl
terraform {
  required_providers {
    cloudflare = {
      source  = "cloudflare/cloudflare"
      version = "~> 3.0"
    }
  }
}

provider "cloudflare" {
  email   = "example@coralogixstg.wpengine.com"
  api_key = "7ae12522bce3d8d988ec5f0ed8b8ef9016e09"
}
```

Important variables to change:

cloudflare.email - Your email used in cloudflare.

cloudflare.api\_key - Your API key for cloudflare.

Then add the module block:

```vhdl
module "logpush-job" {
    source = "coralogix/cloudflare/coralogix//modules/logpush-job"

    coralogix_region   = "Europe"
    coralogix_private_key = "79cf16dc-0dfa-430e-a651-ec76bfa96d01"
    cloudflare_logpush_dataset = "http_requests"
    cloudflare_logpush_fields = "RayID,ZoneName" # can be left empty aswell for all fields
    cloudflare_zone_id = "ca17eeeb371963f662965e4de0ed7403" # to be used with zone-scoped datasets
    # cloudflare_account_id = "bc20385621cb7dc622aeb4810ca235df" # to be used with account-scoped datasets
}
```

Important variables to change:

- coralogix\_region - Associated with your Coralogix [domain](https://coralogixstg.wpengine.com/docs/coralogix-domain/), possible options are \[Europe, Europe2, India, Singapore, US\]

- coralogix\_private\_key - The Coralogix [Send-Your-Data API key](https://coralogixstg.wpengine.com/docs/send-your-data-api-key/) which is used to validate your authenticity

- cloudflare\_logpush\_dataset - The cloudflare logpush job data-set

- cloudflare\_logpush\_fields - The logpush dataset specific fields to log delimited with comma, leave empty to include all fields. the timestamp and its variants are included automatically.

- cloudflare\_zone/account\_id - Your zone/account id, can be retrieved from cloudflare dashboard or API.

By default, the integration will set `application_name` as `Cloudflare`, and `subsystem_name` as the data set name. To overwrite these parameters, add the following:

- `header_CX-Application-Name` - application name override

- `header_CX-Subsystem-Name` - subsystem name override

We also have a [Coralogix Terraform Provider](https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/) to help manage your Coralogix resources such as rules and alerts. Install it [here](https://coralogixstg.wpengine.com/docs/coralogix-terraform-provider/).

If you have any questions, feel free to reach out to our team via our in-app chat!

Values Table:

<table><tbody><tr><td class="has-text-align-left" data-align="left">Dataset</td><td>Fields</td></tr><tr><td class="has-text-align-left" data-align="left">dns_logs</td><td>ColoCode, EDNSSubnet, EDNSSubnetLength, QueryName, QueryType, ResponseCached, ResponseCode, SourceIP</td></tr><tr><td class="has-text-align-left" data-align="left">firewall_events</td><td>Action, ClientASN, ClientASNDescription, ClientCountry, ClientIP, ClientIPClass, ClientRefererHost, ClientRefererPath, ClientRefererQuery, ClientRefererScheme, ClientRequestHost, ClientRequestMethod, ClientRequestPath, ClientRequestProtocol, ClientRequestQuery, ClientRequestScheme, ClientRequestUserAgent, EdgeColoCode, EdgeResponseStatus, Kind, MatchIndex, Metadata, OriginResponseStatus, OriginatorRayID, RayID, RuleID, Source</td></tr><tr><td class="has-text-align-left" data-align="left">http_requests</td><td>BotScoreCloudflare, BotScoreSrc, BotTags, CacheCacheStatus, CacheResponseBytes, CacheResponseStatus, CacheTieredFill, ClientASN, ClientCountry, ClientDeviceType, ClientIP, ClientIPClass, ClientMTLSAuthCertFingerprint, ClientMTLSAuthStatus, ClientRequestBytes, ClientRequestHost, ClientRequestMethod, ClientRequestPath, ClientRequestProtocol, ClientRequestReferer, ClientRequestScheme, ClientRequestSource, ClientRequestURI, ClientRequestUserAgent, ClientSSLCipher, ClientSSLProtocol, ClientSrcPort, ClientTCPRTTMs, ClientXRequestedWith, EdgeCFConnectingO2O, EdgeColoCode, EdgeColoID, EdgeEndTimestamp, EdgePathingOp, EdgePathingSrc, EdgePathingStatus, EdgeRateLimitAction, EdgeRateLimitID, EdgeRequestHost, EdgeResponseBodyBytes, EdgeResponseBytes, EdgeResponseCompressionRatio, EdgeResponseContentType, EdgeResponseStatus, EdgeServerIP, EdgeTimeToFirstByteMs, FirewallMatchesActions, FirewallMatchesRuleIDs, FirewallMatchesSources, JA3Hash, OriginDNSResponseTimeMs, OriginIP, OriginRequestHeaderSendDurationMs, OriginResponseBytes, OriginResponseDurationMs, OriginResponseHTTPExpires, OriginResponseHTTPLastModified, OriginResponseHeaderReceiveDurationMs, OriginResponseStatus, OriginResponseTime, OriginSSLProtocol, OriginTCPHandshakeDurationMs, OriginTLSHandshakeDurationMs, ParentRayID, RayID, RequestHeaders, ResponseHeaders, SecurityLevel, SmartRouteColoID, UpperTierColoID, WAFAction, WAFFlags, WAFMatchedVar, WAFProfile, WAFRuleID, WAFRuleMessage, WorkerCPUTime, WorkerStatus, WorkerSubrequest, WorkerSubrequestCount, ZoneID, ZoneName</td></tr><tr><td class="has-text-align-left" data-align="left">nel_reports</td><td>ClientIPASN, ClientIPASNDescription, ClientIPCountry, LastKnownGoodColoCode, Phase, Type</td></tr><tr><td class="has-text-align-left" data-align="left">spectrum_events</td><td>Application, ClientAsn, ClientBytes, ClientCountry, ClientIP, ClientMatchedIpFirewall, ClientPort, ClientProto, ClientTcpRtt, ClientTlsCipher, ClientTlsClientHelloServerName, ClientTlsProtocol, ClientTlsStatus, ColoCode, ConnectTimestamp, DisconnectTimestamp, Event, IpFirewall, OriginBytes, OriginIP, OriginPort, OriginProto, OriginTcpRtt, OriginTlsCipher, OriginTlsFingerprint, OriginTlsMode, OriginTlsProtocol, OriginTlsStatus, ProxyProtocol, Status</td></tr><tr><td class="has-text-align-left" data-align="left">audit_logs</td><td>ActionResult, ActionType, ActorEmail, ActorID, ActorIP, ActorType, ID, Interface, Metadata, NewValue, OldValue, OwnerID, ResourceID, ResourceType</td></tr><tr><td class="has-text-align-left" data-align="left">gateway_dns</td><td>ColoID, ColoName, DeviceID, DstIP, DstPort, Email, Location, MatchedCategoryIDs, Policy, PolicyID, Protocol, QueryCategoryIDs, QueryName, QueryNameReversed, QuerySize, QueryType, RData, ResolverDecision, SrcIP, SrcPort, UserID</td></tr><tr><td class="has-text-align-left" data-align="left">gateway_http</td><td>AccountID, Action, BlockedFileHash, BlockedFileName, BlockedFileReason, BlockedFileSize, BlockedFileType, DestinationIP, DestinationPort, DeviceID, DownloadedFileNames, Email, HTTPHost, HTTPMethod, HTTPVersion,IsIsolated, PolicyID, Referer, RequestID, SourceIP, SourcePort, URL, UploadedFileNames, UserAgent, UserID</td></tr><tr><td class="has-text-align-left" data-align="left">gateway_network</td><td>AccountID, Action, DestinationIP, DestinationPort, DeviceID, Email, OverrideIP, OverridePort , PolicyID, SNI, SessionID, SourceIP, SourcePort, Transport, UserID</td></tr><tr><td class="has-text-align-left" data-align="left">network_analytics_logs</td><td>AttackCampaignID, AttackID, ColoCountry, ColoGeoHash, ColoID, ColoName, DestinationASN, DestinationASNDescription, DestinationCountry, DestinationGeoHash, DestinationPort, Direction, GREChecksum, GREEthertype, GREHeaderLength, GREKey, GRESequenceNumber, GREVersion, ICMPChecksum, ICMPCode, ICMPType, IPDestinationAddress, IPDestinationSubnet, IPFragmentOffset, IPHeaderLength, IPMoreFragments, IPProtocol, IPProtocolName, IPSourceAddress, IPSourceSubnet, IPTotalLength, IPTotalLengthBuckets, IPTtl, IPTtlBuckets, IPv4Checksum, IPv4DontFragment, IPv4Dscp, IPv4Ecn, IPv4Identification, IPv4Options, IPv6Dscp, IPv6Ecn, IPv6ExtensionHeaders, IPv6FlowLabel, IPv6Identification, MitigationReason, MitigationScope, MitigationSystem, ProtocolState, RuleID, RulesetID, RulesetOverrideID, SampleInterval, SourceASN, SourceASNDescription, SourceCountry, SourceGeoHash, SourcePort, TCPAcknowledgementNumber, TCPChecksum, TCPDataOffset, TCPFlags, TCPFlagsString, TCPMss, TCPOptions, TCPSackBlocks, TCPSacksPermitted, TCPSequenceNumber, TCPTimestampEcr, TCPTimestampValue, TCPUrgentPointer, TCPWindowScale, TCPWindowSize, UDPChecksum, UDPPayloadLength, Verdict</td></tr></tbody></table>

Common errors table:

<table><tbody><tr><td>Error</td><td>Description</td><td></td></tr><tr><td>creating a new job is not allowed: Bot Management fields are not allowed (1004)</td><td>Your cloudflare account plan doesn't allow the specified fields in cloudflare_logpush_fields. contact cloudflare support to enable these fields.</td><td></td></tr><tr><td>creating a new job is not allowed: exceeded max jobs allowed (1004)</td><td>Your cloudflare account plan doesn't allow the specified dataset in cloudflare_logpush_dataset or you have reached your account maximum concurrent jobs. contact cloudflare support to ensure your account can create this logpush dataset and that you didn't exceed your maximum jobs allowed.</td><td></td></tr></tbody></table>
