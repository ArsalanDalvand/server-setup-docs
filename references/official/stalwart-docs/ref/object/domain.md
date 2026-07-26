---
title: "Domain"
source: "https://stalw.art/docs/ref/object/domain"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/domain"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Domain"
---
# Domain

      

Defines an email domain and its DNS, DKIM, and TLS certificate settings.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Domains › Domains

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `DomainName` · required

Domain name

##### aliases

[Section titled “aliases”](#aliases)

Type: `DomainName[]`

List of additional domain names that are aliases of this domain

##### isEnabled

[Section titled “isEnabled”](#isenabled)

Type: `Boolean` · default: `true`

Whether this domain is enabled

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · read-only · server-set

Creation date of the domain

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the domain

##### logo

[Section titled “logo”](#logo)

Type: `String?` · [[server/enterprise|enterprise]]

URL or base64-encoded image representing the domain

##### certificateManagement

[Section titled “certificateManagement”](#certificatemanagement)

Type: [`CertificateManagement`](#certificatemanagement) · required

Whether TLS certificates for this domain are managed manually or automatically by an ACME provider

##### dkimManagement

[Section titled “dkimManagement”](#dkimmanagement)

Type: [`DkimManagement`](#dkimmanagement) · required

Whether DKIM keys for this domain are managed manually or automatically by the server

##### dnsManagement

[Section titled “dnsManagement”](#dnsmanagement)

Type: [`DnsManagement`](#dnsmanagement) · required

Whether DNS records for this domain are managed manually or automatically by a DNS provider

##### dnsZoneFile

[Section titled “dnsZoneFile”](#dnszonefile)

Type: `Text` · server-set

Current DNS zone data for the domain

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this domain belongs to

##### directoryId

[Section titled “directoryId”](#directoryid)

Type: `Id<`[[ref/object/directory|`Directory`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the directory where accounts for this domain are stored, or null to use the internal directory

##### catchAllAddress

[Section titled “catchAllAddress”](#catchalladdress)

Type: `EmailAddress?`

Catch-all email address that receives messages addressed to unknown local recipients

##### subAddressing

[Section titled “subAddressing”](#subaddressing)

Type: [`SubAddressing`](#subaddressing) · required

Whether sub-addressing (plus addressing) is enabled for the domain

##### allowRelaying

[Section titled “allowRelaying”](#allowrelaying)

Type: `Boolean` · default: `false`

Whether to allow relaying for non-local recipients, useful in split delivery scenarios

##### reportAddressUri

[Section titled “reportAddressUri”](#reportaddressuri)

Type: `String?` · default: `"mailto:postmaster"`

Email address to receive DMARC, TLS-RPT and CAA reports for this domain, or null to not receive reports

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Domain object is available via the `urn:stalwart:jmap` capability.

### x:Domain/get

[Section titled “x:Domain/get”](#xdomainget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysDomainGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Domain/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Domain/set

[Section titled “x:Domain/set”](#xdomainset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysDomainCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Domain/set",          {            "create": {              "new1": {                "aliases": {},                "certificateManagement": {                  "@type": "Manual"                },                "dkimManagement": {                  "@type": "Automatic"                },                "dnsManagement": {                  "@type": "Manual"                },                "name": "example.com",                "subAddressing": {                  "@type": "Enabled"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysDomainUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Domain/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysDomainDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Domain/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Domain/query

[Section titled “x:Domain/query”](#xdomainquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysDomainQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Domain/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Domain/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`name`text`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Domain id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Domain \  --field name=example.com \  --field 'aliases={}' \  --field 'certificateManagement={"@type":"Manual"}' \  --field 'dkimManagement={"@type":"Automatic"}' \  --field 'dnsManagement={"@type":"Manual"}' \  --field 'subAddressing={"@type":"Enabled"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Domainstalwart-cli query Domain --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Domain id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Domain --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### CertificateManagement

[Section titled “CertificateManagement”](#certificatemanagement-1)

Defines how TLS certificates for the domain are managed.

`Manual`: Manual TLS certificate management. No additional fields.
`Automatic`: ACME TLS certificate management. Carries the fields of [`CertificateManagementProperties`](#certificatemanagementproperties).

#### CertificateManagementProperties

[Section titled “CertificateManagementProperties”](#certificatemanagementproperties)

Automatic TLS certificate management settings using an ACME provider.

##### acmeProviderId

[Section titled “acmeProviderId”](#acmeproviderid)

Type: `Id<`[[ref/object/acme-provider|`AcmeProvider`]]`>` · required

Identifier for the ACME provider managing certificates for this domain

##### subjectAlternativeNames

[Section titled “subjectAlternativeNames”](#subjectalternativenames)

Type: `String[]`

Additional hostnames to include in the certificate as Subject Alternative Names (SANs).
Enter hostnames only (e.g. `mta-sts`, `autoconfig`), the domain is appended automatically.
To include the apex domain, enter it in full (e.g. `example.org`).
Leave empty to request a wildcard certificate when possible, or to use the default SANs.

### DkimManagement

[Section titled “DkimManagement”](#dkimmanagement-1)

Defines how DKIM keys for the domain are managed.

`Automatic`: Automatic DKIM management. Carries the fields of [`DkimManagementProperties`](#dkimmanagementproperties).
`Manual`: Manual DKIM management. No additional fields.

#### DkimManagementProperties

[Section titled “DkimManagementProperties”](#dkimmanagementproperties)

Automatic DKIM key generation and rotation settings.

##### algorithms

[Section titled “algorithms”](#algorithms)

Type: [`DkimSignatureType`](#dkimsignaturetype)`[]` · default: `["Dkim1Ed25519Sha256","Dkim1RsaSha256"]`

List of signing algorithms to use when generating new DKIM keys

##### selectorTemplate

[Section titled “selectorTemplate”](#selectortemplate)

Type: `String` · default: `"v{version}-{algorithm}-{date-%Y%m%d}"`

Template for generating DKIM selectors during key rotation. Supported variables:

`{algorithm}`: signing algorithm in lowercase (`rsa`, `ed25519`)
`{hash}`: hash algorithm (`sha256`)
`{version}`: DKIM version number (`1`)
`{date-<fmt>}`: current UTC date formatted with chrono strftime (e.g. `{date-%Y%m%d}`)
`{epoch}`: current UTC unix timestamp
`{random}`: random 8-character alphanumeric string

##### rotateAfter

[Section titled “rotateAfter”](#rotateafter)

Type: `Duration` · default: `"90d"`

How often to rotate DKIM keys. Requires automatic DNS management to be enabled for the domain.

##### retireAfter

[Section titled “retireAfter”](#retireafter)

Type: `Duration` · default: `"7d"`

How long to keep the old key’s DNS record published after rotation before removing it. Requires automatic DNS management.

##### deleteAfter

[Section titled “deleteAfter”](#deleteafter)

Type: `Duration` · default: `"30d"`

How long to retain old DKIM keys on the server after rotation before deleting them permanently. Requires automatic DNS management.

### DnsManagement

[Section titled “DnsManagement”](#dnsmanagement-1)

Defines how DNS records for the domain are managed.

`Manual`: Manual DNS management. No additional fields.
`Automatic`: Automatic DNS management. Carries the fields of [`DnsManagementProperties`](#dnsmanagementproperties).

#### DnsManagementProperties

[Section titled “DnsManagementProperties”](#dnsmanagementproperties)

Automatic DNS record management settings using a DNS provider.

##### dnsServerId

[Section titled “dnsServerId”](#dnsserverid)

Type: `Id<`[[ref/object/dns-server|`DnsServer`]]`>` · required

Identifier for the DNS server provider managing DNS records for this domain

##### origin

[Section titled “origin”](#origin)

Type: `String?`

Origin domain used to determine the correct DNS zone for managing records. For example, if the domain is “sub.example.com” and DNS records should be managed in the “example.com” zone, set the origin to “example.com”. Leave empty to use the domain name itself as the zone origin.

##### publishRecords

[Section titled “publishRecords”](#publishrecords)

Type: [`DnsRecordType`](#dnsrecordtype)`[]` · default: `["dkim","spf","mx","dmarc","srv","mtaSts","tlsRpt","caa","autoConfig","autoConfigLegacy","autoDiscover"]` · min items: 1

Which DNS record types should be automatically published and kept in sync

### SubAddressing

[Section titled “SubAddressing”](#subaddressing-1)

Defines sub-addressing (plus addressing) settings for the domain.

`Enabled`: Enable sub-addressing. No additional fields.
`Custom`: Enable custom sub-addressing. Carries the fields of [`SubAddressingCustom`](#subaddressingcustom).
`Disabled`: Disable sub-addressing. No additional fields.

#### SubAddressingCustom

[Section titled “SubAddressingCustom”](#subaddressingcustom)

Custom sub-addressing rules for the domain.

##### customRule

[Section titled “customRule”](#customrule)

Type: [`Expression`](#expression) · required

Expression that defines custom sub-addressing rules for the domain

Available variables: [[ref/expression/variable/mta-rcpt-variable|`MtaRcptVariable`]].

##### Expression

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

##### ExpressionMatch

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

## Enums

[Section titled “Enums”](#enums)

### DkimSignatureType

[Section titled “DkimSignatureType”](#dkimsignaturetype)
ValueLabel`Dkim1Ed25519Sha256`DKIM1 (Ed25519 SHA-256)`Dkim1RsaSha256`DKIM1 (RSA SHA-256)`Dkim2Ed25519Sha256`DKIM2 (Ed25519 SHA-256)`Dkim2RsaSha256`DKIM2 (RSA SHA-256)

### DnsRecordType

[Section titled “DnsRecordType”](#dnsrecordtype)
ValueLabel`dkim`DKIM public keys`tlsa`TLSA records`spf`SPF records`mx`MX records`dmarc`DMARC policy`srv`SRV records`mtaSts`MTA-STS policy record`tlsRpt`TLS reporting record`caa`CAA records`autoConfig`Autoconfig records`autoConfigLegacy`Legacy Autoconfig records`autoDiscover`Microsoft Autodiscover records

## Expression references

[Section titled “Expression references”](#expression-references)

The following expression contexts are used by fields on this page:

[[ref/expression/variable/mta-rcpt-variable|`MtaRcptVariable`]] (Variables)