---
title: "Set password policies"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/manage-passwords/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/manage-passwords"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Set password policies"
---
# Set password policies

Set password policies.

            Redis Software
          

Redis Software provides several ways to manage the passwords of local accounts, including:

[Password complexity rules](/docs/latest/operate/rs/security/access-control/manage-passwords/password-complexity-rules/)

[[operate/rs/security/access-control/manage-passwords/password-expiration|Password expiration]]

[[operate/rs/security/access-control/manage-passwords/rotate-passwords|Password rotation]]

You can also manage a user's ability to [[operate/rs/security/access-control/manage-users/login-lockout#user-login-lockout|sign in]] and control [[operate/rs/security/access-control/manage-users/login-lockout#session-timeout|session timeout]].

To enforce more advanced password policies, we recommend using [[operate/rs/security/access-control/ldap|LDAP integration]] with an external identity provider, such as Active Directory.

Note:
  Redis Software securely stores all user passwords using a cryptographic hash function. The default password hashing algorithm is `SHA-256`, but you can [[operate/rs/security/access-control/manage-passwords/password-hashing-algorithm|change the password hashing algorithm]] to `PBKDF2` as of Redis Software version 7.8.6-13.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)