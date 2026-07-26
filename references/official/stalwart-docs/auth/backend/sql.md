---
title: "SQL Database"
source: "https://stalw.art/docs/auth/backend/sql"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/backend/sql"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SQL Database"
---
# SQL Database

      

Stalwart can authenticate users and look up account metadata against an SQL database such as PostgreSQL, MySQL, or SQLite. This makes it possible to reuse an existing SQL-backed user directory, or to manage accounts in a relational schema that other applications already share.

An SQL directory is configured through the SQL variant of the [[ref/object/directory|Directory]] object (found in the WebUI under  Settings ›  Authentication › Directories).

## Configuration

[Section titled “Configuration”](#configuration)

The main fields on the SQL variant of the Directory object are:

[[ref/object/directory#description|`description`]]: a human-readable description of this directory.
[[ref/object/directory#store|`store`]]: the backend where the account tables live. The `SqlAuthStore` variant selects between `Default` (the server’s data store, when that is itself SQL), `PostgreSql`, `MySql`, and `Sqlite`. Each backend variant carries its own connection parameters (host, port, database, username, and secret) detailed on the [[ref/object/directory|Directory reference page]].

### Directory queries

[Section titled “Directory queries”](#directory-queries)

The SQL variant runs the following queries against the configured store; all of them use positional parameters (`$1`, `$2`, …) and sensible defaults that can be overridden per deployment:

[[ref/object/directory#querylogin|`queryLogin`]]: resolves account details by login value. Default `"SELECT name, secret, description, type FROM accounts WHERE name = $1"`.
[[ref/object/directory#queryrecipient|`queryRecipient`]]: resolves account details by recipient email address or alias. Default `"SELECT name, secret, description, type FROM accounts WHERE name = $1 AND active = true"`.
[[ref/object/directory#querymemberof|`queryMemberOf`]]: returns the groups an account is a member of. Default `"SELECT member_of FROM group_members WHERE name = $1"`.
[[ref/object/directory#queryemailaliases|`queryEmailAliases`]]: returns the email aliases of an account. Default `"SELECT address FROM emails WHERE name = $1"`.

### Column mappings

[Section titled “Column mappings”](#column-mappings)

Column names in the database are mapped to account fields through the following fields on the SQL variant:

[[ref/object/directory#columnemail|`columnEmail`]]: column holding the account login / primary address. Default `"name"`.
[[ref/object/directory#columnsecret|`columnSecret`]]: column holding the password hash. Default `"secret"`. Hashes must be in a [[auth/authentication/password|supported format]]; plain text is possible but not recommended.
[[ref/object/directory#columnclass|`columnClass`]]: column holding the account kind (`individual`, `person`, or `group`). Default `"type"`.
[[ref/object/directory#columndescription|`columnDescription`]]: column holding the account’s full name or description. Default `"description"`.

There is no column mapping for a per-account disk quota: quotas are held on the [[ref/object/account|Account]] and [[ref/object/tenant|Tenant]] objects rather than read from the SQL directory (see [[auth/authorization/quotas|Quotas]]).

For example:

```
{  "@type": "Sql",  "description": "External SQL directory",  "store": {    "@type": "Default"  },  "columnEmail": "name",  "columnSecret": "secret",  "columnDescription": "description",  "columnClass": "type"}
```

## Sample directory schema

[Section titled “Sample directory schema”](#sample-directory-schema)

The following schema is a reference example for an SQL-backed directory. It may need to be adapted for specific deployments; it is not intended to be used as-is.

### Table schema

[Section titled “Table schema”](#table-schema)

#### SQLite

[Section titled “SQLite”](#sqlite)

```
CREATE TABLE accounts (name TEXT PRIMARY KEY, secret TEXT, description TEXT, type TEXT NOT NULL, active BOOLEAN DEFAULT 1)CREATE TABLE group_members (name TEXT NOT NULL, member_of TEXT NOT NULL, PRIMARY KEY (name, member_of))CREATE TABLE emails (name TEXT NOT NULL, address TEXT NOT NULL, type TEXT, PRIMARY KEY (name, address))
```

#### PostgreSQL

[Section titled “PostgreSQL”](#postgresql)

```
CREATE TABLE accounts (name TEXT PRIMARY KEY, secret TEXT, description TEXT, type TEXT NOT NULL, active BOOLEAN DEFAULT true);CREATE TABLE group_members (name TEXT NOT NULL, member_of TEXT NOT NULL, PRIMARY KEY (name, member_of));CREATE TABLE emails (name TEXT NOT NULL, address TEXT NOT NULL, type TEXT, PRIMARY KEY (name, address));
```

#### MySQL

[Section titled “MySQL”](#mysql)

```
CREATE TABLE accounts (name VARCHAR(32) PRIMARY KEY, secret VARCHAR(1024), description VARCHAR(1024), type VARCHAR(32) NOT NULL, active BOOLEAN DEFAULT 1);CREATE TABLE group_members (name VARCHAR(32) NOT NULL, member_of VARCHAR(32) NOT NULL, PRIMARY KEY (name, member_of));CREATE TABLE emails (name VARCHAR(32) NOT NULL, address VARCHAR(128) NOT NULL, type VARCHAR(32), PRIMARY KEY (name, address));
```

### Creating user accounts

[Section titled “Creating user accounts”](#creating-user-accounts)

Before inserting an account, hash the password. One way is via `openssl`, for example to produce a SHA-512 crypt hash:

Terminal window

```
$ openssl passwd -6
```

Once the hashed secret is available, insert the account and its primary address:

```
INSERT INTO accounts (name, secret, description, type) VALUES ('<ACCOUNT_NAME>', '<HASHED_SECRET>', '<ACCOUNT_FULL_NAME>', 'individual')INSERT INTO emails (name, address, type) VALUES ('<ACCOUNT_NAME>', '<PRIMARY_EMAIL_ADDRESS>', 'primary')
```

Replace:

`<ACCOUNT_NAME>` with the account login name, for example `john`.
`<HASHED_SECRET>` with the hashed password produced above.
`<ACCOUNT_FULL_NAME>` with the account’s full name, for example `John Doe`.
`<PRIMARY_EMAIL_ADDRESS>` with the primary address, for example `[email protected]`.

### Adding an email alias

[Section titled “Adding an email alias”](#adding-an-email-alias)

To add an alias to an existing account:

```
INSERT INTO emails (name, address, type) VALUES ('<ACCOUNT_NAME>', '<EMAIL_ALIAS>', 'alias')
```

For example, to add the aliases `[email protected]` and `[email protected]` to the account `john`:

```
INSERT INTO emails (name, address, type) VALUES ('john', '[email protected]', 'alias')INSERT INTO emails (name, address, type) VALUES ('john', '[email protected]', 'alias')
```

Alternatively, the `postmaster` account can be configured as the [[mta/inbound/rcpt#catch-all-addresses|catch-all recipient]] for `example.org` by adding `@example.org` as an alias:

```
INSERT INTO emails (name, address, type) VALUES ('postmaster', '@example.org', 'alias')
```

### Creating group accounts

[Section titled “Creating group accounts”](#creating-group-accounts)

```
INSERT INTO accounts (name, description, type) VALUES ('<GROUP_NAME>', '<GROUP_DESCRIPTION>', 'group')
```

Replace `<GROUP_NAME>` with the group name and `<GROUP_DESCRIPTION>` with a human-readable description.

### Adding members to a group

[Section titled “Adding members to a group”](#adding-members-to-a-group)

```
INSERT INTO group_members (name, member_of) VALUES ('<ACCOUNT_NAME>', '<GROUP_NAME>')
```