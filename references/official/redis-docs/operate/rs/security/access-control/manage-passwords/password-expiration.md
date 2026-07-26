---
title: "Configure password expiration"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/manage-passwords/password-expiration/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/manage-passwords/password-expiration"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Configure password expiration"
---
# Configure password expiration

Configure password expiration to enforce expiration of a user's password after a specified number of days.

            Redis Software
          

## Enable password expiration

Warning:
  Password expiration is calculated from the time the password was last updated, not from when the policy is enabled. Passwords that were set long enough ago to already be expired will immediately be locked out when you enable this policy. Before enabling password expiration, verify all user passwords have been updated recently enough to avoid immediate lockouts.

To enforce an expiration of a user's password after a specified number of days:

Use the Cluster Manager UI:

Go to Cluster > Security > Preferences, then select Edit.

In the Password section, turn on Expiration.

Enter the number of days before passwords expire.

Select Save.

Use the `cluster` endpoint of the REST API

```REST
PUT https://[host][:port]/v1/cluster
{"password_expiration_duration":<number_of_days>}
```

## Deactivate password expiration

To deactivate password expiration:

Use the Cluster Manager UI:

Go to Cluster > Security > Preferences, then select Edit.

In the Password section, turn off Expiration.

Select Save.

Use the `cluster` REST API endpoint to set `password_expiration_duration` to `0` (zero).

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page