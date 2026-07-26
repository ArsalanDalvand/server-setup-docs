---
title: "Configuration"
source: "https://stalw.art/docs/server/tls/acme/configuration"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/tls/acme/configuration"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Configuration"
---
# Configuration

      

ACME providers are registered as [[ref/object/acme-provider|AcmeProvider]] objects (found in the WebUI under  Settings ›  TLS › ACME Providers). Each provider describes where to talk to the CA, which challenge to use, and which contacts are notified. The relevant fields are:

[[ref/object/acme-provider#directory|`directory`]]: the directory URL of the ACME provider. Let’s Encrypt’s production directory is `https://acme-v02.api.letsencrypt.org/directory` (the default); the staging directory `https://acme-staging-v02.api.letsencrypt.org/directory` is used for test runs that do not consume production rate-limit budget.
[[ref/object/acme-provider#challengetype|`challengeType`]]: the [[server/tls/acme/challenges|challenge type]] used to validate control of each domain. One of `TlsAlpn01` (default), `Dns01`, `DnsPersist01`, or `Http01`. DNS-PERSIST-01 is a variant of the DNS-01 challenge that keeps the validation TXT record in place after the initial issuance so that subsequent renewals do not have to republish it; use it with DNS providers where propagation is slow or where keeping the record simplifies automation.
[[ref/object/acme-provider#contact|`contact`]]: one or more contact email addresses. Used by the CA for expiry warnings and security notices.
[[ref/object/acme-provider#renewbefore|`renewBefore`]]: how early renewal starts, expressed as a fraction of the remaining lifetime. Default `R23` (two thirds of the remaining validity period has elapsed).
[[ref/object/acme-provider#maxretries|`maxRetries`]]: number of attempts before giving up on a failed challenge. Default `10`.
[[ref/object/acme-provider#eabkeyid|`eabKeyId`]] / [[ref/object/acme-provider#eabhmackey|`eabHmacKey`]]: External Account Binding (EAB) credentials when the CA requires them.
[[ref/object/acme-provider#membertenantid|`memberTenantId`]]: tenant scope of the provider (Enterprise deployments only).

An AcmeProvider does not list the domains it covers. Instead, each [[ref/object/domain|Domain]] that needs an automatically managed certificate sets its [[ref/object/domain#certificatemanagement|`certificateManagement`]] to the `Automatic` variant, which carries an [[ref/object/domain#certificatemanagementproperties|`acmeProviderId`]] reference to the AcmeProvider and an optional [[ref/object/domain#certificatemanagementproperties|`subjectAlternativeNames`]] list (leave empty to request a wildcard or the default set of SANs). A single AcmeProvider can be referenced from any number of Domain records, so one ACME account issues certificates for many domains. ACME account keys and state are stored in the data store and need no filesystem path.

Default-certificate selection is made on the [[ref/object/system-settings|SystemSettings]] singleton through [[ref/object/system-settings#defaultcertificateid|`defaultCertificateId`]]: whichever Certificate is pointed to there is served to clients that do not send SNI in their ClientHello. The AcmeProvider itself has no “default” flag; a provider becomes the default implicitly by issuing the Certificate selected via [[ref/object/system-settings#defaultcertificateid|`defaultCertificateId`]]. This setting is optional, since clients negotiating with SNI pick their certificate from the domain records directly.

## DNS configuration

[Section titled “DNS configuration”](#dns-configuration)

When [[ref/object/acme-provider#challengetype|`challengeType`]] is set to `Dns01` or `DnsPersist01`, Stalwart publishes the validation record through a configured DNS provider. DNS providers are registered as [[ref/object/dns-server|DnsServer]] objects; see [[server/dns/provider|DNS providers]] for the supported variants and the credentials each one needs.

The DNS zone used for record updates is set per domain on the [[ref/object/domain|Domain]] object. When [[ref/object/domain#dnsmanagement|`dnsManagement`]] is the `Automatic` variant, [[ref/object/domain#dnsmanagementproperties|`dnsServerId`]] picks the DnsServer to drive and [[ref/object/domain#dnsmanagementproperties|`origin`]] names the zone that carries the records (for example, `example.com` for a domain `sub.example.com`). Leaving [[ref/object/domain#dnsmanagementproperties|`origin`]] empty uses the domain name itself as the zone. This lets several domains share one DnsServer while each pins its own zone for DNS-01 and DNS-PERSIST-01 validation.

## Example

[Section titled “Example”](#example)

An ACME provider pointing at the Let’s Encrypt production directory and using TLS-ALPN-01:

```
{  "directory": "https://acme-v02.api.letsencrypt.org/directory",  "challengeType": "TlsAlpn01",  "contact": ["[email protected]"],  "renewBefore": "R23"}
```