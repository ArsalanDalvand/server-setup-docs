---
title: "Imap"
source: "https://stalw.art/docs/ref/object/imap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/imap"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Imap"
---
# Imap

      

Configures IMAP protocol settings including authentication, timeouts, and rate limits.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › IMAP

## Fields

[Section titled “Fields”](#fields)

##### allowPlainTextAuth

[Section titled “allowPlainTextAuth”](#allowplaintextauth)

Type: `Boolean` · default: `false`

Whether to allow plain text authentication on unencrypted connections

##### maxAuthFailures

[Section titled “maxAuthFailures”](#maxauthfailures)

Type: `UnsignedInt` · default: `3` · min: 1

Number of authentication attempts a user can make before being disconnected by the server

##### maxConcurrent

[Section titled “maxConcurrent”](#maxconcurrent)

Type: `UnsignedInt?` · default: `16` · min: 1

The maximum number of concurrent connections

##### maxRequestRate

[Section titled “maxRequestRate”](#maxrequestrate)

Type: [`Rate`](#rate)`?` · default: `{"count":2000,"period":"1m"}`

The maximum number of requests per minute

##### maxRequestSize

[Section titled “maxRequestSize”](#maxrequestsize)

Type: `Size` · default: `"50mb"`

Maximum size of an IMAP request that the server will accept

##### timeoutAnonymous

[Section titled “timeoutAnonymous”](#timeoutanonymous)

Type: `Duration` · default: `"1m"`

Time an unauthenticated session can stay inactive before being ended by the server

##### timeoutAuthenticated

[Section titled “timeoutAuthenticated”](#timeoutauthenticated)

Type: `Duration` · default: `"30m"`

Time an authenticated session can remain idle before the server terminates it

##### timeoutIdle

[Section titled “timeoutIdle”](#timeoutidle)

Type: `Duration` · default: `"30m"`

Time a connection can stay idle in the IMAP IDLE state before the server breaks the connection

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Imap singleton is available via the `urn:stalwart:jmap` capability.

### x:Imap/get

[Section titled “x:Imap/get”](#ximapget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysImapGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Imap/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Imap/set

[Section titled “x:Imap/set”](#ximapset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysImapUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Imap/set",          {            "update": {              "singleton": {                "allowPlainTextAuth": false              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Imap
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Imap --field allowPlainTextAuth=false
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Rate

[Section titled “Rate”](#rate)

Defines a rate limit as a count over a time period.

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0` · min: 1 · max: 1000000

Count

##### period

[Section titled “period”](#period)

Type: `Duration` · default: `"0s"` · min: 1

Period