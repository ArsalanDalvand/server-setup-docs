---
title: "mc admin group — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-group.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-group"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin group — SILO (MinIO Object Storage)"
---
# mc admin group

## Description

The [`mc admin group`](#command-mc.admin.group) command manages groups on a MinIO deployment.

A [[administration/identity-access-management/minio-group-management#minio-groups|group]] is a collection of [[administration/identity-access-management/minio-user-management#minio-users|users]]. Each group can have one or more assigned
[[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] that explicitly list the
actions and resources to which group members are allowed or denied access.
Groups provide a simplified method for managing shared permissions among users
with common access patterns and workloads.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

### Groups and Policy-Based Access Control

MinIO uses Policy-Based Access Control (PBAC) to support authorization of
users who have successfully authenticated to the deployment. Each policy
includes rules that dictate the allowed or denied actions/resources on the
deployment. You can assign one or more [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to a group. Users with membership in the
group inherit the group’s assigned policies. A user’s total set of permissions
includes their explicitly assigned policies and any policies inherited
via group membership.

Newly created groups have no policies by default. To configure a group’s
assigned policies, use the [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] command.

For more information on MinIO users and groups, see
[[administration/identity-access-management/minio-user-management#minio-users|User Management]] and [[administration/identity-access-management/minio-group-management#minio-groups|Group Management]]. For
more information on MinIO policies, see [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy Based Access Control]].

`Deny` overrides `Allow`

MinIO follows the IAM standard where a `Deny` rule overrides `Allow` rule
on the same action or resource. For example, if a user has an explicitly
assigned policy with an `Allow` rule for an action/resource while one of
its groups has an assigned policy with a `Deny` rule for that
action/resource, MinIO would apply only the `Deny` rule.

For more information on IAM policy evaluation logic, see the IAM
documentation on
[Determining Whether a Request is Allowed or Denied Within an Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html#policy-eval-denyallow).

## Examples

### Create a New Group

Use [`mc admin group add`](#mc.admin.group.add) to create a new group to an S3-compatible host:

```
mc admin group add ALIAS GROUPNAME MEMBER [MEMBER...]
```

Replace [`ALIAS`](#mc.admin.group.add.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`GROUPNAME`](#mc.admin.group.add.GROUPNAME) with the name
of the group to create.

Replace [`MEMBER`](#mc.admin.group.add.MEMBERS) with at least one
[[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`user`]] on the S3 host. Specify multiple members
as a list: `MEMBER1 MEMBER2 MEMBER3`

### List Available Groups

Use [`mc admin group ls`](#mc.admin.group.ls) to list list all groups on an S3-compatible
host:

```
mc admin group ls ALIAS
```

Replace [`ALIAS`](#mc.admin.group.ls.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

### View Group Details

Use [`mc admin group info`](#mc.admin.group.info) to view detailed group information on an
S3-compatible host:

```
mc admin group info ALIAS GROUPNAME
```

Replace [`ALIAS`](#mc.admin.group.info.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`GROUPNAME`](#mc.admin.group.info.GROUPNAME) with the name of
the group.

### Remove a Group

Use [`mc admin group rm`](#mc.admin.group.rm) to remove a group from an S3-compatible
host:

```
mc admin group rm ALIAS GROUPNAME
```

Replace [`ALIAS`](#mc.admin.group.rm.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`GROUPNAME`](#mc.admin.group.rm.GROUPNAME) with the
name of the group.

### Disable a Group

Use [`mc admin group disable`](#mc.admin.group.disable) to disable a group on an S3-compatible
host:

```
mc admin group disable ALIAS GROUPNAME
```

Replace [`ALIAS`](#mc.admin.group.disable.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`GROUPNAME`](#mc.admin.group.disable.GROUPNAME) with the name
of the group.

### Enable a Group

Use [`mc admin group enable`](#mc.admin.group.enable) to enable a group on an S3-compatible
host:

```
mc admin group enable ALIAS GROUPNAME
```

Replace [`ALIAS`](#mc.admin.group.enable.TARGET) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`GROUPNAME`](#mc.admin.group.enable.GROUPNAME) with the name
of the group.

## Quick Reference

[`mc admin group add TARGET GROUPNAME MEMBERS`](#mc.admin.group.add)

Adds a user to a group on the MinIO deployment. Creates the group if it
does not exist.

[`mc admin group info TARGET GROUPNAME`](#mc.admin.group.info)

Returns detailed information for a group on the MinIO deployment.

[`mc admin group ls TARGET`](#mc.admin.group.ls)

Returns a list of all groups on the MinIO deployment.

[`mc admin group rm TARGET GROUPNAME`](#mc.admin.group.rm)

Removes a group on the MinIO deployment.

[`mc admin group enable TARGET GROUPNAME`](#mc.admin.group.enable)

Enables a group on the MinIO deployment. Users can only inherit
[[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] assigned to an enabled group.

[`mc admin group disable TARGET GROUPNAME`](#mc.admin.group.disable)

Disables a group on the MinIO deployment. Users cannot inherit [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] assigned to a disabled group.

## Syntax

mc admin group add[](#mc.admin.group.add)

Adds an existing user to the group. The command creates the group if it
does not exist. The command has the following syntax:

```
mc admin group add TARGET GROUPNAME MEMBERS
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.add.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which
the command adds users to the new or existing group

GROUPNAME[](#mc.admin.group.add.GROUPNAME)

The name of the group. The command creates the group if it does not
already exist. Use [`mc admin group ls`](#mc.admin.group.ls) to review the existing
groups on a deployment.

A group name cannot contain the characters `=` (equal sign) or `,` (comma).

MEMBERS[](#mc.admin.group.add.MEMBERS)

The name of the user to add to the group.

The user must exist on the [`TARGET`](#mc.admin.group.add.TARGET) MinIO
deployment. Use [[reference/minio-mc-admin/mc-admin-user-list#command-mc.admin.user.ls|`mc admin user ls`]] to review the available
users on the deployment.

mc admin group info[](#mc.admin.group.info)

Returns details for the group on the target deployment, such as all
[[administration/identity-access-management/minio-user-management#minio-users|users]] with membership in the group and the
assigned [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]]. The command has
the following syntax:

```
mc admin group info TARGET GROUPNAME
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.info.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which
to retrieve the group information.

GROUPNAME[](#mc.admin.group.info.GROUPNAME)

The name of the group.

mc admin group ls, list[](#mc.admin.group.ls)

List all groups on the target MinIO deployment. The command has the
following syntax:

```
mc admin group ls TARGET
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.ls.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from
which to retrieve groups.

mc admin group rm, remove[](#mc.admin.group.rm)

Removes a group on the target MinIO deployment. Removing a group does not
remove any users with membership in the group. Use
[[reference/minio-mc-admin/mc-admin-user-remove#command-mc.admin.user.rm|`mc admin user rm`]] to remove users from a group.

The command has the following syntax:

```
mc admin group rm TARGET GROUPNAME
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.rm.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which
to remove the group.

GROUPNAME[](#mc.admin.group.rm.GROUPNAME)

The name of the group to remove.

mc admin group enable[](#mc.admin.group.enable)

Enables the group on the target MinIO deployment. Users can only inherit
[[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] from an enabled group.
Groups are enabled on creation by default. The command has the following
syntax:

```
mc admin group enable TARGET GROUPNAME
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.enable.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on
which to enable the group.

GROUPNAME[](#mc.admin.group.enable.GROUPNAME)

The name of the group to enable.

mc admin group disable[](#mc.admin.group.disable)

Disables the group on the target MinIO deployment. Users cannot inherit
[[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] from a disabled group. The
command has the following syntax:

```
mc admin group disable TARGET GROUPNAME
```

The command accepts the following arguments:

TARGET[](#mc.admin.group.disable.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which
to disable the group.

GROUPNAME[](#mc.admin.group.disable.GROUPNAME)

The name of the group to disable.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).