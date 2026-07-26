---
title: "MtaOutboundStrategy"
source: "https://stalw.art/docs/ref/object/mta-outbound-strategy"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-outbound-strategy"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaOutboundStrategy"
---
# MtaOutboundStrategy

      

Configures outbound message delivery routing, scheduling, and TLS strategies.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › Strategy

## Fields

[Section titled “Fields”](#fields)

##### connection

[Section titled “connection”](#connection)

Type: [`Expression`](#expression) · default: `{"else":"'default'"}`

An expression that returns the connection strategy to use when delivering messages to remote SMTP servers

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### route

[Section titled “route”](#route)

Type: [`Expression`](#expression) · default: `{"else":"'mx'","match":[{"if":"is_local_domain(rcpt_domain)","then":"'local'"}]}`

An expression that returns the route name to use when delivering queued messages

Available variables: [[ref/expression/variable/mta-queue-rcpt-variable|`MtaQueueRcptVariable`]].

##### schedule

[Section titled “schedule”](#schedule)

Type: [`Expression`](#expression) · default: `{"else":"'remote'","match":[{"if":"is_local_domain(rcpt_domain)","then":"'local'"},{"if":"source == 'dsn'","then":"'dsn'"},{"if":"source == 'report'","then":"'report'"}]}`

An expression that returns the scheduling strategy to use when queueing messages

Available variables: [[ref/expression/variable/mta-queue-rcpt-variable|`MtaQueueRcptVariable`]].

##### tls

[Section titled “tls”](#tls)

Type: [`Expression`](#expression) · default: `{"else":"'default'","match":[{"if":"retry_num > 0 && last_error == 'tls'","then":"'invalid-tls'"}]}`

An expression that returns the TLS strategy to use when delivering messages to remote SMTP servers

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaOutboundStrategy singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaOutboundStrategy/get

[Section titled “x:MtaOutboundStrategy/get”](#xmtaoutboundstrategyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaOutboundStrategyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundStrategy/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaOutboundStrategy/set

[Section titled “x:MtaOutboundStrategy/set”](#xmtaoutboundstrategyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaOutboundStrategyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaOutboundStrategy/set",          {            "update": {              "singleton": {                "connection": {                  "else": "'default'"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaOutboundStrategy
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaOutboundStrategy --field connection='{"else":"'\''default'\''"}'
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

[[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]] (Variables)
[[ref/expression/variable/mta-queue-rcpt-variable|`MtaQueueRcptVariable`]] (Variables)