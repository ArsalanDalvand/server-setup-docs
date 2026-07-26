---
title: "TlsExternalReport"
source: "https://stalw.art/docs/ref/object/tls-external-report"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/tls-external-report"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TlsExternalReport"
---
# TlsExternalReport

      

Stores a TLS aggregate report received from an external source.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Reports › Inbox › TLS

## Fields

[Section titled “Fields”](#fields)

##### report

[Section titled “report”](#report)

Type: [`TlsReport`](#tlsreport) · required

TLS report content

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

The TlsExternalReport object is available via the `urn:stalwart:jmap` capability.

### x:TlsExternalReport/get

[Section titled “x:TlsExternalReport/get”](#xtlsexternalreportget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTlsExternalReportGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsExternalReport/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TlsExternalReport/set

[Section titled “x:TlsExternalReport/set”](#xtlsexternalreportset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTlsExternalReportCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsExternalReport/set",          {            "create": {              "new1": {                "expiresAt": "2026-01-01T00:00:00Z",                "from": "[email protected]",                "receivedAt": "2026-01-01T00:00:00Z",                "report": {                  "dateRangeEnd": "2026-01-01T00:00:00Z",                  "dateRangeStart": "2026-01-01T00:00:00Z",                  "policies": {},                  "reportId": "Example"                },                "subject": "Example",                "to": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTlsExternalReportUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsExternalReport/set",          {            "update": {              "id1": {                "subject": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTlsExternalReportDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsExternalReport/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:TlsExternalReport/query

[Section titled “x:TlsExternalReport/query”](#xtlsexternalreportquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTlsExternalReportQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:TlsExternalReport/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get TlsExternalReport id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create TlsExternalReport \  --field 'report={"dateRangeEnd":"2026-01-01T00:00:00Z","dateRangeStart":"2026-01-01T00:00:00Z","policies":{},"reportId":"Example"}' \  --field [email protected] \  --field subject=Example \  --field 'to={}' \  --field receivedAt=2026-01-01T00:00:00Z \  --field expiresAt=2026-01-01T00:00:00Z
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query TlsExternalReport
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update TlsExternalReport id1 --field subject='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete TlsExternalReport --ids id1
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