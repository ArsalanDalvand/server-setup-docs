---
title: "mc admin policy entities — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-entities.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-entities"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy entities — SILO (MinIO Object Storage)"
---
# mc admin policy entities

## Syntax

List the entities associated with a policy, user, or group on a target MinIO deployment.

Changed in version RELEASE.2023-05-27T05-56-19Z: This command only returns [[administration/identity-access-management/minio-user-management#minio-users|minio-managed users and groups]].

To list entities associated with an Active Directory or LDAP (AD/LDAP) configuration, use [[reference/minio-mc/mc-idp-ldap-policy-entities#command-mc.idp.ldap.policy.entities|`mc idp ldap policy entities`]].

For example, you can list all of the users and groups attached to a policy or list all of the policies attached to a specific user or group.

EXAMPLE

The following command returns a list of the policies associated with the user `bob` on the deployment at alias `myminio`.

```
mc admin policy entities myminio/ --user bob
```

SYNTAX

The command has the following syntax:

```
mc admin policy entities         \
                TARGET           \
                [--user value]   \
                [--group value]  \
                [--policy value]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Important

This command is intended for managing policy associations for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed]] users only.

For managing policies to OpenID-managed users, see [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect Access Management]].

For viewing policies for Active Directory/LDAP users or groups, use [[reference/minio-mc/mc-idp-ldap-policy-entities#command-mc.idp.ldap.policy.entities|`mc idp ldap policy entities`]].

### Parameters

The [`mc admin policy entities`](#command-mc.admin.policy.entities) command accepts the following arguments:

TARGET[](#mc.admin.policy.entities.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which to add the new policy.

--group[](#mc.admin.policy.entities.-group)
Optional

The name of the group identity for which you want to list attached policies.

You may include multiple groups by repeating the flag multiple times.
The command returns each group with a list of associated entities.

--policy[](#mc.admin.policy.entities.-policy)
Optional

The name of a policy for which to list associated entities.

You may include multiple policies by repeating the flag multiple times.
The command returns each policy with a list of all associated entities.

--user[](#mc.admin.policy.entities.-user)
Optional

The username of the identity for which you want to list attached policies.

You may include multiple users by repeating the flag multiple times.
The command returns each user with a list of associated policies.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List all entities and policy associations for a deployment

The following command lists all policies and the entity mappings associated with them on the deployment at alias `myminio`.

```
mc admin policy entities myminio/
```

### List entities associated with two different policies

The following command lists all entities associated with the policies `inteam-policy` and `mlteam-policy` on the deployment at alias `myminio`.

```
mc admin policy entities myminio/ --policy finteam-policy --policy mlteam-policy
```

### List policies associated with two different users

The following command lists all policies associated with the users `bob` and `james` on the deployment at alias `myminio`.

The command outputs the list of policies associated with `bob` then the list of policies associated with `james` on the deployment at alias `myminio`.

```
mc admin policy entities myminio/ --user bob --user james
```

### List policies associated with two different groups

The following command lists all policies associated with the groups `auditors` and `accounting` on the deployment at alias `myminio`.

The command outputs the list of policies associated with the group `auditors` then the list of policies associated with the group `accounting` on the deployment at alias `myminio`.

```
mc admin policy entities play/ --group auditors --group accounting
```

### List policies associated with a policy, a group, and a user

The following command lists all policies associated with the policy `finteam-policy`, the user `bobfisher`, and the group `consulting` on the deployment at alias `myminio`.

The command outputs the list of groups and users associated with the policy `finteam-policy`, then lists the policies associated with the user `bobfisher`, and finally lists the policies associated with the group `consulting` on the deployment at alias `myminio`.

```
mc admin policy entities play/ \
           --policy finteam-policy --user bobfisher --group consulting
```

## Output

The output of the commands resembles the following:

```
Query time: 2023-04-04T20:39:27Z
  Policy -> Entity Mappings:
    Policy: finteam-policy
      User Mappings:
        bobfisher
    Policy: diagnostics
      User Mappings:
        james
        bobfisher
        marcia
      Group Mappings:
        consulting
        auditors
  User -> Policy Mappings:
    User: bobfisher
      ALLOW_PUBLIC_READ
      finteam-policy
      diagnostics
      readonly
      readwrite
      writeonly
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).