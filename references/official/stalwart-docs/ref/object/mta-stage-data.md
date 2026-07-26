---
title: "MtaStageData"
source: "https://stalw.art/docs/ref/object/mta-stage-data"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-stage-data"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaStageData"
---
# MtaStageData

      

Configures message processing rules for the SMTP DATA stage.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › DATA Stage

## Fields

[Section titled “Fields”](#fields)

##### addAuthResultsHeader

[Section titled “addAuthResultsHeader”](#addauthresultsheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add an Authentication-Results header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### addDateHeader

[Section titled “addDateHeader”](#adddateheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add a Date header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### addDeliveredToHeader

[Section titled “addDeliveredToHeader”](#adddeliveredtoheader)

Type: `Boolean` · default: `true`

Whether to add a Delivered-To header to the message

##### addMessageIdHeader

[Section titled “addMessageIdHeader”](#addmessageidheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add a Message-Id header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### addReceivedHeader

[Section titled “addReceivedHeader”](#addreceivedheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add a Received header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### addReceivedSpfHeader

[Section titled “addReceivedSpfHeader”](#addreceivedspfheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add a Received-SPF header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### addReturnPathHeader

[Section titled “addReturnPathHeader”](#addreturnpathheader)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to add a Return-Path header to the message

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### maxMessages

[Section titled “maxMessages”](#maxmessages)

Type: [`Expression`](#expression) · default: `{"else":"10"}`

Maximum number of messages that can be submitted per SMTP session

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### maxReceivedHeaders

[Section titled “maxReceivedHeaders”](#maxreceivedheaders)

Type: [`Expression`](#expression) · default: `{"else":"50"}`

Maximum limit on the number of Received headers, which helps to prevent message loops

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### maxMessageSize

[Section titled “maxMessageSize”](#maxmessagesize)

Type: [`Expression`](#expression) · default: `{"else":"104857600"}`

Maximum size of a message in bytes

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### script

[Section titled “script”](#script)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Which Sieve script to run after the client sends a DATA command

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### enableSpamFilter

[Section titled “enableSpamFilter”](#enablespamfilter)

Type: [`Expression`](#expression) · default: `{"else":"is_empty(authenticated_as)"}`

Whether to enable the spam filter for incoming messages

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaStageData singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaStageData/get

[Section titled “x:MtaStageData/get”](#xmtastagedataget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStageDataGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageData/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaStageData/set

[Section titled “x:MtaStageData/set”](#xmtastagedataset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStageDataUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageData/set",          {            "update": {              "singleton": {                "addAuthResultsHeader": {                  "else": "false",                  "match": [                    {                      "if": "local_port == 25",                      "then": "true"                    }                  ]                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaStageData
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaStageData --field addAuthResultsHeader='{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}'
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

[[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]] (Variables)