---
title: "MtaInboundSession"
source: "https://stalw.art/docs/ref/object/mta-inbound-session"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-inbound-session"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaInboundSession"
---
# MtaInboundSession

      

Configures inbound SMTP session timeouts and transfer limits.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Inbound › Session

## Fields

[Section titled “Fields”](#fields)

##### maxDuration

[Section titled “maxDuration”](#maxduration)

Type: [`Expression`](#expression) · default: `{"else":"10m"}`

The maximum duration of a session

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### timeout

[Section titled “timeout”](#timeout)

Type: [`Expression`](#expression) · default: `{"else":"5m"}`

How long to wait for a client to send a command before timing out

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### transferLimit

[Section titled “transferLimit”](#transferlimit)

Type: [`Expression`](#expression) · default: `{"else":"262144000"}`

The maximum number of bytes that can be transferred per session

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaInboundSession singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaInboundSession/get

[Section titled “x:MtaInboundSession/get”](#xmtainboundsessionget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaInboundSessionGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundSession/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaInboundSession/set

[Section titled “x:MtaInboundSession/set”](#xmtainboundsessionset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaInboundSessionUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundSession/set",          {            "update": {              "singleton": {                "maxDuration": {                  "else": "10m"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaInboundSession
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaInboundSession --field maxDuration='{"else":"10m"}'
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

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]] (Variables)