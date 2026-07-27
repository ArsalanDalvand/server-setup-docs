---
title: "Publish Events to NATS — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-nats.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-nats"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to NATS — SILO (MinIO Object Storage)"
---
# Publish Events to NATS

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to a [NATS](https://nats.io/) service endpoint.

NATS Streaming Deprecated

NATS Streaming is deprecated.
Migrate to [JetStream](https://docs.nats.io/nats-concepts/jetstream) instead.

The related MinIO configuration options and environment variables are deprecated.

## Add a NATS Endpoint to a MinIO Deployment

The following procedure adds a new NATS service endpoint for supporting [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO deployment.

### Prerequisites

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the NATS Endpoint to MinIO

You can configure a new NATS service endpoint using either environment variables or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the NATS service endpoint and associated configuration settings using [[reference/minio-server/settings/notifications/nats#minio-server-envvar-bucket-notification-nats|environment variables]].
The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its next startup.

The following example code sets all  environment variables related to configuring an NATS service endpoint.
The minimum required variables are [[reference/minio-server/settings/notifications/nats#envvar.MINIO_NOTIFY_NATS_ADDRESS|`MINIO_NOTIFY_NATS_ADDRESS`]] and [[reference/minio-server/settings/notifications/nats#envvar.MINIO_NOTIFY_NATS_SUBJECT|`MINIO_NOTIFY_NATS_SUBJECT`]]:

```
export MINIO_NOTIFY_NATS_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_NATS_ADDRESS_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_SUBJECT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_USERNAME_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_PASSWORD_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_TOKEN_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_TLS_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_TLS_SKIP_VERIFY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_PING_INTERVAL_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_QUEUE_DIR_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_QUEUE_LIMIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_CERT_AUTHORITY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_CLIENT_CERT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_CLIENT_KEY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_COMMENT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_NATS_JETSTREAM_<IDENTIFIER>="<string>"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the NATS service endpoint.
Use the same `<IDENTIFIER>` value for all environment variables related to the new target service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing NATS service endpoint on the MinIO deployment, the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_nats`]] to review the currently configured NATS endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the hostname and port of the NATS service endpoint.
For example: `nats-endpoint.example.com:4222`

See [[reference/minio-server/settings/notifications/nats#minio-server-envvar-bucket-notification-nats|NATS Service for Bucket Notifications]] for complete documentation on each environment variable.

Configuration Settings

MinIO supports adding or updating NATS endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats|`notify_nats`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
NATS service endpoint. The minimum required setting are
[[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats.address|`notify_nats address`]] and
[[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats.subject|`notify_nats subject`]]:

```
mc admin config set ALIAS/ notify_nats:IDENTIFIER \
   address="HOSTNAME" \
   subject="<string>" \
   username="<string>" \
   password="<string>" \
   token="<string>" \
   nats_jetstream="<string>" \
   tls="<string>" \
   tls_skip_verify="<string>" \
   ping_interval="<string>" \
   cert_authority="<string>" \
   client_cert="<string>" \
   client_key="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the NATS service endpoint.
The following examples in this procedure assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing NATS service endpoint on the MinIO deployment, the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_nats`]] to review the currently configured NATS endpoints on the MinIO deployment.

Replace `ENDPOINT` with the hostname and port of the NATS service endpoint.
For example: `nats-endpoint.example.com:4222`.

See [[reference/minio-server/settings/notifications/nats#minio-server-config-bucket-notification-nats|NATS Bucket Notification Configuration Settings]] for complete       documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured NATS
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:nats
```

You must specify the ARN resource when configuring bucket notifications with
the associated NATS deployment as a target.

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

For example, `arn:minio:sqs::primary:nats`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:nats`

### 3) Configure Bucket Notifications using the NATS Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured NATS service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:nats \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:nats
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the NATS service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update an NATS Endpoint in a MinIO Deployment

The following procedure updates an existing NATS service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured NATS Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently configured NATS service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_nats
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_nats:primary password="yoursecret" subject="" address="nats-endpoint.example.com:4222"  token="" username="yourusername" ping_interval="0" queue_limit="0" tls="off" tls_skip_verify="off" queue_dir="" streaming_enable="on" nats_jetstream="on"
notify_nats:secondary password="yoursecret" subject="" address="nats-endpoint.example.com:4222"  token="" username="yourusername" ping_interval="0" queue_limit="0" tls="off" tls_skip_verify="off" queue_dir="" streaming_enable="on" nats_jetstream="on"
```

The [[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats|`notify_nats`]] key is the top-level configuration key for an [[reference/minio-server/settings/notifications/nats#minio-server-config-bucket-notification-nats|NATS Notification Settings]].
The [[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats.address|`address`]] key specifies the NATS service endpoint for the given `notify_nats` key.
The `notify_nats:<IDENTIFIER>` suffix describes the unique identifier for that NATS service endpoint.

Note the identifier for the NATS service endpoint you want to update for the next step.

### 2) Update the NATS Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration for the NATS service endpoint:

```
mc admin config set ALIAS/ notify_nats:IDENTIFIER \
   address="HOSTNAME" \
   subject="<string>" \
   username="<string>" \
   password="<string>" \
   token="<string>" \
   tls="<string>" \
   tls_skip_verify="<string>" \
   ping_interval="<string>" \
   nats_jetstream="<string>" \
   cert_authority="<string>" \
   client_cert="<string>" \
   client_key="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The [[reference/minio-server/settings/notifications/nats#mc-conf.notify_nats.address|`notify_nats address`]] configuration setting is the minimum required for an NATS service endpoint.
All other configuration settings are optional.
See [[reference/minio-server/settings/notifications/nats#minio-server-config-bucket-notification-nats|NATS Notification Settings]] for a complete list of NATS configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured NATS target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:nats
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated NATS service endpoint and check the NATS service for the notification data.
The action required depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified when configuring the bucket notification.

For example, if the bucket notification configuration includes the `s3:ObjectCreated:Put` event, you can use the [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger a notification.

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