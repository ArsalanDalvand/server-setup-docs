---
title: "Overview"
source: "https://stalw.art/docs/mta/inbound"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

A “session” refers to a single incoming SMTP connection. Each SMTP session progresses through a series of distinct stages, each with its own function:

[[mta/inbound/connect|Connect]]: the initial stage, where the server and client establish a connection.
[[mta/inbound/ehlo|EHLO]]: the client introduces itself to the server.
[[mta/inbound/auth|AUTH]]: the client provides authentication details.
[[mta/inbound/mail|MAIL]]: the client specifies the sender of the message.
[[mta/inbound/rcpt|RCPT]]: the client provides one or more recipients.
[[mta/inbound/data|DATA]]: the client transmits the actual message content.

Each stage is configured by its own singleton so that specific behaviour can be applied at each point in an SMTP session. Most stages support running [[sieve|Sieve scripts]], allowing custom filters that reject, alter, or reroute incoming messages based on a wide range of criteria.

## Limits

[Section titled “Limits”](#limits)

Session-wide limits are configured on the [[ref/object/mta-inbound-session|MtaInboundSession]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › Session). The relevant fields are [[ref/object/mta-inbound-session#timeout|`timeout`]] (how long to wait for a client command before the connection is terminated, default 5 minutes), [[ref/object/mta-inbound-session#transferlimit|`transferLimit`]] (the maximum number of bytes transferred within a single session, default 262144000, 250 MB), and [[ref/object/mta-inbound-session#maxduration|`maxDuration`]] (the maximum length of a single SMTP session, default 10 minutes). Each field accepts an expression and may branch on listener, remote IP, or other connection-level variables.

Example configuration with all three limits set together:

```
{  "timeout": {"else": "5m"},  "transferLimit": {"else": "262144000"},  "maxDuration": {"else": "10m"}}
```