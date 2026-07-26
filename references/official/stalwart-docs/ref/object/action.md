---
title: "Action"
source: "https://stalw.art/docs/ref/object/action"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/action"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Action"
---
# Action

      

Defines server management actions such as reloads, troubleshooting and cache operations.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Actions

## Fields

[Section titled “Fields”](#fields)

Action is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "ReloadSettings"

[Section titled “@type: "ReloadSettings"”](#type-reloadsettings)

Reload: Server settings

### @type: "ReloadTlsCertificates"

[Section titled “@type: "ReloadTlsCertificates"”](#type-reloadtlscertificates)

Reload: TLS certificates

### @type: "ReloadLookupStores"

[Section titled “@type: "ReloadLookupStores"”](#type-reloadlookupstores)

Reload: Lookup stores

### @type: "ReloadBlockedIps"

[Section titled “@type: "ReloadBlockedIps"”](#type-reloadblockedips)

Reload: Blocked IPs list

### @type: "UpdateApps"

[Section titled “@type: "UpdateApps"”](#type-updateapps)

Application Management: Update applications

### @type: "TroubleshootDmarc"

[Section titled “@type: "TroubleshootDmarc"”](#type-troubleshootdmarc)

DMARC: Troubleshooting

##### remoteIp

[Section titled “remoteIp”](#remoteip)

Type: `IpAddr` · required

Remote IP address of the SMTP client

##### ehloDomain

[Section titled “ehloDomain”](#ehlodomain)

Type: `String` · required

EHLO domain provided by the SMTP client

##### mailFrom

[Section titled “mailFrom”](#mailfrom)

Type: `EmailAddress` · required

MAIL FROM address provided by the SMTP client

##### to

[Section titled “to”](#to)

Type: `EmailAddress[]`

RCPT TO addresses provided by the SMTP client, used for DKIM2 envelope verification

##### message

[Section titled “message”](#message)

Type: `Text?`

Body of the email message used for DMARC troubleshooting, if applicable

##### spfEhloDomain

[Section titled “spfEhloDomain”](#spfehlodomain)

Type: `String` · required

Domain used for SPF check based on EHLO domain

##### spfEhloResult

[Section titled “spfEhloResult”](#spfehloresult)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the SPF check based on EHLO domain

##### spfMailFromDomain

[Section titled “spfMailFromDomain”](#spfmailfromdomain)

Type: `String` · required

Domain used for SPF check based on MAIL FROM address

##### spfMailFromResult

[Section titled “spfMailFromResult”](#spfmailfromresult)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the SPF check based on MAIL FROM address

##### ipRevResult

[Section titled “ipRevResult”](#iprevresult)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the reverse DNS check for the remote IP address

##### ipRevPtr

[Section titled “ipRevPtr”](#iprevptr)

Type: `String[]` · server-set

PTR records returned by the reverse DNS lookup for the remote IP address

##### dkimResults

[Section titled “dkimResults”](#dkimresults)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult)`[]` · server-set

Results of the DKIM signature verification checks

##### dkimPass

[Section titled “dkimPass”](#dkimpass)

Type: `Boolean` · server-set · default: `false`

Whether the DKIM checks passed

##### dkim2Result

[Section titled “dkim2Result”](#dkim2result)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the DKIM2 signature chain verification

##### dkim2Pass

[Section titled “dkim2Pass”](#dkim2pass)

Type: `Boolean` · server-set · default: `false`

Whether the DKIM2 checks passed

##### arcResult

[Section titled “arcResult”](#arcresult)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the ARC validation check

##### dmarcResult

[Section titled “dmarcResult”](#dmarcresult)

Type: [`DmarcTroubleshootAuthResult`](#dmarctroubleshootauthresult) · server-set

Result of the DMARC check

##### dmarcPass

[Section titled “dmarcPass”](#dmarcpass)

Type: `Boolean` · server-set · default: `false`

Whether the DMARC check passed

##### dmarcPolicy

[Section titled “dmarcPolicy”](#dmarcpolicy)

Type: [`DmarcDisposition`](#dmarcdisposition) · server-set

DMARC policy applied to the email message

##### elapsed

[Section titled “elapsed”](#elapsed)

Type: `Duration` · server-set · default: `"0ms"`

Time taken to perform the DMARC troubleshooting

### @type: "ClassifySpam"

[Section titled “@type: "ClassifySpam"”](#type-classifyspam)

Spam Filter: Classify a message

##### message

[Section titled “message”](#message-1)

Type: `Text` · required

Raw email message to classify for spam

##### remoteIp

[Section titled “remoteIp”](#remoteip-1)

Type: `IpAddr` · required

Remote IP address of the SMTP client

##### ehloDomain

[Section titled “ehloDomain”](#ehlodomain-1)

Type: `String` · required

EHLO domain provided by the SMTP client

##### authenticatedAs

[Section titled “authenticatedAs”](#authenticatedas)

Type: `String?`

Authentication identity of the SMTP client, if authenticated

##### isTls

[Section titled “isTls”](#istls)

Type: `Boolean` · default: `true`

Whether the SMTP connection is secured with TLS

##### envFrom

[Section titled “envFrom”](#envfrom)

Type: `EmailAddress` · required

MAIL FROM address provided by the SMTP client

##### envFromParameters

[Section titled “envFromParameters”](#envfromparameters)

Type: [`SpamClassifyParameters`](#spamclassifyparameters)`?`

Parameters related to the MAIL FROM address

##### envRcptTo

[Section titled “envRcptTo”](#envrcptto)

Type: `EmailAddress[]`

List of RCPT TO addresses provided by the SMTP client

##### score

[Section titled “score”](#score)

Type: `Float` · server-set · default: `0`

Spam score for the classified message

##### tags

[Section titled “tags”](#tags)

Type: `Map<String,`[`SpamClassifyTag`](#spamclassifytag)`>` · server-set

List of tags contributing to the spam classification of the message

##### result

[Section titled “result”](#result)

Type: [`SpamClassifyResult`](#spamclassifyresult) · server-set

Overall spam classification result for the message

### @type: "InvalidateCaches"

[Section titled “@type: "InvalidateCaches"”](#type-invalidatecaches)

Cache: Invalidate all caches

### @type: "InvalidateNegativeCaches"

[Section titled “@type: "InvalidateNegativeCaches"”](#type-invalidatenegativecaches)

Cache: Invalidate negative caches

### @type: "PauseMtaQueue"

[Section titled “@type: "PauseMtaQueue"”](#type-pausemtaqueue)

MTA: Pause queue processing

### @type: "ResumeMtaQueue"

[Section titled “@type: "ResumeMtaQueue"”](#type-resumemtaqueue)

MTA: Resume queue processing

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Action object is available via the `urn:stalwart:jmap` capability.

### x:Action/get

[Section titled “x:Action/get”](#xactionget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysActionGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Action/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Action/set

[Section titled “x:Action/set”](#xactionset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysActionCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Action/set",          {            "create": {              "new1": {                "@type": "ReloadSettings"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysActionUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Action/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysActionDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Action/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Action/query

[Section titled “x:Action/query”](#xactionquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysActionQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Action/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Action id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Action/ReloadSettings
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Action
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Action id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Action --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### DmarcTroubleshootAuthResult

[Section titled “DmarcTroubleshootAuthResult”](#dmarctroubleshootauthresult)

Authentication check result for DMARC troubleshooting.

`Pass`: Pass. No additional fields.
`Fail`: Fail. Carries the fields of [`DmarcTroubleshootDetails`](#dmarctroubleshootdetails).
`SoftFail`: SoftFail. Carries the fields of [`DmarcTroubleshootDetails`](#dmarctroubleshootdetails).
`TempError`: TempError. Carries the fields of [`DmarcTroubleshootDetails`](#dmarctroubleshootdetails).
`PermError`: PermError. Carries the fields of [`DmarcTroubleshootDetails`](#dmarctroubleshootdetails).
`Neutral`: Neutral. Carries the fields of [`DmarcTroubleshootDetails`](#dmarctroubleshootdetails).
`None`: None. No additional fields.

#### DmarcTroubleshootDetails

[Section titled “DmarcTroubleshootDetails”](#dmarctroubleshootdetails)

Details for a failed authentication check.

##### details

[Section titled “details”](#details)

Type: `String?` · server-set

Authentication result details

### SpamClassifyTag

[Section titled “SpamClassifyTag”](#spamclassifytag)

A tag and score contributing to spam classification.

##### score

[Section titled “score”](#score-1)

Type: `Float` · server-set · default: `0`

Score associated with the tag

##### disposition

[Section titled “disposition”](#disposition)

Type: [`SpamClassifyTagDisposition`](#spamclassifytagdisposition) · server-set

Disposition associated with the tag

## Enums

[Section titled “Enums”](#enums)

### DmarcDisposition

[Section titled “DmarcDisposition”](#dmarcdisposition)
ValueLabel`none`No specific action requested`quarantine`Treat failing messages as suspicious`reject`Reject failing messages`unspecified`Disposition not specified

### SpamClassifyParameters

[Section titled “SpamClassifyParameters”](#spamclassifyparameters)
ValueLabel`bit7`7-bit message content`bit8Mime - 8-bit MIME message content`bit8Mime - 8-bit MIME message content`binaryMime`Binary MIME message content`smtpUtf8`UTF-8 message content

### SpamClassifyTagDisposition

[Section titled “SpamClassifyTagDisposition”](#spamclassifytagdisposition)
ValueLabel`score`Assign the tag’s score to the overall spam score`reject`Reject the message`discard`Discard the message

### SpamClassifyResult

[Section titled “SpamClassifyResult”](#spamclassifyresult)
ValueLabel`spam`Classify as spam`ham`Classify as non-spam (ham)`reject`Reject message`discard`Discard message