---
title: "Group Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/minio-group-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/minio-group-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Group Management — SILO (MinIO Object Storage)"
---
# Group Management

## Overview

A group is a collection of [[administration/identity-access-management/minio-user-management#minio-users|users]]. Each group
can have one or more assigned [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]]
that explicitly list the actions and resources to which group members are
allowed or denied access.

For example, consider the following groups. Each group is assigned a
[[administration/identity-access-management/policy-based-access-control#minio-policy-built-in|built-in policy]] or supported
[[administration/identity-access-management/policy-based-access-control#minio-policy-actions|policy action]]. Each group also has one or
more assigned users. Each user’s total set of permissions consists of their
explicitly assigned permission and the inherited permissions from each of
their assigned groups. MinIO by default denies access to any resource or
operation not explicitly allowed by a user’s assigned or inherited policies.

Group

Policy

Members

`Operations`

[[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]] on `finance` bucket
[[administration/identity-access-management/policy-based-access-control#userpolicy.readonly|`readonly`]] on `audit` bucket

`john.doe`, `jane.doe`

`Auditing`

[[administration/identity-access-management/policy-based-access-control#userpolicy.readonly|`readonly`]] on `audit` bucket

`jen.doe`, `joe.doe`

`Admin`

[[administration/identity-access-management/policy-based-access-control#policy-action.admin|`admin:*`]]

`greg.doe`, `jen.doe`

Groups provide a simplified method for managing shared permissions among
users with common access patterns and workloads. Client’s cannot authenticate
to a MinIO deployment using a group as an identity.

The [[reference/minio-mc-admin/mc-admin-group#command-mc.admin.group|`mc admin group`]] command supports the creation and management of
groups on the MinIO deployment. See the command reference for examples of
usage.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).