---
title: "Cloud database security"
source: "https://redis.io/docs/latest/operate/rc/security/database-security/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/database-security"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Cloud database security"
---
# Cloud database security

            Redis Cloud
          

Redis Cloud provides several features to help you secure your databases. These include
[[operate/rc/security/access-control/data-access-control/default-user|password-based authentication]] and [[operate/rc/security/access-control/data-access-control/role-based-access-control|role-based access control]],
[[operate/rc/security/database-security/network-security|network security]], [[operate/rc/security/database-security/tls-ssl|TLS]], and [[operate/rc/security/encryption-at-rest|encryption-at-rest]].

## Passwords, users, and roles

All Redis Cloud databases [[operate/rc/security/access-control/data-access-control/default-user|require a password]] to connect. However, we recommend enabling [[operate/rc/security/access-control/data-access-control/role-based-access-control|role-based access control]] (RBAC) for additional security. With RBAC, you can define
all the roles you need, with the appropriate permissions, and assign those roles
to your users.

## Network security

Redis Cloud supports two types of network security: [[operate/rc/security/database-security/network-security#ip|IP Restrictions]] and [[operate/rc/security/database-security/network-security#virtual-private-clouds|VPCs]]. We recommend that you employ at least one of these network security options to constrain access to your databases.

## Transport Layer Security (TLS)

Redis Cloud supports [[operate/rc/security/database-security/tls-ssl|Transport Layer Security]] (TLS) for database access. TLS, often called "SSL", ensures the privacy of the TCP connection between your application and database. When client
authentication is activated, TLS ensures that only clients with an authorized key can access to your Redis databases.

We strongly recommend enabling TLS for any application transmitting sensitive data across the wire.

## Disk encryption

Redis Cloud provides encryption for all data stored on disk in Redis databases. See our [[operate/rc/security/encryption-at-rest|encryption at rest documentation]] for specific details.

## Continue learning with Redis University

See the [Redis Cloud security overview course](https://university.redis.io/course/0b1hkhejlitg3x) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page