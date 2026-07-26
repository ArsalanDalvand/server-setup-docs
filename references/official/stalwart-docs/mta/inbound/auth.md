---
title: "AUTH stage"
source: "https://stalw.art/docs/mta/inbound/auth"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/auth"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AUTH stage"
---
# AUTH stage

      

The `AUTH` command authenticates a user wishing to send an email message through an SMTP server. Once issued, the client supplies credentials using SASL, a framework for authentication and data security in network protocols. SASL supports mechanisms such as username and password, public-key cryptography, and bearer tokens.

## Settings

[Section titled “Settings”](#settings)

SASL authentication is configured on the [[ref/object/mta-stage-auth|MtaStageAuth]] singleton (found in the WebUI under  Settings ›  MTA › Session › AUTH Stage). The relevant fields are:

[[ref/object/mta-stage-auth#saslmechanisms|`saslMechanisms`]]: a list of [SASL mechanisms](https://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml) offered to clients, or an empty list to disable authentication. Stalwart supports `PLAIN`, `LOGIN`, `OAUTHBEARER`, and `XOAUTH2`. The default policy offers `[plain, login, oauthbearer, xoauth2]` on TLS-protected submission ports and `[oauthbearer, xoauth2]` on non-TLS submission ports, while disabling AUTH on port 25.
[[ref/object/mta-stage-auth#require|`require`]]: whether authentication is required to send email messages. The default expression requires authentication when `local_port != 25`.
[[ref/object/mta-stage-auth#mustmatchsender|`mustMatchSender`]]: whether the authenticated user (or one of their associated email addresses) must match the sender of the message. Default `true`.
[[ref/object/mta-stage-auth#maxfailures|`maxFailures`]]: maximum number of authentication errors allowed before the session is disconnected. Default 3.
[[ref/object/mta-stage-auth#waitonfail|`waitOnFail`]]: time to wait after an authentication failure. Default 5 seconds.

Example restricting `PLAIN` and `LOGIN` to TLS-protected submission ports and requiring authentication outside port 25:

```
{  "saslMechanisms": {    "match": [{"if": "local_port != 25 && is_tls", "then": "[plain, login]"}],    "else": "false"  },  "require": {    "match": [{"if": "listener != 'smtp'", "then": "true"}],    "else": "false"  },  "mustMatchSender": {"else": "true"},  "maxFailures": {"else": "3"},  "waitOnFail": {"else": "5s"}}
```

## Directory selection

[Section titled “Directory selection”](#directory-selection)

The authentication backend is selected automatically based on the domain name supplied in the credentials. Stalwart matches the domain against the configured [[ref/object/domain|Domain]] objects and routes the authentication request to the [[ref/object/domain#directoryid|`directoryId`]] associated with that domain, falling back to the internal directory when no explicit directory is set. No per-stage directory field exists on MtaStageAuth.