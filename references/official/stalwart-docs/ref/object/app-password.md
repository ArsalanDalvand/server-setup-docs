---
title: "AppPassword"
source: "https://stalw.art/docs/ref/object/app-password"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/app-password"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AppPassword"
---
# AppPassword

      

App password credential for programmatic access.

This object can be configured from the [[management/webui|WebUI]] under  Account ›  Credentials › App Passwords

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

The AppPassword object is available via the `urn:stalwart:jmap` capability.

### x:AppPassword/get

[Section titled “x:AppPassword/get”](#xapppasswordget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysAppPasswordGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AppPassword/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AppPassword/set

[Section titled “x:AppPassword/set”](#xapppasswordset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysAppPasswordCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AppPassword/set",          {            "create": {              "new1": {                "allowedIps": {},                "description": "Example",                "permissions": {                  "@type": "Inherit"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysAppPasswordUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AppPassword/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysAppPasswordDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AppPassword/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AppPassword/query

[Section titled “x:AppPassword/query”](#xapppasswordquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysAppPasswordQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AppPassword/query",          {            "filter": {              "expiresAt": "2026-01-01T00:00:00Z"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:AppPassword/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`expiresAt`date

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AppPassword id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create AppPassword \  --field description=Example \  --field 'permissions={"@type":"Inherit"}' \  --field 'allowedIps={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query AppPasswordstalwart-cli query AppPassword --where expiresAt=2026-01-01T00:00:00Z
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update AppPassword id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete AppPassword --ids id1
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