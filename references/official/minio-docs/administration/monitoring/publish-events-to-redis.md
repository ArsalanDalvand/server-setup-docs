---
title: "Publish Events to Redis — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-redis.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-redis"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to Redis — SILO (MinIO Object Storage)"
---
# Publish Events to Redis

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to a [Redis](https://redis.io/)
service endpoint.

## Add a Redis Endpoint to a MinIO Deployment

The following procedure adds a new Redis service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the Redis Endpoint to MinIO

You can configure a new Redis service endpoint using either environment variables
or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the Redis service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/redis#minio-server-envvar-bucket-notification-redis|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring an Redis service endpoint. The minimum
required variables are:

[[reference/minio-server/settings/notifications/redis#envvar.MINIO_NOTIFY_REDIS_ENABLE|`MINIO_NOTIFY_REDIS_ENABLE`]]

[[reference/minio-server/settings/notifications/redis#envvar.MINIO_NOTIFY_REDIS_ADDRESS|`MINIO_NOTIFY_REDIS_ADDRESS`]]

[[reference/minio-server/settings/notifications/redis#envvar.MINIO_NOTIFY_REDIS_KEY|`MINIO_NOTIFY_REDIS_KEY`]]

[[reference/minio-server/settings/notifications/redis#envvar.MINIO_NOTIFY_REDIS_FORMAT|`MINIO_NOTIFY_REDIS_FORMAT`]]

```
export MINIO_NOTIFY_REDIS_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_REDIS_ADDRESS_<IDENTIFIER>="<ENDPOINT>"
   export MINIO_NOTIFY_REDIS_KEY_<IDENTIFIER>="<STRING>"
   export MINIO_NOTIFY_REDIS_FORMAT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_REDIS_PASSWORD_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_REDIS_QUEUE_DIR_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_REDIS_QUEUE_LIMIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_REDIS_COMMENT_<IDENTIFIER>="<string>"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
TARGET service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new target service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing Redis service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_redis`]] to
review the currently configured Redis endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the URL of the Redis service endpoint.
For example: `https://redis.example.com:6369`

See [[reference/minio-server/settings/notifications/redis#minio-server-envvar-bucket-notification-redis|Redis Service for Bucket Notifications]] for complete documentation
on each environment variable.

Configuration Settings

MinIO supports adding or updating Redis endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis|`notify_redis`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
Redis service endpoint. The minimum required settings are:

[[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis.address|`notify_redis address`]]

[[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis.key|`notify_redis key`]]

[[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis.format|`notify_redis format`]]

```
mc admin config set ALIAS/ notify_redis:IDENTIFIER \
  address="ENDPOINT" \
  key="<string>" \
  format="<string>" \
  password="<string>" \
  queue_dir="<string>" \
  queue_limit="<string>" \
  comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
Redis service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing Redis service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_redis`]] to
review the currently configured Redis endpoints on the MinIO deployment.

Replace `ENDPOINT` with the URL of the Redis service endpoint.
For example: `https://redis.example.com:6369`

See [[reference/minio-server/settings/notifications/redis#minio-server-config-bucket-notification-redis|Redis Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured Redis
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:redis
```

You must specify the ARN resource when configuring bucket notifications with
the associated Redis deployment as a target.

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

For example, `arn:minio:sqs::primary:redis`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:redis`

### 3) Configure Bucket Notifications using the Redis Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured Redis service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:redis \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:redis
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the Redis service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update an Redis Endpoint in a MinIO Deployment

The following procedure updates an existing Redis service endpoint for
supporting [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured Redis Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured Redis service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_redis
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_redis:primary address="https://redis.example.com:6369" format="namespace" key="minioevent" password="" queue_dir="" queue_limit="0"
notify_redis:secondary address="https://redis.example.com:6369" format="namespace" key="minioevent" password="" queue_dir="" queue_limit="0"
```

The [[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis|`notify_redis`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/redis#minio-server-config-bucket-notification-redis|Redis Notification Settings]]. The
[[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis.address|`address`]] key specifies the Redis service endpoint
for the given notify_redis key. The `notify_redis:<IDENTIFIER>` suffix
describes the unique identifier for that Redis service endpoint.

Note the identifier for the Redis service endpoint you want to update for
the next step.

### 2) Update the Redis Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the Redis service endpoint:

```
mc admin config set ALIAS/ notify_redis:IDENTIFIER \
   address="ENDPOINT" \
   key="<string>" \
   format="<string>" \
   password="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The [[reference/minio-server/settings/notifications/redis#mc-conf.notify_redis.address|`notify_redis address`]] configuration setting
is the minimum required for an Redis service endpoint. All other configuration
settings are optional. See
[[reference/minio-server/settings/notifications/redis#minio-server-config-bucket-notification-redis|Redis Notification Settings]] for a complete list of
Redis configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured Redis
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:redis
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
Redis service endpoint and check the Redis service for the notification data. The
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