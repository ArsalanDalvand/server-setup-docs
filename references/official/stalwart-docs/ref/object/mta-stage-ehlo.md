---
title: "MtaStageEhlo"
source: "https://stalw.art/docs/ref/object/mta-stage-ehlo"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-stage-ehlo"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaStageEhlo"
---
# MtaStageEhlo

      

Configures EHLO command requirements and validation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › EHLO Stage

## Fields

[Section titled “Fields”](#fields)

##### rejectNonFqdn

[Section titled “rejectNonFqdn”](#rejectnonfqdn)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}`

Whether to reject EHLO commands that do not include a fully-qualified domain name as a parameter

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### require

[Section titled “require”](#require)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Whether the remote client must send an EHLO command before starting an SMTP transaction

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

##### script

[Section titled “script”](#script)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Which Sieve script to run after the client sends an EHLO command

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaStageEhlo singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaStageEhlo/get

[Section titled “x:MtaStageEhlo/get”](#xmtastageehloget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStageEhloGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageEhlo/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaStageEhlo/set

[Section titled “x:MtaStageEhlo/set”](#xmtastageehloset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStageEhloUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageEhlo/set",          {            "update": {              "singleton": {                "rejectNonFqdn": {                  "else": "false",                  "match": [                    {                      "if": "local_port == 25",                      "then": "true"                    }                  ]                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaStageEhlo
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaStageEhlo --field rejectNonFqdn='{"else":"false","match":[{"if":"local_port == 25","then":"true"}]}'
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