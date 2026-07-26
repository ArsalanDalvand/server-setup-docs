---
title: "ArfExternalReport"
source: "https://stalw.art/docs/ref/object/arf-external-report"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/arf-external-report"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ArfExternalReport"
---
# ArfExternalReport

      

Stores an ARF feedback report received from an external source.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Reports › Inbox › ARF

## Fields

[Section titled “Fields”](#fields)

##### report

[Section titled “report”](#report)

Type: [`ArfFeedbackReport`](#arffeedbackreport) · required

Parsed ARF feedback report content

##### from

[Section titled “from”](#from)

Type: `EmailAddress` · required

Email address of the report sender

##### subject

[Section titled “subject”](#subject)

Type: `String` · required

Subject line of the report email

##### to

[Section titled “to”](#to)

Type: `EmailAddress[]`

List of recipient email addresses

##### receivedAt

[Section titled “receivedAt”](#receivedat)

Type: `UTCDateTime` · required

When the report email was received

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime` · required

When the report is scheduled to be deleted

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this report belongs to

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ArfExternalReport object is available via the `urn:stalwart:jmap` capability.

### x:ArfExternalReport/get

[Section titled “x:ArfExternalReport/get”](#xarfexternalreportget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysArfExternalReportGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArfExternalReport/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ArfExternalReport/set

[Section titled “x:ArfExternalReport/set”](#xarfexternalreportset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysArfExternalReportCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArfExternalReport/set",          {            "create": {              "new1": {                "expiresAt": "2026-01-01T00:00:00Z",                "from": "[email protected]",                "receivedAt": "2026-01-01T00:00:00Z",                "report": {                  "authFailure": "adsp",                  "authenticationResults": {},                  "deliveryResult": "delivered",                  "feedbackType": "abuse",                  "identityAlignment": "none",                  "reportedDomains": {},                  "reportedUris": {}                },                "subject": "Example",                "to": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysArfExternalReportUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArfExternalReport/set",          {            "update": {              "id1": {                "subject": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysArfExternalReportDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArfExternalReport/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ArfExternalReport/query

[Section titled “x:ArfExternalReport/query”](#xarfexternalreportquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysArfExternalReportQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ArfExternalReport/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ArfExternalReport id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create ArfExternalReport \  --field 'report={"authFailure":"adsp","authenticationResults":{},"deliveryResult":"delivered","feedbackType":"abuse","identityAlignment":"none","reportedDomains":{},"reportedUris":{}}' \  --field [email protected] \  --field subject=Example \  --field 'to={}' \  --field receivedAt=2026-01-01T00:00:00Z \  --field expiresAt=2026-01-01T00:00:00Z
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query ArfExternalReport
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update ArfExternalReport id1 --field subject='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete ArfExternalReport --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### ArfFeedbackReport

[Section titled “ArfFeedbackReport”](#arffeedbackreport)

Parsed content of an ARF feedback report.

##### feedbackType

[Section titled “feedbackType”](#feedbacktype)

Type: [`ArfFeedbackType`](#arffeedbacktype) · required

Type of feedback being reported

##### arrivalDate

[Section titled “arrivalDate”](#arrivaldate)

Type: `UTCDateTime?`

When the original message arrived

##### authenticationResults

[Section titled “authenticationResults”](#authenticationresults)

Type: `String[]`

Authentication-Results header values from the original message

##### incidents

[Section titled “incidents”](#incidents)

Type: `UnsignedInt` · default: `0`

Number of incidents represented by this report

##### originalEnvelopeId

[Section titled “originalEnvelopeId”](#originalenvelopeid)

Type: `String?`

Original SMTP envelope ID (ENVID)

##### originalMailFrom

[Section titled “originalMailFrom”](#originalmailfrom)

Type: `EmailAddress?`

Original envelope sender address (MAIL FROM)

##### originalRcptTo

[Section titled “originalRcptTo”](#originalrcptto)

Type: `EmailAddress?`

Original envelope recipient address (RCPT TO)

##### reportedDomains

[Section titled “reportedDomains”](#reporteddomains)

Type: `DomainName[]`

Domains being reported

##### reportedUris

[Section titled “reportedUris”](#reporteduris)

Type: `Uri[]`

URIs being reported

##### reportingMta

[Section titled “reportingMta”](#reportingmta)

Type: `String?`

Hostname of the MTA generating this report

##### sourceIp

[Section titled “sourceIp”](#sourceip)

Type: `IpAddr?`

IP address of the original message source

##### sourcePort

[Section titled “sourcePort”](#sourceport)

Type: `UnsignedInt?` · min: 1 · max: 65535

Port of the original message source

##### userAgent

[Section titled “userAgent”](#useragent)

Type: `String?`

Software that generated this report

##### version

[Section titled “version”](#version)

Type: `UnsignedInt` · default: `1`

ARF format version

##### authFailure

[Section titled “authFailure”](#authfailure)

Type: [`ArfAuthFailureType`](#arfauthfailuretype) · required

Type of authentication failure (for auth-failure reports)

##### deliveryResult

[Section titled “deliveryResult”](#deliveryresult)

Type: [`ArfDeliveryResult`](#arfdeliveryresult) · required

What happened to the original message

##### dkimAdspDns

[Section titled “dkimAdspDns”](#dkimadspdns)

Type: `String?`

DKIM ADSP DNS record content

##### dkimCanonicalizedBody

[Section titled “dkimCanonicalizedBody”](#dkimcanonicalizedbody)

Type: `String?`

Message body after DKIM canonicalization

##### dkimCanonicalizedHeader

[Section titled “dkimCanonicalizedHeader”](#dkimcanonicalizedheader)

Type: `String?`

Message headers after DKIM canonicalization

##### dkimDomain

[Section titled “dkimDomain”](#dkimdomain)

Type: `String?`

Domain from the DKIM signature

##### dkimIdentity

[Section titled “dkimIdentity”](#dkimidentity)

Type: `String?`

Identity from the DKIM signature (i= tag)

##### dkimSelector

[Section titled “dkimSelector”](#dkimselector)

Type: `String?`

Selector from the DKIM signature

##### dkimSelectorDns

[Section titled “dkimSelectorDns”](#dkimselectordns)

Type: `String?`

DKIM selector DNS record content

##### spfDns

[Section titled “spfDns”](#spfdns)

Type: `String?`

SPF DNS record content

##### identityAlignment

[Section titled “identityAlignment”](#identityalignment)

Type: [`ArfIdentityAlignment`](#arfidentityalignment) · required

Which identities were aligned

##### message

[Section titled “message”](#message)

Type: `String?`

Original message content that triggered the report

##### headers

[Section titled “headers”](#headers)

Type: `String?`

Original message headers that triggered the report

## Enums

[Section titled “Enums”](#enums)

### ArfFeedbackType

[Section titled “ArfFeedbackType”](#arffeedbacktype)
ValueLabel`abuse`Message was reported as abusive or unwanted`authFailure`Message failed authentication checks`fraud`Message was reported as fraudulent`notSpam`Message was incorrectly classified as spam`virus`Message contained a virus`other`Other feedback type

### ArfAuthFailureType

[Section titled “ArfAuthFailureType”](#arfauthfailuretype)
ValueLabel`adsp`DKIM ADSP policy failure`bodyHash`DKIM body hash verification failed`revoked`DKIM key has been revoked`signature`DKIM signature verification failed`spf`SPF authentication failed`dmarc`DMARC authentication failed`unspecified`Authentication failure type not specified

### ArfDeliveryResult

[Section titled “ArfDeliveryResult”](#arfdeliveryresult)
ValueLabel`delivered`Message was delivered to recipient`spam`Message was delivered to spam folder`policy`Message was handled according to policy`reject`Message was rejected`other`Other delivery result`unspecified`Delivery result not specified

### ArfIdentityAlignment

[Section titled “ArfIdentityAlignment”](#arfidentityalignment)
ValueLabel`none`No identity alignment`spf`SPF identity aligned`dkim`DKIM identity aligned`dkimSpf`Both DKIM and SPF identities aligned`unspecified`Identity alignment not specified