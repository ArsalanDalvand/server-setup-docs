---
title: "Trace"
source: "https://stalw.art/docs/ref/object/trace"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/trace"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Trace"
---
# Trace

      

Stores a message delivery trace with associated events.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Emails › History › Inbound Delivery Management ›  Emails › History › Outbound Delivery

## Fields

[Section titled “Fields”](#fields)

##### events

[Section titled “events”](#events)

Type: [`TraceEvent`](#traceevent)`[]`

List of events associated with the trace entry

##### timestamp

[Section titled “timestamp”](#timestamp)

Type: `UTCDateTime` · server-set

Timestamp of the trace entry

##### from

[Section titled “from”](#from)

Type: `String` · server-set

Sender address

##### to

[Section titled “to”](#to)

Type: `String` · server-set

Recipient addresses

##### size

[Section titled “size”](#size)

Type: `Size` · server-set

Size of the message

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Trace object is available via the `urn:stalwart:jmap` capability.

### x:Trace/get

[Section titled “x:Trace/get”](#xtraceget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTraceGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Trace/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Trace/set

[Section titled “x:Trace/set”](#xtraceset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTraceCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Trace/set",          {            "create": {              "new1": {                "events": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTraceUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Trace/set",          {            "update": {              "id1": {                "events": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTraceDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Trace/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Trace/query

[Section titled “x:Trace/query”](#xtracequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTraceQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Trace/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Trace/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`timestamp`date`queueId`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Trace id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Trace \  --field 'events={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Trace
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Trace id1 --field events='{}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Trace --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### TraceEvent

[Section titled “TraceEvent”](#traceevent)

A single event within a delivery trace.

##### event

[Section titled “event”](#event)

Type: [[ref/events|`EventType`]] · required

Event type

##### timestamp

[Section titled “timestamp”](#timestamp-1)

Type: `UTCDateTime` · required

Timestamp when the event occurred

##### keyValues

[Section titled “keyValues”](#keyvalues)

Type: [`TraceKeyValue`](#tracekeyvalue)`[]`

List of key-value pairs associated with the trace entry

#### TraceKeyValue

[Section titled “TraceKeyValue”](#tracekeyvalue)

A key-value pair associated with a trace event.

##### key

[Section titled “key”](#key)

Type: [[ref/events#keys|`Key`]] · required

Key name

##### value

[Section titled “value”](#value)

Type: [`TraceValue`](#tracevalue) · required

Key value

##### TraceValue

[Section titled “TraceValue”](#tracevalue)

A typed value in a trace key-value pair.

`String`: String value. Carries the fields of [`TraceValueString`](#tracevaluestring).
`UnsignedInt`: Unsigned integer value. Carries the fields of [`TraceValueUnsignedInt`](#tracevalueunsignedint).
`Integer`: Integer value. Carries the fields of [`TraceValueInteger`](#tracevalueinteger).
`Boolean`: Boolean value. Carries the fields of [`TraceValueBoolean`](#tracevalueboolean).
`Float`: Float value. Carries the fields of [`TraceValueFloat`](#tracevaluefloat).
`UTCDateTime`: UTC date and time value. Carries the fields of [`TraceValueUTCDateTime`](#tracevalueutcdatetime).
`Duration`: Duration value in seconds. Carries the fields of [`TraceValueDuration`](#tracevalueduration).
`IpAddr`: IP address value. Carries the fields of [`TraceValueIpAddr`](#tracevalueipaddr).
`List`: Value list. Carries the fields of [`TraceValueList`](#tracevaluelist).
`Event`: Nested event value. Carries the fields of [`TraceValueEvent`](#tracevalueevent).
`Null`: Null value. No additional fields.

##### TraceValueString

[Section titled “TraceValueString”](#tracevaluestring)

A string trace value.

##### value

[Section titled “value”](#value-1)

Type: `String` · required

String value

##### TraceValueUnsignedInt

[Section titled “TraceValueUnsignedInt”](#tracevalueunsignedint)

An unsigned integer trace value.

##### value

[Section titled “value”](#value-2)

Type: `UnsignedInt` · default: `0`

Unsigned integer value

##### TraceValueInteger

[Section titled “TraceValueInteger”](#tracevalueinteger)

An integer trace value.

##### value

[Section titled “value”](#value-3)

Type: `Integer` · default: `0`

Integer value

##### TraceValueBoolean

[Section titled “TraceValueBoolean”](#tracevalueboolean)

A boolean trace value.

##### value

[Section titled “value”](#value-4)

Type: `Boolean` · default: `false`

Boolean value

##### TraceValueFloat

[Section titled “TraceValueFloat”](#tracevaluefloat)

A floating-point trace value.

##### value

[Section titled “value”](#value-5)

Type: `Float` · default: `0`

Float value

##### TraceValueUTCDateTime

[Section titled “TraceValueUTCDateTime”](#tracevalueutcdatetime)

A UTC date-time trace value.

##### value

[Section titled “value”](#value-6)

Type: `UTCDateTime` · required

UTC date and time value

##### TraceValueDuration

[Section titled “TraceValueDuration”](#tracevalueduration)

A duration trace value in seconds.

##### value

[Section titled “value”](#value-7)

Type: `UnsignedInt` · default: `0`

Duration value in seconds

##### TraceValueIpAddr

[Section titled “TraceValueIpAddr”](#tracevalueipaddr)

An IP address trace value.

##### value

[Section titled “value”](#value-8)

Type: `IpAddr` · required

IP address value

##### TraceValueList

[Section titled “TraceValueList”](#tracevaluelist)

A list of trace values.

##### value

[Section titled “value”](#value-9)

Type: [`TraceValue`](#tracevalue)`[]`

Value list

##### TraceValueEvent

[Section titled “TraceValueEvent”](#tracevalueevent)

A nested event trace value.

##### event

[Section titled “event”](#event-1)

Type: [[ref/events|`EventType`]] · required

Event type

##### value

[Section titled “value”](#value-10)

Type: [`TraceKeyValue`](#tracekeyvalue)`[]`

List of key-value pairs associated with the trace entry