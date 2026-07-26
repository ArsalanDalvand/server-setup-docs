---
title: "Create users"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/create-users/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/create-users"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create users"
---
# Create users

Create users and assign access control roles.

            Redis Software
          

## Prerequisites

Before you create other users:

Review the [[operate/rs/security/access-control|access control overview]] to learn how to use role-based access control (RBAC) to manage users' cluster access and database access.

Create roles you can assign to users. See [Create roles with cluster access only](/docs/latest/operate/rs/security/access-control/create-cluster-roles/), [Create roles with database access only](/docs/latest/operate/rs/security/access-control/create-db-roles/), or [Create roles with combined access](/docs/latest/operate/rs/security/access-control/create-combined-roles/) for instructions.

## Add users

To add a user to the cluster:

From the Access Control > Users tab in the Cluster Manager UI, select + Add user.

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/users-screen.png)

Enter the name, email, and password of the new user.

Note:

Usernames must contain ASCII characters only.

To use [[operate/rs/security/access-control/saml-sso|single sign-on (SSO)]], users must have email addresses.

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/create-user-panel.png)

Select the Alerts the user should receive by email:

Receive alerts for databases - The alerts that are enabled for the selected databases will be sent to the user. Choose All databases or Customize to select the individual databases to send alerts for.

Receive cluster alerts - The alerts that are enabled for the cluster in Cluster > Alerts Settings are sent to the user.

Click Add permissions.

Assign a Role to the user to grant permissions for cluster management and data access.

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/create-user-role-select.png)

Click Save user.

## Assign roles to users

Assign a role, associated with specific databases and access control lists (ACLs), to a user to grant database access:

From the Access Control > Users tab in the Cluster Manager UI, select an existing user from the list of users.

Click the Edit button.

In the User roles section, click Edit.

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/edit-user-roles.png)

Select a role to assign to the user.

[](/docs/latest/images/rs/screenshots/access-control/7-22-updates/edit-user-select-role.png)

Click Done to close the Roles dialog.

Click Save.

## Next steps

Depending on the type of the user's assigned role (cluster management role or data access role), the user can now:

[Connect to a database](/docs/latest/operate/rs/databases/connect/) associated with the role and run limited Redis commands, depending on the role's Redis ACLs.

Sign in to the Redis Software Cluster Manager UI.

Make a [REST API](/docs/latest/operate/rs/references/rest-api/) request.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page