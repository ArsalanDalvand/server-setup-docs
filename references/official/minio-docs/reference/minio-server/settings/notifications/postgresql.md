---
title: "PostgreSQL Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/postgresql.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/postgresql"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "PostgreSQL Notification Settings — SILO (MinIO Object Storage)"
---
# PostgreSQL Notification Settings

This page documents settings for configuring an POSTGRES service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-postgresql#minio-bucket-notifications-publish-postgresql|Publish Events to PostgreSQL]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple PostgreSQL Targets

You can specify multiple PostgreSQL service endpoints by appending a unique identifier `_ID` for each set of related PostgreSQL settings on to the top level key.
For example, the following commands set two distinct PostgreSQL service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_POSTGRES_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_POSTGRES_CONNECTION_STRING_PRIMARY="host=postgresql-endpoint.example.net port=4222..."
export MINIO_NOTIFY_POSTGRES_TABLE_PRIMARY="minioevents"
export MINIO_NOTIFY_POSTGRES_FORMAT_PRIMARY="namespace"

export MINIO_NOTIFY_POSTGRES_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_POSTGRES_CONNECTION_STRING_SECONDARY="host=postgresql-endpoint.example.net port=4222..."
export MINIO_NOTIFY_POSTGRES_TABLE_SECONDARY="minioevents"
export MINIO_NOTIFY_POSTGRES_FORMAT_SECONDARY="namespace"
```

Configuration Settings

```
mc admin config set notify_postgres:primary \
   connection_string="host=postgresql.example.com port=5432..."
   table="minioevents" \
   format="namespace" \
   [ARGUMENT=VALUE ...]

mc admin config set notify_postgres:secondary \
   connection_string="host=postgresql.example.com port=5432..."
   table="minioevents" \
   format="namespace" \
   [ARGUMENT=VALUE ...]
```

With these settings, [`MINIO_NOTIFY_POSTGRES_ENABLE_PRIMARY`](#envvar.MINIO_NOTIFY_POSTGRES_ENABLE) indicates the environment variable is associated to an PostgreSQL service endpoint with ID of `PRIMARY`.

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_POSTGRES_ENABLE[](#envvar.MINIO_NOTIFY_POSTGRES_ENABLE)

Specify `on` to enable publishing bucket notifications to a PostgreSQL service endpoint.

Defaults to `off`.

Requires specifying the following additional environment variables if set to `on`:

[`MINIO_NOTIFY_POSTGRES_CONNECTION_STRING`](#envvar.MINIO_NOTIFY_POSTGRES_CONNECTION_STRING)

[`MINIO_NOTIFY_POSTGRES_TABLE`](#envvar.MINIO_NOTIFY_POSTGRES_TABLE)

[`MINIO_NOTIFY_POSTGRES_FORMAT`](#envvar.MINIO_NOTIFY_POSTGRES_FORMAT)

Configuration Setting

notify_postgres[](#mc-conf.notify_postgres)

The top-level configuration key for defining an PostgreSQL service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an PostgreSQL service endpoint.
The following arguments are required for each target:

[`connection_string`](#mc-conf.notify_postgres.connection_string)

[`table`](#mc-conf.notify_postgres.table)

[`format`](#mc-conf.notify_postgres.format)

Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_postgres                            \
  connection_string="host=postgresql.example.com port=5432..." \
  table="minioevents"                                          \
  format="namespace"                                           \
  [ARGUMENT="VALUE"] ...
```

### Connection String

Required

Environment Variable

MINIO_NOTIFY_POSTGRES_CONNECTION_STRING[](#envvar.MINIO_NOTIFY_POSTGRES_CONNECTION_STRING)

Configuration Setting

notify_postgres connection_string[](#mc-conf.notify_postgres.connection_string)

Specify the [URI connection string](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING) of the PostgreSQL service endpoint.
MinIO supports `key=value` format for the PostgreSQL connection string.
For example:

`"host=https://postgresql.example.com port=5432 ..."`

For more complete documentation on supported PostgreSQL connection string parameters, see the [PostgreSQL Connection Strings documentation](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING).

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Table

Required

Environment Variable

MINIO_NOTIFY_POSTGRES_TABLE[](#envvar.MINIO_NOTIFY_POSTGRES_TABLE)

Configuration Setting

notify_postgres table[](#mc-conf.notify_postgres.table)

Specify the name of the PostgreSQL table to which MinIO publishes event notifications.

### Format

Required

Environment Variable

MINIO_NOTIFY_POSTGRES_FORMAT[](#envvar.MINIO_NOTIFY_POSTGRES_FORMAT)

Configuration Setting

notify_postgres format[](#mc-conf.notify_postgres.format)

Specify the format of event data written to the PostgreSQL service endpoint.
MinIO supports the following values:

`namespace`

For each bucket event, MinIO creates a JSON document with the bucket and object name from the event as the document ID and the actual event as part of the document body.
Additional updates to that object modify the existing table entry for that object.
Similarly, deleting the object also deletes the corresponding table entry.

`access`

For each bucket event, MinIO creates a JSON document with the event details and appends it to the table with a PostgreSQL-generated random ID.
Additional updates to an object result in new index entries,    and existing entries remain unmodified.

### Max Open Connections

Optional

Environment Variable

MINIO_NOTIFY_POSTGRES_MAX_OPEN_CONNECTIONS[](#envvar.MINIO_NOTIFY_POSTGRES_MAX_OPEN_CONNECTIONS)

Configuration Setting

notify_postgres max_open_connections[](#mc-conf.notify_postgres.max_open_connections)

Specify the maximum number of open connections to the PostgreSQL database.

Defaults to `2`.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_POSTGRES_QUEUE_DIR[](#envvar.MINIO_NOTIFY_POSTGRES_QUEUE_DIR)

Configuration Setting

notify_postgres queue_dir[](#mc-conf.notify_postgres.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the PostgreSQL server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_POSTGRES_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_POSTGRES_QUEUE_LIMIT)

Configuration Setting

notify_postgres queue_limit[](#mc-conf.notify_postgres.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_POSTGRES_COMMENT[](#envvar.MINIO_NOTIFY_POSTGRES_COMMENT)

Configuration Setting

notify_postgres comment[](#mc-conf.notify_postgres.comment)

Specify a comment to associate with the PostgreSQL configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).