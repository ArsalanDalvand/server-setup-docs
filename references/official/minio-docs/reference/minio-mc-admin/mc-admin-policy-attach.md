---
title: "mc admin policy attach — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-attach.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-attach"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy attach — SILO (MinIO Object Storage)"
---
# mc admin policy attach

## Syntax

Attaches one or more IAM policies to either a [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed user or a group]].

Changed in version RELEASE.2023-05-27T05-56-19Z: To successfully attach a policy, the referenced user or group must exist.

Exactly one [`--user`](#mc.admin.policy.attach.-user) or one [`--group`](#mc.admin.policy.attach.-group) is required.

EXAMPLE

The following command attaches the `readonly` policy to the user `james` on the deployment at [[glossary#term-alias|alias]] `myminio`.

```
mc admin policy attach myminio readonly --user james
```

SYNTAX

The command has the following syntax:

```
mc admin policy attach                       \
                TARGET                       \
                POLICY                       \
                [POLICY...]                  \
                [--user USER | --group GROUP]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Important

This command is intended for managing policy associations for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed]] users only.

For attaching policies to OpenID-managed users, see [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect Access Management]].

For attaching policies to Active Directory/LDAP users or groups, use [[reference/minio-mc/mc-idp-ldap-policy-attach#command-mc.idp.ldap.policy.attach|`mc idp ldap policy attach`]].

### Parameters

The [`mc admin policy attach`](#command-mc.admin.policy.attach) command accepts the following arguments:

TARGET[](#mc.admin.policy.attach.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment with the user or group for which you want to attach one or more policies.

POLICY[](#mc.admin.policy.attach.POLICY)
Required

The name of the policy to attach to either the user or the group.

You may attach multiple policies at once by separating each policy name with a space.

MinIO deployments include the following [[administration/identity-access-management/policy-based-access-control#minio-policy-built-in|built-in policies]] by default:

[[administration/identity-access-management/policy-based-access-control#userpolicy.readonly|`readonly`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.diagnostics|`diagnostics`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.writeonly|`writeonly`]]

--user[](#mc.admin.policy.attach.-user)
Optional

The username of the identity you want to attach the policy or policies to.
You may only list one user.

You must include either the `--user` flag or the `--group` flag.
You may not use the `--user` flag at the same time as the `--group` flag.

--group[](#mc.admin.policy.attach.-group)
Optional

The name of the group identity you want to attach the policy or policies to.
You may only list one group.

All users with membership in the group inherit the policies associated to the group.

You must include either the `--group` flag or the `--user` flag.
You may not use the `--group` flag at the same time as the `--user` flag.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Attach the `readonly` policy to user `james` on the deployment at alias `myminio`.

```
mc admin policy attach myminio readonly --user james
```

Attach the `audit-policy` and `acct-policy` policies to group `legal` on the deployment at alias `myminio`.

```
mc admin policy attach myminio audit-policy acct-policy --group legal
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).