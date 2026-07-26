---
title: "DNS blocklists"
source: "https://stalw.art/docs/spamfilter/dnsbl"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/dnsbl"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DNS blocklists"
---
# DNS blocklists

      

In email security and spam filtering, two widely used tools are DNSBL (DNS-based Block List) and DNSWL (DNS-based Allowlist). Both are maintained by external organisations and updated continuously to reflect the latest information about spam and malicious activity. The lists are queried over DNS, so lookups are fast and cacheable, and are used to decide whether to accept, reject, or further scrutinise an incoming message.

A DNSBL lists IP addresses or domain names known to be sources of spam, malicious activity, or other undesirable content. When a message is received, the mail server queries the DNSBL to determine whether the sender appears on any of the lists. A positive match can cause the message to be flagged, rejected, or subjected to additional checks, depending on configuration.

A DNSWL is the converse: a list of trusted IP addresses or domain names known to send legitimate email. Querying a DNSWL allows the mail server to identify and prioritise messages from reputable senders, reducing false positives.

The following DNSBL checks are performed by Stalwart:

IP address: the reputation of the remote host, as well as of every IP address found in the `Received` headers of a message, is verified against configured lists. Checking the full received chain improves the accuracy of spam detection and reduces the risk of legitimate messages being misclassified.
Domain: domain names extracted from incoming messages are checked against DNS-based block and allow lists.
URL: URLs embedded in messages are checked against URL-oriented blocklists to identify malicious links.
Hashes: hashes of specific components of the message are checked against hash-based blocklists, so that threats can be identified even when senders slightly modify their tactics or domain names.

## DNSBL checks

[Section titled “DNSBL checks”](#dnsbl-checks)

Each DNSBL check is defined as a [[ref/object/spam-dnsbl-server|SpamDnsblServer]] object (found in the WebUI under  Settings ›  Spam Filter › DNSBL › Servers). This is a multi-variant object: the variant selects which part of the message the DNSBL applies to (`Ip`, `Domain`, `Url`, `Email`, `Header`, `Body`, or `Any`).

Each SpamDnsblServer instance carries:

[[ref/object/spam-dnsbl-server#enable|`enable`]]: whether the server is active.
[[ref/object/spam-dnsbl-server#zone|`zone`]]: an [[ref/object/spam-dnsbl-server#expression|`Expression`]] that evaluates to the DNS zone to query. If the expression evaluates to `false`, the DNSBL server is skipped for that message.
[[ref/object/spam-dnsbl-server#tag|`tag`]]: an [[ref/object/spam-dnsbl-server#expression|`Expression`]] that evaluates to the tag to apply to the message based on the DNS query result. If the expression evaluates to `false`, no tag is applied.

Example reproducing the Mailspike IP DNSBL using the `Ip` variant:

```
{  "@type": "Ip",  "name": "STWT_RBL_MAILSPIKE_IP",  "enable": true,  "zone": {    "match": [{"if": "location == 'tcp'", "then": "ip_reverse + '.rep.mailspike.net'"}],    "else": "false"  },  "tag": {    "match": [      {"if": "octets[0] != 127", "then": "false"},      {"if": "octets[3] == 10", "then": "'RBL_MAILSPIKE_WORST'"},      {"if": "octets[3] == 11", "then": "'RBL_MAILSPIKE_VERYBAD'"},      {"if": "octets[3] == 12", "then": "'RBL_MAILSPIKE_BAD'"},      {"if": "octets[3] >= 13 && octets[3] <= 16", "then": "'RWL_MAILSPIKE_NEUTRAL'"},      {"if": "octets[3] == 17", "then": "'RWL_MAILSPIKE_POSSIBLE'"},      {"if": "octets[3] == 18", "then": "'RWL_MAILSPIKE_GOOD'"},      {"if": "octets[3] == 19", "then": "'RWL_MAILSPIKE_VERYGOOD'"},      {"if": "octets[3] == 20", "then": "'RWL_MAILSPIKE_EXCELLENT'"}    ],    "else": "false"  }}
```

## Core DNSBL servers

[Section titled “Core DNSBL servers”](#core-dnsbl-servers)

Stalwart ships a set of predefined DNSBL server definitions maintained by Stalwart Labs. These are identified by a name starting with `STWT_` and cover Spamhaus, Spamcop, Barracuda, and dozens of other popular providers. Core DNSBL servers are regularly updated to remain effective against the latest threat sources.

The latest version of the core list is maintained in the [Spam Filter repository](https://github.com/stalwartlabs/spam-filter). Stalwart can be configured to download and apply the latest definitions automatically; see [[spamfilter/settings/general#updates|updates]].

Core definitions should not be modified directly, because any changes are overwritten on the next update. The only safe in-place modification is toggling [[ref/object/spam-dnsbl-server#enable|`enable`]] on a core entry, which is preserved across updates. To apply custom behaviour beyond enabling or disabling a core entry, disable the core server and create a custom SpamDnsblServer with the desired values.

For example, to disable a core entry and add a custom server:

```
{  "@type": "Ip",  "name": "STWT_DNSBL_SERVER",  "enable": false}
```

```
{  "@type": "Ip",  "name": "STWT_MY_DNSBL_SERVER",  "enable": true,  "zone": {"else": "zone expression"},  "tag": {"else": "tag expression"}}
```

This keeps customisations intact while still benefiting from ongoing updates to the core list.

## Limits

[Section titled “Limits”](#limits)

The maximum number of DNSBL queries that can be performed per message is configured on the [[ref/object/spam-dnsbl-settings|SpamDnsblSettings]] singleton (found in the WebUI under  Settings ›  Spam Filter › DNSBL › Settings). The per-message limits are:

[[ref/object/spam-dnsbl-settings#iplimit|`ipLimit`]]: maximum number of IP address queries.
[[ref/object/spam-dnsbl-settings#domainlimit|`domainLimit`]]: maximum number of domain name queries.
[[ref/object/spam-dnsbl-settings#emaillimit|`emailLimit`]]: maximum number of email address queries.
[[ref/object/spam-dnsbl-settings#urllimit|`urlLimit`]]: maximum number of URL queries.

All four limits default to 50.