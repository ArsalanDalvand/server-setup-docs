---
title: "Change the password hashing algorithm"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/manage-passwords/password-hashing-algorithm/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/manage-passwords/password-hashing-algorithm"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Change the password hashing algorithm"
---
# Change the password hashing algorithm

Change the password hashing algorithm for user passwords in a Redis Software cluster.

            Redis Software
          

Redis Software securely stores all user passwords using a cryptographic hash function. The default password hashing algorithm is `SHA-256`, but `PBKDF2` is also supported as of Redis Software version 7.8.6-13.

You can change the password hashing algorithm using [`rladmin`](/docs/latest/operate/rs/references/cli-utilities/rladmin/) or the [REST API](/docs/latest/operate/rs/references/rest-api/). When you change the password hashing algorithm, the cluster rehashes the administrator password and passwords for all users, including default users.

## Command-line method

To change the password hashing algorithm from the command line, run [`rladmin cluster change_password_hashing_algorithm`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/change_password_hashing_algorithm/):

```sh
rladmin cluster change_password_hashing_algorithm PBKDF2
```

## REST API method

You can [change the password hashing algorithm](/docs/latest/operate/rs/references/rest-api/requests/cluster/change_password_hashing_algorithm/#patch-change-password-hashing-algorithm) using a REST API request:

```sh
PATCH /v1/cluster/change_password_hashing_algorithm
{ "algorithm": "PBKDF2" }
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page