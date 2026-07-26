---
title: "Jmap"
source: "https://stalw.art/docs/ref/object/jmap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/jmap"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Jmap"
---
# Jmap

      

Configures JMAP protocol limits for requests, uploads, and push notifications.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › JMAP › Limits Settings ›  Network › JMAP › Push Settings ›  Network › JMAP › WebSocket

## Fields

[Section titled “Fields”](#fields)

##### parseLimitEvent

[Section titled “parseLimitEvent”](#parselimitevent)

Type: `UnsignedInt` · default: `10` · min: 1

Limits the maximum number of iCalendar items that can be parsed in a single request

##### parseLimitContact

[Section titled “parseLimitContact”](#parselimitcontact)

Type: `UnsignedInt` · default: `10` · min: 1

Limits the maximum number of vCard items that can be parsed in a single request

##### parseLimitEmail

[Section titled “parseLimitEmail”](#parselimitemail)

Type: `UnsignedInt` · default: `10` · min: 1

Limits the maximum number of e-mail message that can be parsed in a single request

##### changesMaxResults

[Section titled “changesMaxResults”](#changesmaxresults)

Type: `UnsignedInt` · default: `5000` · min: 1

Determines the maximum number of change objects that a Changes method can return

##### getMaxResults

[Section titled “getMaxResults”](#getmaxresults)

Type: `UnsignedInt` · default: `500` · min: 1

Determines the maximum number of objects that can be fetched in a single method call

##### queryMaxResults

[Section titled “queryMaxResults”](#querymaxresults)

Type: `UnsignedInt` · default: `5000` · min: 1

Sets the maximum number of results that a Query method can return

##### maxMethodCalls

[Section titled “maxMethodCalls”](#maxmethodcalls)

Type: `UnsignedInt` · default: `16` · min: 1

Limits the maximum number of method calls that can be included in a single request

##### maxConcurrentRequests

[Section titled “maxConcurrentRequests”](#maxconcurrentrequests)

Type: `UnsignedInt?` · default: `4` · min: 1

Restricts the number of concurrent requests a user can make to the JMAP server

##### maxRequestSize

[Section titled “maxRequestSize”](#maxrequestsize)

Type: `Size` · default: `10000000` · min: 1

Defines the maximum size of a single request, in bytes, that the server will accept

##### setMaxObjects

[Section titled “setMaxObjects”](#setmaxobjects)

Type: `UnsignedInt` · default: `500` · min: 1

Establishes the maximum number of objects that can be modified in a single method call

##### snippetMaxResults

[Section titled “snippetMaxResults”](#snippetmaxresults)

Type: `UnsignedInt` · default: `100` · min: 1

Maximum number of search snippets to return in a single request

##### maxConcurrentUploads

[Section titled “maxConcurrentUploads”](#maxconcurrentuploads)

Type: `UnsignedInt?` · default: `4` · min: 1

Restricts the number of concurrent file uploads a user can perform

##### maxUploadSize

[Section titled “maxUploadSize”](#maxuploadsize)

Type: `UnsignedInt` · default: `50000000` · min: 1

Defines the maximum file size for file uploads to the server

##### maxUploadCount

[Section titled “maxUploadCount”](#maxuploadcount)

Type: `UnsignedInt` · default: `1000` · min: 1

Specifies the maximum number of files that a user can upload within a certain period

##### uploadQuota

[Section titled “uploadQuota”](#uploadquota)

Type: `UnsignedInt` · default: `50000000` · min: 1

Defines the total size of files that a user can upload within a certain period

##### uploadTtl

[Section titled “uploadTtl”](#uploadttl)

Type: `Duration` · default: `"1h"`

Specifies the Time-To-Live (TTL) for each uploaded file, after which the file is deleted from temporary storage

##### eventSourceThrottle

[Section titled “eventSourceThrottle”](#eventsourcethrottle)

Type: `Duration` · default: `"1s"`

Specifies the minimum time between two event source notifications

##### pushAttemptWait

[Section titled “pushAttemptWait”](#pushattemptwait)

Type: `Duration` · default: `"1m"`

Time to wait between push attempts

##### pushMaxAttempts

[Section titled “pushMaxAttempts”](#pushmaxattempts)

Type: `UnsignedInt` · default: `3` · min: 1

Maximum number of push attempts before a notification is discarded

##### pushRetryWait

[Section titled “pushRetryWait”](#pushretrywait)

Type: `Duration` · default: `"1s"`

Time to wait between retry attempts

##### pushThrottle

[Section titled “pushThrottle”](#pushthrottle)

Type: `Duration` · default: `"1s"`

Time to wait before sending a new request to the push service

##### pushRequestTimeout

[Section titled “pushRequestTimeout”](#pushrequesttimeout)

Type: `Duration` · default: `"10s"`

Time before a connection with a push service URL times out

##### pushVerifyTimeout

[Section titled “pushVerifyTimeout”](#pushverifytimeout)

Type: `Duration` · default: `"1m"`

Time to wait for the push service to verify a subscription

##### pushShardsTotal

[Section titled “pushShardsTotal”](#pushshardstotal)

Type: `UnsignedInt` · default: `1` · min: 1

Total number of shards for push notification processing across multiple nodes

##### websocketHeartbeat

[Section titled “websocketHeartbeat”](#websocketheartbeat)

Type: `Duration` · default: `"1m"`

Time to wait before sending a new heartbeat to the WebSocket client

##### websocketThrottle

[Section titled “websocketThrottle”](#websocketthrottle)

Type: `Duration` · default: `"1s"`

Amount of time to wait before sending a batch of notifications to a WS client

##### websocketTimeout

[Section titled “websocketTimeout”](#websockettimeout)

Type: `Duration` · default: `"10m"`

Time before an inactive WebSocket connection times out

##### maxSubscriptions

[Section titled “maxSubscriptions”](#maxsubscriptions)

Type: `UnsignedInt?` · default: `15` · min: 1

The default maximum number of push subscriptions a user can create

##### webPushKey

[Section titled “webPushKey”](#webpushkey)

Type: [`SecretTextOptional`](#secrettextoptional) · required

ECDSA P-256 private key (PKCS#8 PEM) used to sign VAPID (RFC 8292) authentication tokens for Web Push.

##### webPushContact

[Section titled “webPushContact”](#webpushcontact)

Type: `String?`

Optional contact URI (a mailto: or https: address) included as the sub claim of VAPID tokens so that push services can reach the server operator. Leave empty to omit the claim.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Jmap singleton is available via the `urn:stalwart:jmap` capability.

### x:Jmap/get

[Section titled “x:Jmap/get”](#xjmapget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysJmapGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Jmap/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Jmap/set

[Section titled “x:Jmap/set”](#xjmapset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysJmapUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Jmap/set",          {            "update": {              "singleton": {                "webPushContact": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Jmap
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Jmap --field webPushContact='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretTextOptional

[Section titled “SecretTextOptional”](#secrettextoptional)

An optional secret text value, or none.

`None`: No secret. No additional fields.
`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `Text` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from