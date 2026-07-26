---
title: "Security"
source: "https://redis.io/docs/latest/operate/rc/security/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Security"
---
# Security

            Redis Cloud
          

Redis Cloud provides a number of ways to secure subscriptions and databases.

As a Redis Cloud user, consider these security domains:

The [Redis Cloud console](#admin-console-security)
[[operate/rc/security/database-security|Your databases]]
The [[operate/rc/api/get-started/enable-the-api|Redis Cloud API]]

First, you might want to review our [[operate/rc/security/shared-responsibility-model|shared responsibility model]] for security.

## Redis Cloud console security

The Redis Cloud console is the web application you use to manage your Redis Cloud deployments.

Secure access to the Redis Cloud console by:

Assigning appropriate roles to [[operate/rc/security/access-control/access-management#team-management-roles|team members with access]].

Enabling [[operate/rc/security/access-control/multi-factor-authentication|multi-factor authentication]].

Enabling [[operate/rc/security/access-control/saml-sso|SAML SSO]].

Note:
  A user session on Redis Cloud expires after 30 minutes. You'll be signed out of Redis Cloud after 30 minutes of inactivity.

## Database security

You have several options when it comes to securing your Redis Cloud databases. For more information, see [[operate/rc/security/database-security|Cloud database security]]. Options include:

[[operate/rc/security/encryption-at-rest|Encryption at rest]]
[[operate/rc/security/access-control/data-access-control/role-based-access-control|Role-based access control]]
[[operate/rc/security/database-security/tls-ssl|TLS]]
[[operate/rc/security/database-security/network-security|Network security]] using
[[operate/rc/security/vpc-peering|VPC peering]] and [[operate/rc/security/cidr-whitelist|CIDR whitelist]]

## API security

The Redis Cloud API allows you to programmatically administer your subscriptions and database deployments. This API is disabled by default. When you [[operate/rc/api/get-started/enable-the-api|enable the API]], you can then [[operate/rc/api/get-started/manage-api-keys|manage the API keys]] for all owners of your Redis Cloud account. For an overview of the security features of the API, see the [[operate/rc/api/get-started|API authentication documentation]].

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