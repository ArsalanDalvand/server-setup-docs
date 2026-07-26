---
title: "Rate limiting"
source: "https://stalw.art/docs/email/settings/ratelimit"
source_format: official-html-to-markdown
language: "en"
doc_slug: "email/settings/ratelimit"
category: "email"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Rate limiting"
---
# Rate limiting

      

Rate limiting is a strategy to limit network traffic by capping how often a given action (for example, a login attempt) can be repeated within a time window. Rate limiting helps mitigate abuse such as brute-force attacks and reduces load on the mail server.

## IMAP/POP3

[Section titled “IMAP/POP3”](#imappop3)

IMAP and POP3 rate-limit fields live on the [[ref/object/imap|Imap]] singleton (found in the WebUI under  Settings ›  Network › IMAP).

### Concurrency

[Section titled “Concurrency”](#concurrency)

The maximum number of concurrent IMAP and POP3 connections a user may hold is set by [[ref/object/imap#maxconcurrent|`maxConcurrent`]]. The default is `16`; lowering it, for example to four connections per user:

```
{  "maxConcurrent": 4}
```

### Requests

[Section titled “Requests”](#requests)

The per-minute request cap is set by [[ref/object/imap#maxrequestrate|`maxRequestRate`]], which takes a `Rate` value of `count` and `period`. The default is 2000 requests per minute.

For example, to allow 2000 requests per minute:

```
{  "maxRequestRate": {"count": 2000, "period": "1m"}}
```

## JMAP

[Section titled “JMAP”](#jmap)

Refer to the [[http/jmap/protocol|JMAP Protocol limits]] page for details on how JMAP request limits and concurrency are configured on the [[ref/object/jmap|Jmap]] singleton.