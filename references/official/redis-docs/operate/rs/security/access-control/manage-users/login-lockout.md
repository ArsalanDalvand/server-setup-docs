---
title: "Manage user login"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/manage-users/login-lockout/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/manage-users/login-lockout"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Manage user login"
---
# Manage user login

Manage user login lockout and session timeout.

            Redis Software
          

Redis Software secures user access in a few different ways, including automatically:

Locking user accounts after a series of authentication failures (invalid passwords)

Signing sessions out after a period of inactivity

Here, you learn how to configure the relevant settings.

## User login lockout

By default, after 5 failed login attempts within 15 minutes, the user account is locked for 30 minutes. You can change the user login lockout settings in the Cluster Manager UI or with [`rladmin`](/docs/latest/operate/rs/references/cli-utilities/rladmin/).

### View login lockout settings

You can view the cluster's user login lockout settings from Cluster > Security > Preferences > Lockout threshold in the Cluster Manager UI or with [`rladmin info cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/info/#info-cluster):

```sh
$ rladmin info cluster | grep login_lockout
    login_lockout_counter_reset_after: 900
    login_lockout_duration: 1800
    login_lockout_threshold: 5
```

### Configure user login lockout

To change the user login lockout settings using the Cluster Manager UI:

Go to Cluster > Security > Preferences, then select Edit.

In the Lockout threshold section, make sure the checkbox is selected.

[](/docs/latest/images/rs/screenshots/cluster/security-preferences-lockout-threshold.png)

Configure the following Lockout threshold settings:

Log-in attempts until user is revoked - The number of failed login attempts allowed before the user account is locked.

Time between failed login attempts in seconds, minutes, or hours - The amount of time during which failed login attempts are counted.

For Unlock method, select one of the following:

Locked duration to set how long the user account is locked after excessive failed login attempts.

Only Admin can unlock the user by resetting the password.

Select Save.

### Change allowed login attempts

To change the number of failed login attempts allowed before the user account is locked, use one of the following methods:

[Cluster Manager UI](#configure-user-login-lockout)

[`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/#tune-cluster):

```sh
rladmin tune cluster login_lockout_threshold <integer>
```

For example, to set the lockout threshold to 10 failed login attempts, run:

```sh
rladmin tune cluster login_lockout_threshold 10
```

If you set the lockout threshold to 0, it turns off account lockout, and the cluster settings show `login_lockout_threshold: disabled`.

```sh
rladmin tune cluster login_lockout_threshold 0
```

### Change time before login attempts reset

To change the amount of time during which failed login attempts are counted, use one of the following methods:

[Cluster Manager UI](#configure-user-login-lockout)

[`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/#tune-cluster):

```sh
rladmin tune cluster login_lockout_counter_reset_after <seconds>
```

For example, to set the lockout reset to 1 hour, run:

```sh
rladmin tune cluster login_lockout_counter_reset_after 3600
```

### Change login lockout duration

To change the amount of time that the user account is locked after excessive failed login attempts, use one of the following methods:

[Cluster Manager UI](#configure-user-login-lockout)

[`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/#tune-cluster):

```sh
rladmin tune cluster login_lockout_duration <login_lockout_duration>
```

For example, to set the lockout duration to 1 hour, run:

```sh
rladmin tune cluster login_lockout_duration 3600
```

If you set the lockout duration to 0, then the account can be unlocked only when an administrator changes the account's password.

```sh
rladmin tune cluster login_lockout_duration 0
```

The cluster settings now show `login_lockout_duration: admin-release`.

### Unlock locked user accounts

To unlock a locked user account as an admin, use one of the following methods:

        Cluster Manager UI
      

        rladmin
      

        REST API
      

To unlock a user account in the Cluster Manager UI:

Go to Access Control > Users. Locked users have a "User is locked out" label:

Point to the "User is locked out" label, then click Reset to unlock when it appears:

In the Reset user password dialog, enter a new password for the user:

Select Save to reset the user's password and unlock their account.

To unlock a user account or reset a user password with `rladmin`, run the [`rladmin cluster reset_password`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/reset_password/) command:

```sh
rladmin cluster reset_password <user_email>
```

To unlock a user account or reset a user password with the REST API, use [`PUT /v1/users`](/docs/latest/operate/rs/references/rest-api/requests/users/#put-user):

```sh
PUT /v1/users
{"password": "<new_password>"}
```

### Turn off login lockout

To turn off user login lockout and allow unlimited login attempts, use one of the following methods:

        Cluster Manager UI
      

        rladmin
      

To turn off user login lockout using the Cluster Manager UI:

Go to Cluster > Security > Preferences, then select Edit.

Clear the Lockout threshold checkbox.

Select Save.

To turn off user login lockout using `rladmin`, run the [`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/#tune-cluster) command:

```sh
rladmin tune cluster login_lockout_threshold 0
```

The cluster settings show `login_lockout_threshold: disabled`.

## Configure session timeout

The Redis Software Cluster Manager UI supports session timeouts. By default, users are automatically logged out after 15 minutes of inactivity.

To customize the session timeout, use one of the following methods:

        Cluster Manager UI
      

        rladmin
      

To configure the session timeout using the Cluster Manager UI:

Go to Cluster > Security > Preferences, then select Edit.

For Session timeout, select minutes or hours from the list and enter the timeout value.

Select Save.

To configure the session timeout using `rladmin`, run the [`rladmin cluster config`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/config/) command:

```sh
rladmin cluster config cm_session_timeout_minutes <number_of_min>
```

The `<number_of_min>` is the number of minutes after which sessions will time out.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page