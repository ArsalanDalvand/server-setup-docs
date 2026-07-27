---
title: "Publish Events to MQTT — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/publish-events-to-mqtt.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/publish-events-to-mqtt"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Events to MQTT — SILO (MinIO Object Storage)"
---
# Publish Events to MQTT

MinIO supports publishing [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification]] events to [MQTT](https://www.mqtt.org/)
server/broker endpoint.

## Add an MQTT Endpoint to a MinIO Deployment

The following procedure adds a new MQTT service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MQTT 3.1 or 3.1.1 Server/Broker

This procedure assumes an existing MQTT 3.1 or 3.1.1 server/broker to which the
MinIO deployment has connectivity. See the
[mqtt.org software listing](https://mqtt.org/software/) for a list of
MQTT-compatible server/brokers.

If the MQTT service requires authentication, you must provide an appropriate
username and password during the configuration process to grant MinIO access
to the service.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) Add the MQTT Endpoint to MinIO

You can configure a new MQTT service endpoint using either environment variables
or by setting runtime configuration settings.

Environment Variables

MinIO supports specifying the MQTT service endpoint and associated
configuration settings using
[[reference/minio-server/settings/notifications/mqtt#minio-server-envvar-bucket-notification-mqtt|environment variables]]. The
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its
next startup.

The following example code sets all  environment variables
related to configuring an MQTT service endpoint. The minimum required
variables are:

[[reference/minio-server/settings/notifications/mqtt#envvar.MINIO_NOTIFY_MQTT_ENABLE|`MINIO_NOTIFY_MQTT_ENABLE`]]

[[reference/minio-server/settings/notifications/mqtt#envvar.MINIO_NOTIFY_MQTT_BROKER|`MINIO_NOTIFY_MQTT_BROKER`]]

[[reference/minio-server/settings/notifications/mqtt#envvar.MINIO_NOTIFY_MQTT_TOPIC|`MINIO_NOTIFY_MQTT_TOPIC`]]

[[reference/minio-server/settings/notifications/mqtt#envvar.MINIO_NOTIFY_MQTT_USERNAME|`MINIO_NOTIFY_MQTT_USERNAME`]] Required if the MQTT server/broker enforces authentication/authorization

[[reference/minio-server/settings/notifications/mqtt#envvar.MINIO_NOTIFY_MQTT_PASSWORD|`MINIO_NOTIFY_MQTT_PASSWORD`]] Required if the MQTT server/broker enforces authentication/authorization

```
export MINIO_NOTIFY_MQTT_ENABLE_<IDENTIFIER>="on"
   export MINIO_NOTIFY_MQTT_BROKER_<IDENTIFIER>="ENDPOINT"
   export MINIO_NOTIFY_MQTT_TOPIC_<IDENTIFIER>="TOPIC"
   export MINIO_NOTIFY_MQTT_USERNAME_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_PASSWORD_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_QOS_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_KEEP_ALIVE_INTERVAL_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_RECONNECT_INTERVAL_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_QUEUE_DIR_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_QUEUE_LIMIT_<IDENTIFIER>="<string>"
   export MINIO_NOTIFY_MQTT_COMMENT_<IDENTIFIER>="<string>"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
MQTT service endpoint. Use the same `<IDENTIFIER>` value for all
environment variables related to the new MQTT service endpoint.
The following examples assume an identifier of `PRIMARY`.

If the specified `<IDENTIFIER>` matches an existing MQTT service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_mqtt`]] to
review the currently configured MQTT endpoints on the MinIO deployment.

Replace `<ENDPOINT>` with the URL of the MQTT service endpoint.
For example:

`tcp://hostname:port`

Replace `TOPIC` with the MQTT topic to which MinIO associates
events published to the server/broker.

See [[reference/minio-server/settings/notifications/mqtt#minio-server-envvar-bucket-notification-mqtt|MQTT Service for Bucket Notifications]] for complete documentation
on each environment variable.

Configuration Settings

MinIO supports adding or updating MQTT endpoints on a running
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command
and the [[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt|`notify_mqtt`]] configuration key. You must restart the
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] process to apply any new or updated configuration
settings.

The following example code sets all  settings related to configuring an
MQTT service endpoint. The following configuration settings are the
minimum required for an MQTT server/broker endpoint:

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.broker|`broker`]]

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.topic|`topic`]]

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.username|`username`]] Required if the MQTT server/broker enforces authentication/authorization

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.password|`password`]] Required if the MQTT server/broker enforces authentication/authorization

```
mc admin config set ALIAS/ notify_mqtt:IDENTIFIER \
   broker="ENDPOINT" \
   topic="TOPIC" \
   username="username" \
   password="password" \
   qos="<integer>" \
   keep_alive_interval="60s|m|h|d"
   reconnect_interval="60s|m|h|d"
   queue_dir="<string>" \
   queue_limit="<string>" \
   comment="<string>"
```

Replace `IDENTIFIER` with a unique descriptive string for the
MQTT service endpoint. The following examples in this procedure
assume an identifier of `PRIMARY`.

If the specified `IDENTIFIER` matches an existing MQTT service
endpoint on the MinIO deployment, the new settings override
any existing settings for that endpoint. Use
[[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get notify_mqtt`]] to
review the currently configured MQTT endpoints on the MinIO deployment.

Replace `ENDPOINT` with the URL of the MQTT service endpoint.
For example:

`tcp://hostname:port`

Replace `TOPIC` with the MQTT topic to which MinIO associates
events published to the server/broker.

See [[reference/minio-server/settings/notifications/mqtt#minio-server-config-bucket-notification-mqtt|MQTT Bucket Notification Configuration Settings]] for complete
documentation on each setting.

### 1) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured MQTT
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:mqtt
```

You must specify the ARN resource when configuring bucket notifications with
the associated MQTT deployment as a target.

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

For example, `arn:minio:sqs::primary:mqtt`.

Use jq to parse the JSON for the value

[Install jq](https://stedolan.github.io/jq/)

Copy and run the following command, replacing `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment.

```
mc admin info --json ALIAS | jq  .info.sqsARN
```

This returns the ARN to use for notifications, such as `arn:minio:sqs::primary:mqtt`

### 1) Configure Bucket Notifications using the MQTT Endpoint as a Target

Use the [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command to add a new bucket notification
event with the configured MQTT service as a target:

```
mc event add ALIAS/BUCKET arn:minio:sqs::primary:mqtt \
  --event EVENTS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment.

Replace `BUCKET` with the name of the bucket in which to configure the
event.

Replace `EVENTS` with a comma-separated list of [[reference/minio-mc/mc-event-add#mc-event-supported-events|events]] for which MinIO triggers notifications.

Use [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] to view all configured bucket events for
a given notification target:

```
mc event ls ALIAS/BUCKET arn:minio:sqs::primary:MQTT
```

### 4) Validate the Configured Events

Perform an action on the bucket for which you configured the new event and
check the MQTT service for the notification data. The action required
depends on which [[reference/minio-mc/mc-event-add#mc.event.add.-event|`events`]] were specified
when configuring the bucket notification.

For example, if the bucket notification configuration includes the
`s3:ObjectCreated:Put` event, you can use the
[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] command to create a new object in the bucket and trigger
a notification.

```
mc cp ~/data/new-object.txt ALIAS/BUCKET
```

## Update an MQTT Endpoint in a MinIO Deployment

The following procedure updates an existing MQTT service endpoint for supporting
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]] in a MinIO
deployment.

### Prerequisites

#### MQTT 3.1 or 3.1.1 Server/Broker Endpoint

This procedure assumes an existing MQTT 3.1 or 3.1.1 server/broker to which the
MinIO deployment has connectivity. See the
[mqtt.org software listing](https://mqtt.org/software/) for a list of
MQTT-compatible server/brokers.

If the MQTT service requires authentication, you must provide an appropriate
username and password during the configuration process to grant MinIO access
to the service.

#### MinIO mc Command Line Tool

This procedure uses the [[reference/minio-mc#command-mc|`mc`]] command line tool for certain actions.
See the `mc` [[reference/minio-mc#mc-install|Quickstart]] for installation instructions.

### 1) List Configured MQTT Endpoints In The Deployment

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]] command to list the currently
configured MQTT service endpoints in the deployment:

```
mc admin config get ALIAS/ notify_mqtt
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

The command output resembles the following:

```
notify_mqtt:primary  broker="tcp://mqtt-primary.example.net:port" password="" queue_dir="" queue_limit="0" reconnect_interval="0s"  keep_alive_interval="0s" qos="0" topic="" username=""
notify_mqtt:secondary  broker="tcp://mqtt-primary.example.net:port" password="" queue_dir="" queue_limit="0" reconnect_interval="0s"  keep_alive_interval="0s" qos="0" topic="" username=""
```

The [[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt|`notify_mqtt`]] key is the top-level configuration key for an
[[reference/minio-server/settings/notifications/mqtt#minio-server-config-bucket-notification-mqtt|MQTT Notification Settings]]. The
[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.broker|`broker`]] key specifies the MQTT server/broker endpoint
for the given notify_mqtt key. The `notify_mqtt:<IDENTIFIER>` suffix
describes the unique identifier for that MQTT service endpoint.

Note the identifier for the MQTT service endpoint you want to update for
the next step.

### 2) Update the MQTT Endpoint

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to set the new configuration
for the MQTT service endpoint:

```
mc admin config set ALIAS/ notify_mqtt:<IDENTIFIER> \
   url="MQTT://user:password@hostname:port" \
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

The following configuration settings are the minimum required for an
MQTT server/broker endpoint:

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.broker|`broker`]]

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.topic|`topic`]]

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.username|`username`]] Required if the MQTT server/broker enforces authentication/authorization

[[reference/minio-server/settings/notifications/mqtt#mc-conf.notify_mqtt.password|`password`]] Required if the MQTT server/broker enforces authentication/authorization

All other configuration settings are optional. See
[[reference/minio-server/settings/notifications/mqtt#minio-server-config-bucket-notification-mqtt|MQTT Notification Settings]] for a complete list of MQTT
configuration settings.

### 3) Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to
restart.

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process prints a line on startup for each configured MQTT
target similar to the following:

```
SQS ARNs: arn:minio:sqs::primary:mqtt
```

### 3) Validate the Changes

Perform an action on a bucket which has an event configuration using the updated
MQTT service endpoint and check the MQTT service for the notification data. The
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