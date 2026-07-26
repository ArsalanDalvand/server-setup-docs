---
title: "AcmeProvider"
source: "https://stalw.art/docs/ref/object/acme-provider"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/acme-provider"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AcmeProvider"
---
# AcmeProvider

      

Defines an ACME provider for automatic TLS certificate management.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  TLS › ACME Providers

## Fields

[Section titled “Fields”](#fields)

##### challengeType

[Section titled “challengeType”](#challengetype)

Type: [`AcmeChallengeType`](#acmechallengetype) · default: `"TlsAlpn01"`

The ACME challenge type used to validate domain ownership

##### contact

[Section titled “contact”](#contact)

Type: `EmailAddress[]` · min items: 1

Contact email address, which is used for important communications regarding your ACME account and certificates

##### directory

[Section titled “directory”](#directory)

Type: `Uri` · read-only · default: `"https://acme-v02.api.letsencrypt.org/directory"`

The URL of the ACME directory endpoint

##### eabHmacKey

[Section titled “eabHmacKey”](#eabhmackey)

Type: `String?` · read-only · secret

The External Account Binding (EAB) HMAC key

##### eabKeyId

[Section titled “eabKeyId”](#eabkeyid)

Type: `String?` · read-only

The External Account Binding (EAB) key ID

##### accountKey

[Section titled “accountKey”](#accountkey)

Type: `String` · server-set · secret

The account key used to authenticate with the ACME provider.

##### accountUri

[Section titled “accountUri”](#accounturi)

Type: `Uri` · server-set

The account URI returned by the ACME server after registration. Used for CAA record accounturi binding.

##### renewBefore

[Section titled “renewBefore”](#renewbefore)

Type: [`AcmeRenewBefore`](#acmerenewbefore) · default: `"R23"`

How long before expiration the certificate should be renewed

##### maxRetries

[Section titled “maxRetries”](#maxretries)

Type: `Integer` · default: `10`

Maximum number of retry attempts for failed challenges

##### memberTenantId

[Section titled “memberTenantId”](#membertenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>?` · [[server/enterprise|enterprise]]

Identifier for the tenant this ACME provider belongs to

##### preferredChain

[Section titled “preferredChain”](#preferredchain)

Type: `String?`

Preferred certificate chain to use when multiple chains are available

##### reuseKey

[Section titled “reuseKey”](#reusekey)

Type: `Boolean` · default: `false`

Whether to reuse the existing private key when renewing a certificate

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The AcmeProvider object is available via the `urn:stalwart:jmap` capability.

### x:AcmeProvider/get

[Section titled “x:AcmeProvider/get”](#xacmeproviderget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysAcmeProviderGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AcmeProvider/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AcmeProvider/set

[Section titled “x:AcmeProvider/set”](#xacmeproviderset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysAcmeProviderCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AcmeProvider/set",          {            "create": {              "new1": {                "contact": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysAcmeProviderUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AcmeProvider/set",          {            "update": {              "id1": {                "preferredChain": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysAcmeProviderDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AcmeProvider/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AcmeProvider/query

[Section titled “x:AcmeProvider/query”](#xacmeproviderquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysAcmeProviderQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AcmeProvider/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:AcmeProvider/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text`memberTenantId`id of Tenant

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AcmeProvider id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create AcmeProvider \  --field 'contact={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query AcmeProviderstalwart-cli query AcmeProvider --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update AcmeProvider id1 --field preferredChain='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete AcmeProvider --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### AcmeChallengeType

[Section titled “AcmeChallengeType”](#acmechallengetype)
ValueLabel`TlsAlpn01`TLS-ALPN-01`DnsPersist01`DNS-PERSIST-01`Dns01`DNS-01`Http01`HTTP-01

### AcmeRenewBefore

[Section titled “AcmeRenewBefore”](#acmerenewbefore)
ValueLabel`R12`1/2 of the remaining time until expiration`R23`2/3 of the remaining time until expiration`R34`3/4 of the remaining time until expiration`R45`4/5 of the remaining time until expiration