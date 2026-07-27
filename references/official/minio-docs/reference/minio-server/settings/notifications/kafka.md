---
title: "Kafka Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/kafka.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/kafka"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Kafka Notification Settings — SILO (MinIO Object Storage)"
---
# Kafka Notification Settings

This page documents settings for configuring an Kafka service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-kafka#minio-bucket-notifications-publish-kafka|Publish Events to Kafka]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple Kafka Targets

You can specify multiple Kafka service endpoints by appending a unique identifier `_ID` for each set of related Kafka settings on to the top level key.

### Examples

For example, the following commands set two distinct Kafka service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variable

```
export MINIO_NOTIFY_KAFKA_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_KAFKA_BROKERS_PRIMARY="https://kafka1.example.net:9200, https://kafka2.example.net:9200"

export MINIO_NOTIFY_KAFKA_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_KAFKA_BROKERS_SECONDARY="https://kafka1.example.net:9200, https://kafka2.example.net:9200"
```

Configuration Setting

```
mc admin config set notify_kafka:primary \
   brokers="https://kafka1.example.net:9200, https://kafka2.example.net:9200"
   [ARGUMENT=VALUE ...]

mc admin config set notify_kafka:secondary \
   brokers="https://kafka1.example.net:9200, https://kafka2.example.net:9200"
   [ARGUMENT=VALUE ...]
```

Notice that for configuration settings, the unique identifier appends to `notify_kafka` only, not to each individual argument.

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_KAFKA_ENABLE[](#envvar.MINIO_NOTIFY_KAFKA_ENABLE)

Specify `on` to enable publishing bucket notifications to a Kafka service endpoint.

Defaults to `off`.

Configuration Setting

notify_kafka[](#mc-conf.notify_kafka)

The top-level configuration key for defining an Kafka service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an Kafka service endpoint.
The [`brokers`](#mc-conf.notify_kafka.brokers) argument is required for each target.
Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_kafka \
  brokers="https://kafka1.example.net:9200, https://kafka2.example.net:9200"
  [ARGUMENT="VALUE"] ... \
```

### Brokers

Required

Environment Variable

MINIO_NOTIFY_KAFKA_BROKERS[](#envvar.MINIO_NOTIFY_KAFKA_BROKERS)

Configuration Setting

notify_kafka brokers[](#mc-conf.notify_kafka.brokers)

Specify a comma-separated list of Kafka broker addresses.
For example:

`"kafka1.example.com:2021,kafka2.example.com:2021"`

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Topic

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_TOPIC[](#envvar.MINIO_NOTIFY_KAFKA_TOPIC)

Configuration Setting

notify_kafka topic[](#mc-conf.notify_kafka.topic)

Specify the name of the Kafka topic to which MinIO publishes bucket events.

### SASL

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_SASL[](#envvar.MINIO_NOTIFY_KAFKA_SASL)

Configuration Setting

notify_kafka sasl[](#mc-conf.notify_kafka.sasl)

Specify `on` to enable SASL authentication.

### SASL Username

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_SASL_USERNAME[](#envvar.MINIO_NOTIFY_KAFKA_SASL_USERNAME)

Configuration Setting

notify_kafka sasl_username[](#mc-conf.notify_kafka.sasl_username)

Specify the username for performing SASL/PLAIN or SASL/SCRAM authentication to the Kafka broker(s).

### SASL Password

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_SASL_PASSWORD[](#envvar.MINIO_NOTIFY_KAFKA_SASL_PASSWORD)

Configuration Setting

notify_kafka sasl_password[](#mc-conf.notify_kafka.sasl_password)

Specify the password for performing SASL/PLAIN or SASL/SCRAM authentication to the Kafka broker(s).

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### SASL Mechanism

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_SASL_MECHANISM[](#envvar.MINIO_NOTIFY_KAFKA_SASL_MECHANISM)

Configuration Setting

notify_kafka sasl_mechanism[](#mc-conf.notify_kafka.sasl_mechanism)

Specify the SASL mechanism to use for authenticating to the Kafka broker(s).
MinIO supports the following mechanisms:

`PLAIN` (Default)

`SHA256`

`SHA512`

### TLS Client Auth

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_TLS_CLIENT_AUTH[](#envvar.MINIO_NOTIFY_KAFKA_TLS_CLIENT_AUTH)

Configuration Setting

notify_kafka tls_client_auth[](#mc-conf.notify_kafka.tls_client_auth)

Specify the client authentication type of the Kafka broker(s).
The following table lists the supported values and their mappings

Value

Authentication Type

0

`NoClientCert`

1

`RequestClientCert`

2

`RequireAnyClientCert`

3

`VerifyClientCertIfGiven`

4

`RequireAndVerifyClientCert`

See [ClientAuthType](https://golang.org/pkg/crypto/tls/#ClientAuthType) for more information on each client auth type.

### TLS

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_TLS[](#envvar.MINIO_NOTIFY_KAFKA_TLS)

Configuration Setting

notify_kafka tls[](#mc-conf.notify_kafka.tls)

Specify `on` to enable TLS connectivity to the Kafka broker(s).

### TLS Skip Verify

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_TLS_SKIP_VERIFY[](#envvar.MINIO_NOTIFY_KAFKA_TLS_SKIP_VERIFY)

Configuration Setting

notify_kafka tls_skip_verify[](#mc-conf.notify_kafka.tls_skip_verify)

Enables or disables TLS verification of the NATS service endpoint TLS certificates.

Specify `on` to disable TLS verification (Default).

Specify `off` to enable TLS verification.

### Client TLS Cert

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_CLIENT_TLS_CERT[](#envvar.MINIO_NOTIFY_KAFKA_CLIENT_TLS_CERT)

Configuration Setting

notify_kafka client_tls_cert[](#mc-conf.notify_kafka.client_tls_cert)

Specify the path to the client certificate to use for performing mTLS authentication to the Kafka broker(s).

### Client TLS Key

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_CLIENT_TLS_KEY[](#envvar.MINIO_NOTIFY_KAFKA_CLIENT_TLS_KEY)

Configuration Setting

notify_kafka client_tls_key[](#mc-conf.notify_kafka.client_tls_key)

Specify the path to the client private key to use for performing mTLS authentication to the Kafka broker(s).

### Version

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_VERSION[](#envvar.MINIO_NOTIFY_KAFKA_VERSION)

Configuration Setting

notify_kafka version[](#mc-conf.notify_kafka.version)

Specify the version of the Kafka cluster to assume when performing operations against that cluster.
See the [sarama reference documentation](https://github.com/shopify/sarama/blob/v1.20.1/config.go#L327) for more information on this field’s behavior.

### Batch Size

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_BATCH_SIZE[](#envvar.MINIO_NOTIFY_KAFKA_BATCH_SIZE)

Configuration Setting

notify_kafka batch_size[](#mc-conf.notify_kafka.batch_size)

Specify the integer value to use as the [batch size](https://kafka.apache.org/documentation/#producerconfigs_batch.size) for sending records to Kafka.

Changed in version RELEASE.2023-12-02T10-51-33Z: MinIO previously limited this value to `100`.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_QUEUE_DIR[](#envvar.MINIO_NOTIFY_KAFKA_QUEUE_DIR)

Configuration Setting

notify_kafka queue_dir[](#mc-conf.notify_kafka.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the Kafka server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_KAFKA_QUEUE_LIMIT)

Configuration Setting

notify_kafka queue_limit[](#mc-conf.notify_kafka.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_COMMENT[](#envvar.MINIO_NOTIFY_KAFKA_COMMENT)

Configuration Setting

notify_kafka comment[](#mc-conf.notify_kafka.comment)

Specify a comment to associate with the Kafka configuration.

### Compression Codec

New in version MinIO: Server RELEASE.2023-12-09T18-17-51Z

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_PRODUCER_COMPRESSION_CODEC[](#envvar.MINIO_NOTIFY_KAFKA_PRODUCER_COMPRESSION_CODEC)

Configuration Setting

notify_kafka compression_codec[](#mc-conf.notify_kafka.compression_codec)

Specify the compression codec to use when sending records to Kafka.

Supports the following values:

`none`

`snappy`

`gzip`

`lz4`

`zstd`

### Compression Level

New in version MinIO: Server RELEASE.2023-12-09T18-17-51Z

Optional

Environment Variable

MINIO_NOTIFY_KAFKA_PRODUCER_COMPRESSION_LEVEL[](#envvar.MINIO_NOTIFY_KAFKA_PRODUCER_COMPRESSION_LEVEL)

Configuration Setting

notify_kafka compression_level[](#mc-conf.notify_kafka.compression_level)

Controls the level of compression applied based on the configured compression codec.

Specify an integer value greater than or equal to `0`.
The effect of the value depends on the selected codec.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).