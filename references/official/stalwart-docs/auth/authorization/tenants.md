---
title: "Tenants"
source: "https://stalw.art/docs/auth/authorization/tenants"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authorization/tenants"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Tenants"
---
# Tenants

      

Multi-tenancy allows several independent organisations, referred to as tenants, to share the same Stalwart deployment while keeping their data and resources isolated from one another. A tenant is a logical division within the server: it has its own accounts, groups, mailing lists, and domains, and principals that belong to one tenant cannot reach resources that belong to another.

Each tenant is represented by a [[ref/object/tenant|Tenant]] object (found in the WebUI under  Management ›  Directory › Tenants). Principals associated with a tenant (accounts, groups, lists, and similar) carry a `memberTenantId` field referencing the owning tenant and are restricted to resources of that same tenant.

Permissions granted within a tenant are bounded by the tenant’s own permissions. If a tenant administrator assigns a permission to a principal that is not enabled on the tenant, the grant has no effect. This ensures a tenant cannot exceed the privileges allocated to it at the server level.

## Properties

[Section titled “Properties”](#properties)

A Tenant object exposes the following fields:

[[ref/object/tenant#name|`name`]]: the tenant’s name, used as its identifier in the system.
[[ref/object/tenant#logo|`logo`]]: a URL or base64-encoded image used when rendering the tenant in the WebUI.
[[ref/object/tenant#roles|`roles`]]: the roles assigned to the tenant itself, modelled as a `Roles` variant that can be either the default role set or a custom set referencing specific roles.
[[ref/object/tenant#permissions|`permissions`]]: the permissions the tenant is allowed to grant to its principals, modelled as a `Permissions` variant (`Inherit`, `Merge`, or `Replace`) combined with an explicit enable/disable list.
[[ref/object/tenant#quotas|`quotas`]]: a map from `TenantStorageQuota` keys to numeric limits. This includes the tenant’s total disk quota and the maximum number of principals of each kind (accounts, groups, lists, domains) that the tenant can create.
[[ref/object/tenant#useddiskquota|`usedDiskQuota`]]: the amount of disk space currently consumed by the tenant, computed by the server.

The [[ref/object/tenant#name|`name`]] field carries the tenant’s human-readable label. There is no separate description field on the Tenant object.

Isolation, quotas, and custom role sets together allow multi-tenant operation, while still giving administrators the ability to regulate how much of the server each tenant can consume.

## Quotas

[Section titled “Quotas”](#quotas)

A tenant’s disk quota is the maximum storage consumed by all of its members combined. The sum of every member’s used storage must fit within this value. For example, when a tenant has a 100GB quota, if member A has consumed 60GB and member B has consumed 40GB, no further mail can be accepted for any account in the tenant until usage is reduced.

Tenants also carry principal-count quotas, which limit the number of accounts, groups, lists, or domains the tenant can create. Both kinds of quota are expressed through the tenant’s [[ref/object/tenant#quotas|`quotas`]] map.

## Branding

[Section titled “Branding”](#branding)

Stalwart supports per-tenant branding in the WebUI through the [[ref/object/tenant#logo|`logo`]] field on the Tenant object. The logo is displayed when a user belonging to the tenant accesses the [[management/webui|WebUI]], letting each tenant present its own visual identity in multi-tenant deployments.