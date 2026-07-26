---
title: "MtaStageConnect"
source: "https://stalw.art/docs/ref/object/mta-stage-connect"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-stage-connect"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaStageConnect"
---
# MtaStageConnect

      

Configures SMTP connection greeting and hostname settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › Connect Stage

## Fields

[Section titled “Fields”](#fields)

##### smtpGreeting

[Section titled “smtpGreeting”](#smtpgreeting)

Type: [`Expression`](#expression) · default: `{"else":"system('hostname') + ' Stalwart ESMTP at your service'"}`

The greeting message sent by the SMTP/LMTP server

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### hostname

[Section titled “hostname”](#hostname)

Type: [`Expression`](#expression) · default: `{"else":"system('hostname')"}`

The SMTP server hostname

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### script

[Section titled “script”](#script)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Which Sieve script to run when a client connects

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaStageConnect singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaStageConnect/get

[Section titled “x:MtaStageConnect/get”](#xmtastageconnectget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStageConnectGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageConnect/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaStageConnect/set

[Section titled “x:MtaStageConnect/set”](#xmtastageconnectset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStageConnectUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageConnect/set",          {            "update": {              "singleton": {                "smtpGreeting": {                  "else": "system('hostname') + ' Stalwart ESMTP at your service'"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaStageConnect
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaStageConnect --field smtpGreeting='{"else":"system('\''hostname'\'') + '\'' Stalwart ESMTP at your service'\''"}'
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