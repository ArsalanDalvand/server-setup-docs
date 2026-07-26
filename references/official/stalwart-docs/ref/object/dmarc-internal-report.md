---
title: "DmarcInternalReport"
source: "https://stalw.art/docs/ref/object/dmarc-internal-report"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dmarc-internal-report"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DmarcInternalReport"
---
# DmarcInternalReport

      

Stores an outbound DMARC aggregate report pending delivery.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Reports › Outbox › DMARC

## Fields

[Section titled “Fields”](#fields)

##### rua

[Section titled “rua”](#rua)

Type: `EmailAddress[]`

Reporting email addresses from the DMARC policy

##### policyIdentifier

[Section titled “policyIdentifier”](#policyidentifier)

Type: `UnsignedInt` · default: `0`

Identifier for the DMARC policy that generated this report

##### report

[Section titled “report”](#report)

Type: [`DmarcReport`](#dmarcreport) · required

DMARC report content

##### domain

[Section titled “domain”](#domain)

Type: `DomainName` · required

Domain this report is associated with

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · required

When the report was created

##### deliverAt

[Section titled “deliverAt”](#deliverat)

Type: `UTCDateTime` · required

When the report is scheduled to be delivered

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DmarcInternalReport object is available via the `urn:stalwart:jmap` capability.

### x:DmarcInternalReport/get

[Section titled “x:DmarcInternalReport/get”](#xdmarcinternalreportget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysDmarcInternalReportGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcInternalReport/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DmarcInternalReport/set

[Section titled “x:DmarcInternalReport/set”](#xdmarcinternalreportset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysDmarcInternalReportCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcInternalReport/set",          {            "create": {              "new1": {                "createdAt": "2026-01-01T00:00:00Z",                "deliverAt": "2026-01-01T00:00:00Z",                "domain": "example.com",                "report": {                  "dateRangeBegin": "2026-01-01T00:00:00Z",                  "dateRangeEnd": "2026-01-01T00:00:00Z",                  "email": "[email protected]",                  "errors": {},                  "extensions": {},                  "orgName": "Example",                  "policyAdkim": "relaxed",                  "policyAspf": "relaxed",                  "policyDisposition": "none",                  "policyDomain": "Example",                  "policyFailureReportingOptions": {},                  "policySubdomainDisposition": "none",                  "records": {},                  "reportId": "Example"                },                "rua": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysDmarcInternalReportUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcInternalReport/set",          {            "update": {              "id1": {                "rua": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysDmarcInternalReportDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcInternalReport/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DmarcInternalReport/query

[Section titled “x:DmarcInternalReport/query”](#xdmarcinternalreportquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysDmarcInternalReportQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DmarcInternalReport/query",          {            "filter": {              "domain": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:DmarcInternalReport/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`domain`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DmarcInternalReport id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create DmarcInternalReport \  --field 'rua={}' \  --field 'report={"dateRangeBegin":"2026-01-01T00:00:00Z","dateRangeEnd":"2026-01-01T00:00:00Z","email":"[email protected]","errors":{},"extensions":{},"orgName":"Example","policyAdkim":"relaxed","policyAspf":"relaxed","policyDisposition":"none","policyDomain":"Example","policyFailureReportingOptions":{},"policySubdomainDisposition":"none","records":{},"reportId":"Example"}' \  --field domain=example.com \  --field createdAt=2026-01-01T00:00:00Z \  --field deliverAt=2026-01-01T00:00:00Z
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query DmarcInternalReportstalwart-cli query DmarcInternalReport --where domain=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update DmarcInternalReport id1 --field rua='{}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete DmarcInternalReport --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### DmarcReport

[Section titled “DmarcReport”](#dmarcreport)

Content of a DMARC aggregate report.

##### version

[Section titled “version”](#version)

Type: `Float` · default: `1.0`

DMARC report format version

##### orgName

[Section titled “orgName”](#orgname)

Type: `String` · required

Name of the organization that generated the report

##### email

[Section titled “email”](#email)

Type: `EmailAddress` · required

Contact email address of the reporting organization

##### extraContactInfo

[Section titled “extraContactInfo”](#extracontactinfo)

Type: `String?`

Additional contact information for the reporting organization

##### reportId

[Section titled “reportId”](#reportid)

Type: `String` · required

Unique identifier for this report

##### dateRangeBegin

[Section titled “dateRangeBegin”](#daterangebegin)

Type: `UTCDateTime` · required

Start of the reporting period

##### dateRangeEnd

[Section titled “dateRangeEnd”](#daterangeend)

Type: `UTCDateTime` · required

End of the reporting period

##### errors

[Section titled “errors”](#errors)

Type: `String[]`

Errors encountered during report generation

##### policyDomain

[Section titled “policyDomain”](#policydomain)

Type: `String` · required

Domain for which the DMARC policy is published

##### policyVersion

[Section titled “policyVersion”](#policyversion)

Type: `String?`

Version of the published DMARC policy

##### policyAdkim

[Section titled “policyAdkim”](#policyadkim)

Type: [`DmarcAlignment`](#dmarcalignment) · required

DKIM alignment mode specified in the policy

##### policyAspf

[Section titled “policyAspf”](#policyaspf)

Type: [`DmarcAlignment`](#dmarcalignment) · required

SPF alignment mode specified in the policy

##### policyDisposition

[Section titled “policyDisposition”](#policydisposition)

Type: [`DmarcDisposition`](#dmarcdisposition) · required

Requested handling policy for failing messages

##### policySubdomainDisposition

[Section titled “policySubdomainDisposition”](#policysubdomaindisposition)

Type: [`DmarcDisposition`](#dmarcdisposition) · required

Requested handling policy for failing messages from subdomains

##### policyTestingMode

[Section titled “policyTestingMode”](#policytestingmode)

Type: `Boolean` · default: `false`

Whether the policy is in testing mode

##### policyFailureReportingOptions

[Section titled “policyFailureReportingOptions”](#policyfailurereportingoptions)

Type: [`FailureReportingOption`](#failurereportingoption)`[]`

Conditions under which failure reports should be generated

##### records

[Section titled “records”](#records)

Type: [`DmarcReportRecord`](#dmarcreportrecord)`[]`

Aggregated authentication results grouped by source

##### extensions

[Section titled “extensions”](#extensions)

Type: [`DmarcExtension`](#dmarcextension)`[]`

Custom vendor-specific extensions to the report

##### generator

[Section titled “generator”](#generator)

Type: `String?`

Name and version of the software that generated the report

##### policyNp

[Section titled “policyNp”](#policynp)

Type: [`DmarcDisposition`](#dmarcdisposition) · default: `"unspecified"`

Requested handling policy for failing messages from non-existent subdomains

##### policyDiscoveryMethod

[Section titled “policyDiscoveryMethod”](#policydiscoverymethod)

Type: [`DmarcDiscovery`](#dmarcdiscovery) · default: `"unspecified"`

Method used to discover the DMARC policy record

#### DmarcReportRecord

[Section titled “DmarcReportRecord”](#dmarcreportrecord)

An aggregated authentication result record from a single source.

##### sourceIp

[Section titled “sourceIp”](#sourceip)

Type: `IpAddr?`

IP address of the sending mail server

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0`

Number of messages from this source matching this result

##### evaluatedDisposition

[Section titled “evaluatedDisposition”](#evaluateddisposition)

Type: [`DmarcActionDisposition`](#dmarcactiondisposition) · required

Action taken on the messages

##### evaluatedDkim

[Section titled “evaluatedDkim”](#evaluateddkim)

Type: [`DmarcResult`](#dmarcresult) · required

DMARC result based on DKIM authentication

##### evaluatedSpf

[Section titled “evaluatedSpf”](#evaluatedspf)

Type: [`DmarcResult`](#dmarcresult) · required

DMARC result based on SPF authentication

##### policyOverrideReasons

[Section titled “policyOverrideReasons”](#policyoverridereasons)

Type: [`DmarcPolicyOverrideReason`](#dmarcpolicyoverridereason)`[]`

Reasons why the evaluated disposition differs from the published policy

##### envelopeTo

[Section titled “envelopeTo”](#envelopeto)

Type: `String?`

Envelope recipient domain

##### envelopeFrom

[Section titled “envelopeFrom”](#envelopefrom)

Type: `String` · required

Envelope sender domain (MAIL FROM)

##### headerFrom

[Section titled “headerFrom”](#headerfrom)

Type: `String` · required

Domain from the message From header

##### dkimResults

[Section titled “dkimResults”](#dkimresults)

Type: [`DmarcDkimResult`](#dmarcdkimresult)`[]`

DKIM authentication results for the messages

##### spfResults

[Section titled “spfResults”](#spfresults)

Type: [`DmarcSpfResult`](#dmarcspfresult)`[]`

SPF authentication results for the messages

##### extensions

[Section titled “extensions”](#extensions-1)

Type: [`DmarcExtension`](#dmarcextension)`[]`

Custom vendor-specific extensions to this record

##### DmarcPolicyOverrideReason

[Section titled “DmarcPolicyOverrideReason”](#dmarcpolicyoverridereason)

Reason for a DMARC policy override.

##### overrideType

[Section titled “overrideType”](#overridetype)

Type: [`DmarcPolicyOverride`](#dmarcpolicyoverride) · required

Type of policy override applied

##### comment

[Section titled “comment”](#comment)

Type: `String?`

Additional explanation for the override

##### DmarcDkimResult

[Section titled “DmarcDkimResult”](#dmarcdkimresult)

DKIM authentication result within a DMARC report record.

##### domain

[Section titled “domain”](#domain-1)

Type: `DomainName` · required

Domain that signed the message

##### selector

[Section titled “selector”](#selector)

Type: `String` · required

DKIM selector used for signing

##### result

[Section titled “result”](#result)

Type: [`DkimAuthResult`](#dkimauthresult) · required

DKIM verification result

##### humanResult

[Section titled “humanResult”](#humanresult)

Type: `String?`

Human-readable explanation of the result

##### DmarcSpfResult

[Section titled “DmarcSpfResult”](#dmarcspfresult)

SPF authentication result within a DMARC report record.

##### domain

[Section titled “domain”](#domain-2)

Type: `DomainName` · required

Domain checked for SPF

##### scope

[Section titled “scope”](#scope)

Type: [`SpfDomainScope`](#spfdomainscope) · required

Which identity was checked

##### result

[Section titled “result”](#result-1)

Type: [`SpfAuthResult`](#spfauthresult) · required

SPF verification result

##### humanResult

[Section titled “humanResult”](#humanresult-1)

Type: `String?`

Human-readable explanation of the result

##### DmarcExtension

[Section titled “DmarcExtension”](#dmarcextension)

A vendor-specific extension in a DMARC report.

##### name

[Section titled “name”](#name)

Type: `String` · required

Extension identifier

##### definition

[Section titled “definition”](#definition)

Type: `String` · required

Extension content or value

## Enums

[Section titled “Enums”](#enums)

### DmarcAlignment

[Section titled “DmarcAlignment”](#dmarcalignment)
ValueLabel`relaxed`Organizational domain match is sufficient`strict`Exact domain match is required`unspecified`Alignment mode not specified

### DmarcDisposition

[Section titled “DmarcDisposition”](#dmarcdisposition)
ValueLabel`none`No specific action requested`quarantine`Treat failing messages as suspicious`reject`Reject failing messages`unspecified`Disposition not specified

### FailureReportingOption

[Section titled “FailureReportingOption”](#failurereportingoption)
ValueLabel`all`Generate report if all authentication mechanisms fail`any`Generate report if any authentication mechanism fails`dkimFailure`Generate report if DKIM authentication fails`spfFailure`Generate report if SPF authentication fails

### DmarcActionDisposition

[Section titled “DmarcActionDisposition”](#dmarcactiondisposition)
ValueLabel`none`No action taken`pass`Message passed evaluation`quarantine`Message was quarantined`reject`Message was rejected`unspecified`Disposition not specified

### DmarcResult

[Section titled “DmarcResult”](#dmarcresult)
ValueLabel`pass`Authentication passed`fail`Authentication failed`unspecified`Result not specified

### DmarcPolicyOverride

[Section titled “DmarcPolicyOverride”](#dmarcpolicyoverride)
ValueLabel`Forwarded`Message was forwarded`SampledOut`Message was excluded by policy sampling (pct)`TrustedForwarder`Message came from a trusted forwarder`MailingList`Message came from a mailing list`LocalPolicy`Local policy override was applied`Other`Other reason for override`PolicyTestMode`Message exempted because the DMARC policy is in test mode

### DkimAuthResult

[Section titled “DkimAuthResult”](#dkimauthresult)
ValueLabel`none`No DKIM signature present`pass`DKIM signature verified successfully`fail`DKIM signature verification failed`policy`DKIM signature not accepted due to policy`neutral`DKIM verification returned neutral`tempError`Temporary error during verification`permError`Permanent error in DKIM record or signature

### SpfDomainScope

[Section titled “SpfDomainScope”](#spfdomainscope)
ValueLabel`helo`SPF check performed on HELO/EHLO identity`mailFrom`SPF check performed on MAIL FROM identity`unspecified`Scope not specified

### SpfAuthResult

[Section titled “SpfAuthResult”](#spfauthresult)
ValueLabel`none`No SPF record found`neutral`SPF record returned neutral`pass`SPF check passed`fail`SPF check failed (hard fail)`softFail`SPF check returned soft fail`tempError`Temporary error during SPF check`permError`Permanent error in SPF record

### DmarcDiscovery

[Section titled “DmarcDiscovery”](#dmarcdiscovery)
ValueLabel`psl`Public Suffix List method (RFC 7489)`treewalk`DNS Tree Walk method (RFC 9989)`unspecified`Discovery method not specified