---
title: "MySQL Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/mysql.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/mysql"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MySQL Notification Settings — SILO (MinIO Object Storage)"
---
# MySQL Notification Settings

This page documents settings for configuring a MYSQL service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-mysql#minio-bucket-notifications-publish-mysql|Publish Events to MySQL]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple MYSQL Targets

You can specify multiple MySQL service endpoints by appending a unique identifier `_ID` for each set of related MySQL settings on to the top level key.

### Examples

The following commands set two distinct MySQL service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_MYSQL_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_MYSQL_DSN_STRING_PRIMARY="username:password@tcp(mysql.example.com:3306)/miniodb"
export MINIO_NOTIFY_MYSQL_TABLE_PRIMARY="minioevents"
export MINIO_NOTIFY_MYSQL_FORMAT_PRIMARY="namespace"

export MINIO_NOTIFY_MYSQL_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_MYSQL_DSN_STRING_SECONDARY="username:password@tcp(mysql.example.com:3306)/miniodb"
export MINIO_NOTIFY_MYSQL_TABLE_SECONDARY="minioevents"
export MINIO_NOTIFY_MYSQL_FORMAT_SECONDARY="namespace"
```

With these settings, [`MINIO_NOTIFY_MYSQL_ENABLE_PRIMARY`](#envvar.MINIO_NOTIFY_MYSQL_ENABLE) indicates the environment variable is associated to a MySQL service endpoint with ID of `PRIMARY`.

Configuration Settings

```
mc admin config set notify_mysql:primary \
   dsn_string="username:password@tcp(mysql.example.com:3306)/miniodb"
   table="minioevents" \
   format="namespace" \
   [ARGUMENT=VALUE ...]

mc admin config set notify_mysql:secondary \
   dsn_string="username:password@tcp(mysql.example.com:3306)/miniodb"
   table="minioevents" \
   format="namespace" \
   [ARGUMENT=VALUE ...]
```

## Settings

### Enable

Required

Environment Variables

MINIO_NOTIFY_MYSQL_ENABLE[](#envvar.MINIO_NOTIFY_MYSQL_ENABLE)

Specify `on` to enable publishing bucket notifications to a MySQL service endpoint.

Defaults to `off`.

Requires specifying the following additional environment variables if set to `on`:

[`MINIO_NOTIFY_MYSQL_DSN_STRING`](#envvar.MINIO_NOTIFY_MYSQL_DSN_STRING)

[`MINIO_NOTIFY_MYSQL_TABLE`](#envvar.MINIO_NOTIFY_MYSQL_TABLE)

[`MINIO_NOTIFY_MYSQL_FORMAT`](#envvar.MINIO_NOTIFY_MYSQL_FORMAT)

Configuration Settings

notify_mysql[](#mc-conf.notify_mysql)

The top-level configuration key for defining an MySQL service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an MySQL service endpoint.
The following arguments are required for each target:

[`dsn_string`](#mc-conf.notify_mysql.dsn_string)

[`table`](#mc-conf.notify_mysql.table)

[`format`](#mc-conf.notify_mysql.format)

Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_mysql \
  dsn_string="username:password@tcp(mysql.example.com:3306)/miniodb"
  table="minioevents" \
  format="namespace" \
  [ARGUMENT="VALUE"] ... \
```

### Data Source Name (DSN) String

Required

Environment Variable

MINIO_NOTIFY_MYSQL_DSN_STRING[](#envvar.MINIO_NOTIFY_MYSQL_DSN_STRING)

Configuration Setting

notify_mysql dsn_string[](#mc-conf.notify_mysql.dsn_string)

Specify the data source name (DSN) of the MySQL service endpoint. MinIO expects the following format:

`<user>:<password>@tcp(<host>:<port>)/<database>`

For example:

`"username:password@tcp(mysql.example.com:3306)/miniodb"`

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Table

Required

Environment Variable

MINIO_NOTIFY_MYSQL_TABLE[](#envvar.MINIO_NOTIFY_MYSQL_TABLE)

Configuration Setting

notify_mysql table[](#mc-conf.notify_mysql.table)

Specify the name of the MySQL table to which MinIO publishes event notifications.

### Format

Required

Environment Variable

MINIO_NOTIFY_MYSQL_FORMAT[](#envvar.MINIO_NOTIFY_MYSQL_FORMAT)

Configuration Setting

notify_mysql format[](#mc-conf.notify_mysql.format)

Specify the format of event data written to the MySQL service endpoint.
MinIO supports the following values:

`namespace`

For each bucket event, MinIO creates a JSON document with the bucket and object name from the event as the document ID and the actual event as part of the document body.
Additional updates to that object modify the existing table entry for that object.
Similarly, deleting the object also deletes the corresponding table entry.

`access`

For each bucket event, MinIO creates a JSON document with the event details and appends it to the table with a MySQL-generated random ID.
Additional updates to an object result in new index entries,    and existing entries remain unmodified.

### Max Open Connections

Optional

Environment Variable

MINIO_NOTIFY_MYSQL_MAX_OPEN_CONNECTIONS[](#envvar.MINIO_NOTIFY_MYSQL_MAX_OPEN_CONNECTIONS)

Configuration Setting

notify_mysql max_open_connections[](#mc-conf.notify_mysql.max_open_connections)

Specify the maximum number of open connections to the MySQL database.

Defaults to `2`.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_MYSQL_QUEUE_DIR[](#envvar.MINIO_NOTIFY_MYSQL_QUEUE_DIR)

Configuration Setting

notify_mysql queue_dir[](#mc-conf.notify_mysql.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the MySQL server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_MYSQL_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_MYSQL_QUEUE_LIMIT)

Configuration Setting

notify_mysql queue_limit[](#mc-conf.notify_mysql.queue_limit)

Specify the maximum limit for undelivered messages. Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_MYSQL_COMMENT[](#envvar.MINIO_NOTIFY_MYSQL_COMMENT)

Configuration Setting

notify_mysql comment[](#mc-conf.notify_mysql.comment)

Specify a comment to associate with the MySQL configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).