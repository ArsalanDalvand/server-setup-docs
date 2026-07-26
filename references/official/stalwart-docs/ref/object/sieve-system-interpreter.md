---
title: "SieveSystemInterpreter"
source: "https://stalw.art/docs/ref/object/sieve-system-interpreter"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sieve-system-interpreter"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SieveSystemInterpreter"
---
# SieveSystemInterpreter

      

Configures the system-level Sieve script interpreter settings and limits.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Sieve › System Interpreter

## Fields

[Section titled “Fields”](#fields)

##### defaultFromAddress

[Section titled “defaultFromAddress”](#defaultfromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'MAILER-DAEMON@' + system('domain')"}`

Default email address to use for the from field in email notifications sent from a Sieve script

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### defaultFromName

[Section titled “defaultFromName”](#defaultfromname)

Type: [`Expression`](#expression) · default: `{"else":"'Automated Message'"}`

Default name to use for the from field in email notifications sent from a Sieve script

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### messageIdHostname

[Section titled “messageIdHostname”](#messageidhostname)

Type: `String?`

Override the default local hostname to use when generating a Message-Id header

##### duplicateExpiry

[Section titled “duplicateExpiry”](#duplicateexpiry)

Type: `Duration` · default: `"7d"`

Default expiration time for IDs stored by the duplicate extension from trusted scripts

##### noCapabilityCheck

[Section titled “noCapabilityCheck”](#nocapabilitycheck)

Type: `Boolean` · default: `true`

If enabled, language extensions can be used without being explicitly declared using the require statement

##### defaultReturnPath

[Section titled “defaultReturnPath”](#defaultreturnpath)

Type: [`Expression`](#expression)

Default return path to use in email notifications sent from a Sieve script

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the email notifications sent from a Sieve script

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### maxCpuCycles

[Section titled “maxCpuCycles”](#maxcpucycles)

Type: `UnsignedInt` · default: `1048576` · min: 1

Maximum number CPU cycles a script can use

##### maxNestedIncludes

[Section titled “maxNestedIncludes”](#maxnestedincludes)

Type: `UnsignedInt` · default: `5` · min: 1

Maximum number of nested includes

##### maxOutMessages

[Section titled “maxOutMessages”](#maxoutmessages)

Type: `UnsignedInt` · default: `5`

Maximum number of outgoing messages

##### maxReceivedHeaders

[Section titled “maxReceivedHeaders”](#maxreceivedheaders)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of received headers

##### maxRedirects

[Section titled “maxRedirects”](#maxredirects)

Type: `UnsignedInt` · default: `3`

Maximum number of redirects

##### maxVarSize

[Section titled “maxVarSize”](#maxvarsize)

Type: `UnsignedInt` · default: `52428800` · min: 1

Maximum size of a variable

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SieveSystemInterpreter singleton is available via the `urn:stalwart:jmap` capability.

### x:SieveSystemInterpreter/get

[Section titled “x:SieveSystemInterpreter/get”](#xsievesysteminterpreterget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSieveSystemInterpreterGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemInterpreter/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveSystemInterpreter/set

[Section titled “x:SieveSystemInterpreter/set”](#xsievesysteminterpreterset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSieveSystemInterpreterUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemInterpreter/set",          {            "update": {              "singleton": {                "messageIdHostname": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SieveSystemInterpreter
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SieveSystemInterpreter --field messageIdHostname='updated value'
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