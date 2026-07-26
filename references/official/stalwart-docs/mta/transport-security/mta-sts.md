---
title: "MTA-STS"
source: "https://stalw.art/docs/mta/transport-security/mta-sts"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/transport-security/mta-sts"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MTA-STS"
---
# MTA-STS

      

MTA-STS, or Mail Transfer Agent Strict Transport Security, is a security mechanism for email systems to protect against eavesdropping and tampering of emails during transmission. It is designed to ensure that email is sent and received over secure connections, such as TLS.

MTA-STS works by publishing a policy statement in the form of a well-known DNS TXT record, specifying the email domains that support MTA-STS and the requirements for securing the email transmission. The receiving email server will retrieve the MTA-STS policy statement and verify that the incoming connection meets the security requirements specified in the policy. If the connection does not meet the requirements, the email will not be accepted and will be rejected. MTA-STS provides an additional layer of security for email transmission, making it harder for attackers to eavesdrop on or tamper with email messages. This can help prevent sensitive information from being intercepted or altered in transit, and provide a more secure email experience for users.

## Outbound configuration

[Section titled “Outbound configuration”](#outbound-configuration)

Whether to use MTA-STS on outbound connections is configured per TLS strategy on the [[ref/object/mta-tls-strategy|MtaTlsStrategy]] object (found in the WebUI under  Settings ›  MTA › Outbound › TLS Strategies). The [[ref/object/mta-tls-strategy#mtasts|`mtaSts`]] field accepts one of:

`optional`: use MTA-STS only if an STS policy for the domain has been published.
`require`: require MTA-STS and refuse delivery unless a valid STS policy is available. Not recommended as a global default.
`disable`: never use MTA-STS.

## Policy publishing

[Section titled “Policy publishing”](#policy-publishing)

An MTA-STS policy lets domain owners declare that their mail servers support TLS and that messages should only be delivered over a secure connection. The policy reduces the risk of man-in-the-middle attacks and ensures transport-layer encryption is used consistently.

The MTA-STS policy is published at `/.well-known/mta-sts.txt` under the HTTPS origin of the domain hosting the SMTP service. The policy file includes:

version: the version of the MTA-STS standard. Currently `STSv1`.
mode: the operational mode of the policy (`none`, `testing`, or `enforce`). `enforce` mode requires sending servers to connect only over secure connections, while `testing` mode lets domain owners monitor policy failures without affecting mail delivery.
mx: the list of mail servers permitted to receive mail for the domain.
max_age: the length of time, in seconds, the sender should cache and apply the policy.

Stalwart can automate the publication of MTA-STS policy files for all hosted domains, keeping every policy up to date without manual intervention. Policy publishing is configured on the [[ref/object/mta-sts|MtaSts]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › MTA-STS):

[[ref/object/mta-sts#mode|`mode`]]: operational mode of the policy. Accepted values are `enforce`, `testing`, and `disable`. Default `testing`.
[[ref/object/mta-sts#maxage|`maxAge`]]: how long clients should cache the policy. Default 7 days.
[[ref/object/mta-sts#mxhosts|`mxHosts`]]: optional override for the list of mail servers permitted to receive mail for the domain. If left empty, the hostnames included in all TLS certificates for the domain are used.

Example:

```
{  "mode": "testing",  "maxAge": "7d",  "mxHosts": ["mx1.example.com", "mx2.example.com"]}
```

Stalwart can also automatically generate MTA-STS DNS records for hosted domains; the records are available in the [[management/webui|WebUI]] under Management > Directory > Domains.