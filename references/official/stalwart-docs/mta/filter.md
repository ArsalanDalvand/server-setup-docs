---
title: "Overview"
source: "https://stalw.art/docs/mta/filter"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/filter"
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

      

Email filtering is an essential part of any modern mail server. Filtering covers the management of incoming and outgoing messages, blocking or flagging spam, removing viruses from attachments, automating the organisation of messages, and implementing user-defined rules for message handling. Effective filtering protects the security of the mail system and supports user productivity.

Stalwart supports several filtering mechanisms, which can be combined:

[[mta/filter/spam|Spam filter]]: a built-in spam and phishing filter.
[[sieve|Sieve scripts]]: scripts written in the Sieve language, a standard scripting language for mail filtering. Sieve scripts let administrators and users specify rules for handling incoming mail.
[[mta/filter/milter|Milter]]: a dynamic extension for mail servers that allows external software to inspect or modify messages as they are being processed. Milters support spam filtering, virus scanning, and similar workflows.
[[mta/filter/mtahooks|MTA Hooks]]: an HTTP-based alternative to milter for managing and processing email transactions.