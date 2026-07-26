---
title: "MtaStageAuth"
source: "https://stalw.art/docs/ref/object/mta-stage-auth"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-stage-auth"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaStageAuth"
---
# MtaStageAuth

      

Configures SMTP authentication requirements and error handling.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › AUTH Stage

## Fields

[Section titled “Fields”](#fields)

##### maxFailures

[Section titled “maxFailures”](#maxfailures)

Type: [`Expression`](#expression) · default: `{"else":"3"}`

Maximum number of authentication errors allowed before the session is disconnected

Available variables: [[ref/expression/variable/mta-ehlo-variable|`MtaEhloVariable`]].

##### waitOnFail

[Section titled “waitOnFail”](#waitonfail)

Type: [`Expression`](#expression) · default: `{"else":"5s"}`

Time interval to wait after an authentication failure

Available variables: [[ref/expression/variable/mta-ehlo-variable|`MtaEhloVariable`]].

##### saslMechanisms

[Section titled “saslMechanisms”](#saslmechanisms)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"local_port != 25 && is_tls","then":"[plain, login, oauthbearer, xoauth2]"},{"if":"local_port != 25","then":"[oauthbearer, xoauth2]"}]}`

A list of SASL authentication mechanisms offered to clients, or an empty list to disable authentication. Stalwart supports PLAIN, LOGIN, and OAUTHBEARER mechanisms

Available variables: [[ref/expression/variable/mta-ehlo-variable|`MtaEhloVariable`]].

Available constants: [[ref/expression/constant/mta-auth-type-constant|`MtaAuthTypeConstant`]].

##### mustMatchSender

[Section titled “mustMatchSender”](#mustmatchsender)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Specifies whether the authenticated user or any of their associated e-mail addresses must match the sender of the email message

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### require

[Section titled “require”](#require)

Type: [`Expression`](#expression) · default: `{"else":"local_port != 25"}`

Specifies whether authentication is necessary to send email messages

Available variables: [[ref/expression/variable/mta-ehlo-variable|`MtaEhloVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaStageAuth singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaStageAuth/get

[Section titled “x:MtaStageAuth/get”](#xmtastageauthget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStageAuthGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageAuth/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaStageAuth/set

[Section titled “x:MtaStageAuth/set”](#xmtastageauthset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStageAuthUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaStageAuth/set",          {            "update": {              "singleton": {                "maxFailures": {                  "else": "3"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaStageAuth
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaStageAuth --field maxFailures='{"else":"3"}'
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

[[ref/expression/variable/mta-ehlo-variable|`MtaEhloVariable`]] (Variables)
[[ref/expression/constant/mta-auth-type-constant|`MtaAuthTypeConstant`]] (Constants)
[[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]] (Variables)