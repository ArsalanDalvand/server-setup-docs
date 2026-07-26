---
title: "DkimReportSettings"
source: "https://stalw.art/docs/ref/object/dkim-report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dkim-report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DkimReportSettings"
---
# DkimReportSettings

      

Configures DKIM authentication failure report generation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › DKIM

## Fields

[Section titled “Fields”](#fields)

##### fromAddress

[Section titled “fromAddress”](#fromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'noreply-dkim@' + system('domain')"}`

Email address that will be used in the From header of the DKIM report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### fromName

[Section titled “fromName”](#fromname)

Type: [`Expression`](#expression) · default: `{"else":"'Report Subsystem'"}`

Name that will be used in the From header of the DKIM report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### sendFrequency

[Section titled “sendFrequency”](#sendfrequency)

Type: [`Expression`](#expression) · default: `{"else":"[1, 1d]"}`

Rate at which DKIM reports will be sent to a given email address. When this rate is exceeded, no further DKIM failure reports will be sent to that address

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the DKIM report

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### subject

[Section titled “subject”](#subject)

Type: [`Expression`](#expression) · default: `{"else":"'DKIM Authentication Failure Report'"}`

Subject name that will be used in the DKIM report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DkimReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:DkimReportSettings/get

[Section titled “x:DkimReportSettings/get”](#xdkimreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysDkimReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DkimReportSettings/set

[Section titled “x:DkimReportSettings/set”](#xdkimreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysDkimReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimReportSettings/set",          {            "update": {              "singleton": {                "fromAddress": {                  "else": "'noreply-dkim@' + system('domain')"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DkimReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update DkimReportSettings --field fromAddress='{"else":"'\''noreply-dkim@'\'' + system('\''domain'\'')"}'
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