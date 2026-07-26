---
title: "Collaborative digests"
source: "https://stalw.art/docs/spamfilter/digests"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/digests"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Collaborative digests"
---
# Collaborative digests

      

Collaborative spam detection networks such as Pyzor, Razor, and DCC rely on message digests. When a user identifies an email as spam, the system generates a unique hash of the message and shares it across the network. Incoming messages are then compared against the repository of spam hashes; if a hash matches, the message is flagged. The principle is straightforward: once one participant in the network identifies a spam wave, all other participants are immediately protected from the same messages.

## Pyzor

[Section titled “Pyzor”](#pyzor)

Pyzor is an open-source collaborative spam detection system that uses collective intelligence to identify and filter spam. When users across the network mark messages as spam, Pyzor creates and shares their hashes; it also tracks messages users have reported as legitimate, reducing the risk that genuine mail is mistakenly flagged.

### Configuration

[Section titled “Configuration”](#configuration)

Pyzor is configured through the [[ref/object/spam-pyzor|SpamPyzor]] singleton (found in the WebUI under  Settings ›  Spam Filter › Pyzor). It is enabled by default and points at the public Pyzor server `public.pyzor.org` with a five-second lookup timeout.

Relevant fields are:

[[ref/object/spam-pyzor#enable|`enable`]]: enables or disables Pyzor.
[[ref/object/spam-pyzor#host|`host`]]: the Pyzor server hostname.
[[ref/object/spam-pyzor#port|`port`]]: the Pyzor server port.
[[ref/object/spam-pyzor#timeout|`timeout`]]: maximum time to wait for a response before the check is treated as failed.
[[ref/object/spam-pyzor#blockcount|`blockCount`]]: minimum number of times a hash must appear in the Pyzor blocklist for the message to be considered.
[[ref/object/spam-pyzor#allowcount|`allowCount`]]: minimum number of times a hash must appear in the Pyzor allowlist for the message to be considered.
[[ref/object/spam-pyzor#ratio|`ratio`]]: the ratio of blocklist hits to allowlist hits above which the message is treated as spam.

Example configuration matching the default public server:

```
{  "enable": true,  "host": "public.pyzor.org",  "port": 24441,  "timeout": "5s",  "blockCount": 5,  "allowCount": 10,  "ratio": 0.2}
```