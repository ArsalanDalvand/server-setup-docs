---
title: "Tenant"
source: "https://stalw.art/docs/ref/object/tenant"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/tenant"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Tenant"
---
# Tenant

      

Defines a tenant for multi-tenant environments with isolated resources and quotas.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Directory › Tenants

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · required

Name of the tenant

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Creation date of the tenant

##### logo

[Section titled “logo”](#logo)

Type: `String?`

URL or base64-encoded image representing the tenant

##### roles

[Section titled “roles”](#roles)

Type: [`Roles`](#roles) · required

Roles assigned to this tenant

##### permissions

[Section titled “permissions”](#permissions)

Type: [`Permissions`](#permissions) · required

Permissions assigned to this tenant

##### quotas

[Section titled “quotas”](#quotas)

Type: `Map<`[`TenantStorageQuota`](#tenantstoragequota)`, UnsignedInt>`

Quotas for different object types within this tenant

##### usedDiskQuota

[Section titled “usedDiskQuota”](#useddiskquota)

Type: `Size` · server-set

Amount of disk space currently used by this tenant (bytes)

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Tenant object is available via the `urn:stalwart:jmap` capability.

### x:Tenant/get

[Section titled “x:Tenant/get”](#xtenantget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTenantGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tenant/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Tenant/set

[Section titled “x:Tenant/set”](#xtenantset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTenantCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tenant/set",          {            "create": {              "new1": {                "name": "Example",                "permissions": {                  "@type": "Inherit"                },                "quotas": {},                "roles": {                  "@type": "Default"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTenantUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tenant/set",          {            "update": {              "id1": {                "name": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTenantDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tenant/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Tenant/query

[Section titled “x:Tenant/query”](#xtenantquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTenantQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Tenant/query",          {            "filter": {              "text": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Tenant/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`text`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Tenant id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Tenant \  --field name=Example \  --field 'roles={"@type":"Default"}' \  --field 'permissions={"@type":"Inherit"}' \  --field 'quotas={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Tenantstalwart-cli query Tenant --where text=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Tenant id1 --field name='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Tenant --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Roles

[Section titled “Roles”](#roles-1)

Role assignment for groups and tenants.

`Default`: Default role. No additional fields.
`Custom`: Custom role. Carries the fields of [`CustomRoles`](#customroles).

#### CustomRoles

[Section titled “CustomRoles”](#customroles)

Custom role assignment with specific role references.

##### roleIds

[Section titled “roleIds”](#roleids)

Type: `Id<`[[ref/object/role|`Role`]]`>[]`

List of roles assigned to this principal.

### Permissions

[Section titled “Permissions”](#permissions-1)

Permission assignment mode for accounts, groups, and tenants.

`Inherit`: Inherited permissions. No additional fields.
`Merge`: Permissions are combined with inherited permissions. Carries the fields of [`PermissionsList`](#permissionslist).
`Replace`: Permissions replace all inherited permissions. Carries the fields of [`PermissionsList`](#permissionslist).

#### PermissionsList

[Section titled “PermissionsList”](#permissionslist)

Explicit permission grants and denials.

##### enabledPermissions

[Section titled “enabledPermissions”](#enabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly enabled.

##### disabledPermissions

[Section titled “disabledPermissions”](#disabledpermissions)

Type: [[ref/permissions|`Permission`]]`[]`

List of permissions that are explicitly disabled, even if they would be inherited through other roles or groups. This takes precedence over enabled permissions.

## Enums

[Section titled “Enums”](#enums)

### TenantStorageQuota

[Section titled “TenantStorageQuota”](#tenantstoragequota)
ValueLabel`maxAccounts`Maximum number of accounts`maxGroups`Maximum number of groups`maxDomains`Maximum number of domains`maxMailingLists`Maximum number of mailing lists`maxRoles`Maximum number of roles`maxOauthClients`Maximum number of OAuth clients`maxDkimKeys`Maximum number of DKIM keys`maxDnsServers`Maximum number of DNS servers`maxDirectories`Maximum number of external directories`maxAcmeProviders`Maximum number of ACME providers`maxDiskQuota`Maximum disk space allocated (bytes)