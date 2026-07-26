---
title: "Enable Role-based access control"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/data-access-control/role-based-access-control/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/data-access-control/role-based-access-control"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable Role-based access control"
---
# Enable Role-based access control

Lets you define multiple users with fine-grained data authorization features.

            Redis Cloud
          

Role-based access control (RBAC) lets you define roles with specific sets of permissions. You can then assign users to these roles
to provide appropriate levels of access.

RBAC effectively lets you implement the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege). For example, you can provide
read-only access to an application whose only job is to display Redis data. Similarly, you can prevent new developers from running dangerous administrative commands.

## Set up RBAC

To set up RBAC, first navigate to the Data Access Control screen.

There are three tabs on this screen: Users, Roles, and Redis ACLs.

In the Redis ACLs tab, you [[operate/rc/security/access-control/data-access-control/configure-acls|define named permissions]] for specific Redis commands, keys, and pub/sub channels.

[](/docs/latest/images/rc/data-access-control-acls.png)

In the Roles tab, you [create roles](/docs/latest/operate/rc/security/access-control/data-access-control/create-roles/). Each role consists of a set of permissions for one or more Redis Cloud databases.

[](/docs/latest/images/rc/data-access-control-roles.png)

Finally, in the Users tab, you [[operate/rc/security/access-control/data-access-control/create-assign-users|create users]] and [[operate/rc/security/access-control/data-access-control/create-assign-users#assign-roles-to-existing-users|assign each user a role]].

[](/docs/latest/images/rc/data-access-control-users.png)

Note:
  Database access users are different from account access users. To learn more, see [[operate/rc/security/access-control|Access management]].

## Redis ACLs vs. Redis Cloud RBAC

In Redis, you can create users and assign ACLs to them using the `ACL` command. However,
Redis does not support generic roles.

In Redis Cloud, you configure RBAC using the Redis Cloud console. As a result, certain Redis ACL
subcommands are not available in Redis Cloud. The following table shows which ACL commands are supported.

Command
Supported

[ACL CAT](/docs/latest/commands/acl-cat/)
â Supported

[ACL DELUSER](/docs/latest/commands/acl-deluser/)
â Not supported

[ACL DRYRUN](/docs/latest/commands/acl-dryrun/)
â Supported

[ACL GENPASS](/docs/latest/commands/acl-genpass/)
â Not supported

[ACL GETUSER](/docs/latest/commands/acl-getuser/)
â Supported

[ACL HELP](/docs/latest/commands/acl-help/)
â Supported

[ACL LIST](/docs/latest/commands/acl-list/)
â Supported

[ACL LOAD](/docs/latest/commands/acl-load/)
â Not supported

[ACL LOG](/docs/latest/commands/acl-log/)
â Not supported

[ACL SAVE](/docs/latest/commands/acl-save/)
â Not supported

[ACL SETUSER](/docs/latest/commands/acl-setuser/)
â Not supported

[ACL USERS](/docs/latest/commands/acl-users/)
â Supported

[ACL WHOAMI](/docs/latest/commands/acl-whoami/)
â Supported

In Redis, you must explicitly provide access to the `MULTI`, `EXEC`, and `DISCARD` commands.
In Redis Cloud, these commands, which are used in transactions, are always permitted. However, the commands
run within the transaction block are subject to RBAC permissions.

When you run multi-key commands on multi-slot keys, the return value is `failure` but the command runs on the keys that are allowed.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page