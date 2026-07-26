---
title: "ApiKey"
source: "https://stalw.art/docs/ref/object/api-key"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/api-key"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ApiKey"
---
# ApiKey

      

API key credential for programmatic access.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Credentials › API Keys

## Fields

[Section titled “Fields”](#fields)

##### description

[Section titled “description”](#description)

Type: `String` · required

Description of the credential

##### secret

[Section titled “secret”](#secret)

Type: `String` · read-only · server-set · secret

Secret value of the credential

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · read-only · server-set

Creation date of the credential

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

Expiration date of the credential

##### permissions

[Section titled “permissions”](#permissions)

Type: [`CredentialPermissions`](#credentialpermissions) · required

List of permissions assigned to this credential

##### allowedIps

[Section titled “allowedIps”](#allowedips)

Type: `IpMask[]`

List of allowed IP addresses or CIDR ranges for this credential

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The ApiKey object is available via the `urn:stalwart:jmap` capability.

### x:ApiKey/get

[Section titled “x:ApiKey/get”](#xapikeyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysApiKeyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ApiKey/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ApiKey/set

[Section titled “x:ApiKey/set”](#xapikeyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysApiKeyCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ApiKey/set",          {            "create": {              "new1": {                "allowedIps": {},                "description": "Example",                "permissions": {                  "@type": "Inherit"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysApiKeyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ApiKey/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysApiKeyDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ApiKey/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:ApiKey/query

[Section titled “x:ApiKey/query”](#xapikeyquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysApiKeyQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:ApiKey/query",          {            "filter": {              "expiresAt": "2026-01-01T00:00:00Z"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:ApiKey/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`expiresAt`date

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get ApiKey id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create ApiKey \  --field description=Example \  --field 'permissions={"@type":"Inherit"}' \  --field 'allowedIps={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query ApiKeystalwart-cli query ApiKey --where expiresAt=2026-01-01T00:00:00Z
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update ApiKey id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete ApiKey --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### CredentialPermissions

[Section titled “CredentialPermissions”](#credentialpermissions)

Permission assignment mode for a credential.

`Inherit`: Same permissions as account. No additional fields.
`Disable`: Disable some permissions. Carries the fields of [`CredentialPermissionsList`](#credentialpermissionslist).
`Replace`: Replace all permissions. Carries the fields of [`CredentialPermissionsList`](#credentialpermissionslist).

#### CredentialPermissionsList

[Section titled “CredentialPermissionsList”](#credentialpermissionslist)

List of permissions to assign to a credential.

##### permissions

[Section titled “permissions”](#permissions-1)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions to assign.