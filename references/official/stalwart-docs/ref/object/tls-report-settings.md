---
title: "TlsReportSettings"
source: "https://stalw.art/docs/ref/object/tls-report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/tls-report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TlsReportSettings"
---
# TlsReportSettings

      

Configures TLS aggregate report generation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › TLS

## Fields

[Section titled “Fields”](#fields)

##### contactInfo

[Section titled “contactInfo”](#contactinfo)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Contact information to be included in the report

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### fromAddress

[Section titled “fromAddress”](#fromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'noreply-tls@' + system('domain')"}`

Email address that will be used in the From header of the TLS aggregate report email

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### fromName

[Section titled “fromName”](#fromname)

Type: [`Expression`](#expression) · default: `{"else":"'Report Subsystem'"}`

Name that will be used in the From header of the TLS aggregate report email

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### maxReportSize

[Section titled “maxReportSize”](#maxreportsize)

Type: [`Expression`](#expression) · default: `{"else":"5242880"}`

Maximum size of the TLS aggregate report in bytes

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### orgName

[Section titled “orgName”](#orgname)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Name of the organization to be included in the report

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### sendFrequency

[Section titled “sendFrequency”](#sendfrequency)

Type: [`Expression`](#expression) · default: `{"else":"daily"}`

Frequency at which the TLS aggregate reports will be sent. The options are hourly, daily, weekly, or disable to disable reporting

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

Available constants: [[ref/expression/constant/mta-aggregate-constant|`MtaAggregateConstant`]].

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the TLS aggregate report

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

##### subject

[Section titled “subject”](#subject)

Type: [`Expression`](#expression) · default: `{"else":"'TLS Aggregate Report'"}`

Subject name that will be used in the TLS aggregate report email

Available variables: [[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The TlsReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:TlsReportSettings/get

[Section titled “x:TlsReportSettings/get”](#xtlsreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysTlsReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TlsReportSettings/set

[Section titled “x:TlsReportSettings/set”](#xtlsreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysTlsReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsReportSettings/set",          {            "update": {              "singleton": {                "contactInfo": {                  "else": "false"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get TlsReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update TlsReportSettings --field contactInfo='{"else":"false"}'
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

[[ref/expression/variable/mta-queue-host-variable|`MtaQueueHostVariable`]] (Variables)
[[ref/expression/constant/mta-aggregate-constant|`MtaAggregateConstant`]] (Constants)