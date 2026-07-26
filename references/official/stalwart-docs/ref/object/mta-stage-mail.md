---
title: "MtaStageMail"
source: "https://stalw.art/docs/ref/object/mta-stage-mail"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-stage-mail"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaStageMail"
---
# MtaStageMail

      

Configures MAIL FROM stage processing and sender validation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › MAIL FROM Stage

## Fields

[Section titled “Fields”](#fields)

##### isSenderAllowed

[Section titled “isSenderAllowed”](#issenderallowed)

Type: [`Expression`](#expression) · default: `{"else":"!is_empty(authenticated_as) || !key_exists('spam-block', sender_domain)","match":[]}`

Expression that returns true when the sender is allowed to send

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### rewrite

[Section titled “rewrite”](#rewrite)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Expression to rewrite the sender address

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### script

[Section titled “script”](#script)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Which Sieve script to run after the client sends a MAIL command

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaStageMail singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaStageMail/get

[Section titled “x:MtaStageMail/get”](#xmtastagemailget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStageMailGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageMail/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaStageMail/set

[Section titled “x:MtaStageMail/set”](#xmtastagemailset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStageMailUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageMail/set",          {            "update": {              "singleton": {                "isSenderAllowed": {                  "else": "!is_empty(authenticated_as) || !key_exists('spam-block', sender_domain)",                  "match": []                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaStageMail
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaStageMail --field isSenderAllowed='{"else":"!is_empty(authenticated_as) || !key_exists('\''spam-block'\'', sender_domain)","match":[]}'
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

[[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]] (Variables)