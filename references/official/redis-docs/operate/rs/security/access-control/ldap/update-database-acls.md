---
title: "Update database ACLs"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/ldap/update-database-acls/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/ldap/update-database-acls"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Update database ACLs"
---
# Update database ACLs

Describes how to use the Cluster Manager UI to update database access control lists (ACLs) to authorize access to roles authorizing LDAP user access.

            Redis Software
          

To grant LDAP users access to a database, assign the mapped access role to the access control list (ACL) for the database.

In the Cluster Manager UI, go to Databases, then select the database from the list.

From the Security tab, select the Edit button.

In the Access Control List section, select + Add ACL.

[](/docs/latest/images/rs/screenshots/databases/security-access-control-acl-only.png)

Select the appropriate roles and then save your changes.

If you assign multiple roles to an ACL and a user is authorized by more than one of these roles, their access is determined by the first "matching" rule in the list.

If the first rule gives them read access and the third rule authorizes write access, the user will only be able to read data.

As a result, we recommend ordering roles so that higher access roles appear before roles with more limited access.

## More info

Enable and configure [[operate/rs/security/access-control/ldap/enable-role-based-ldap|role-based LDAP]]
Map LDAP groups to [access control roles](/docs/latest/operate/rs/security/access-control/ldap/map-ldap-groups-to-roles/)
Learn more about Redis Software [[operate/rs/security|security and practices]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page