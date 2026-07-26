---
title: "Data access control"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/data-access-control/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/data-access-control"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Data access control"
---
# Data access control

Control who can access your databases using the default user database password and role-based access control.

            Redis Cloud
          

## Default user

When you create a Redis Cloud database, your database is given a randomly generated password called the [[operate/rc/security/access-control/data-access-control/default-user|Default user password]]. Learn how to [[operate/rc/security/access-control/data-access-control/default-user#change-password|change the default user password]] or [[operate/rc/security/access-control/data-access-control/default-user#turn-off-default-user|turn off default user access]].

## Role-based access control

With [[operate/rc/security/access-control/data-access-control/role-based-access-control|role-based access control (RBAC)]], you create roles and assign users to those roles to grant different levels of access to the database.

[[operate/rc/security/access-control/data-access-control/role-based-access-control|Enable RBAC]]
[[operate/rc/security/access-control/data-access-control/configure-acls|Configure ACLs]]
[Create roles](/docs/latest/operate/rc/security/access-control/data-access-control/create-roles/)
[[operate/rc/security/access-control/data-access-control/create-assign-users|Create and edit database users]]
[Active-Active roles](/docs/latest/operate/rc/security/access-control/data-access-control/active-active-roles/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page