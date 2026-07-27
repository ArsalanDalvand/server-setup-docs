---
title: "Publish Events to AMQP (RabbitMQ) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-amqp.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-amqp"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to AMQP (RabbitMQ) — SILO (MinIO Object Storage)"
---
# Publish Events to AMQP (RabbitMQ)

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to a [AMQP 0-9-1](https://www.amqp.org/)
service endpoint such as [RabbitMQ](https://www.rabbitmq.com).

MinIO relies on the [https://github.com/streadway/amqp](https://github.com/streadway/amqp) project for AMQP connectivity. The
project is primarily tested against [RabbitMQ](https://www.rabbitmq.com/)
deployments, though other [AMQP 0-9-1-compatible](https://www.amqp.org/)
services may also work. The procedures on this page assume a RabbitMQ
deployment using the AMQP 0-9-1 protocol as the service endpoint.

## Add an AMQP Endpoint to a MinIO Deployment

The following procedure adds a new AMQP service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### AMQP 0-9-1 Service Endpoint

MinIO relies on the [https://github.com/streadway/amqp](https://github.com/streadway/amqp) project for AMQP connectivity. The
project is primarily tested against [RabbitMQ](https://www.rabbitmq.com/)
deployments, though other [AMQP 0-9-1-compatible](https://www.amqp.org/)
services may also work. This procedure assumes a RabbitMQ deployment
using the 0-9-1 protocol as the service endpoint.

If the AMQP service requires authentication, you must provide an appropriate
username and password during the configuration process to grant MinIO access
to the service.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the AMQP Endpoint to MinIO

You can configure a new AMQP service endpoint using either environment variables
or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the AMQP service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/amqp#minio-server-envvar-bucket-notification-amqp|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring an AMQP service endpoint. The minimum
required variables are
[[reference/minio-server/settings/notifications/amqp#envvar.MINIO_NOTIFY_AMQP_ENABLE|`MINIO_NOTIFY_AMQP_ENABLE`]] and [[reference/minio-server/settings/notifications/amqp#envvar.MINIO_NOTIFY_AMQP_URL|`MINIO_NOTIFY_AMQP_URL`]]:

```
export MINIO_NOTIFY_AMQP_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_AMQP_URL_<IDENTIFIER>="<ENDPOINT>"
   export MINIO_NOTIFY_AMQP_EXCHANGE_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_EXCHANGE_TYPE_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_ROUTING_KEY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_MANDATORY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_DURABLE_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_NO_WAIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_INTERNAL_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_AUTO_DELETED_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_DELIVERY_MODE_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_QUEUE_DIR_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_QUEUE_LIMIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_AMQP_COMMENT_<IDENTIFIER>="<string>"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
AMQP service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new AMQP service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing AMQP service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_amqp`]] to
review the currently configured AMQP endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the URL of the AMQP service endpoint.
For example:

`amqp://user:password@hostname:port`

See [[reference/minio-server/settings/notifications/amqp#minio-server-envvar-bucket-notification-amqp|AMQP Service for Bucket Notifications]] for complete documentation
on each environment variable.

Configuration Settings

MinIO supports adding or updating AMQP endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/amqp#mc-conf.notify_amqp|`notify_amqp`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
AMQP service endpoint. The minimum required setting is
[[reference/minio-server/settings/notifications/amqp#mc-conf.notify_amqp.url|`notify_amqp url`]]:

```
mc admin config set ALIAS/ notify_amqp:IDENTIFIER \
  url="ENDPOINT" \
  exchange="<string>" \
  exchange_type="<string>" \
  routing_key="<string>" \
  mandatory="<string>" \
  durable="<string>" \
  no_wait="<string>" \
  internal="<string>" \
  auto_deleted="<string>" \
  delivery_mode="<string>" \
  queue_dir="<string>" \
  queue_limit="<string>" \
  comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
AMQP service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing AMQP service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_amqp`]] to
review the currently configured AMQP endpoints on the MinIO deployment.

Replace `ENDPOINT` with the URL of the AMQP service endpoint.
For example:

`amqp://user:password@hostname:port`

See [[reference/minio-server/settings/notifications/amqp#minio-server-config-bucket-notification-amqp|AMQP Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured AMQP
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:amqp
```

You must specify the ARN resource when configuring bucket notifications with
the associated AMQP deployment as a target.

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

For example, `arn:minio:sqs::primary:amqp`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:amqp`

### 3) Configure Bucket Notifications using the AMQP Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured AMQP service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:amqp \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:amqp
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the AMQP service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update an AMQP Endpoint in a MinIO Deployment

The following procedure updates an existing AMQP service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### AMQP 0-9-1 Service Endpoint

MinIO relies on the [https://github.com/streadway/amqp](https://github.com/streadway/amqp) project for AMQP connectivity. The
project is primarily tested against [RabbitMQ](https://www.rabbitmq.com/)
deployments, though other [AMQP 0-9-1-compatible](https://www.amqp.org/)
services may also work. This procedure assumes a RabbitMQ deployment
as the service endpoint.

If the AMQP service requires authentication, you must provide an appropriate
username and password during the configuration process to grant MinIO access
to the service.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured AMQP Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured AMQP service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_amqp
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_amqp:primary delivery_mode="0" exchange_type="" no_wait="off" queue_dir="" queue_limit="0"  url="amqp://user:password@hostname:port" auto_deleted="off" durable="off" exchange="" internal="off" mandatory="off" routing_key=""
notify_amqp:secondary delivery_mode="0" exchange_type="" no_wait="off" queue_dir="" queue_limit="0"  url="amqp://user:password@hostname:port" auto_deleted="off" durable="off" exchange="" internal="off" mandatory="off" routing_key=""
```

The [[reference/minio-server/settings/notifications/amqp#mc-conf.notify_amqp|`notify_amqp`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/amqp#minio-server-config-bucket-notification-amqp|AMQP Notification Settings]]. The
[[reference/minio-server/settings/notifications/amqp#mc-conf.notify_amqp.url|`url`]] key specifies the AMQP service endpoint
for the given notify_amqp key. The `notify_amqp:<IDENTIFIER>` suffix
describes the unique identifier for that AMQP service endpoint.

Note the identifier for the AMQP service endpoint you want to update for
the next step.

### 2) Update the AMQP Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the AMQP service endpoint:

```
mc admin config set ALIAS/ notify_amqp:<IDENTIFIER> \
   url="amqp://user:password@hostname:port" \
   exchange="<string>" \
   exchange_type="<string>" \
   routing_key="<string>" \
   mandatory="<string>" \
   durable="<string>" \
   no_wait="<string>" \
   internal="<string>" \
   auto_deleted="<string>" \
   delivery_mode="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The [[reference/minio-server/settings/notifications/amqp#mc-conf.notify_amqp.url|`notify_amqp url`]] configuration setting is the
minimum required for an AMQP service endpoint. All other configuration
settings are optional. See [[reference/minio-server/settings/notifications/amqp#minio-server-config-bucket-notification-amqp|AMQP Notification Settings]]
for a complete list of AMQP configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured AMQP
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:amqp
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
AMQP service endpoint and check the AMQP service for the notification data. The
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