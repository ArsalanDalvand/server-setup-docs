---
title: "WebHook"
source: "https://stalw.art/docs/ref/object/web-hook"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/web-hook"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "WebHook"
---
# WebHook

      

Defines a webhook endpoint for event notifications.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Telemetry › Webhooks

## Fields

[Section titled “Fields”](#fields)

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether Stalwart should connect to a webhook endpoint that has an invalid TLS certificate

##### signatureKey

[Section titled “signatureKey”](#signaturekey)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The HMAC key used to sign the webhook request body to prevent tampering

##### throttle

[Section titled “throttle”](#throttle)

Type: `Duration` · default: `"1s"`

The minimum amount of time that must pass between each request to the webhook endpoint

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Maximum amount of time that Stalwart will wait for a response from this webhook

##### discardAfter

[Section titled “discardAfter”](#discardafter)

Type: `Duration` · default: `"5m"`

The duration after which the webhook will be discarded if it cannot be delivered

##### url

[Section titled “url”](#url)

Type: `Uri` · required

URL of the webhook endpoint

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Enable or disable the tracer

##### level

[Section titled “level”](#level)

Type: [`TracingLevel`](#tracinglevel) · default: `"info"`

The logging level for this tracer

##### lossy

[Section titled “lossy”](#lossy)

Type: `Boolean` · default: `false`

Whether to drop log entries if there is backlog

##### events

[Section titled “events”](#events)

Type: [[ref/events|`EventType`]]`[]`

List of events to include or exclude based on filter mode

##### eventsPolicy

[Section titled “eventsPolicy”](#eventspolicy)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the events list

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The WebHook object is available via the `urn:stalwart:jmap` capability.

### x:WebHook/get

[Section titled “x:WebHook/get”](#xwebhookget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysWebHookGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebHook/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:WebHook/set

[Section titled “x:WebHook/set”](#xwebhookset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysWebHookCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebHook/set",          {            "create": {              "new1": {                "events": {},                "httpAuth": {                  "@type": "Unauthenticated"                },                "httpHeaders": {},                "signatureKey": {                  "@type": "None"                },                "url": "https://example.com"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysWebHookUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebHook/set",          {            "update": {              "id1": {                "allowInvalidCerts": false              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysWebHookDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebHook/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:WebHook/query

[Section titled “x:WebHook/query”](#xwebhookquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysWebHookQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:WebHook/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get WebHook id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create WebHook \  --field 'signatureKey={"@type":"None"}' \  --field url=https://example.com \  --field 'httpAuth={"@type":"Unauthenticated"}' \  --field 'httpHeaders={}' \  --field 'events={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query WebHook
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update WebHook id1 --field allowInvalidCerts=false
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete WebHook --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

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

### HttpAuth

[Section titled “HttpAuth”](#httpauth-1)

Defines the HTTP authentication method to use for HTTP requests.

`Unauthenticated`: Anonymous. No additional fields.
`Basic`: Basic Authentication. Carries the fields of [`HttpAuthBasic`](#httpauthbasic).
`Bearer`: Bearer Token. Carries the fields of [`HttpAuthBearer`](#httpauthbearer).

#### HttpAuthBasic

[Section titled “HttpAuthBasic”](#httpauthbasic)

HTTP Basic authentication credentials.

##### username

[Section titled “username”](#username)

Type: `String` · required

Username for HTTP Basic Authentication

##### secret

[Section titled “secret”](#secret-1)

Type: [`SecretKey`](#secretkey) · required

Password for HTTP Basic Authentication

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication

## Enums

[Section titled “Enums”](#enums)

### TracingLevel

[Section titled “TracingLevel”](#tracinglevel)
ValueLabel`error`Error`warn`Warning`info`Info`debug`Debug`trace`Trace

### EventPolicy

[Section titled “EventPolicy”](#eventpolicy)
ValueLabel`include`Only include the specified events`exclude`Exclude the specified events