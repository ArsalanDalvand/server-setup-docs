---
title: "DkimSignature"
source: "https://stalw.art/docs/ref/object/dkim-signature"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dkim-signature"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DkimSignature"
---
# DkimSignature

      

Defines a DKIM signature used to sign outgoing email messages.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Domains › DKIM Signatures

## Fields

[Section titled “Fields”](#fields)

DkimSignature is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Dkim1Ed25519Sha256"

[Section titled “@type: "Dkim1Ed25519Sha256"”](#type-dkim1ed25519sha256)

DKIM1 (Ed25519 SHA-256)

##### auid

[Section titled “auid”](#auid)

Type: `String?`

Agent or user identifier included in the DKIM signature header

##### canonicalization

[Section titled “canonicalization”](#canonicalization)

Type: [`DkimCanonicalization`](#dkimcanonicalization) · default: `"relaxed/relaxed"`

Canonicalization algorithm applied to the headers and body before signing

##### expire

[Section titled “expire”](#expire)

Type: `Duration?`

Time after which this DKIM signature expires and should no longer be considered valid

##### headers

[Section titled “headers”](#headers)

Type: `String[]` · default: `["From","To","Date","Subject","Message-ID"]`

List of message headers to include in the DKIM signature

##### privateKey

[Section titled “privateKey”](#privatekey)

Type: [`SecretText`](#secrettext) · required

PEM-encoded private key used to sign outgoing messages

##### publicKey

[Section titled “publicKey”](#publickey)

Type: `Text` · server-set

PEM-encoded public key used to verify signatures, derived from the private key

##### report

[Section titled “report”](#report)

Type: `Boolean` · default: `true`

Whether to request failure reports when signature verification fails on the recipient side

##### thirdParty

[Section titled “thirdParty”](#thirdparty)

Type: `String?`

Authorized third-party signature value, used when signing on behalf of another domain

##### thirdPartyHash

[Section titled “thirdPartyHash”](#thirdpartyhash)

Type: [`DkimHash`](#dkimhash)`?`

Hashing algorithm used to verify the authorized third-party signature DNS record

##### domainId

[Section titled “domainId”](#domainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this DKIM signature is associated with

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this DKIM signature belongs to

##### selector

[Section titled “selector”](#selector)

Type: `String` · required

Selector used to locate the DKIM public key in DNS

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Creation date of the DKIM signature

##### nextTransitionAt

[Section titled “nextTransitionAt”](#nexttransitionat)

Type: `UTCDateTime?`

Date when this key will transition to the next rotation stage, or null if no transition is scheduled

##### stage

[Section titled “stage”](#stage)

Type: [`DkimRotationStage`](#dkimrotationstage) · default: `"active"`

Current stage of the DKIM key in its rotation lifecycle

### @type: "Dkim1RsaSha256"

[Section titled “@type: "Dkim1RsaSha256"”](#type-dkim1rsasha256)

DKIM1 (RSA SHA-256)

##### auid

[Section titled “auid”](#auid-1)

Type: `String?`

Agent or user identifier included in the DKIM signature header

##### canonicalization

[Section titled “canonicalization”](#canonicalization-1)

Type: [`DkimCanonicalization`](#dkimcanonicalization) · default: `"relaxed/relaxed"`

Canonicalization algorithm applied to the headers and body before signing

##### expire

[Section titled “expire”](#expire-1)

Type: `Duration?`

Time after which this DKIM signature expires and should no longer be considered valid

##### headers

[Section titled “headers”](#headers-1)

Type: `String[]` · default: `["From","To","Date","Subject","Message-ID"]`

List of message headers to include in the DKIM signature

##### privateKey

[Section titled “privateKey”](#privatekey-1)

Type: [`SecretText`](#secrettext) · required

PEM-encoded private key used to sign outgoing messages

##### publicKey

[Section titled “publicKey”](#publickey-1)

Type: `Text` · server-set

PEM-encoded public key used to verify signatures, derived from the private key

##### report

[Section titled “report”](#report-1)

Type: `Boolean` · default: `true`

Whether to request failure reports when signature verification fails on the recipient side

##### thirdParty

[Section titled “thirdParty”](#thirdparty-1)

Type: `String?`

Authorized third-party signature value, used when signing on behalf of another domain

##### thirdPartyHash

[Section titled “thirdPartyHash”](#thirdpartyhash-1)

Type: [`DkimHash`](#dkimhash)`?`

Hashing algorithm used to verify the authorized third-party signature DNS record

##### domainId

[Section titled “domainId”](#domainid-1)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this DKIM signature is associated with

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-1)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this DKIM signature belongs to

##### selector

[Section titled “selector”](#selector-1)

Type: `String` · required

Selector used to locate the DKIM public key in DNS

##### createdAt

[Section titled “createdAt”](#createdat-1)

Type: `UTCDateTime` · server-set

Creation date of the DKIM signature

##### nextTransitionAt

[Section titled “nextTransitionAt”](#nexttransitionat-1)

Type: `UTCDateTime?`

Date when this key will transition to the next rotation stage, or null if no transition is scheduled

##### stage

[Section titled “stage”](#stage-1)

Type: [`DkimRotationStage`](#dkimrotationstage) · default: `"active"`

Current stage of the DKIM key in its rotation lifecycle

### @type: "Dkim2Ed25519Sha256"

[Section titled “@type: "Dkim2Ed25519Sha256"”](#type-dkim2ed25519sha256)

DKIM2 (Ed25519 SHA-256)

##### flags

[Section titled “flags”](#flags)

Type: [`Dkim2Flag`](#dkim2flag)`[]`

Policy flags added to the signature, requesting downstream handlers to honor delivery constraints or provide feedback

##### privateKey

[Section titled “privateKey”](#privatekey-2)

Type: [`SecretText`](#secrettext) · required

PEM-encoded private key used to sign outgoing messages

##### publicKey

[Section titled “publicKey”](#publickey-2)

Type: `Text` · server-set

PEM-encoded public key used to verify signatures, derived from the private key

##### domainId

[Section titled “domainId”](#domainid-2)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this DKIM signature is associated with

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-2)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this DKIM signature belongs to

##### selector

[Section titled “selector”](#selector-2)

Type: `String` · required

Selector used to locate the DKIM public key in DNS

##### createdAt

[Section titled “createdAt”](#createdat-2)

Type: `UTCDateTime` · server-set

Creation date of the DKIM signature

##### nextTransitionAt

[Section titled “nextTransitionAt”](#nexttransitionat-2)

Type: `UTCDateTime?`

Date when this key will transition to the next rotation stage, or null if no transition is scheduled

##### stage

[Section titled “stage”](#stage-2)

Type: [`DkimRotationStage`](#dkimrotationstage) · default: `"active"`

Current stage of the DKIM key in its rotation lifecycle

### @type: "Dkim2RsaSha256"

[Section titled “@type: "Dkim2RsaSha256"”](#type-dkim2rsasha256)

DKIM2 (RSA SHA-256)

##### flags

[Section titled “flags”](#flags-1)

Type: [`Dkim2Flag`](#dkim2flag)`[]`

Policy flags added to the signature, requesting downstream handlers to honor delivery constraints or provide feedback

##### privateKey

[Section titled “privateKey”](#privatekey-3)

Type: [`SecretText`](#secrettext) · required

PEM-encoded private key used to sign outgoing messages

##### publicKey

[Section titled “publicKey”](#publickey-3)

Type: `Text` · server-set

PEM-encoded public key used to verify signatures, derived from the private key

##### domainId

[Section titled “domainId”](#domainid-3)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Identifier for the domain this DKIM signature is associated with

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid-3)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this DKIM signature belongs to

##### selector

[Section titled “selector”](#selector-3)

Type: `String` · required

Selector used to locate the DKIM public key in DNS

##### createdAt

[Section titled “createdAt”](#createdat-3)

Type: `UTCDateTime` · server-set

Creation date of the DKIM signature

##### nextTransitionAt

[Section titled “nextTransitionAt”](#nexttransitionat-3)

Type: `UTCDateTime?`

Date when this key will transition to the next rotation stage, or null if no transition is scheduled

##### stage

[Section titled “stage”](#stage-3)

Type: [`DkimRotationStage`](#dkimrotationstage) · default: `"active"`

Current stage of the DKIM key in its rotation lifecycle

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DkimSignature object is available via the `urn:stalwart:jmap` capability.

### x:DkimSignature/get

[Section titled “x:DkimSignature/get”](#xdkimsignatureget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysDkimSignatureGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimSignature/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DkimSignature/set

[Section titled “x:DkimSignature/set”](#xdkimsignatureset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysDkimSignatureCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimSignature/set",          {            "create": {              "new1": {                "@type": "Dkim1Ed25519Sha256",                "domainId": "<Domain id>",                "privateKey": {                  "@type": "Text",                  "secret": "Example"                },                "selector": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysDkimSignatureUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimSignature/set",          {            "update": {              "id1": {                "auid": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysDkimSignatureDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimSignature/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DkimSignature/query

[Section titled “x:DkimSignature/query”](#xdkimsignaturequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysDkimSignatureQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DkimSignature/query",          {            "filter": {              "domainId": "id1"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:DkimSignature/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`domainId`id of Domain`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DkimSignature id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create DkimSignature/Dkim1Ed25519Sha256 \  --field 'privateKey={"@type":"Text","secret":"Example"}' \  --field 'domainId=<Domain id>' \  --field selector=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query DkimSignaturestalwart-cli query DkimSignature --where domainId=id1
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update DkimSignature id1 --field auid='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete DkimSignature --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretText

[Section titled “SecretText”](#secrettext)

A secret text value provided directly, from an environment variable, or from a file.

`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `Text` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

## Enums

[Section titled “Enums”](#enums)

### DkimCanonicalization

[Section titled “DkimCanonicalization”](#dkimcanonicalization)
ValueLabel`relaxed/relaxed`Relaxed/Relaxed`simple/simple`Simple/Simple`relaxed/simple`Relaxed/Simple`simple/relaxed`Simple/Relaxed

### DkimHash

[Section titled “DkimHash”](#dkimhash)
ValueLabel`sha256`SHA-256`sha1`SHA-1

### DkimRotationStage

[Section titled “DkimRotationStage”](#dkimrotationstage)
ValueLabel`active`DKIM key is published in DNS and used for signing`pending`DKIM key is scheduled for DNS publication and not yet active`retiring`DKIM key has been superseded by a new key but still published in DNS`retired`DKIM key has been removed from DNS and is pending deletion

### Dkim2Flag

[Section titled “Dkim2Flag”](#dkim2flag)
ValueLabel`donotmodify`Request that the message not be modified in transit`donotexplode`Request that the message not be delivered to more than one recipient`feedback`Request feedback about how the message is handled during delivery