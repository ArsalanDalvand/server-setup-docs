---
title: "Default user"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/data-access-control/default-user/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/data-access-control/default-user"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Default user"
---
# Default user

Learn how to change your default user password or turn off access using the default user password.

            Redis Cloud
          

Password-based authentication is a basic but essential Redis security feature. When you create a Redis Cloud database, your database is given a randomly generated password called the Default user password.

This password appears in the Security section of the Configuration tab of the database details screen for Pro databases. For Essentials databases, go to Security > Default user and select Configure to view the default user password.

Use the copy button to copy the password to the clipboard:

[](/docs/latest/images/rc/button-copy.png)

You'll need to use this password whenever you connect to your database using a Redis client. See [Connect to a database](/docs/latest/operate/rc/databases/connect/) for more info.

If you have [[operate/rc/security/database-security/block-public-endpoints|blocked the public endpoint]] for your Redis Cloud Pro subscription, you can also turn on passwordless authentication for the default user. See [[operate/rc/security/database-security/block-public-endpoints#turn-on-passwordless-authentication-for-the-default-user|Block public endpoints]] for more info.

See your [[develop/clients|Redis client's documentation]] to learn how to provide your password when connecting.

## Change password

How you change the default user password depends on your plan.

        Essentials
      

        Pro
      

In the Security section of the Configuration tab, select Configure for the default user.

Enter the new password. Database passwords must be less than 50 characters long.

Select OK to save your changes.

From the database Configuration tab, select Edit.

Under the Security section, enter the new password in the Default user password field. Database passwords must be less than 50 characters long.

Select Save database to update the password.

## Turn off default user

After you set up [[operate/rc/security/access-control/data-access-control/role-based-access-control|role-based access control]] to limit who can access your database, we recommend that you turn off default user access.

How you turn off the default user depends on your plan.

        Essentials
      

        Pro
      

In the Security section of the Configuration tab, select Configure for the default user.

Turn off the Default user switch.

Select OK to save your changes.

From the database Configuration tab, select Edit.

Under the Security section, select the Default User switch to turn it off.

Select Save database.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page