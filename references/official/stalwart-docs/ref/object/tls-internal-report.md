---
title: "TlsInternalReport"
source: "https://stalw.art/docs/ref/object/tls-internal-report"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/tls-internal-report"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TlsInternalReport"
---
# TlsInternalReport

      

Stores an outbound TLS aggregate report pending delivery.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Reports › Outbox › TLS

## Fields

[Section titled “Fields”](#fields)

##### policyIdentifiers

[Section titled “policyIdentifiers”](#policyidentifiers)

Type: `UnsignedInt[]`

Identifiers for the TLS policies that generated this report

##### mailRua

[Section titled “mailRua”](#mailrua)

Type: `EmailAddress[]`

Reporting email addresses from the TLS policy

##### httpRua

[Section titled “httpRua”](#httprua)

Type: `Uri[]`

Reporting URIs from the TLS policy

##### report

[Section titled “report”](#report)

Type: [`TlsReport`](#tlsreport) · required

TLS report content

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

The TlsInternalReport object is available via the `urn:stalwart:jmap` capability.

### x:TlsInternalReport/get

[Section titled “x:TlsInternalReport/get”](#xtlsinternalreportget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTlsInternalReportGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsInternalReport/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TlsInternalReport/set

[Section titled “x:TlsInternalReport/set”](#xtlsinternalreportset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTlsInternalReportCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsInternalReport/set",          {            "create": {              "new1": {                "createdAt": "2026-01-01T00:00:00Z",                "deliverAt": "2026-01-01T00:00:00Z",                "domain": "example.com",                "httpRua": {},                "mailRua": {},                "policyIdentifiers": {},                "report": {                  "dateRangeEnd": "2026-01-01T00:00:00Z",                  "dateRangeStart": "2026-01-01T00:00:00Z",                  "policies": {},                  "reportId": "Example"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTlsInternalReportUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsInternalReport/set",          {            "update": {              "id1": {                "policyIdentifiers": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTlsInternalReportDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsInternalReport/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TlsInternalReport/query

[Section titled “x:TlsInternalReport/query”](#xtlsinternalreportquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTlsInternalReportQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsInternalReport/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get TlsInternalReport id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create TlsInternalReport \  --field 'policyIdentifiers={}' \  --field 'mailRua={}' \  --field 'httpRua={}' \  --field 'report={"dateRangeEnd":"2026-01-01T00:00:00Z","dateRangeStart":"2026-01-01T00:00:00Z","policies":{},"reportId":"Example"}' \  --field domain=example.com \  --field createdAt=2026-01-01T00:00:00Z \  --field deliverAt=2026-01-01T00:00:00Z
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query TlsInternalReport
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update TlsInternalReport id1 --field policyIdentifiers='{}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete TlsInternalReport --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### TlsReport

[Section titled “TlsReport”](#tlsreport)

Content of a TLS aggregate report.

##### organizationName

[Section titled “organizationName”](#organizationname)

Type: `String?`

Name of the organization that generated the report

##### contactInfo

[Section titled “contactInfo”](#contactinfo)

Type: `String?`

Contact information for the reporting organization

##### reportId

[Section titled “reportId”](#reportid)

Type: `String` · required

Unique identifier for this report

##### dateRangeStart

[Section titled “dateRangeStart”](#daterangestart)

Type: `UTCDateTime` · required

Start of the reporting period

##### dateRangeEnd

[Section titled “dateRangeEnd”](#daterangeend)

Type: `UTCDateTime` · required

End of the reporting period

##### policies

[Section titled “policies”](#policies)

Type: [`TlsReportPolicy`](#tlsreportpolicy)`[]`

Policy evaluation results for each domain

#### TlsReportPolicy

[Section titled “TlsReportPolicy”](#tlsreportpolicy)

TLS policy evaluation result for a specific domain.

##### policyType

[Section titled “policyType”](#policytype)

Type: [`TlsPolicyType`](#tlspolicytype) · required

Type of TLS policy that was evaluated

##### policyStrings

[Section titled “policyStrings”](#policystrings)

Type: `String[]`

Raw policy strings as retrieved

##### policyDomain

[Section titled “policyDomain”](#policydomain)

Type: `DomainName` · required

Domain the policy applies to

##### mxHosts

[Section titled “mxHosts”](#mxhosts)

Type: `String[]`

MX hostnames covered by the policy

##### totalSuccessfulSessions

[Section titled “totalSuccessfulSessions”](#totalsuccessfulsessions)

Type: `UnsignedInt` · default: `0`

Number of sessions that successfully established TLS

##### totalFailedSessions

[Section titled “totalFailedSessions”](#totalfailedsessions)

Type: `UnsignedInt` · default: `0`

Number of sessions that failed TLS establishment

##### failureDetails

[Section titled “failureDetails”](#failuredetails)

Type: [`TlsFailureDetails`](#tlsfailuredetails)`[]`

Details of TLS failures encountered

##### TlsFailureDetails

[Section titled “TlsFailureDetails”](#tlsfailuredetails)

Details of a TLS failure encountered during delivery.

##### resultType

[Section titled “resultType”](#resulttype)

Type: [`TlsResultType`](#tlsresulttype) · required

Type of failure encountered

##### sendingMtaIp

[Section titled “sendingMtaIp”](#sendingmtaip)

Type: `IpAddr?`

IP address of the sending mail server

##### receivingMxHostname

[Section titled “receivingMxHostname”](#receivingmxhostname)

Type: `String?`

Hostname of the receiving mail server

##### receivingMxHelo

[Section titled “receivingMxHelo”](#receivingmxhelo)

Type: `String?`

HELO/EHLO string of the receiving mail server

##### receivingIp

[Section titled “receivingIp”](#receivingip)

Type: `IpAddr?`

IP address of the receiving mail server

##### failedSessionCount

[Section titled “failedSessionCount”](#failedsessioncount)

Type: `UnsignedInt` · default: `0`

Number of sessions that failed with this error

##### additionalInformation

[Section titled “additionalInformation”](#additionalinformation)

Type: `String?`

Additional context about the failure

##### failureReasonCode

[Section titled “failureReasonCode”](#failurereasoncode)

Type: `String?`

Error code or reason string for the failure

## Enums

[Section titled “Enums”](#enums)

### TlsPolicyType

[Section titled “TlsPolicyType”](#tlspolicytype)
ValueLabel`tlsa`DANE TLSA policy`sts`MTA-STS policy`noPolicyFound`No TLS policy was found for the domain`other`Other or unrecognized policy type

### TlsResultType

[Section titled “TlsResultType”](#tlsresulttype)
ValueLabel`startTlsNotSupported`Remote server does not support STARTTLS`certificateHostMismatch`Certificate hostname does not match server`certificateExpired`Certificate has expired`certificateNotTrusted`Certificate is not trusted`validationFailure`General certificate validation failure`tlsaInvalid`DANE TLSA record is invalid`dnssecInvalid`DNSSEC validation failed`daneRequired`DANE is required but not available`stsPolicyFetchError`Failed to fetch MTA-STS policy`stsPolicyInvalid`MTA-STS policy is invalid`stsWebpkiInvalid`MTA-STS WebPKI validation failed`other`Other or unrecognized failure type