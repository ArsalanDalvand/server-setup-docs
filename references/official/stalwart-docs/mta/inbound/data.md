---
title: "DATA stage"
source: "https://stalw.art/docs/mta/inbound/data"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/data"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DATA stage"
---
# DATA stage

      

The `DATA` command initiates the data transfer phase of message delivery. Once the sender has successfully completed the `MAIL FROM` and `RCPT TO` commands, the `DATA` (or `BDAT` when chunking is used) command begins transmission of the message. After the message has been transmitted, the server responds with a status code indicating whether it was accepted or rejected.

DATA-stage behaviour is configured on the [[ref/object/mta-stage-data|MtaStageData]] singleton (found in the WebUI under  Settings ›  MTA › Session › DATA Stage).

## Message filtering

[Section titled “Message filtering”](#message-filtering)

Once a message has been submitted with `DATA` or `BDAT`, it is possible to run [[sieve|Sieve scripts]], [[mta/filter/milter|Milter filters]], or [[mta/filter/mtahooks|MTA Hooks]] that accept, reject, or modify the message. When multiple filter types are configured, Stalwart executes Milter filters first, then Sieve scripts, and finally MTA Hooks.

### Sieve

[Section titled “Sieve”](#sieve)

The [[ref/object/mta-stage-data#script|`script`]] field selects the [[sieve|Sieve script]] to run for the DATA stage. Sieve can rewrite the message, add or remove headers, process each MIME part, and apply conditional actions using the full set of supported [[development/rfcs#sieve|Sieve extensions]].

For example, setting [[ref/object/mta-stage-data#script|`script`]] to the expression `"'data'"` runs a Sieve script named `data` which can iterate over MIME parts and rewrite their contents:

```
require ["envelope", "variables", "replace", "mime", "foreverypart", "editheader", "extracttext"];
if envelope :domain :is "to" "foobar.net" {    set "counter" "a";    foreverypart {        if header :mime :contenttype "content-type" "text/html" {            extracttext :upper "text_content";            replace "${text_content}";        }        set :length "part_num" "${counter}";        addheader :last "X-Part-Number" "${part_num}";        set "counter" "${counter}a";    }}
```

### Milter

[Section titled “Milter”](#milter)

Milter filters are defined as [[ref/object/mta-milter|MtaMilter]] objects. Each configured filter can inspect and potentially modify the message, adding, changing, or removing headers, altering the body, or rejecting the message outright. For details see the [[mta/filter/milter|Milter]] section.

### MTA Hooks

[Section titled “MTA Hooks”](#mta-hooks)

MTA Hooks provide an HTTP-based alternative to milter. They are defined as [[ref/object/mta-hook|MtaHook]] objects and can inspect or modify messages through a JSON protocol. For details see the [[mta/filter/mtahooks|MTA Hooks]] section.

## Spam filtering

[Section titled “Spam filtering”](#spam-filtering)

Whether to run the [[spamfilter|spam filter]] on incoming messages is controlled by [[ref/object/mta-stage-data#enablespamfilter|`enableSpamFilter`]], an expression that returns a boolean. The default policy enables the filter for unauthenticated sessions (`is_empty(authenticated_as)`). To restrict filtering to a specific listener only:

```
{  "enableSpamFilter": {"else": "listener == 'smtp'"}}
```

## Message limits

[Section titled “Message limits”](#message-limits)

Message limits prevent abusive use of a single session and protect against message loops. Relevant fields are [[ref/object/mta-stage-data#maxmessages|`maxMessages`]] (maximum number of messages per SMTP session, default 10), [[ref/object/mta-stage-data#maxmessagesize|`maxMessageSize`]] (maximum size of a single message in bytes, default 104857600, 100 MB), and [[ref/object/mta-stage-data#maxreceivedheaders|`maxReceivedHeaders`]] (maximum number of `Received` headers allowed in a message, which helps prevent message loops, default 50):

```
{  "maxMessages": {"else": "10"},  "maxMessageSize": {"else": "104857600"},  "maxReceivedHeaders": {"else": "50"}}
```

## Headers

[Section titled “Headers”](#headers)

Headers automatically added to accepted messages are controlled by the following fields, each an expression returning a boolean unless noted:

[[ref/object/mta-stage-data#addreceivedheader|`addReceivedHeader`]]: add a `Received` header containing the client IP address and TLS information.
[[ref/object/mta-stage-data#addreceivedspfheader|`addReceivedSpfHeader`]]: add a `Received-SPF` header with SPF results.
[[ref/object/mta-stage-data#addauthresultsheader|`addAuthResultsHeader`]]: add an `Authentication-Results` header with DMARC, DKIM, SPF, ARC, and iprev results.
[[ref/object/mta-stage-data#addmessageidheader|`addMessageIdHeader`]]: add a `Message-ID` header when the message lacks one.
[[ref/object/mta-stage-data#adddateheader|`addDateHeader`]]: add a `Date` header when the message lacks one.
[[ref/object/mta-stage-data#addreturnpathheader|`addReturnPathHeader`]]: add a `Return-Path` header containing the address specified in `MAIL FROM`.
[[ref/object/mta-stage-data#adddeliveredtoheader|`addDeliveredToHeader`]]: add a `Delivered-To` header containing the recipient address. This field is a plain boolean; default `true`.

The default policy for every expression-valued header is to add it only when `local_port == 25`. The following example adds `Received`, `Received-SPF`, and `Authentication-Results` on the SMTP listener; lets submission listeners generate `Message-ID` and `Date` when absent; always adds `Delivered-To`; and never adds `Return-Path`:

```
{  "addReceivedHeader": {    "match": [{"if": "listener == 'smtp'", "then": "true"}],    "else": "false"  },  "addReceivedSpfHeader": {    "match": [{"if": "listener == 'smtp'", "then": "true"}],    "else": "false"  },  "addAuthResultsHeader": {    "match": [{"if": "listener == 'smtp'", "then": "true"}],    "else": "false"  },  "addMessageIdHeader": {    "match": [{"if": "listener == 'smtp'", "then": "false"}],    "else": "true"  },  "addDateHeader": {    "match": [{"if": "listener == 'smtp'", "then": "false"}],    "else": "true"  },  "addReturnPathHeader": {"else": "false"},  "addDeliveredToHeader": true}
```