---
title: "MtaMilter"
source: "https://stalw.art/docs/ref/object/mta-milter"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-milter"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaMilter"
---
# MtaMilter

      

Defines a Milter filter endpoint for message processing.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Filters › Milters

## Fields

[Section titled “Fields”](#fields)

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether Stalwart should connect to a Milter filter server that has an invalid TLS certificate

##### enable

[Section titled “enable”](#enable)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Expression that determines whether to enable this milter

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### hostname

[Section titled “hostname”](#hostname)

Type: `String` · required

Hostname or IP address of the server where the Milter filter is running

##### maxResponseSize

[Section titled “maxResponseSize”](#maxresponsesize)

Type: `Size` · default: `52428800`

Maximum size, in bytes, of a response that Stalwart will accept from this Milter server

##### tempFailOnError

[Section titled “tempFailOnError”](#tempfailonerror)

Type: `Boolean` · default: `true`

Whether to respond with a temporary failure (typically a 4xx SMTP status code) when Stalwart encounters an error while communicating with this Milter server

##### protocolVersion

[Section titled “protocolVersion”](#protocolversion)

Type: [`MilterVersion`](#milterversion) · default: `"v6"`

Version of the Milter protocol that Stalwart should use when communicating with the Milter server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `11332` · max: 65535 · min: 1

Network port on the Milter filter host server

##### stages

[Section titled “stages”](#stages)

Type: [`MtaStage`](#mtastage)`[]` · default: `["data"]` · min items: 1

Which SMTP stages to run the milter on

##### timeoutCommand

[Section titled “timeoutCommand”](#timeoutcommand)

Type: `Duration` · default: `"30s"`

How long Stalwart will wait to send a command to the Milter server

##### timeoutConnect

[Section titled “timeoutConnect”](#timeoutconnect)

Type: `Duration` · default: `"30s"`

Maximum amount of time that Stalwart will wait to establish a connection with this Milter server

##### timeoutData

[Section titled “timeoutData”](#timeoutdata)

Type: `Duration` · default: `"60s"`

Maximum amount of time Stalwart will wait for a response from the Milter server

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `false`

Whether to use Transport Layer Security (TLS) for the connection between Stalwart and the Milter filter

##### flagsAction

[Section titled “flagsAction”](#flagsaction)

Type: `UnsignedInt?`

Optional flags to set on the Milter connection. See the Milter protocol documentation for details on available flags.

##### flagsProtocol

[Section titled “flagsProtocol”](#flagsprotocol)

Type: `UnsignedInt?`

Optional protocol flags to set on the Milter connection. See the Milter protocol documentation for details on available protocol flags.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaMilter object is available via the `urn:stalwart:jmap` capability.

### x:MtaMilter/get

[Section titled “x:MtaMilter/get”](#xmtamilterget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaMilterGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaMilter/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaMilter/set

[Section titled “x:MtaMilter/set”](#xmtamilterset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaMilterCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaMilter/set",          {            "create": {              "new1": {                "hostname": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaMilterUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaMilter/set",          {            "update": {              "id1": {                "hostname": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaMilterDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaMilter/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaMilter/query

[Section titled “x:MtaMilter/query”](#xmtamilterquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaMilterQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaMilter/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaMilter id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaMilter \  --field hostname=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaMilter
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaMilter id1 --field hostname='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaMilter --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Expression

[Section titled “Expression”](#expression)

A conditional expression with match rules and a default value.

##### match

[Section titled “match”](#match)

Type: [`ExpressionMatch`](#expressionmatch)`[]`

List of conditions and their corresponding results

##### else

[Section titled “else”](#else)

Type: `String` · required

Else condition

#### ExpressionMatch

[Section titled “ExpressionMatch”](#expressionmatch)

A single condition-result pair in an expression.

##### if

[Section titled “if”](#if)

Type: `String` · required

If condition

##### then

[Section titled “then”](#then)

Type: `String` · required

Then clause

## Enums

[Section titled “Enums”](#enums)

### MilterVersion

[Section titled “MilterVersion”](#milterversion)
ValueLabel`v2`Version 2`v6`Version 6

### MtaStage

[Section titled “MtaStage”](#mtastage)
ValueLabel`connect`Connect`ehlo`EHLO`auth`AUTH`mail`MAIL FROM`rcpt`RCPT TO`data`DATA

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]] (Variables)