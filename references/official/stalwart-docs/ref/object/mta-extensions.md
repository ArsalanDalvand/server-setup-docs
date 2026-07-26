---
title: "MtaExtensions"
source: "https://stalw.art/docs/ref/object/mta-extensions"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-extensions"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaExtensions"
---
# MtaExtensions

      

Configures SMTP protocol extensions offered to clients.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Session › Extensions

## Fields

[Section titled “Fields”](#fields)

##### chunking

[Section titled “chunking”](#chunking)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Enables chunking (RFC 1830), an extension that allows large messages to be transferred in chunks which may reduce the load on the network and server.

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### deliverBy

[Section titled “deliverBy”](#deliverby)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"15d"}]}`

Specifies the maximum delivery time for a message using the DELIVERBY (RFC 2852) extension, which allows the sender to request a specific delivery time for a message

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### dsn

[Section titled “dsn”](#dsn)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"true"}]}`

Enables delivery status notifications (RFC 3461), which allows the sender to request a delivery status notification (DSN) from the recipient’s mail server

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### expn

[Section titled “expn”](#expn)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"true"}]}`

Specifies whether to enable the EXPN command, which allows the sender to request the membership of a mailing list. It is recommended to disable this command to prevent spammers from harvesting email addresses

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### futureRelease

[Section titled “futureRelease”](#futurerelease)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"7d"}]}`

Specifies the maximum time that a message can be held for delivery using the FUTURERELEASE (RFC 4865) extension

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### mtPriority

[Section titled “mtPriority”](#mtpriority)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"mixer"}]}`

Specifies the priority assignment policy to advertise on the MT-PRIORITY (RFC 6710) extension, which allows the sender to specify a priority for a message. Available policies are mixer, stanag4406 and nsep, or false to disable this extension

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

Available constants: [[ref/expression/constant/mta-priority-constant|`MtaPriorityConstant`]].

##### noSoliciting

[Section titled “noSoliciting”](#nosoliciting)

Type: [`Expression`](#expression) · default: `{"else":"''"}`

Specifies the text to include in the NOSOLICITING (RFC 3865) message, which indicates that the server does not accept unsolicited commercial email (UCE or spam)

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### pipelining

[Section titled “pipelining”](#pipelining)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Enables SMTP pipelining (RFC 2920), which enables multiple commands to be sent in a single request to speed up communication between the client and server

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### requireTls

[Section titled “requireTls”](#requiretls)

Type: [`Expression`](#expression) · default: `{"else":"true"}`

Enables require TLS (RFC 8689), an extension that allows clients to require TLS encryption for the SMTP session

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### vrfy

[Section titled “vrfy”](#vrfy)

Type: [`Expression`](#expression) · default: `{"else":"false","match":[{"if":"!is_empty(authenticated_as)","then":"true"}]}`

Specifies whether to enable the VRFY command, which allows the sender to verify the existence of a mailbox. It is recommended to disable this command to prevent spammers from harvesting email addresses

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaExtensions singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaExtensions/get

[Section titled “x:MtaExtensions/get”](#xmtaextensionsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaExtensionsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaExtensions/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaExtensions/set

[Section titled “x:MtaExtensions/set”](#xmtaextensionsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaExtensionsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaExtensions/set",          {            "update": {              "singleton": {                "chunking": {                  "else": "true"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaExtensions
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaExtensions --field chunking='{"else":"true"}'
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
[[ref/expression/constant/mta-priority-constant|`MtaPriorityConstant`]] (Constants)