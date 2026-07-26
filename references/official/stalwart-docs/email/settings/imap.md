---
title: "IMAP and POP3"
source: "https://stalw.art/docs/email/settings/imap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "email/settings/imap"
category: "email"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "IMAP and POP3"
---
# IMAP and POP3

      

IMAP protocol settings cover request handling, authentication, timeouts, and rate limits. Several of these settings also apply to the POP3 server, as both protocols share the same configuration surface. IMAP and POP3 settings are carried on the [[ref/object/imap|Imap]] singleton (found in the WebUI under  Settings ›  Network › IMAP). Default special-use folders, which are used by IMAP clients as well as JMAP, are configured on the [[ref/object/email|Email]] singleton (found in the WebUI under  Settings ›  Email › Defaults,  Settings ›  Email › Encryption,  Settings ›  Email › Limits,  Settings ›  Email › Storage).

## Request size

[Section titled “Request size”](#request-size)

The [[ref/object/imap#maxrequestsize|`maxRequestSize`]] field sets the maximum size of an IMAP request that the server will accept. Requests larger than this limit are rejected. The default is `"50mb"`.

## Authentication

[Section titled “Authentication”](#authentication)

Two fields on the Imap singleton control IMAP and POP3 authentication behaviour:

[[ref/object/imap#maxauthfailures|`maxAuthFailures`]]: maximum number of authentication attempts allowed before the session is disconnected. Default `3`.
[[ref/object/imap#allowplaintextauth|`allowPlainTextAuth`]]: whether plain-text authentication is permitted over an unencrypted connection. For security reasons, this should be left at its default `false` unless strictly required, so that credentials are not transmitted in the clear.

Example:

```
{  "maxAuthFailures": 3,  "allowPlainTextAuth": false}
```

## Default folders

[Section titled “Default folders”](#default-folders)

Default special-use folders created for new accounts are defined by [[ref/object/email#defaultfolders|`defaultFolders`]] on the [[ref/object/email|Email]] singleton. The field is a map keyed by special-use type; supported keys are `inbox`, `trash`, `junk`, `drafts`, `archive`, `sent`, `shared`, `important`, `memos`, `scheduled`, and `snoozed`.

Each entry carries the fields of the nested `EmailFolder` type:

[[ref/object/email#name|`name`]]: display name of the folder. Required.
[[ref/object/email#create|`create`]]: whether the folder is created automatically. Default `true`.
[[ref/object/email#subscribe|`subscribe`]]: whether the folder is subscribed by default. Default `true`.
[[ref/object/email#aliases|`aliases`]]: additional names under which the folder is recognised.

Example:

```
{  "defaultFolders": {    "sent": {"name": "Sent Items", "create": true, "subscribe": true},    "junk": {"name": "SPAM", "create": true, "subscribe": false},    "shared": {"name": "Shared Folders"}  }}
```

## Timeouts

[Section titled “Timeouts”](#timeouts)

Idle timeouts are controlled by three fields on the Imap singleton:

[[ref/object/imap#timeoutauthenticated|`timeoutAuthenticated`]]: time an authenticated session can remain idle before the server terminates it. Default `"30m"`.
[[ref/object/imap#timeoutanonymous|`timeoutAnonymous`]]: time an anonymous (unauthenticated) session can stay inactive before being ended by the server. Default `"1m"`.
[[ref/object/imap#timeoutidle|`timeoutIdle`]]: time a connection can stay idle in the IMAP `IDLE` state before the server breaks the connection. Default `"30m"`.

Example:

```
{  "timeoutAuthenticated": "30m",  "timeoutAnonymous": "1m",  "timeoutIdle": "30m"}
```