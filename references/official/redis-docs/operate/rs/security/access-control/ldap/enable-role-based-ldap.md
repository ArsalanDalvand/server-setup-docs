---
title: "Enable role-based LDAP"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/ldap/enable-role-based-ldap/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/ldap/enable-role-based-ldap"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable role-based LDAP"
---
# Enable role-based LDAP

Describes how to enable role-based LDAP authentication and authorization using the Cluster Manager UI.

            Redis Software
          

Redis Software uses a role-based mechanism to enable LDAP authentication and authorization.

When a user attempts to access Redis Software resources using LDAP credentials, the credentials are passed to the LDAP server in a bind request. If the request succeeds, the userâs groups are searched for a group that authorizes access to the original resource.

Role-based LDAP lets you authorize cluster management users (previously known as external users) and database users. As with any access control role, you can define the level of access authorized by the role.

## Set up LDAP connection

To configure and enable LDAP from the Cluster Manager UI:

Go to Access Control > LDAP > Configuration.

Select + Create.

In Set LDAP, configure [LDAP server settings](#ldap-server-settings), [bind credentials](#bind-credentials), [authentication query](#authentication-query), and [authorization query](#authorization-query).

[](/docs/latest/images/rs/screenshots/access-control/ldap-config.png)

Select Save & Enable.

### LDAP server settings

The LDAP server settings define the communication settings used for LDAP authentication and authorization. These include:

Setting
Description

Protocol type
Underlying communication protocol; must be LDAP, LDAPS, or STARTTLS

Host
URL of the LDAP server

Port
LDAP server port number

Trusted CA certificate
(LDAPS or STARTTLS protocols only) Certificate for the trusted certificate authority (CA)

When defining multiple LDAP hosts, the organization tree structure must be identical for all hosts.

### Bind credentials

These settings define the credentials for the bind query:

Setting
Description

Distinguished Name
Example: `cd=admin,dc=example,dc=org`

Password
Example: `admin1`

Client certificate authentication
(LDAPS or STARTTLS protocols only) Place checkmark to enable

Client public key
(LDAPS or STARTTLS protocols only) The client public key for authentication

Client private key
(LDAPS or STARTTLS protocols only) The client private key for authentication

### Authentication query

These settings define the authentication query:

Setting
Description

Search user by
Either Template or Query

Template
(template search) Example: `cn=%u,ou=dev,dc=example,dc=com`

Base
(query search) Example: `ou=dev,dc=example,dc=com`

Filter
(query search) Must be enclosed in parentheses. Example: `(cn=%u)`

Scope
(query search) Must be baseObject, singleLevel, or wholeSubtree

In this example, `%u` is replaced by the username attempting to access the Redis Software resource.

### Authorization query

These settings define the group authorization query:

Setting
Description

Search groups by
Either Attribute or Query

Attribute
(attribute search) Example: `memberOf` (case-sensitive)

Base
(query search) Example: `ou=groups,dc=example,dc=com`

Filter
(query search) Must be enclosed in parentheses. Example: `(members=%D)`

Scope
(query search) Must be baseObject, singleLevel, or wholeSubtree

In this example, `%D` is replaced by the Distinguished Name of the user attempting to access the Redis Software resource.

### Authentication timeout

The Authentication timeout setting determines the connection timeout to the LDAP server during user authentication.

By default, the timeout is 5 seconds, which is recommended for most cases.

However, if you enable multi-factor authentication (MFA) for your LDAP server, you might need to increase the timeout to provide enough time for MFA verification. You can set it to any integer in the range of 5-60 seconds.

## More info

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