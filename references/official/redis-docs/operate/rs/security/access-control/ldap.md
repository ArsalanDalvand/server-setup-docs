---
title: "LDAP authentication"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/ldap/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/ldap"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "LDAP authentication"
---
# LDAP authentication

Describes how Redis Software integrates LDAP authentication and authorization. Also describes how to enable LDAP for your deployment of Redis Enterprise Software.

            Redis Software
          

Redis Software supports [Lightweight Directory Access Protocol](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol) (LDAP) authentication and authorization through its [[operate/rs/security/access-control|role-based access controls]] (RBAC).  You can use LDAP to authorize access to the Cluster Manager UI and to control database access.

You can configure LDAP roles using the Redis Software Cluster Manager UI or [REST API](/docs/latest/operate/rs/references/rest-api/requests/ldap_mappings/).

## How it works

Here's how role-based LDAP integration works:

[](/docs/latest/images/rs/access-control-ldap-diagram.png)

A user signs in with their LDAP credentials.

Based on the LDAP configuration details, the username is mapped to an LDAP Distinguished Name.

A simple [LDAP bind request](https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol#Bind_(authenticate)) is attempted using the Distinguished Name and the password.  The sign-in fails if the bind fails.

Obtain the userâs LDAP group memberships.

Using configured LDAP details, obtain a list of the userâs group memberships.

Compare the userâs LDAP group memberships to those mapped to local roles.

Determine if one of the user's groups is authorized to access the target resource.  If so, the user is granted the level of access authorized to the role.

To access the Cluster Manager UI, the user needs to belong to an LDAP group mapped to an administrative role.

For database access, the user needs to belong to an LDAP group mapped to a role listed in the databaseâs access control list (ACL).  The rights granted to the group determine the user's level of access.

## Prerequisites

Before you enable LDAP in Redis Software, you need:

The following LDAP details:

Server URI, including host, port, and protocol details.
Certificate details for secure protocols.
Bind credentials, including Distinguished Name, password, and (optionally) client public and private keys for certificate authentication.
Authentication query details, whether template or query.
Authorization query details, whether attribute or query.
The Distinguished Names of LDAP groups youâll use to authorize access to Redis Software resources.

The LDAP groups that correspond to the levels of access you wish to authorize.  Each LDAP group will be mapped to a Redis Software access control role.

A Redis Software access control role for each LDAP group. Before you enable LDAP, you need to set up [[operate/rs/security/access-control|role-based access controls]] (RBAC).

## Enable LDAP

To enable LDAP:

From Access Control > LDAP in the Cluster Manager UI, select the Configuration tab and [[operate/rs/security/access-control/ldap/enable-role-based-ldap|enable LDAP access]].

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/ldap-config.png)

Map LDAP groups to [access control roles](/docs/latest/operate/rs/security/access-control/ldap/map-ldap-groups-to-roles/).

Update database access control lists (ACLs) to [[operate/rs/security/access-control/ldap/update-database-acls|authorize role access]].

If you already have appropriate roles, you can update them to include LDAP groups.

## More info

Enable and configure [[operate/rs/security/access-control/ldap/enable-role-based-ldap|role-based LDAP]]
Map LDAP groups to [access control roles](/docs/latest/operate/rs/security/access-control/ldap/map-ldap-groups-to-roles/)
Update database ACLs to [[operate/rs/security/access-control/ldap/update-database-acls|authorize LDAP access]]
Learn more about Redis Software [[operate/rs/security|security and practices]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page