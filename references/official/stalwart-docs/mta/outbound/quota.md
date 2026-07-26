---
title: "Quotas"
source: "https://stalw.art/docs/mta/outbound/quota"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/outbound/quota"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Quotas"
---
# Quotas

      

Quotas set limits on the message queue to control its size and total number of messages. Stalwart supports dynamic quotas, which can limit the total size and number of messages waiting to be delivered based on multiple variables. When a queue quota is exceeded, messages are temporarily rejected with a 4xx SMTP code, protecting the server from becoming overwhelmed while ensuring important messages continue to be delivered.

## Settings

[Section titled “Settings”](#settings)

Each queue quota is defined as an [[ref/object/mta-queue-quota|MtaQueueQuota]] object (found in the WebUI under  Settings ›  MTA › Rates & Quotas › Queue Quotas). The relevant fields are:

[[ref/object/mta-queue-quota#enable|`enable`]]: whether the quota is active. Default `true`.
[[ref/object/mta-queue-quota#messages|`messages`]]: optional maximum number of messages that the queue will accept.
[[ref/object/mta-queue-quota#size|`size`]]: optional maximum total size of messages in the queue.
[[ref/object/mta-queue-quota#key|`key`]]: list of context variables that determine the grouping of this quota (see [Groups](#groups) below).
[[ref/object/mta-queue-quota#match|`match`]]: expression evaluated to decide whether the quota applies to a given message.

A quota may define only a message limit, only a size limit, or both. An MtaQueueQuota with an empty `key` list and a `match` expression that always returns true imposes a global limit.

A global quota of 100,000 messages / 10 GB:

```
{  "enable": true,  "key": [],  "match": {"else": "true"},  "messages": 100000,  "size": 10737418240}
```

## Groups

[Section titled “Groups”](#groups)

The [[ref/object/mta-queue-quota#key|`key`]] field creates quota groups based on a combination of context variables. Available values are:

`sender`: the return path specified in the `MAIL FROM` command.
`senderDomain`: the domain component of the return path.
`rcpt`: the recipient address specified in the `RCPT TO` command.
`rcptDomain`: the domain component of the recipient address.

For example, to limit to 10 the total number of queued messages for any single recipient:

```
{  "enable": true,  "key": ["rcpt"],  "match": {"else": "true"},  "messages": 10}
```

And, to limit the queue size to 5 MB per combination of sender domain and recipient domain:

```
{  "enable": true,  "key": ["senderDomain", "rcptDomain"],  "match": {"else": "true"},  "size": 5242880}
```

## Conditional quotas

[Section titled “Conditional quotas”](#conditional-quotas)

The [[ref/object/mta-queue-quota#match|`match`]] expression can restrict a quota to specific conditions. For example, applying a 900-message / 7 MB quota by recipient only to messages sent from `example.org`:

```
{  "enable": true,  "key": ["rcpt"],  "match": {"else": "sender_domain == 'example.org'"},  "messages": 900,  "size": 7340032}
```