---
title: "mc admin policy — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy — SILO (MinIO Object Storage)"
---
# mc admin policy

Changed in version mc: RELEASE.2023-03-20T17-17-53Z

The following commands are deprecated:

`mc admin policy add` use [[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`mc admin policy create`]] instead

`mc admin policy set` use [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] instead

`mc admin policy unset` use [[reference/minio-mc-admin/mc-admin-policy-detach#command-mc.admin.policy.detach|`mc admin policy detach`]] instead

`mc admin policy update` use [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`attach`]] or [[reference/minio-mc-admin/mc-admin-policy-detach#command-mc.admin.policy.detach|`detach`]] instead

The following command is added:

[[reference/minio-mc-admin/mc-admin-policy-entities#command-mc.admin.policy.entities|`mc admin policy entities`]]

## Description

The [`mc admin policy`](#command-mc.admin.policy) commands manage policies for use with [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy-Based Access Control]] (PBAC).
MinIO PBAC uses IAM-compatible policy JSON documents to define rules for accessing resources on a MinIO server.

For complete documentation on MinIO PBAC, including policy document JSON structure and syntax, see [[administration/identity-access-management/policy-based-access-control#minio-policy|Access Management]]. To manage policies for deployments that use LDAP authentication, see [[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]].

## Subcommands

[`mc admin policy`](#command-mc.admin.policy) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`attach`]]

Attaches one or more IAM policies to either a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user or a group]].

[[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`create`]]

Creates a new policy on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-policy-detach#command-mc.admin.policy.detach|`detach`]]

Remove one or more IAM policies from either a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user or a group]].

[[reference/minio-mc-admin/mc-admin-policy-entities#command-mc.admin.policy.entities|`entities`]]

List the entities associated with a policy, user, or group on a target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-policy-info#command-mc.admin.policy.info|`info`]]

Returns the specified policy in JSON format if it exists on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-policy-list#command-mc.admin.policy.ls|`ls`]]

Lists all policies on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-policy-remove#command-mc.admin.policy.rm|`rm`]]

Removes an IAM policy from the target MinIO deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).