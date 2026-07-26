---
title: "SenderAuth"
source: "https://stalw.art/docs/ref/object/sender-auth"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sender-auth"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SenderAuth"
---
# SenderAuth

      

Configures sender authentication verification including DKIM, SPF, DMARC, and ARC.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Inbound › Sender Authentication

## Fields

[Section titled “Fields”](#fields)

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"is_local_domain(sender_domain) && !is_empty(authenticated_as)","then":"sender_domain"}]}`

Domain to use for DKIM signing

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### dkimStrict

[Section titled “dkimStrict”](#dkimstrict)

Type: `Boolean` · default: `true`

Whether to ignore insecure DKIM signatures such as those containing a length parameter

##### dkimVerify

[Section titled “dkimVerify”](#dkimverify)

Type: [`Expression`](#expression) · default: `{"else":"relaxed","match":[]}`

Whether DKIM verification is strict, relaxed or disabled

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

##### spfEhloVerify

[Section titled “spfEhloVerify”](#spfehloverify)

Type: [`Expression`](#expression) · default: `{"else":"disable","match":[{"if":"local_port == 25","then":"relaxed"}]}`

Whether SPF EHLO verification is strict, relaxed or disabled

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

##### spfFromVerify

[Section titled “spfFromVerify”](#spffromverify)

Type: [`Expression`](#expression) · default: `{"else":"disable","match":[{"if":"local_port == 25","then":"relaxed"}]}`

Whether SPF MAIL FROM verification is strict, relaxed or disabled

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

##### arcVerify

[Section titled “arcVerify”](#arcverify)

Type: [`Expression`](#expression) · default: `{"else":"disable","match":[]}`

Whether ARC verification is strict, relaxed or disabled.

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

##### dmarcVerify

[Section titled “dmarcVerify”](#dmarcverify)

Type: [`Expression`](#expression) · default: `{"else":"disable","match":[{"if":"local_port == 25","then":"relaxed"}]}`

Whether DMARC verification is strict, relaxed or disabled

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

##### reverseIpVerify

[Section titled “reverseIpVerify”](#reverseipverify)

Type: [`Expression`](#expression) · default: `{"else":"disable","match":[{"if":"local_port == 25","then":"relaxed"}]}`

How strict to be when verifying the reverse DNS of the client IP

Available variables: [[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]].

Available constants: [[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SenderAuth singleton is available via the `urn:stalwart:jmap` capability.

### x:SenderAuth/get

[Section titled “x:SenderAuth/get”](#xsenderauthget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSenderAuthGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SenderAuth/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SenderAuth/set

[Section titled “x:SenderAuth/set”](#xsenderauthset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSenderAuthUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SenderAuth/set",          {            "update": {              "singleton": {                "dkimSignDomain": {                  "else": "false",                  "match": [                    {                      "if": "is_local_domain(sender_domain) && !is_empty(authenticated_as)",                      "then": "sender_domain"                    }                  ]                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SenderAuth
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SenderAuth --field dkimSignDomain='{"else":"false","match":[{"if":"is_local_domain(sender_domain) && !is_empty(authenticated_as)","then":"sender_domain"}]}'
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
[[ref/expression/constant/mta-verify-constant|`MtaVerifyConstant`]] (Constants)
[[ref/expression/variable/mta-connection-variable|`MtaConnectionVariable`]] (Variables)