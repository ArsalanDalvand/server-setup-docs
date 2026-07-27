---
title: "Publish Events to Kafka — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-kafka.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-kafka"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to Kafka — SILO (MinIO Object Storage)"
---
# Publish Events to Kafka

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to a [Kafka](https://kafka.apache.org/)
service endpoint.

MinIO relies on the [https://github.com/Shopify/sarama](https://github.com/Shopify/sarama) project for Kafka connectivity
and shares that project’s Kafka support. See the
`sarama` [Compatibility and API stability](https://github.com/Shopify/sarama/#compatibility-and-api-stability) section for more details.

## Add a Kafka Endpoint to a MinIO Deployment

The following procedure adds a new Kafka service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### Kafka Minimum Versions and Supported Versions

MinIO relies on the [https://github.com/Shopify/sarama](https://github.com/Shopify/sarama) project for Kafka connectivity
and shares that project’s Kafka support. See the
`sarama` [Compatibility and API stability](https://github.com/Shopify/sarama/#compatibility-and-api-stability) section for more details.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the Kafka Endpoint to MinIO

You can configure a new Kafka service endpoint using either environment variables
or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the Kafka service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/kafka#minio-server-envvar-bucket-notification-kafka|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring a Kafka service endpoint. The minimum
required variables are
[[reference/minio-server/settings/notifications/kafka#envvar.MINIO_NOTIFY_KAFKA_ENABLE|`MINIO_NOTIFY_KAFKA_ENABLE`]] and
[[reference/minio-server/settings/notifications/kafka#envvar.MINIO_NOTIFY_KAFKA_BROKERS|`MINIO_NOTIFY_KAFKA_BROKERS`]]:

```
export MINIO_NOTIFY_KAFKA_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_KAFKA_BROKERS_<IDENTIFIER>="<ENDPOINT>"
   export MINIO_NOTIFY_KAFKA_TOPIC_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_SASL_USERNAME_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_SASL_PASSWORD_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_SASL_MECHANISM_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_TLS_CLIENT_AUTH_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_SASL_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_TLS_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_TLS_SKIP_VERIFY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_CLIENT_TLS_CERT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_CLIENT_TLS_KEY_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_QUEUE_DIR_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_QUEUE_LIMIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_VERSION_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_KAFKA_COMMENT_<IDENTIFIER>="<string>"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
Kafka service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new target service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing Kafka service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_kafka`]] to
review the currently configured Kafka endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with a comma-separated list of Kafka brokers.
For example:

`"kafka1.example.com:2021,kafka2.example.com:2021"`

See [[reference/minio-server/settings/notifications/kafka#minio-server-envvar-bucket-notification-kafka|Kafka Service for Bucket Notifications]] for complete documentation
on each environment variable.

Configuration Settings

MinIO supports adding or updating Kafka endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/kafka#mc-conf.notify_kafka|`notify_kafka`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
Kafka service endpoint. The minimum required setting is
[[reference/minio-server/settings/notifications/kafka#mc-conf.notify_kafka.brokers|`notify_kafka brokers`]]:

```
mc admin config set ALIAS/ notify_kafka:IDENTIFIER \
   brokers="<ENDPOINT>" \
   topic="<string>" \
   sasl_username="<string>" \
   sasl_password="<string>" \
   sasl_mechanism="<string>" \
   tls_client_auth="<string>" \
   tls="<string>" \
   tls_skip_verify="<string>" \
   client_tls_cert="<string>" \
   client_tls_key="<string>" \
   version="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
Kafka service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing Kafka service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_kafka`]] to
review the currently configured Kafka endpoints on the MinIO deployment.

Replace `ENDPOINT` with a comma separated list of Kafka brokers.
For example:

`"kafka1.example.com:2021,kafka2.example.com:2021"`

See [[reference/minio-server/settings/notifications/kafka#minio-server-config-bucket-notification-kafka|Kafka Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured
Kafka target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:kafka
```

You must specify the ARN resource when configuring bucket notifications with
the associated Kafka deployment as a target.

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

For example, `arn:minio:sqs::primary:kafka`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:kafka`

### 3) Configure Bucket Notifications using the Kafka Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured Kafka service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:kafka \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:kafka
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the Kafka service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update a Kafka Endpoint in a MinIO Deployment

The following procedure updates an existing Kafka service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### Kafka Minimum Versions and Supported Versions

MinIO relies on the [https://github.com/Shopify/sarama](https://github.com/Shopify/sarama) project for Kafka connectivity
and shares that project’s Kafka support. See the
`sarama` [Compatibility and API stability](https://github.com/Shopify/sarama/#compatibility-and-api-stability) section for more details.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured Kafka Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured Kafka service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_kafka
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_kafka:primary tls_skip_verify="off"  queue_dir="" queue_limit="0" sasl="off" sasl_password="" sasl_username="" tls_client_auth="0" tls="off" brokers="" topic="" client_tls_cert="" client_tls_key="" version=""
notify_kafka:secondary tls_skip_verify="off"  queue_dir="" queue_limit="0" sasl="off" sasl_password="" sasl_username="" tls_client_auth="0" tls="off" brokers="" topic="" client_tls_cert="" client_tls_key="" version=""
```

The [[reference/minio-server/settings/notifications/kafka#mc-conf.notify_kafka|`notify_kafka`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/kafka#minio-server-config-bucket-notification-kafka|Kafka Notification Settings]]. The
[[reference/minio-server/settings/notifications/kafka#mc-conf.notify_kafka.brokers|`brokers`]] key specifies the Kafka service
endpoint for the given notify_kafka key. The `notify_kafka:<IDENTIFIER>`
suffix describes the unique identifier for that Kafka service endpoint.

Note the identifier for the Kafka service endpoint you want to update for
the next step.

### 2) Update the Kafka Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the Kafka service endpoint:

```
mc admin config set ALIAS/ notify_kafka:<IDENTIFIER> \
   brokers="https://kafka1.example.net:9200, https://kafka2.example.net:9200" \
   topic="<string>" \
   sasl_username="<string>" \
   sasl_password="<string>" \
   sasl_mechanism="<string>" \
   tls_client_auth="<string>" \
   tls="<string>" \
   tls_skip_verify="<string>" \
   client_tls_cert="<string>" \
   client_tls_key="<string>" \
   version="<string>" \
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

The [[reference/minio-server/settings/notifications/kafka#mc-conf.notify_kafka.brokers|`notify_kafka brokers`]] configuration setting
is the minimum required for a Kafka service endpoint. All other configuration
settings are optional. See
[[reference/minio-server/settings/notifications/kafka#minio-server-config-bucket-notification-kafka|Kafka Notification Settings]] for a complete list of
Kafka configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured
Kafka target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:kafka
```

### 4) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
Kafka service endpoint and check the Kafka service for the notification data.
The action required depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were
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