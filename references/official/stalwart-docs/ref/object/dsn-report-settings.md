---
title: "DsnReportSettings"
source: "https://stalw.art/docs/ref/object/dsn-report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dsn-report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DsnReportSettings"
---
# DsnReportSettings

      

Configures Delivery Status Notification (DSN) report generation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › DSN

## Fields

[Section titled “Fields”](#fields)

##### fromAddress

[Section titled “fromAddress”](#fromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'MAILER-DAEMON@' + system('domain')"}`

Email address that will be used in the From header of Delivery Status Notifications (DSN) reports

Available variables: [[ref/expression/variable/mta-queue-sender-variable|`MtaQueueSenderVariable`]].

##### fromName

[Section titled “fromName”](#fromname)

Type: [`Expression`](#expression) · default: `{"else":"'Mail Delivery Subsystem'"}`

Name that will be used in the From header of Delivery Status Notifications (DSN) reports

Available variables: [[ref/expression/variable/mta-queue-sender-variable|`MtaQueueSenderVariable`]].

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the Delivery Status Notifications

Available variables: [[ref/expression/variable/mta-queue-sender-variable|`MtaQueueSenderVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DsnReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:DsnReportSettings/get

[Section titled “x:DsnReportSettings/get”](#xdsnreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysDsnReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DsnReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DsnReportSettings/set

[Section titled “x:DsnReportSettings/set”](#xdsnreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysDsnReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DsnReportSettings/set",          {            "update": {              "singleton": {                "fromAddress": {                  "else": "'MAILER-DAEMON@' + system('domain')"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DsnReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update DsnReportSettings --field fromAddress='{"else":"'\''MAILER-DAEMON@'\'' + system('\''domain'\'')"}'
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

[[ref/expression/variable/mta-queue-sender-variable|`MtaQueueSenderVariable`]] (Variables)