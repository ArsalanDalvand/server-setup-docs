---
title: "Publish Events to PostgreSQL — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-postgresql.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-postgresql"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to PostgreSQL — SILO (MinIO Object Storage)"
---
# Publish Events to PostgreSQL

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to
[PostgreSQL](https://www.postgresql.org/). MinIO supports
PostgreSQL 9.5 and later only.

## Add a PostgreSQL Endpoint to a MinIO Deployment

The following procedure adds a new PostgreSQL service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### PostgreSQL 9.5 and later

MinIO relies on features introduced with PostgreSQL 9.5.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the PostgreSQL Endpoint to MinIO

You can configure a new PostgreSQL service endpoint using either environment
variables or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the PostgreSQL service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/postgresql#minio-server-envvar-bucket-notification-postgresql|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring a PostgreSQL service endpoint. The minimum
required variables are:

[[reference/minio-server/settings/notifications/postgresql#envvar.MINIO_NOTIFY_POSTGRES_CONNECTION_STRING|`MINIO_NOTIFY_POSTGRES_CONNECTION_STRING`]]

[[reference/minio-server/settings/notifications/postgresql#envvar.MINIO_NOTIFY_POSTGRES_TABLE|`MINIO_NOTIFY_POSTGRES_TABLE`]]

[[reference/minio-server/settings/notifications/postgresql#envvar.MINIO_NOTIFY_POSTGRES_FORMAT|`MINIO_NOTIFY_POSTGRES_FORMAT`]]

```
export MINIO_NOTIFY_POSTGRES_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_POSTGRES_CONNECTION_STRING_<IDENTIFIER>="host=postgresql-endpoint.example.net port=4222"
   export MINIO_NOTIFY_POSTGRES_TABLE_<IDENTIFIER>="minioevents"
   export MINIO_NOTIFY_POSTGRES_FORMAT_<IDENTIFIER>="namespace|access"
   export MINIO_NOTIFY_POSTGRES_MAX_OPEN_CONNECTIONS_<IDENTIFIER>="2"
   export MINIO_NOTIFY_POSTGRES_QUEUE_DIR_<IDENTIFIER>="/opt/minio/events"
   export MINIO_NOTIFY_POSTGRES_QUEUE_LIMIT_<IDENTIFIER>="100000"
   export MINIO_NOTIFY_POSTGRES_COMMENT_<IDENTIFIER>="PostgreSQL Notification Event Logging for MinIO"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
PostgreSQL service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new target service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing PostgreSQL service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_postgres`]] to
review the currently configured PostgreSQL endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the [PostgreSQL Connection String](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING)
for PostgreSQL service endpoint. MinIO supports `key=value` format for
the connection string. For example:

`"host=https://postgresql.example.com port=5432 ..."`

For more complete documentation on supported PostgreSQL connection
string parameters, see [PostgreSQL Connection String](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING).

See [[reference/minio-server/settings/notifications/postgresql#minio-server-envvar-bucket-notification-postgresql|PostgreSQL Service for Bucket Notifications]] for complete
documentation on each environment variable.

Configuration Settings

MinIO supports adding or updating PostgreSQL endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres|`notify_postgres`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
PostgreSQL service endpoint. The minimum required setting are:

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.connection_string|`notify_postgres connection_string`]]

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.table|`notify_postgres table`]]

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.format|`notify_postgres format`]]

```
mc admin config set ALIAS/ notify_postgres:IDENTIFIER \
   connection_string="ENDPOINT" \
   table="<string>" \
   format="<string>" \
   max_open_connections="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
PostgreSQL service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing PostgreSQL service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_postgres`]] to
review the currently configured PostgreSQL endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the [PostgreSQL URI connection string](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING)
of the PostgreSQL service endpoint. MinIO supports `key=value` format
for the PostgreSQL connection string. For example:

`"host=https://postgresql.example.com port=5432 ..."`

For more complete documentation on supported PostgreSQL connection
string parameters, see [PostgreSQL Connection String](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING).

See [[reference/minio-server/settings/notifications/postgresql#minio-server-config-bucket-notification-postgresql|PostgreSQL Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured PostgreSQL
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:postgresql
```

You must specify the ARN resource when configuring bucket notifications with
the associated PostgreSQL deployment as a target.

Identifying the ARN for your bucket notifications

You defined the `<IDENTIFIER>` to assign to the target ARN for your bucket notifications when creating the endpoint previously.
The steps below return the ARNs configured on the deployment.
Identify the ARN created previously by looking for the `<IDENTIFIER>` you specified.

Review the JSON output

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS
```

In the JSON output, look for the key `info.sqsARN`.

The ARN you need is the value of that key that matches the `<IDENTIFIER>` you specified.

For example, `arn:minio:sqs::primary:postgresql`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:postgresql`

### 3) Configure Bucket Notifications using the PostgreSQL Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured PostgreSQL service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:postgresql \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the ßevent.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:postgresql
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the PostgreSQL service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update a PostgreSQL Endpoint in a MinIO Deployment

The following procedure updates an existing PostgreSQL service endpoint for
supporting [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### PostgreSQL 9.5 and later

MinIO relies on features introduced with PostgreSQL 9.5.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured PostgreSQL Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured PostgreSQL service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_postgres
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_postgres:primary queue_dir="" connection_string="postgresql://" queue_limit="0"  table="" format="namespace"
notify_postgres:secondary queue_dir="" connection_string="" queue_limit="0"  table="" format="namespace"
```

The [[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres|`notify_postgres`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/postgresql#minio-server-config-bucket-notification-postgresql|PostgreSQL Notification Settings]]. The
[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.connection_string|`connection_string`]] key specifies
the PostgreSQL service endpoint for the given notify_postgres key. The
`notify_postgres:<IDENTIFIER>` suffix describes the unique identifier for
that PostgreSQL service endpoint.

Note the identifier for the PostgreSQL service endpoint you want to update for
the next step.

### 2) Update the PostgreSQL Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the PostgreSQL service endpoint:

```
mc admin config set ALIAS/ notify_postgres:IDENTIFIER \
   connection_string="ENDPOINT" \
   table="<string>" \
   format="<string>" \
   max_open_connections="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The following configuration settings are the minimum required for a
PostgreSQL service endpoint:

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.connection_string|`notify_postgres connection_string`]]

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.table|`notify_postgres table`]]

[[reference/minio-server/settings/notifications/postgresql#mc-conf.notify_postgres.format|`notify_postgres format`]]

All other configuration settings are optional. See
[[reference/minio-server/settings/notifications/postgresql#minio-server-config-bucket-notification-postgresql|PostgreSQL Notification Settings]] for a complete list of
PostgreSQL configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured PostgreSQL
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:postgresql
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
PostgreSQL service endpoint and check the PostgreSQL service for the notification data. The
action required depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were
specified when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).