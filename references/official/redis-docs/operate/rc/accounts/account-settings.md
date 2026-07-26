---
title: "Manage account settings"
source: "https://redis.io/docs/latest/operate/rc/accounts/account-settings/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/accounts/account-settings"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Manage account settings"
---
# Manage account settings

Describes the settings for a Redis Cloud account.

            Redis Cloud
          

To review or manage the settings associated with your Redis Cloud account, sign in to the [Redis Cloud console](https://cloud.redis.io/) and then select Account Settings from the menu.

This displays the Account Settings screen:

[](/docs/latest/images/rc/account-settings-account-tab.png)

The available tabs depend on your account, and may include:

The Account tab displays basic information associated with your account, including general info, address details, time zone setting, security settings, and provider integration details.

The Cloud Account tab is displayed for accounts with Redis Cloud Bring your own Cloud enabled. To learn more, see [Bring your own Cloud](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/).

The Integrations tab lets you manage certain integrations.

You can change some settings by selecting Edit. For help changing other settings, [contact Support](https://redis.io/support/).

[](/docs/latest/images/rc/button-database-edit.png)

## Redis account info settings

The Redis Account Info section provides basic details about your account, including:

Setting
Description

Redis account name
Name associated with the Redis Cloud account

Redis account number
Internal ID of the Redis account

Date created
Date the user's Redis Cloud account was created, which may differ from the organization account creation date

Last updated
Date of the last administrative change to the owner's account, typically reflects access changes or other administrative updates

Select Edit to change the Redis Account name.

[](/docs/latest/images/rc/button-database-edit.png)

After changing the account name, use the Save changes button to save changes or Discard changes to revert them.

[](/docs/latest/images/rc/account-settings-buttons-save-discard.png)

You cannot change the email address associated with a Redis Cloud account.  Instead, create a new account with the updated email address, assign it as an administrator to the organization account, and then use the new account to delete the account with the invalid email address.

## Contacts & Business information

The Contacts & Business information section shows the company name and business address associated with the current Redis Cloud account. Redis uses the company name and business address for invoice and tax purposes.

In addition, this section may include fields unique to your location.  For example, certain regions require tax IDs or other regulatory details.

Select Edit to change the account's company name and business address.

[](/docs/latest/images/rc/button-database-edit.png)
[](/docs/latest/images/rc/account-settings-change-business-info.png)

When you change your business address, you must approve the use of the information in this section for communication and billing purposes before you can save. After approving, select Save changes to save your changes or Discard changes to revert them.

[](/docs/latest/images/rc/account-settings-buttons-save-discard.png)

## Security settings

The Security section lets you:

Manage [[operate/rc/security/access-control/multi-factor-authentication|multi-factor authentication]] (MFA) for your Redis Cloud account.

Download the [[operate/rc/security/database-security/tls-ssl#download-certificates|Redis Cloud certificate authority (CA) bundle]] as a [PEM](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) file, which contains the certificates associated with your Redis Cloud account.

## Time zone settings

To change the time zone settings, select Edit and then select the desired time zone from the Time zone drop-down.

Select Save changes to save your changes or Discard changes to revert them.

[](/docs/latest/images/rc/account-settings-buttons-save-discard.png)

## Integration settings

The Integrations tab includes settings that help you manage integrations of your Redis Cloud account with other services.

For more details, see:

[Use the Redis Sink Confluent Connector](/docs/latest/integrate/confluent-with-redis-cloud/)
[Prometheus and Grafana with Redis Cloud](/docs/latest/integrate/prometheus-with-redis-cloud/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page