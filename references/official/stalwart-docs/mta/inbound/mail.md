---
title: "MAIL stage"
source: "https://stalw.art/docs/mta/inbound/mail"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/mail"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MAIL stage"
---
# MAIL stage

      

The `MAIL FROM` command initiates an SMTP message transfer by identifying the sender of the message. After the command is issued, the receiving mail server validates the sender address and checks whether the sender is authorised to send messages on behalf of that domain. If valid, the transaction proceeds to the `RCPT TO` stage.

MAIL-stage behaviour is configured on the [[ref/object/mta-stage-mail|MtaStageMail]] singleton (found in the WebUI under  Settings ›  MTA › Session › MAIL FROM Stage).

## Address rewriting

[Section titled “Address rewriting”](#address-rewriting)

The [[ref/object/mta-stage-mail#rewrite|`rewrite`]] field accepts an expression that can modify the sender address. This is useful for obfuscating the sender for privacy, or for changing the domain to match organisational branding. For background on the expression-based rewrite model see the [[mta/rewrite/address|address rewriting]] documentation.

For example, the following configuration removes any subdomain from the sender address on non-SMTP listeners:

```
{  "rewrite": {    "match": [{"if": "listener != 'smtp' & matches('^([^.]+)@([^.]+)\\.(.+)$', rcpt)", "then": "$1 + '@' + $3"}],    "else": "false"  }}
```

## Allowed senders

[Section titled “Allowed senders”](#allowed-senders)

The [[ref/object/mta-stage-mail#issenderallowed|`isSenderAllowed`]] field accepts an expression that determines whether the sender is accepted. If the expression evaluates to `false`, the sender is rejected. The default policy allows the sender when authenticated or when the sender domain is not present in the `spam-block` list.

For example, to block domains present in the `spam-block` list:

```
{  "isSenderAllowed": {"else": "!key_exists('spam-block', sender_domain)"}}
```

## Sieve script

[Section titled “Sieve script”](#sieve-script)

The [[ref/object/mta-stage-mail#script|`script`]] field selects a [[sieve|Sieve script]] to run after a successful `MAIL FROM` command. Typical uses include rejecting specific senders or rewriting the sender address using the `envelope` Sieve extension.

For example, setting [[ref/object/mta-stage-mail#script|`script`]] to the expression `"'return_path_filter'"` runs a Sieve script named `return_path_filter` which can reject messages whose return path matches a known-bad local part:

```
require ["variables", "envelope", "reject"];
if envelope :localpart :is "from" "known_spammer" {    reject "Messages from this sender are not accepted.";}
```