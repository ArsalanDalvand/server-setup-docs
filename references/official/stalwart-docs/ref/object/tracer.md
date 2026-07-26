---
title: "Tracer"
source: "https://stalw.art/docs/ref/object/tracer"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/tracer"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Tracer"
---
# Tracer

      

Defines a logging and tracing output method.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Telemetry › Tracers

## Fields

[Section titled “Fields”](#fields)

Tracer is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Log"

[Section titled “@type: "Log"”](#type-log)

Log file

##### path

[Section titled “path”](#path)

Type: `String` · required

The path to the log file

##### prefix

[Section titled “prefix”](#prefix)

Type: `String` · default: `"stalwart"`

The prefix for the log file

##### rotate

[Section titled “rotate”](#rotate)

Type: [`LogRotateFrequency`](#logrotatefrequency) · default: `"daily"`

The frequency to rotate the log file

##### ansi

[Section titled “ansi”](#ansi)

Type: `Boolean` · default: `true`

Whether to use ANSI colors in logs

##### multiline

[Section titled “multiline”](#multiline)

Type: `Boolean` · default: `false`

Whether to write log entries as a single line or multiline

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

### @type: "Stdout"

[Section titled “@type: "Stdout"”](#type-stdout)

Console

##### buffered

[Section titled “buffered”](#buffered)

Type: `Boolean` · default: `true`

Whether to buffer log entries before writing to console

##### ansi

[Section titled “ansi”](#ansi-1)

Type: `Boolean` · default: `false`

Whether to use ANSI colors in logs

##### multiline

[Section titled “multiline”](#multiline-1)

Type: `Boolean` · default: `false`

Whether to write log entries as a single line or multiline

##### enable

[Section titled “enable”](#enable-1)

Type: `Boolean` · default: `true`

Enable or disable the tracer

##### level

[Section titled “level”](#level-1)

Type: [`TracingLevel`](#tracinglevel) · default: `"info"`

The logging level for this tracer

##### lossy

[Section titled “lossy”](#lossy-1)

Type: `Boolean` · default: `false`

Whether to drop log entries if there is backlog

##### events

[Section titled “events”](#events-1)

Type: [[ref/events|`EventType`]]`[]`

List of events to include or exclude based on filter mode

##### eventsPolicy

[Section titled “eventsPolicy”](#eventspolicy-1)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the events list

### @type: "Journal"

[Section titled “@type: "Journal"”](#type-journal)

Systemd Journal

##### enable

[Section titled “enable”](#enable-2)

Type: `Boolean` · default: `true`

Enable or disable the tracer

##### level

[Section titled “level”](#level-2)

Type: [`TracingLevel`](#tracinglevel) · default: `"info"`

The logging level for this tracer

##### lossy

[Section titled “lossy”](#lossy-2)

Type: `Boolean` · default: `false`

Whether to drop log entries if there is backlog

##### events

[Section titled “events”](#events-2)

Type: [[ref/events|`EventType`]]`[]`

List of events to include or exclude based on filter mode

##### eventsPolicy

[Section titled “eventsPolicy”](#eventspolicy-2)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the events list

### @type: "OtelHttp"

[Section titled “@type: "OtelHttp"”](#type-otelhttp)

Open Telemetry (HTTP)

##### endpoint

[Section titled “endpoint”](#endpoint)

Type: `Uri` · required

The endpoint for Open Telemetry

##### enableLogExporter

[Section titled “enableLogExporter”](#enablelogexporter)

Type: `Boolean` · default: `true`

Whether to export logs to OpenTelemetry

##### enableSpanExporter

[Section titled “enableSpanExporter”](#enablespanexporter)

Type: `Boolean` · default: `true`

Whether to export spans to OpenTelemetry

##### throttle

[Section titled “throttle”](#throttle)

Type: `Duration` · default: `"1s"`

The minimum amount of time that must pass between each request to the OpenTelemetry endpoint

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"10s"`

Maximum amount of time that Stalwart will wait for a response from the OpenTelemetry endpoint

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

##### enable

[Section titled “enable”](#enable-3)

Type: `Boolean` · default: `true`

Enable or disable the tracer

##### level

[Section titled “level”](#level-3)

Type: [`TracingLevel`](#tracinglevel) · default: `"info"`

The logging level for this tracer

##### lossy

[Section titled “lossy”](#lossy-3)

Type: `Boolean` · default: `false`

Whether to drop log entries if there is backlog

##### events

[Section titled “events”](#events-3)

Type: [[ref/events|`EventType`]]`[]`

List of events to include or exclude based on filter mode

##### eventsPolicy

[Section titled “eventsPolicy”](#eventspolicy-3)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the events list

### @type: "OtelGrpc"

[Section titled “@type: "OtelGrpc"”](#type-otelgrpc)

Open Telemetry (gRPC)

##### endpoint

[Section titled “endpoint”](#endpoint-1)

Type: `Uri?`

The endpoint for Open Telemetry

##### enableLogExporter

[Section titled “enableLogExporter”](#enablelogexporter-1)

Type: `Boolean` · default: `true`

Whether to export logs to OpenTelemetry

##### enableSpanExporter

[Section titled “enableSpanExporter”](#enablespanexporter-1)

Type: `Boolean` · default: `true`

Whether to export spans to OpenTelemetry

##### throttle

[Section titled “throttle”](#throttle-1)

Type: `Duration` · default: `"1s"`

The minimum amount of time that must pass between each request to the OpenTelemetry endpoint

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"10s"`

Maximum amount of time that Stalwart will wait for a response from the OpenTelemetry endpoint

##### httpAuth

[Section titled “httpAuth”](#httpauth-1)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders-1)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

##### enable

[Section titled “enable”](#enable-4)

Type: `Boolean` · default: `true`

Enable or disable the tracer

##### level

[Section titled “level”](#level-4)

Type: [`TracingLevel`](#tracinglevel) · default: `"info"`

The logging level for this tracer

##### lossy

[Section titled “lossy”](#lossy-4)

Type: `Boolean` · default: `false`

Whether to drop log entries if there is backlog

##### events

[Section titled “events”](#events-4)

Type: [[ref/events|`EventType`]]`[]`

List of events to include or exclude based on filter mode

##### eventsPolicy

[Section titled “eventsPolicy”](#eventspolicy-4)

Type: [`EventPolicy`](#eventpolicy) · default: `"exclude"`

How to interpret the events list

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Tracer object is available via the `urn:stalwart:jmap` capability.

### x:Tracer/get

[Section titled “x:Tracer/get”](#xtracerget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTracerGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tracer/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Tracer/set

[Section titled “x:Tracer/set”](#xtracerset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTracerCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tracer/set",          {            "create": {              "new1": {                "@type": "Log",                "events": {},                "path": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTracerUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tracer/set",          {            "update": {              "id1": {                "path": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTracerDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tracer/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Tracer/query

[Section titled “x:Tracer/query”](#xtracerquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTracerQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tracer/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Tracer id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Tracer/Log \  --field path=Example \  --field 'events={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Tracer
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Tracer id1 --field path='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Tracer --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### HttpAuth

[Section titled “HttpAuth”](#httpauth-2)

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

[Section titled “secret”](#secret)

Type: [`SecretKey`](#secretkey) · required

Password for HTTP Basic Authentication

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret-1)

Type: `String` · required · secret

Password or secret value

##### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

##### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

#### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication

## Enums

[Section titled “Enums”](#enums)

### LogRotateFrequency

[Section titled “LogRotateFrequency”](#logrotatefrequency)
ValueLabel`daily`Daily`hourly`Hourly`minutely`Minutely`never`Never

### TracingLevel

[Section titled “TracingLevel”](#tracinglevel)
ValueLabel`error`Error`warn`Warning`info`Info`debug`Debug`trace`Trace

### EventPolicy

[Section titled “EventPolicy”](#eventpolicy)
ValueLabel`include`Only include the specified events`exclude`Exclude the specified events