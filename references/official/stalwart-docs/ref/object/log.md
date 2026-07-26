---
title: "Log"
source: "https://stalw.art/docs/ref/object/log"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/log"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Log"
---
# Log

      

Represents a server log entry.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Observability › Logs

## Fields

[Section titled “Fields”](#fields)

##### timestamp

[Section titled “timestamp”](#timestamp)

Type: `UTCDateTime` · required

Timestamp of the log entry

##### level

[Section titled “level”](#level)

Type: [`TracingLevel`](#tracinglevel) · required

Severity level of the log entry

##### event

[Section titled “event”](#event)

Type: [[ref/events|`EventType`]] · required

Event type of the log entry

##### details

[Section titled “details”](#details)

Type: `Text` · required

Log message

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Log object is available via the `urn:stalwart:jmap` capability.

### x:Log/get

[Section titled “x:Log/get”](#xlogget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysLogGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Log/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Log/set

[Section titled “x:Log/set”](#xlogset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysLogCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Log/set",          {            "create": {              "new1": {                "details": "Example",                "level": "error",                "timestamp": "2026-01-01T00:00:00Z"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysLogUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Log/set",          {            "update": {              "id1": {                "details": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysLogDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Log/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Log/query

[Section titled “x:Log/query”](#xlogquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysLogQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Log/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Log/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Log id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Log \  --field timestamp=2026-01-01T00:00:00Z \  --field level=error \  --field details=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Logstalwart-cli query Log --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Log id1 --field details='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Log --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### TracingLevel

[Section titled “TracingLevel”](#tracinglevel)
ValueLabel`error`Error`warn`Warning`info`Info`debug`Debug`trace`Trace