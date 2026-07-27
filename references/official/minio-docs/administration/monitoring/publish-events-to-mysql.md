---
title: "Publish Events to MySQL — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-mysql.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-mysql"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to MySQL — SILO (MinIO Object Storage)"
---
# Publish Events to MySQL

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to a [MySQL](https://www.mysql.com/)
service endpoint. MinIO supports MySQL 5.7.8 and later only.

## Add a MySQL Endpoint to a MinIO Deployment

The following procedure adds a new MySQL service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MySQL 5.7.8 and later

MinIO relies on features introduced with MySQL 5.7.8.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the MySQL Endpoint to MinIO

You can configure a new MySQL service endpoint using either environment variables
or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the MySQL service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/mysql#minio-server-envvar-bucket-notification-mysql|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring a MySQL service endpoint. The minimum
required variables are:

[[reference/minio-server/settings/notifications/mysql#envvar.MINIO_NOTIFY_MYSQL_ENABLE|`MINIO_NOTIFY_MYSQL_ENABLE`]]

[[reference/minio-server/settings/notifications/mysql#envvar.MINIO_NOTIFY_MYSQL_DSN_STRING|`MINIO_NOTIFY_MYSQL_DSN_STRING`]]

[[reference/minio-server/settings/notifications/mysql#envvar.MINIO_NOTIFY_MYSQL_TABLE|`MINIO_NOTIFY_MYSQL_TABLE`]]

[[reference/minio-server/settings/notifications/mysql#envvar.MINIO_NOTIFY_MYSQL_FORMAT|`MINIO_NOTIFY_MYSQL_FORMAT`]]

```
export MINIO_NOTIFY_MYSQL_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_MYSQL_DSN_STRING_<IDENTIFIER>="user:password@tcp(hostname:port)/database"
   export MINIO_NOTIFY_MYSQL_TABLE_<IDENTIFIER>="minio-events"
   export MINIO_NOTIFY_MYSQL_FORMAT_<IDENTIFIER>="namespace|access"
   export MINIO_NOTIFY_MYSQL_MAX_OPEN_CONNECTIONS_<IDENTIFIER>="2"
   export MINIO_NOTIFY_MYSQL_QUEUE_DIR_<IDENTIFIER>="/opt/minio/events"
   export MINIO_NOTIFY_MYSQL_QUEUE_LIMIT_<IDENTIFIER>="100000"
   export MINIO_NOTIFY_MYSQL_COMMENT_<IDENTIFIER>="MySQL Event Notification Logging for MinIO"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
MySQL service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new target service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing MySQL service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_mysql`]] to
review the currently configured MySQL endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the DSN of the MySQL service endpoint.
MinIO expects the following format:

`<user>:<password>@tcp(<host>:<port>)/<database>`

For example:

`"username:password@tcp(mysql.example.com:3306)/miniodb"`

See [[reference/minio-server/settings/notifications/mysql#minio-server-envvar-bucket-notification-mysql|MySQL Service for Bucket Notifications]] for complete documentation
on each environment variable.

Configuration Settings

MinIO supports adding or updating MySQL endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql|`notify_mysql`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
MySQL service endpoint. The minimum required settings are:

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.dsn_string|`notify_mysql dsn_string`]]

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.table|`notify_mysql table`]]

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.format|`notify_mysql format`]]

```
mc admin config set ALIAS/ notify_mysql:IDENTIFIER \
   dsn_string="<ENDPOINT>" \
   table="<string>" \
   format="<string>" \
   max_open_connections="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
MySQL service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing MySQL service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_mysql`]] to
review the currently configured MySQL endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the DSN of the MySQL service endpoint.
MinIO expects the following format:

`<user>:<password>@tcp(<host>:<port>)/<database>`

For example:

`"username:password@tcp(mysql.example.com:3306)/miniodb"`

See [[reference/minio-server/settings/notifications/mysql#minio-server-config-bucket-notification-mysql|MySQL Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured MySQL
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:mysql
```

You must specify the ARN resource when configuring bucket notifications with
the associated MySQL deployment as a target.

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

For example, `arn:minio:sqs::primary:mysql`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:mysql`

### 3) Configure Bucket Notifications using the MySQL Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured MySQL service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:mysql \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:mysql
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the MySQL service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update a MySQL Endpoint in a MinIO Deployment

The following procedure updates an existing MySQL service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MySQL 5.7.8 and later

MinIO relies on features introduced with MySQL 5.7.8.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured MySQL Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured MySQL service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_mysql
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_mysql:primary format="namespace" table="minio_images" dsn_string="user:pass@tcp(mysql.example.com:3306)/miniodb"
notify_mysql:secondary format="namespace" table="minio_images" dsn_string="user:pass@tcp(mysql.example.com:3306)/miniodb"
```

The [[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql|`notify_mysql`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/mysql#minio-server-config-bucket-notification-mysql|MySQL Notification Settings]]. The
[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.dsn_string|`dsn_string`]] key specifies the MySQL service
endpoint for the given notify_mysql key. The `notify_mysql:<IDENTIFIER>`
suffix describes the unique identifier for that MySQL service endpoint.

Note the identifier for the MySQL service endpoint you want to update for
the next step.

### 2) Update the MySQL Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the MySQL service endpoint:

```
mc admin config set ALIAS/ notify_mysql:IDENTIFIER \
   dsn_string="<ENDPOINT>" \
   table="<string>" \
   format="<string>" \
   max_open_connections="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The following configuration settings are the minimum required for a MySQL
service endpoint:

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.dsn_string|`notify_mysql dsn_string`]]

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.table|`notify_mysql table`]]

[[reference/minio-server/settings/notifications/mysql#mc-conf.notify_mysql.format|`notify_mysql format`]]

All other configuration settings are optional. See
[[reference/minio-server/settings/notifications/mysql#minio-server-config-bucket-notification-mysql|MySQL Notification Settings]] for a complete list of
MySQL configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured MySQL
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:mysql
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
MySQL service endpoint and check the MySQL service for the notification data. The
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