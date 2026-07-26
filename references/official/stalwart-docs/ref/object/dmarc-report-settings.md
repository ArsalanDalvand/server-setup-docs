---
title: "DmarcReportSettings"
source: "https://stalw.art/docs/ref/object/dmarc-report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dmarc-report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DmarcReportSettings"
---
# DmarcReportSettings

      

Configures DMARC aggregate and failure report generation.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › DMARC

## Fields

[Section titled “Fields”](#fields)

##### aggregateContactInfo

[Section titled “aggregateContactInfo”](#aggregatecontactinfo)

Type: [`Expression`](#expression) · default: `{"else":"false"}`

Contact information to be included in the report

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateFromAddress

[Section titled “aggregateFromAddress”](#aggregatefromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'noreply-dmarc@' + system('domain')"}`

Email address that will be used in the From header of the DMARC aggregate report email

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateFromName

[Section titled “aggregateFromName”](#aggregatefromname)

Type: [`Expression`](#expression) · default: `{"else":"'Report Subsystem'"}`

Name that will be used in the From header of the DMARC aggregate report email

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateMaxReportSize

[Section titled “aggregateMaxReportSize”](#aggregatemaxreportsize)

Type: [`Expression`](#expression) · default: `{"else":"5242880"}`

Maximum size of the DMARC aggregate report in bytes

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateOrgName

[Section titled “aggregateOrgName”](#aggregateorgname)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Name of the organization to be included in the report

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateSendFrequency

[Section titled “aggregateSendFrequency”](#aggregatesendfrequency)

Type: [`Expression`](#expression) · default: `{"else":"daily"}`

Frequency at which the DMARC aggregate reports will be sent. The options are hourly, daily, weekly, or disable to disable reporting

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

Available constants: [[ref/expression/constant/mta-aggregate-constant|`MtaAggregateConstant`]].

##### aggregateDkimSignDomain

[Section titled “aggregateDkimSignDomain”](#aggregatedkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the DMARC aggregate report

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### aggregateSubject

[Section titled “aggregateSubject”](#aggregatesubject)

Type: [`Expression`](#expression) · default: `{"else":"'DMARC Aggregate Report'"}`

Subject name that will be used in the DMARC aggregate report email

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

##### failureFromAddress

[Section titled “failureFromAddress”](#failurefromaddress)

Type: [`Expression`](#expression) · default: `{"else":"'noreply-dmarc@' + system('domain')"}`

Email address that will be used in the From header of the DMARC authentication failure report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### failureFromName

[Section titled “failureFromName”](#failurefromname)

Type: [`Expression`](#expression) · default: `{"else":"'Report Subsystem'"}`

Name that will be used in the From header of the DMARC report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### failureSendFrequency

[Section titled “failureSendFrequency”](#failuresendfrequency)

Type: [`Expression`](#expression) · default: `{"else":"[1, 1d]"}`

Rate at which DMARC reports will be sent to a given email address. When this rate is exceeded, no further DMARC failure reports will be sent to that address

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### failureDkimSignDomain

[Section titled “failureDkimSignDomain”](#failuredkimsigndomain)

Type: [`Expression`](#expression) · default: `{"else":"system('domain')"}`

Which domain’s DKIM signatures to use when signing the DMARC authentication failure report

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

##### failureSubject

[Section titled “failureSubject”](#failuresubject)

Type: [`Expression`](#expression) · default: `{"else":"'DMARC Authentication Failure Report'"}`

Subject name that will be used in the DMARC authentication failure report email

Available variables: [[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DmarcReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:DmarcReportSettings/get

[Section titled “x:DmarcReportSettings/get”](#xdmarcreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysDmarcReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DmarcReportSettings/set

[Section titled “x:DmarcReportSettings/set”](#xdmarcreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysDmarcReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcReportSettings/set",          {            "update": {              "singleton": {                "aggregateContactInfo": {                  "else": "false"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DmarcReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update DmarcReportSettings --field aggregateContactInfo='{"else":"false"}'
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

[[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]] (Variables)
[[ref/expression/constant/mta-aggregate-constant|`MtaAggregateConstant`]] (Constants)
[[ref/expression/variable/mta-rcpt-to-variable|`MtaRcptToVariable`]] (Variables)