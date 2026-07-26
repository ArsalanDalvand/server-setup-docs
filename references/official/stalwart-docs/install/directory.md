---
title: "Choosing a directory"
source: "https://stalw.art/docs/install/directory"
source_format: official-html-to-markdown
language: "en"
doc_slug: "install/directory"
category: "install"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Choosing a directory"
---
# Choosing a directory

      

In Stalwart, a directory refers to the authentication backend responsible for managing user identities and access credentials. Directories are essential for validating login attempts, retrieving account-related information, and, in some cases, managing user configuration. Stalwart supports both internal and external directory configurations, giving administrators flexibility in how authentication is handled.

When using the [[auth/backend#internal-directory|internal directory]], all user account data (including usernames, passwords, and quotas) is stored within Stalwart’s own [[storage/data|data store]]. In this setup, Stalwart fully manages user-related data, and administrators can perform all account management tasks directly within the system. This includes creating new accounts, updating credentials, configuring mail quotas, and managing account-level settings. The internal directory is ideal for standalone or small-scale deployments where centralized control and simplicity are important.

Alternatively, Stalwart can be configured to use an [[auth/backend#external-directory|external directory]], which delegates authentication and user information to a third-party system. Supported external directories include LDAP, OpenID Connect, and SQL-based authentication backends. In this configuration, Stalwart relies on the external system for user authentication and account data. However, it does not have the ability to modify user details (such as resetting passwords or creating accounts) within the external directory. All user management must be performed directly in the external system by its respective tools or APIs.

Choosing between an internal or external directory depends on your operational model. The internal directory offers simplicity and full integration for deployments where Stalwart operates as a self-contained platform. On the other hand, using an external directory is recommended for organizations that already maintain a centralized identity infrastructure and require integration with existing systems.

Stalwart’s directory system is flexible and designed to fit a wide range of use cases, from independent setups to enterprise environments with federated authentication. Available authentication backend are:

[[auth/backend/internal|Internal]]: An internal directory that is automatically created and managed by Stalwart. It uses the same database backend as the data store.
[[auth/backend/ldap|LDAP]]: LDAP servers, including OpenLDAP and Active Directory.
[[auth/backend/sql|SQL]]: SQL databases, including PostgreSQL, MySQL and SQLite.
[[auth/backend/oidc|OpenID Connect]]: OpenID Connect servers, including Authentik, Keycloak, etc.