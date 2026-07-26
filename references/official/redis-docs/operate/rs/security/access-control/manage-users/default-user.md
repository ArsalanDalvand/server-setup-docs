---
title: "Manage default user"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/manage-users/default-user/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/manage-users/default-user"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Manage default user"
---
# Manage default user

Manage a database's default user.

            Redis Software
          

When you [create a database](/docs/latest/operate/rs/databases/create/), default user database access is enabled by default (Unauthenticated access is selected). This gives the default user full access to the database and enables compatibility with versions of Redis before Redis 6.

Select Password-only authentication, then enter and confirm a default database password to require authentication for connections to the database.

[](/docs/latest/images/rs/screenshots/databases/security-access-control-password-only.png)

## Authenticate as default user

When you configure a password for your database, all connections to the database must authenticate using the [AUTH](/docs/latest/commands/auth/) command. See Redis security's [[operate/oss_and_stack/management/security#authentication|authentication]] section for more information.

```sh
AUTH <default-database-password>
```

## Change default database password

To change the default user's password:

From the database's Security tab, select Edit.

In the Access Control section, select Password-only authentication as the Access method.

Enter and re-enter the new password.

Select Save.

## Deactivate default user

If you set up [[operate/rs/security/access-control|role-based access control]] with [access control lists](/docs/latest/operate/rs/security/access-control/create-db-roles/) (ACLs) for your database and don't require backwards compatibility with versions earlier than Redis 6, you can [[operate/rs/security/access-control/manage-users/default-user|deactivate the default user]].

Warning:

Before you deactivate default user access, make sure the role associated with the database is [[operate/rs/security/access-control/create-users|assigned to a user]]. Otherwise, the database will be inaccessible.

If you enabled the default database password during the creation of an [Active-Active database](/docs/latest/operate/rs/databases/active-active/), you should not turn off the default database password because it could prevent the removal of participating database instances.

To deactivate the default user:

From the database's Security tab, select Edit.

In the Access Control section, select Using ACL only as the Access method.

[](/docs/latest/images/rs/screenshots/databases/security-access-control-acl-only.png)

Choose at least one role and Redis ACL to access the database.

Select Save.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page