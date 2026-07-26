---
title: "Create and edit database users"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/data-access-control/create-assign-users/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/data-access-control/create-assign-users"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create and edit database users"
---
# Create and edit database users

Create a database user and assign it a role.

            Redis Cloud
          

Before you create a database user, you must [create a data access role](/docs/latest/operate/rc/security/access-control/data-access-control/create-roles/) to assign to that user.

## Create a user

To create a user:

Go to Data Access Control from the [Redis Cloud console](https://cloud.redis.io/#/) menu.

[](/docs/latest/images/rc/data-access-control-menu.png)

Select the Users tab.

[](/docs/latest/images/rc/data-access-control-users-no-users.png)

Select `+` to create a new user.

[](/docs/latest/images/rc/data-access-control-users-add-or-edit.png)

Enter a username in the Username field.

[](/docs/latest/images/rc/data-access-control-users-add.png)

Note:
  An error occurs if a user tries to connect to a memcached database with the username `admin`. Do not use `admin` for a username if the user will be connecting to a memcached database.

Select a [Role](/docs/latest/operate/rc/security/access-control/data-access-control/create-roles/) from the list.

[](/docs/latest/images/rc/data-access-control-users-add-role.png)

Enter and confirm the user's password. ACL user passwords must be between 8 and 128 characters long.

Then, select the check mark to save the user.

[](/docs/latest/images/rc/data-access-control-users-password-and-finish.png)

## Assign roles to existing users

To assign a data access role to an existing user:

Go to Data Access Control from the [Redis Cloud console](https://cloud.redis.io/#/) menu.

[](/docs/latest/images/rc/data-access-control-menu.png)

Select the Users tab.

[](/docs/latest/images/rc/data-access-control-users.png)

Point to the user and select the Edit* icon when it appears.

[](/docs/latest/images/rc/data-access-control-users-add-or-edit.png)

Select a [Role](/docs/latest/operate/rc/security/access-control/data-access-control/create-roles/) from the list.

[](/docs/latest/images/rc/data-access-control-users-add-role.png)

Select the check mark to save the user.

[](/docs/latest/images/rc/data-access-control-users-password-and-finish.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page