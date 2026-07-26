---
title: "MtaInboundThrottle"
source: "https://stalw.art/docs/ref/object/mta-inbound-throttle"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-inbound-throttle"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaInboundThrottle"
---
# MtaInboundThrottle

      

Defines an inbound rate limit rule for SMTP connections.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Rates & Quotas › Inbound Rate Limits

## Fields

[Section titled “Fields”](#fields)

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this throttle

##### description

[Section titled “description”](#description)

Type: `String` · read-only

Short description for the throttle

##### key

[Section titled “key”](#key)

Type: [`MtaInboundThrottleKey`](#mtainboundthrottlekey)`[]`

Optional list of context variables that determine where this throttle should be applied

##### match

[Section titled “match”](#match)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Enable the imposition of concurrency and rate limits only when a specific condition is met

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### rate

[Section titled “rate”](#rate)

Type: [`Rate`](#rate) · required

Number of incoming requests over a period of time that the rate limiter will allow

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaInboundThrottle object is available via the `urn:stalwart:jmap` capability.

### x:MtaInboundThrottle/get

[Section titled “x:MtaInboundThrottle/get”](#xmtainboundthrottleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaInboundThrottleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundThrottle/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaInboundThrottle/set

[Section titled “x:MtaInboundThrottle/set”](#xmtainboundthrottleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaInboundThrottleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundThrottle/set",          {            "create": {              "new1": {                "key": {},                "rate": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaInboundThrottleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundThrottle/set",          {            "update": {              "id1": {                "enable": true              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaInboundThrottleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundThrottle/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaInboundThrottle/query

[Section titled “x:MtaInboundThrottle/query”](#xmtainboundthrottlequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaInboundThrottleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaInboundThrottle/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaInboundThrottle id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaInboundThrottle \  --field 'key={}' \  --field 'rate={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaInboundThrottle
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaInboundThrottle id1 --field enable=true
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaInboundThrottle --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Expression

[Section titled “Expression”](#expression)

A conditional expression with match rules and a default value.

##### match

[Section titled “match”](#match-1)

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

### Rate

[Section titled “Rate”](#rate-1)

Defines a rate limit as a count over a time period.

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0` · min: 1 · max: 1000000

Count

##### period

[Section titled “period”](#period)

Type: `Duration` · default: `"0s"` · min: 1

Period

## Enums

[Section titled “Enums”](#enums)

### MtaInboundThrottleKey

[Section titled “MtaInboundThrottleKey”](#mtainboundthrottlekey)
ValueLabel`listener`Listener`remoteIp`Remote IP`localIp`Local IP`authenticatedAs`Authenticated As`heloDomain`EHLO Domain`sender`Sender`senderDomain`Sender Domain`rcpt`Recipient`rcptDomain`Recipient Domain

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]] (Variables)