---
title: "EHLO stage"
source: "https://stalw.art/docs/mta/inbound/ehlo"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/ehlo"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "EHLO stage"
---
# EHLO stage

      

The `EHLO` (Extended Hello) command is sent by an email client to the server to initiate an SMTP session and negotiate the features and extensions that will be used during the session. The `EHLO` command provides a way for the email client to identify itself, and for the server to advertise the capabilities and extensions it supports, such as authentication mechanisms, message size limits, and others. The response to the `EHLO` command provides information to the client about the server’s capabilities, which the client can then use to determine the best way to send the email.

## Settings

[Section titled “Settings”](#settings)

EHLO-stage behaviour is configured on the [[ref/object/mta-stage-ehlo|MtaStageEhlo]] singleton (found in the WebUI under  Settings ›  MTA › Session › EHLO Stage). The relevant fields are [[ref/object/mta-stage-ehlo#require|`require`]] (whether the remote client must send an `EHLO`, `HELO`, or `LHLO` command before starting an SMTP transaction; enabling this lets Stalwart verify the [[mta/authentication/spf|SPF]] EHLO identity of the client, default `true`), [[ref/object/mta-stage-ehlo#rejectnonfqdn|`rejectNonFqdn`]] (whether to reject `EHLO` commands that do not include a fully-qualified domain name as a parameter; the default enables the check only on `local_port == 25`, so that submission listeners accept client hostnames that may not be fully qualified), and [[ref/object/mta-stage-ehlo#script|`script`]] (the [[sieve|Sieve script]] to run after a successful `EHLO` command; a typical use is to block specific HELO domains by matching the `helo_domain` variable against an in-memory list).

Example configuration setting all three fields together, including a per-listener override for `rejectNonFqdn` and a Sieve script selector:

```
{  "require": {"else": "true"},  "rejectNonFqdn": {    "match": [{"if": "listener == 'smtp'", "then": "true"}],    "else": "false"  },  "script": {"else": "'ehlo'"}}
```

A companion Sieve script `ehlo` rejects HELO domains present in a blocked-domain list:

```
require ["variables", "extlists", "reject"];
if string :list "${env.helo_domain}" "blocked-domain" {    reject "551 5.1.1 Sending domain '${env.helo_domain}' has been blocklisted.";}
```

The `blocked-domain` list used by the `:list` match is backed by entries on the [[ref/object/memory-lookup-key|MemoryLookupKey]] object (found in the WebUI under  Settings ›  Lookups › In-Memory Keys,  Settings ›  Spam Filter › Lists › Blocked Domains,  Settings ›  Spam Filter › Lists › Spam Traps,  Settings ›  Spam Filter › Lists › Trusted Domains,  Settings ›  Spam Filter › Lists › URL Redirectors). Each blocked domain is a separate key created under the `blocked-domain` [[ref/object/memory-lookup-key#namespace|`namespace`]], with the domain name itself as the [[ref/object/memory-lookup-key#key|`key`]]:

```
{  "namespace": "blocked-domain",  "key": "spammer.example",  "isGlobPattern": false}
```

Additional entries use the same namespace with different key values, and glob patterns such as `*.spammer.example` can be enabled by setting [[ref/object/memory-lookup-key#isglobpattern|`isGlobPattern`]] to `true`.

## SMTP extensions

[Section titled “SMTP extensions”](#smtp-extensions)

Stalwart supports a range of [[development/rfcs#smtp|SMTP extensions]], configured on the [[ref/object/mta-extensions|MtaExtensions]] singleton (found in the WebUI under  Settings ›  MTA › Session › Extensions). Each field accepts an expression that returns either a boolean, a duration, or the extension-specific value described below.

[[ref/object/mta-extensions#pipelining|`pipelining`]]: SMTP pipelining (RFC 2920). Multiple commands can be sent in a single request.
[[ref/object/mta-extensions#chunking|`chunking`]]: chunking (RFC 3030). Large messages can be transferred in chunks.
[[ref/object/mta-extensions#requiretls|`requireTls`]]: REQUIRETLS (RFC 8689). Clients can require TLS encryption for the SMTP session.
[[ref/object/mta-extensions#nosoliciting|`noSoliciting`]]: text advertised via `NOSOLICITING` (RFC 3865), indicating the server does not accept unsolicited commercial email.
[[ref/object/mta-extensions#dsn|`dsn`]]: delivery status notifications (RFC 3461). Senders can request a DSN from the recipient’s mail server.
[[ref/object/mta-extensions#futurerelease|`futureRelease`]]: maximum time a message can be held for delivery via the `FUTURERELEASE` (RFC 4865) extension. Returning `false` disables the extension.
[[ref/object/mta-extensions#deliverby|`deliverBy`]]: maximum delivery time allowed by the `DELIVERBY` (RFC 2852) extension. Returning `false` disables the extension.
[[ref/object/mta-extensions#mtpriority|`mtPriority`]]: priority-assignment policy advertised on the `MT-PRIORITY` (RFC 6710) extension. Accepted values are `mixer`, `stanag4406`, and `nsep`, or `false` to disable the extension.
[[ref/object/mta-extensions#vrfy|`vrfy`]]: whether to advertise the `VRFY` command, used by senders to verify the existence of a mailbox. Disabling it by default prevents address harvesting; the default policy only enables it for authenticated sessions.
[[ref/object/mta-extensions#expn|`expn`]]: whether to advertise the `EXPN` command, used to request the membership of a mailing list. The default policy mirrors `vrfy`.

The `SIZE`, `ENHANCEDSTATUSCODES`, `8BITMIME`, and `SMTPUTF8` extensions are always advertised and are not configurable. The `AUTH` extension is governed by the [[mta/inbound/auth|AUTH stage]].

Example configuration restricting advanced extensions to authenticated sessions:

```
{  "pipelining": {"else": "true"},  "chunking": {"else": "true"},  "requireTls": {"else": "true"},  "noSoliciting": {"else": "''"},  "dsn": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "true"}],    "else": "false"  },  "futureRelease": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "7d"}],    "else": "false"  },  "deliverBy": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "15d"}],    "else": "false"  },  "mtPriority": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "mixer"}],    "else": "false"  },  "vrfy": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "true"}],    "else": "false"  },  "expn": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "true"}],    "else": "false"  }}
```