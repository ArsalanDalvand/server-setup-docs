---
title: "EventTracingLevel"
source: "https://stalw.art/docs/ref/object/event-tracing-level"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/event-tracing-level"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "EventTracingLevel"
---
# EventTracingLevel

      

Defines a custom logging level override for a specific event type.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Telemetry › Event Levels

## Fields

[Section titled “Fields”](#fields)

##### event

[Section titled “event”](#event)

Type: [[ref/events|`EventType`]] · read-only

Unique identifier of the event

##### level

[Section titled “level”](#level)

Type: [`TracingLevelOpt`](#tracinglevelopt) · default: `"info"`

The logging level for this event

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The EventTracingLevel object is available via the `urn:stalwart:jmap` capability.

### x:EventTracingLevel/get

[Section titled “x:EventTracingLevel/get”](#xeventtracinglevelget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysEventTracingLevelGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:EventTracingLevel/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:EventTracingLevel/set

[Section titled “x:EventTracingLevel/set”](#xeventtracinglevelset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysEventTracingLevelCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:EventTracingLevel/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysEventTracingLevelUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:EventTracingLevel/set",          {            "update": {              "id1": {                "level": "info"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysEventTracingLevelDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:EventTracingLevel/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:EventTracingLevel/query

[Section titled “x:EventTracingLevel/query”](#xeventtracinglevelquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysEventTracingLevelQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:EventTracingLevel/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get EventTracingLevel id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create EventTracingLevel
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query EventTracingLevel
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update EventTracingLevel id1 --field level=info
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete EventTracingLevel --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### TracingLevelOpt

[Section titled “TracingLevelOpt”](#tracinglevelopt)
ValueLabel`disable`Disabled`error`Error - Only errors are logged`warn`Warning - Errors and warnings are logged`info`Info - Errors, warnings and info are logged`debug`Debug - Errors, warnings, info and debug are logged`trace`Trace - Errors, warnings, info, debug and trace are logged