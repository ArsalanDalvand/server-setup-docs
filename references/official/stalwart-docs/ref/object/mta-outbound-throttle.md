---
title: "MtaOutboundThrottle"
source: "https://stalw.art/docs/ref/object/mta-outbound-throttle"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-outbound-throttle"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaOutboundThrottle"
---
# MtaOutboundThrottle

      

Defines an outbound rate limit rule for message delivery.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Rates & Quotas › Outbound Rate Limits

## Fields

[Section titled “Fields”](#fields)

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable this throttle

##### description

[Section titled “description”](#description)

Type: `String` · required

Short description for the throttle

##### key

[Section titled “key”](#key)

Type: [`MtaOutboundThrottleKey`](#mtaoutboundthrottlekey)`[]`

Optional list of context variables that determine where this throttle should be applied

##### match

[Section titled “match”](#match)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Enable the imposition of concurrency and rate limits only when a specific condition is met

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### rate

[Section titled “rate”](#rate)

Type: [`Rate`](#rate) · required

Number of incoming requests over a period of time that the rate limiter will allow

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaOutboundThrottle object is available via the `urn:stalwart:jmap` capability.

### x:MtaOutboundThrottle/get

[Section titled “x:MtaOutboundThrottle/get”](#xmtaoutboundthrottleget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaOutboundThrottleGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundThrottle/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaOutboundThrottle/set

[Section titled “x:MtaOutboundThrottle/set”](#xmtaoutboundthrottleset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaOutboundThrottleCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundThrottle/set",          {            "create": {              "new1": {                "description": "Example",                "key": {},                "rate": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaOutboundThrottleUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundThrottle/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaOutboundThrottleDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundThrottle/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaOutboundThrottle/query

[Section titled “x:MtaOutboundThrottle/query”](#xmtaoutboundthrottlequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaOutboundThrottleQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundThrottle/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaOutboundThrottle id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaOutboundThrottle \  --field description=Example \  --field 'key={}' \  --field 'rate={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaOutboundThrottle
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaOutboundThrottle id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaOutboundThrottle --ids id1
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

### MtaOutboundThrottleKey

[Section titled “MtaOutboundThrottleKey”](#mtaoutboundthrottlekey)
ValueLabel`mx`MX Host`remoteIp`Remote IP`localIp`Local IP`sender`Sender`senderDomain`Sender Domain`rcptDomain`Recipient Domain

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]] (Variables)