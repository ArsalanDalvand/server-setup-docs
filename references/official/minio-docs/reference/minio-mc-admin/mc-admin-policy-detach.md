---
title: "mc admin policy detach — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-detach.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-detach"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy detach — SILO (MinIO Object Storage)"
---
# mc admin policy detach

## Syntax

Remove one or more IAM policies from either a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user or a group]].

Exactly one [`--user`](#mc.admin.policy.detach.-user) or one [`--group`](#mc.admin.policy.detach.-group) is required.

EXAMPLE

The following command detaches the policy `readonly` from the user `james` on the deployment at alias `myminio`.

```
mc admin policy detach myminio readonly --user james
```

SYNTAX

The command has the following syntax:

```
mc admin policy detach TARGET                         \
                       POLICY                         \
                       [POLICY...]                    \
                       [--user USER | --group GROUP]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Important

This command is intended for managing policy associations for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed]] users only.

For managing policies to OpenID-managed users, see [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect Access Management]].

For detaching policies from Active Directory/LDAP users or groups, use [[reference/minio-mc/mc-idp-ldap-policy-detach#command-mc.idp.ldap.policy.detach|`mc idp ldap policy detach`]].

### Parameters

The [`mc admin policy detach`](#command-mc.admin.policy.detach) command accepts the following arguments:

TARGET[](#mc.admin.policy.detach.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment with the user or group for which you want to detach one or more policies.

POLICY[](#mc.admin.policy.detach.POLICY)
Required

The name of the policy to detach from either the user or the group.
You may detach multiple policies at once by separating each policy name with a space.

MinIO deployments include the following [[administration/identity-access-management/policy-based-access-control#minio-policy-built-in|built-in policies]] by default:

[[administration/identity-access-management/policy-based-access-control#userpolicy.readonly|`readonly`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.diagnostics|`diagnostics`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.writeonly|`writeonly`]]

--user[](#mc.admin.policy.detach.-user)
Optional

The username of the identity you want to detach the policy or policies from.
You may only list one user.

You must include either the `--user` flag or the `--group` flag.
You may not use the `--user` flag at the same time as the `--group` flag.

--group[](#mc.admin.policy.detach.-group)
Optional

The name of the group identity you want to detach the policy or policies from.
You may only list one group.

All users with membership in the group lose access to any permissions granted by the policies associated to the group, unless those are granted by other policies or groups the users belong to.

You must include either the `--group` flag or the `--user` flag.
You may not use the `--group` flag at the same time as the `--user` flag.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Detach the policy `readonly` from the user `james` on the deployment at alias `myminio`.

```
mc admin policy detach myminio readonly --user james
```

Detach the `audit-policy` and `acct-policy` policies from group `legal` on the deployment at alias `myminio`.

```
mc admin policy detach myminio audit-policy acct-policy --group legal
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).