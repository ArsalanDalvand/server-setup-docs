---
title: "Migrate to role-based LDAP"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/ldap/migrate-to-role-based-ldap/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/ldap/migrate-to-role-based-ldap"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Migrate to role-based LDAP"
---
# Migrate to role-based LDAP

Describes how to migrate existing cluster-based LDAP deployments to role-based LDAP.

            Redis Software
          

Redis Software supports LDAP through a [[operate/rs/security/access-control/ldap|role-based mechanism]], first introduced [in v6.0.20](/docs/latest/operate/rs/release-notes/rs-6-0-20-april-2021/).

Earlier versions of Redis Software supported a cluster-based mechanism; however, that mechanism was removed in v6.2.12.

If you're using the cluster-based mechanism to enable LDAP authentication, you need to migrate to the role-based mechanism before upgrading to Redis Software v6.2.12 or later.

## Migration checklist

This checklist covers the basic process:

Identify accounts per app on the customer end.

Create or identify an LDAP user account on the server that is responsible for LDAP authentication and authorization.

Create or identify an LDAP group that contains the app team members.

Verify or configure the Redis Software ACLs.

Configure each database ACL.

Remove the earlier "external" (LDAP) users from Redis Software.

(Recommended) Update cluster configuration to replace the cluster-based configuration file.

You can use `rladmin` to update the cluster configuration:

```bash
$ touch /tmp/saslauthd_empty.conf
$ rladmin cluster config saslauthd_ldap_conf \
     /tmp/saslauthd_empty.conf
```

Here, a blank file replaces the earlier configuration.

Use Access Control > LDAP > Configuration to enable role-based LDAP.

Map your LDAP groups to access control roles.

Test application connectivity using the LDAP credentials of an app team member.

(Recommended) Turn off default access for the database to avoid anonymous client connections.

Because deployments and requirements vary, youâll likely need to adjust these guidelines.

## Test LDAP access

To test your LDAP integration, you can:

Connect with `redis-cli` and use the [`AUTH` command](/docs/latest/commands/auth/) to test LDAP username/password credentials.

Sign in to the Cluster Manager UI using LDAP credentials authorized for admin access.

Use [Redis Insight](/docs/latest/develop/tools/insight/) to access a database using authorized LDAP credentials.

Use the [REST API](/docs/latest/operate/rs/references/rest-api/) to connect using authorized LDAP credentials.

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