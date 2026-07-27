---
title: "MQTT Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/mqtt.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/mqtt"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MQTT Notification Settings — SILO (MinIO Object Storage)"
---
# MQTT Notification Settings

This page documents settings for configuring an MQTT service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-mqtt#minio-bucket-notifications-publish-mqtt|Publish Events to MQTT]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple MQTT Targets

You can specify multiple MQTT service endpoints by appending a unique identifier `_ID` for each set of related MQTT settings to the top level key.
For example, the following commands set two distinct MQTT service endpoints as `PRIMARY` and `SECONDARY`, respectively:

Environment Variables

```
export MINIO_NOTIFY_MQTT_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_MQTT_BROKER_PRIMARY="tcp://user:[email protected]:1883"

export MINIO_NOTIFY_MQTT_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_MQTT_BROKER_SECONDARY="tcp://user:[email protected]:1883"
```

Configuration Setting

```
mc admin config set notify_mqtt:primary \
   broker="tcp://endpoint:port" \
   topic="minio/bucket-name/events/" \
   username="username" \
   password="password" \
   [ARGUMENT="VALUE"] ... \

mc admin config set notify_mqtt:secondary \
   broker="tcp://endpoint:port" \
   topic="minio/bucket-name/events/" \
   username="username" \
   password="password" \
   [ARGUMENT="VALUE"] ... \
```

With these settings, [`MINIO_NOTIFY_MQTT_ENABLE_PRIMARY`](#envvar.MINIO_NOTIFY_MQTT_ENABLE) indicates the environment variable is associated to an MQTT service endpoint with an ID of `PRIMARY`.

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_MQTT_ENABLE[](#envvar.MINIO_NOTIFY_MQTT_ENABLE)

Specify `on` to enable publishing bucket notifications to an MQTT endpoint.

Defaults to `off`.

Configuration Setting

notify_mqtt[](#mc-conf.notify_mqtt)

The top-level configuration key for defining an MQTT server/broker endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an MQTT server/broker endpoint.
The following arguments are required for each endpoint:

[`broker`](#mc-conf.notify_mqtt.broker)

[`topic`](#mc-conf.notify_mqtt.topic)

[`username`](#mc-conf.notify_mqtt.username) Optional if MQTT server/broker does not enforce authentication/authorization

[`password`](#mc-conf.notify_mqtt.password) Optional if MQTT server/broker does not enforce authentication/authorization

Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_mqtt \
   broker="tcp://endpoint:port" \
   topic="minio/bucket-name/events/" \
   username="username" \
   password="password" \
   [ARGUMENT="VALUE"] ... \
```

### Broker

Required

Environment Variable

MINIO_NOTIFY_MQTT_BROKER[](#envvar.MINIO_NOTIFY_MQTT_BROKER)

Configuration Setting

notify_mqtt broker[](#mc-conf.notify_mqtt.broker)

Specify the MQTT server/broker endpoint.
MinIO supports TCP, TLS, or Websocket connections to the server/broker URL.
For example:

`tcp://mqtt.example.net:1883`

`tls://mqtt.example.net:1883`

`ws://mqtt.example.net:1883`

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Topic

Required

Environment Variable

MINIO_NOTIFY_MQTT_TOPIC[](#envvar.MINIO_NOTIFY_MQTT_TOPIC)

Configuration Setting

notify_mqtt topic[](#mc-conf.notify_mqtt.topic)

Specify the name of the MQTT topic to associate with events published by MinIO to the MQTT endpoint.

### Username

Required if the MQTT server/broker enforces authentication/authorization

Environment Variable

MINIO_NOTIFY_MQTT_USERNAME[](#envvar.MINIO_NOTIFY_MQTT_USERNAME)

Configuration Setting

notify_mqtt username[](#mc-conf.notify_mqtt.username)

Specify the MQTT username MinIO should use to authenticate to the MQTT server/broker.

### Password

Required if the MQTT server/broker enforces authentication/authorization

Environment Variable

MINIO_NOTIFY_MQTT_PASSWORD[](#envvar.MINIO_NOTIFY_MQTT_PASSWORD)

Configuration Setting

notify_mqtt password[](#mc-conf.notify_mqtt.password)

Specify the password for the MQTT username MinIO uses to authenticate to the MQTT server/broker.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### Quality of Service

Optional

Environment Variable

MINIO_NOTIFY_MQTT_QOS[](#envvar.MINIO_NOTIFY_MQTT_QOS)

Configuration Setting

notify_mqtt qos[](#mc-conf.notify_mqtt.qos)

Specify the Quality of Service priority for the published events.

Defaults to `0`.

### Keep Alive Interval

Optional

Environment Variable

MINIO_NOTIFY_MQTT_KEEP_ALIVE_INTERVAL[](#envvar.MINIO_NOTIFY_MQTT_KEEP_ALIVE_INTERVAL)

Configuration Setting

notify_mqtt keep_alive_interval[](#mc-conf.notify_mqtt.keep_alive_interval)

Specify the keep-alive interval for the MQTT connections. MinIO
supports the following units of time measurement:

`s` - seconds, “60s”

`m` - minutes, “60m”

`h` - hours, “24h”

`d` - days, “7d”

### Reconnect Interval

Optional

Environment Variable

MINIO_NOTIFY_MQTT_RECONNECT_INTERVAL[](#envvar.MINIO_NOTIFY_MQTT_RECONNECT_INTERVAL)

Configuration Setting

notify_mqtt reconnect_interval[](#mc-conf.notify_mqtt.reconnect_interval)

Specify the reconnect interval for the MQTT connections. MinIO
supports the following units of time measurement:

`s` - seconds, “60s”

`m` - minutes, “60m”

`h` - hours, “24h”

`d` - days, “7d”

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_MQTT_QUEUE_DIR[](#envvar.MINIO_NOTIFY_MQTT_QUEUE_DIR)

Configuration Setting

notify_mqtt queue_dir[](#mc-conf.notify_mqtt.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the MQTT server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_MQTT_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_MQTT_QUEUE_LIMIT)

Configuration Setting

notify_mqtt queue_limit[](#mc-conf.notify_mqtt.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_MQTT_COMMENT[](#envvar.MINIO_NOTIFY_MQTT_COMMENT)

Configuration Setting

notify_mqtt comment[](#mc-conf.notify_mqtt.comment)

Specify a comment to associate with the MQTT configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).