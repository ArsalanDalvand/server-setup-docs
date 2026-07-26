---
title: "Manage user account and profile"
source: "https://redis.io/docs/latest/operate/rc/accounts/user-profile/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/accounts/user-profile"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Manage user account and profile"
---
# Manage user account and profile

Describes the how to manage your user account profile and how to switch between Redis Cloud accounts.

            Redis Cloud
          

When you sign in to the [Redis Cloud console](https://cloud.redis.io/), you use a profile associated with one or more Redis Cloud accounts.

This account has a profile with settings that you can manage using the Profile control located near the top, right corner of the Redis Cloud console:

[](/docs/latest/images/rc/account-selector-single-account.png)

When you open the Profile control, you can:

Review and manage your user account profile.

Sign out from the Redis Cloud console.

Switch between Redis Cloud subscriptions administered by your user account.

## Manage user profile

To review your user profile settings, select User profile from the Profile control.  This displays the User Profile screen:

[](/docs/latest/images/rc/user-profile-settings.png)

This screen contains up to three sections, including:

The User details section includes basic information about your account, including First name, Last name, Job title, Email, and the date the account was created.  The names and job title can be edited; other settings are read-only.

The Password section lets you change the password for accounts created and managed by Redis Cloud.

If you're using single sign-on authentication, you cannot change the password using the User Profile screen.  Such accounts are managed by an identity provider (IdP).  For help changing (or recovering) the passwords for these accounts, consult your identity provider docs.

The Multi-factor authentication (MFA) section lets you [[operate/rc/security/access-control/multi-factor-authentication|manage MFA settings]] for the current user account.

When you activate a mobile device, you can use SMS MFA as a second authentication factor.

To use an authentication app as the factor, you need to activate a mobile device and then use that device to enable the app.

## Sign out

To sign out from the Redis Cloud console, select Logout from the profile control.

## Switch Redis cloud accounts

When your user account is authorized to manage multiple Redis Cloud accounts, each account is displayed in the Profile control.

[](/docs/latest/images/rc/account-selector-switch-account.png)

To switch accounts, select the desired account from the list shown in the Profile control.

Note:
  To create another Redis Cloud account associated with the same email address, you need to [[operate/rc/rc-quickstart|sign up for Redis Cloud]] again with a plus address of the email address. For many email providers, you can do this by adding `+` and any string to the end of your username. For example, if your address is `[email protected]`, enter `[email protected]`.

## Save or discard changes

Use the Discard changes button to cancel user profile setting changes or the Save changes button to save changes.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page