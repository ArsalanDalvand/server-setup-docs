---
title: "SpfReportSettings"
source: "https://stalw.art/docs/ref/object/spf-report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spf-report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpfReportSettings"
---
# SpfReportSettings

      

Configures SPF authentication failure report generation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › SPF

## Fields

[Section titled “Fields”](#fields)

##### fromAddress

[Section titled “fromAddress”](#fromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'noreply-spf@' + system('domain')"}`

Email address that will be used in the From header of the SPF authentication failure report email

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### fromName

[Section titled “fromName”](#fromname)

Type: [`Expression`](#expression) · default: `{"else":"'Report Subsystem'"}`

Name that will be used in the From header of the SPF authentication failure report email

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### sendFrequency

[Section titled “sendFrequency”](#sendfrequency)

Type: [`Expression`](#expression) · default: `{"else":"[1, 1d]"}`

Rate at which SPF reports will be sent to a given email address. When this rate is exceeded, no further SPF failure reports will be sent to that address

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### dkimSignDomain

[Section titled “dkimSignDomain”](#dkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the SPF authentication failure report

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

##### subject

[Section titled “subject”](#subject)

Type: [`Expression`](#expression) · default: `{"else":"'SPF Authentication Failure Report'"}`

Subject name that will be used in the SPF authentication failure report email

Available variables: [[ref/expression/variable/mta-mail-from-variable|`MtaMailFromVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpfReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:SpfReportSettings/get

[Section titled “x:SpfReportSettings/get”](#xspfreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpfReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpfReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpfReportSettings/set

[Section titled “x:SpfReportSettings/set”](#xspfreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpfReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpfReportSettings/set",          {            "update": {              "singleton": {                "fromAddress": {                  "else": "'noreply-spf@' + system('domain')"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpfReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpfReportSettings --field fromAddress='{"else":"'\''noreply-spf@'\'' + system('\''domain'\'')"}'
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