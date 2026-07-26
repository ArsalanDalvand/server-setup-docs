---
title: "ReportSettings"
source: "https://stalw.art/docs/ref/object/report-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/report-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ReportSettings"
---
# ReportSettings

      

Configures inbound report analysis and outbound report settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Reports › General

## Fields

[Section titled “Fields”](#fields)

##### inboundReportAddresses

[Section titled “inboundReportAddresses”](#inboundreportaddresses)

Type: `String[]` · default: `["postmaster@*"]`

List of addresses (which may include wildcards) from which reports will be intercepted and analyzed

##### inboundReportForwarding

[Section titled “inboundReportForwarding”](#inboundreportforwarding)

Type: `Boolean` · default: `true`

Whether reports should be forwarded to their final recipient after analysis

##### outboundReportDomain

[Section titled “outboundReportDomain”](#outboundreportdomain)

Type: `DomainName?`

The default domain name used for DSNs and other reports. If left empty, the default domain will be used.

##### outboundReportSubmitter

[Section titled “outboundReportSubmitter”](#outboundreportsubmitter)

Type: [`Expression`](#expression) · default: `{"else":"system('hostname')"}`

Report submitter address or leave empty to use the default hostname

Available variables: [[ref/expression/variable/mta-rcpt-domain-variable|`MtaRcptDomainVariable`]].

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ReportSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:ReportSettings/get

[Section titled “x:ReportSettings/get”](#xreportsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysReportSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ReportSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ReportSettings/set

[Section titled “x:ReportSettings/set”](#xreportsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysReportSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ReportSettings/set",          {            "update": {              "singleton": {                "inboundReportAddresses": {                  "postmaster@*": true                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ReportSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update ReportSettings --field inboundReportAddresses='{"postmaster@*":true}'
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