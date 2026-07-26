---
title: "Access management"
source: "https://redis.io/docs/latest/operate/rc/security/access-control/access-management/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/access-control/access-management"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Access management"
---
# Access management

Control who can make changes to your databases using the Redis Cloud console.

            Redis Cloud
          

The Access management screen helps you manage:

The team of users allowed to access your subscription and its databases.
The API keys that authenticate application access to your account.
[[operate/rc/security/access-control/saml-sso|Single sign-on (SSO) with SAML]].

Here, you learn how to manage your team's users and control their level of access.

For help managing API keys, see [[operate/rc/api/get-started/manage-api-keys|Manage API keys]].

## Manage team access

The Team tab lets you manage the people allowed to access your account. Each authorized person is assigned to a role that specifies their privileges.

[](/docs/latest/images/rc/access-management-team-tab.png)

The list contains one entry summarizing the team settings for each user in your team. By default, the list includes the account owner.

The list includes several buttons and icons to help you manage the list:

Icon
Description

[](/docs/latest/images/rc/icon-add.png#no-click)

The Add button lets you add members to your team

[](/docs/latest/images/rc/icon-edit.png#no-click)

The Edit button lets you edit the settings for the selected team member

[](/docs/latest/images/rc/icon-delete-teal.png#no-click)

The Delete button lets you remove members from your team

[](/docs/latest/images/rc/icon-list-sort-asc.png#no-click)
[](/docs/latest/images/rc/icon-list-sort-desc.png#no-click)

The Sort ascending and Sort descending icons display the list according to the selected order

You can also use the list search to find a specific user or filter by Role, User Type, or Options.

### Add user

When you add a member to your team, the Add user dialog appears.

[](/docs/latest/images/rc/access-mgmt-add-user-dialog.png)

Use the dialog to specify these values.

Setting
Description

First name
First name of the user displayed in the Redis Cloud console and in email messages

Last name
Last name of the user displayed in the Redis Cloud console and in email messages

Role
The role identifies their subscription and account privileges.  For details, see [Team management roles](#team-management-roles).

Email
The address used for alerts and other email messages regarding the account

Alert emails
Enable to be notified when subscription databases cross certain thresholds, such as exceeding memory limits or latency requirements

Operational emails
Notifications about subscription and database changes, such as creating or deleting a database, and [[operate/rc/subscriptions/maintenance|subscription and database maintenance]]

Billing emails
Notifications about billing, such as when bills are issued and paid

Multi-factor authentication
Whether MFA is enabled for the member.  This is deactivated when members have not enabled or confirmed MFA in their user profile settings.

Use the Add user button to save your new team member details.

Redis will send an activation email to the user once their details are saved. After following the activation link, they can sign in.

### Edit user

To edit user team details, select the user from the list and then select the Edit button.
The Edit user dialog displays the details you can change.

[](/docs/latest/images/rc/access-mgmt-edit-user-dialog.png)

You can change any detail except the team member's email address.

Select Save user to save your changes.

### Delete user

To remove a member from your team, select them from the list and then select the Delete button. A confirmation dialog appears.

[](/docs/latest/images/rc/access-management-delete-user-dialog.png)

Select Delete user to confirm removal. This is a permanent action that cannot be undone.

## Team management roles

Each team member is assigned a role that identifies their privileges and limits their activities in the Redis Cloud console.

Roles and responsibilities are:

Owner can view, create, and edit any settings in the account.

Each subscription must have at least one account owner. Accounts can have multiple owners.

Owners can also manage subscriptions, databases, and API keys.

Billing Admin can view and edit settings related to billing and payments.

Billing Admins can add and remove payment methods and change the payment method for a subscription, but they cannot change any other subscription or database settings.

Manager can view, create, and edit any setting in the subscription.

Managers can change subscription costs and change payment methods associated with a subscription, but they cannot add or remove available payment methods.

Member can view, create, and edit databases in ways that do not impact costs.

Members cannot create databases or edit databases in ways that impact costs.

Viewer can view all databases and their configurations, including database secrets.

Logs viewer can not access the Redis Cloud console. They are only allowed access to the [[operate/rc/api|Redis Cloud API]] [[operate/rc/api/api-reference#tag/Account/operation/getAccountSystemLogs|`GET logs/`]] endpoint.

This table shows each role's ability to perform common tasks.

Task
Owner
Billing Admin
Manager
Member
Viewer
Logs Viewer

Access management
â Yes
â No
â No
â No
â No
â No

Account settings
â Yes
â Yes[1](#table-note-1)
â No
â No
â No
â No

Billing & payments
â Yes
â Yes
â No[2](#table-note-2)
â No
â No
â No

Create subscription
â Yes
â No
â Yes
â No
â No
â No

Edit subscription
â Yes
â Yes[3](#table-note-3)
â Yes
â No
â No
â No

Create database (affects cost)
â Yes
â No
â Yes
â No
â No
â No

Edit database (affects cost)
â Yes
â No
â Yes
â No
â No
â No

Create database (no cost impact)
â Yes
â No
â Yes
â No
â No
â No

Edit database (no cost impact)
â Yes
â No
â Yes
â Yes
â No
â No

View subscription
â Yes
â Yes
â Yes
â Yes
â Yes
â No

View database
â Yes
â Yes[4](#table-note-4)
â Yes
â Yes
â Yes
â No

Use the [[operate/rc/api|REST API]]
â Yes
â Yes[5](#table-note-5)
â No
â No
â Yes[6](#table-note-6)
â Yes[7](#table-note-7)

Billing Admins can only edit the account billing address in Account Settings.

Managers are able to view existing payment methods and assign them as payment methods for subscriptions, but they cannot add or remove payment methods.

Billing Admins can only change the payment method when editing a subscription.

Billing Admins can see the list of databases, but can not see database details, including connection details.

Billing admins can use REST API for billing data.

Viewers can use the REST API for GET requests, but cannot modify subscription or database details.

Logs viewers can only use the [[operate/rc/api/api-reference#tag/Account/operation/getAccountSystemLogs|`GET logs/`]] endpoint of the REST API.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page