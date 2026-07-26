---
title: "Roles"
source: "https://stalw.art/docs/auth/authorization/roles"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authorization/roles"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Roles"
---
# Roles

      

Roles group [[auth/authorization/permissions|permissions]] so that access can be managed across individuals, groups, and tenants without assigning every permission individually. Each role is represented by a [[ref/object/role|Role]] object (found in the WebUI under  Management ›  Directory › Roles) and can reference other roles, producing a hierarchical model in which a parent role inherits the permissions of its subroles.

A Role object carries the following fields:

[[ref/object/role#description|`description`]]: a human-readable description of the role’s purpose. This also serves as the role’s display label in the WebUI.
[[ref/object/role#membertenantid|`memberTenantId`]]: the tenant that owns the role, used in multi-tenant deployments to scope roles to a specific tenant.
[[ref/object/role#roleids|`roleIds`]]: a list of roles this role extends. Permissions from the listed roles are inherited by this role.
[[ref/object/role#enabledpermissions|`enabledPermissions`]]: permissions explicitly granted by the role.
[[ref/object/role#disabledpermissions|`disabledPermissions`]]: permissions explicitly denied by the role. Disabled permissions take precedence over inherited permissions.

The role’s display label in listings is drawn from [[ref/object/role#description|`description`]]; there is no separate `name` field. Membership is not stored on the Role object itself, it is tracked on the principals that reference the role, through the `roles` field on [[ref/object/account|Account]] (including its `Group` variant) and on [[ref/object/tenant|Tenant]].

Assigning a role to an account is done from the target principal. On an Account object (found in the WebUI under  Management ›  Directory › Groups,  Management ›  Directory › Accounts), the [[ref/object/account#roles|`roles`]] field selects either a built-in role or a `Custom` role set that references one or more Role ids. Groups and tenants expose their own `roles` field of the same shape.

## Built-in Roles

[Section titled “Built-in Roles”](#built-in-roles)

Stalwart provides three built-in roles for common scenarios. The Account object references them through the `UserRoles` variants `User` and `Admin`, and tenant-level administration is covered by a dedicated role set.

user: grants access to the regular services offered by the server, such as sending and receiving email and accessing mailboxes. It is intended for standard end-user accounts.
admin: grants every permission defined by the server, including configuration and principal management. Intended for system administrators.
tenant-admin: grants the administrative permissions required to manage users, groups, and settings within a single tenant or domain, without exposing global configuration changes.

Default roles automatically assigned to new accounts, groups, tenants, and administrators are configured on the [[ref/object/authentication|Authentication]] singleton (found in the WebUI under  Settings ›  Authentication › General) through [[ref/object/authentication#defaultuserroleids|`defaultUserRoleIds`]], [[ref/object/authentication#defaultgrouproleids|`defaultGroupRoleIds`]], [[ref/object/authentication#defaulttenantroleids|`defaultTenantRoleIds`]], and [[ref/object/authentication#defaultadminroleids|`defaultAdminRoleIds`]].