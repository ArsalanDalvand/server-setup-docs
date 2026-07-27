---
title: "Metrics and Logging Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/metrics-and-logging.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/metrics-and-logging"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Metrics and Logging Settings — SILO (MinIO Object Storage)"
---
# Metrics and Logging Settings

This page covers settings that control behavior related to MinIO metrics and logging.
See [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|Metrics and alerts]] for more information.

These settings configure publishing regular [[reference/minio-server/minio-server#command-minio.server|`minio server`]] logs and audit logs to an HTTP webhook.
See [[operations/monitoring/minio-logging#minio-logging|Publish Server or Audit Logs to an External Service]] for more complete documentation.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

[Server Logs](#minio-server-envvar-logging-regular)

[Webhook Audit Logs](#minio-server-envvar-logging-audit)

[Kafka Audit Logs](#minio-server-envvar-logging-audit-kafka)

## Prometheus Authentication

This setting controls how MinIO authenticates to Prometheus.

Environment Variable

MINIO_PROMETHEUS_AUTH_TYPE[](#envvar.MINIO_PROMETHEUS_AUTH_TYPE)

Configuration Setting

This setting does not have a configuration setting option.

Specifies the authentication mode for the Prometheus [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|scraping endpoints]].

`jwt` - Default MinIO requires that the scraping client specify a JWT token for authenticating requests.

Use [[reference/minio-mc-admin/mc-admin-prometheus-generate#command-mc.admin.prometheus.generate|`mc admin prometheus generate`]] to generate the necessary JWT bearer tokens.

`public` MinIO does not require that scraping clients authenticate their requests.

## Server Logs

The following section documents settings for configuring MinIO to publish [[reference/minio-server/minio-server#command-minio.server|`minio server`]] logs to an HTTP webhook endpoint.
See [[operations/monitoring/minio-logging#minio-logging-publish-server-logs|Publish Server Logs to HTTP Webhook]] for more complete documentation and tutorials on using these settings.

### Defining Multiple Endpoints

You can specify multiple webhook endpoints as log targets by appending a unique identifier `_ID` for each set of related logging environment variables.
For example, the following settings define two distinct server logs webhook endpoints:

Environment Variables

```
export MINIO_LOGGER_WEBHOOK_ENABLE_PRIMARY="on"
export MINIO_LOGGER_WEBHOOK_AUTH_TOKEN_PRIMARY="TOKEN"
export MINIO_LOGGER_WEBHOOK_ENDPOINT_PRIMARY="http://webhook-1.example.net"

export MINIO_LOGGER_WEBHOOK_ENABLE_SECONDARY="on"
export MINIO_LOGGER_WEBHOOK_AUTH_TOKEN_SECONDARY="TOKEN"
export MINIO_LOGGER_WEBHOOK_ENDPOINT_SECONDARY="http://webhook-2.example.net"
```

Configuration Setting

```
mc admin config set logger_webhook:primary \
   endpoint="http://webhook-01.example.net" [ARGUMENTS=VALUE ...]

mc admin config set logger_webhook:secondary \
   endpoint="http://webhook-02.example.net" [ARGUMENTS=VALUE ...]
```

### Settings

#### Enable

Environment Variable

MINIO_LOGGER_WEBHOOK_ENABLE[](#envvar.MINIO_LOGGER_WEBHOOK_ENABLE)

Specify `"on"` to enable publishing [[reference/minio-server/minio-server#command-minio.server|`minio server`]] logs to the HTTP webhook endpoint.

Requires specifying [`MINIO_LOGGER_WEBHOOK_ENDPOINT`](#envvar.MINIO_LOGGER_WEBHOOK_ENDPOINT).

Configuration Setting

logger_webhook[](#mc-conf.logger_webhook)

The top level key for the configuration settings to configure logging to an HTTP webhook endpoint.

#### Endpoint

Required

Environment Variable

MINIO_LOGGER_WEBHOOK_ENDPOINT[](#envvar.MINIO_LOGGER_WEBHOOK_ENDPOINT)

Configuration Setting

logger_webhook endpoint[](#mc-conf.logger_webhook.endpoint)

The HTTP endpoint of the webhook.

#### Auth Token

Optional

Environment Variable

MINIO_LOGGER_WEBHOOK_AUTH_TOKEN[](#envvar.MINIO_LOGGER_WEBHOOK_AUTH_TOKEN)

An authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example: for a Bearer token, prepend `Bearer`:

```
export MINIO_LOGGER_WEBHOOK_AUTH_TOKEN_myendpoint="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
export MINIO_LOGGER_WEBHOOK_AUTH_TOKEN_xyz="ServiceXYZ 1a2b3c4f5e"
```

Consult the documentation for the desired service for more details.

This environment variable corresponds with the [`logger_webhook auth_token`](#mc-conf.logger_webhook.auth_token) configuration setting.

Configuration Setting

logger_webhook auth_token[](#mc-conf.logger_webhook.auth_token)

An authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example: for a Bearer token, prepend `Bearer`:

```
mc admin config set myminio logger_webhook   \
      endpoint="https://webhook-1.example.net"  \
      auth_token="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
mc admin config set myminio logger_webhook   \
        endpoint="https://webhook-1.example.net"  \
      auth_token="ServiceXYZ 1a2b3c4f5e"
```

Consult the documentation for the desired service for more details.

#### Batch Size

New in version MinIO: Server RELEASE.2024-03-10T02-53-48Z

Optional

Environment Variable

MINIO_LOGGER_WEBHOOK_BATCH_SIZE[](#envvar.MINIO_LOGGER_WEBHOOK_BATCH_SIZE)

Configuration Setting

logger_webhook batch_size[](#mc-conf.logger_webhook.batch_size)

Collect and send the specified number of events to the webhook as a batch.
If not set, MinIO sends one event per request.

#### Client Certificate

Optional

Requires also setting the Client Key.

Environment Variable

MINIO_LOGGER_WEBHOOK_CLIENT_CERT[](#envvar.MINIO_LOGGER_WEBHOOK_CLIENT_CERT)

Configuration Setting

logger_webhook client_cert[](#mc-conf.logger_webhook.client_cert)

The path to the mTLS certificate to use for authenticating to the webhook logger.

#### Client Key

Optional

Required if you define the Client Certificate.

Environment Variable

MINIO_LOGGER_WEBHOOK_CLIENT_KEY[](#envvar.MINIO_LOGGER_WEBHOOK_CLIENT_KEY)

Configuration Setting

logger_webhook client_key[](#mc-conf.logger_webhook.client_key)

The path to the mTLS certificate key to use to authenticate with the webhook logger service.

#### Proxy

Optional

Environment Variable

MINIO_LOGGER_WEBHOOK_PROXY[](#envvar.MINIO_LOGGER_WEBHOOK_PROXY)

Configuration Setting

logger_webhook proxy[](#mc-conf.logger_webhook.proxy)

New in version MinIO: RELEASE.2023-02-22T18-23-45Z

Define a proxy to use for the webhook logger when communicating from MinIO to external webhooks.

#### Queue Directory

Optional

New in version RELEASE.2023-05-18T00-05-36Z.

Environment Variable

MINIO_LOGGER_WEBHOOK_QUEUE_DIR[](#envvar.MINIO_LOGGER_WEBHOOK_QUEUE_DIR)

Configuration Setting

logger_webhook queue_dir[](#mc-conf.logger_webhook.queue_dir)

Specify the directory path, such as `/opt/minio/events`, to enable MinIO’s persistent event store for undelivered messages.
The MinIO process must have read, write, and list access on the specified directory.

MinIO stores undelivered events in the specified store while the webhook service is offline and replays the stored events when connectivity resumes.

#### Queue Size

Optional

Environment Variable

MINIO_LOGGER_WEBHOOK_QUEUE_SIZE[](#envvar.MINIO_LOGGER_WEBHOOK_QUEUE_SIZE)

Configuration Setting

logger_webhook queue_size[](#mc-conf.logger_webhook.queue_size)

An integer value to use for the queue size for logger webhook targets.

## Webhook Audit Logs

The following section documents environment variables for configuring MinIO to publish audit logs to an HTTP webhook endpoint.
See [[operations/monitoring/minio-logging#minio-logging-publish-audit-logs|Publish Audit Logs to HTTP Webhook]] for more complete documentation and tutorials on using these environment variables.

### Multiple Targets

You can specify multiple webhook endpoints as audit log targets by appending a unique identifier `_ID` for each set of related logging settings.

For example, the following commands set two distinct audit log webhook endpoints:

Environment Variables

```
export MINIO_AUDIT_WEBHOOK_ENABLE_PRIMARY="on"
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_PRIMARY="TOKEN"
export MINIO_AUDIT_WEBHOOK_ENDPOINT_PRIMARY="http://webhook-1.example.net"
export MINIO_AUDIT_WEBHOOK_CLIENT_CERT_SECONDARY="/tmp/cert.pem"
export MINIO_AUDIT_WEBHOOK_CLIENT_KEY_SECONDARY="/tmp/key.pem"

export MINIO_AUDIT_WEBHOOK_ENABLE_SECONDARY="on"
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_SECONDARY="TOKEN"
export MINIO_AUDIT_WEBHOOK_ENDPOINT_SECONDARY="http://webhook-1.example.net"
export MINIO_AUDIT_WEBHOOK_CLIENT_CERT_SECONDARY="/tmp/cert.pem"
export MINIO_AUDIT_WEBHOOK_CLIENT_KEY_SECONDARY="/tmp/key.pem"
```

Configuration Setting

audit_webhook[](#mc-conf.audit_webhook)

The top-level configuration key for defining an HTTP webhook target for
publishing [[operations/monitoring/minio-logging#minio-logging|MinIO audit logs]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an HTTP webhook target.
Specify additional optional arguments as a whitespace (`" "`)-delimited
list.

```
mc admin config set audit_webhook \
   endpoint="http://webhook.example.net" [ARGUMENTS=VALUE ...]
```

You can specify multiple HTTP webhook targets by appending
`[:name]` to the top-level key. For example, the following commands
set two distinct HTTP webhook targets as `primary` and `secondary`
respectively:

```
mc admin config set audit_webhook:primary \
   endpoint="http://webhook-01.example.net" [ARGUMENTS=VALUE ...]

mc admin config set audit_webhook:secondary \
   endpoint="http://webhook-02.example.net" [ARGUMENTS=VALUE ...]
```

### Settings

#### Enable

Environment Variable

MINIO_AUDIT_WEBHOOK_ENABLE[](#envvar.MINIO_AUDIT_WEBHOOK_ENABLE)

Specify `"on"` to enable publishing audit logs to the HTTP webhook endpoint.

Requires specifying [`MINIO_AUDIT_WEBHOOK_ENDPOINT`](#envvar.MINIO_AUDIT_WEBHOOK_ENDPOINT).

Configuration Setting

Configure an audit webhook to enable it.
There is not a separate `enable` configuration setting.

#### Endpoint

Required

Environment Variable

MINIO_AUDIT_WEBHOOK_ENDPOINT[](#envvar.MINIO_AUDIT_WEBHOOK_ENDPOINT)

Configuration Setting

audit_webhook endpoint[](#mc-conf.audit_webhook.endpoint)

The HTTP endpoint of the webhook.

#### Auth Token

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_AUTH_TOKEN[](#envvar.MINIO_AUDIT_WEBHOOK_AUTH_TOKEN)

Configuration Setting

audit_webhook auth_token[](#mc-conf.audit_webhook.auth_token)

An authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

Environment Variable

For example, for a Bearer token, prepend `Bearer`:

```
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_myendpoint="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.

A custom authentication format could resemble the following:

```
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_xyz="ServiceXYZ 1a2b3c4f5e"
```

Configuration Setting

```
mc admin config set myminio audit_webhook       \
         endpoint="http://webhook.example.net"  \
         auth_token="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.

A command for a custom authentication format could resemble the following:

```
mc admin config set myminio audit_webhook       \
         endpoint="http://webhook.example.net"  \
         auth_token="ServiceXYZ 1a2b3c4f5e"
```

Consult the documentation for the desired service for more details.

#### Batch Size

New in version MinIO: Server RELEASE.2024-03-10T02-53-48Z

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_BATCH_SIZE[](#envvar.MINIO_AUDIT_WEBHOOK_BATCH_SIZE)

Configuration Setting

audit_webhook batch_size[](#mc-conf.audit_webhook.batch_size)

Collect and send the specified number of events to the webhook as a batch.
If not set, MinIO sends one event per request.

#### Client Certificate

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_CLIENT_CERT[](#envvar.MINIO_AUDIT_WEBHOOK_CLIENT_CERT)

Requires also specifying [`MINIO_AUDIT_WEBHOOK_CLIENT_KEY`](#envvar.MINIO_AUDIT_WEBHOOK_CLIENT_KEY).

Configuration Setting

audit_webhook client_cert[](#mc-conf.audit_webhook.client_cert)

Requires also specifying [`client_key`](#mc-conf.audit_webhook.client_key).

The x.509 client certificate to present to the HTTP webhook.
Omit for webhooks which do not require clients to present a known TLS certificate.

#### Client Key

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_CLIENT_KEY[](#envvar.MINIO_AUDIT_WEBHOOK_CLIENT_KEY)

Requires also specifying [`MINIO_AUDIT_WEBHOOK_CLIENT_CERT`](#envvar.MINIO_AUDIT_WEBHOOK_CLIENT_CERT).

Configuration Setting

audit_webhook client_key[](#mc-conf.audit_webhook.client_key)

Requires specifying [`client_cert`](#mc-conf.audit_webhook.client_cert).

The x.509 private key to present to the HTTP webhook.
Omit for webhooks which do not require clients to present a known TLS certificate.

#### Queue Directory

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_QUEUE_DIR[](#envvar.MINIO_AUDIT_WEBHOOK_QUEUE_DIR)

Configuration Setting

audit_webhook queue_dir[](#mc-conf.audit_webhook.queue_dir)

New in version RELEASE.2023-05-18T00-05-36Z.

Specify the directory path, such as `/opt/minio/events`, to enable MinIO’s persistent event store for undelivered messages.
The MinIO process must have read, write, and list access on the specified directory.

MinIO stores undelivered events in the specified store while the webhook service is offline and replays the stored events when connectivity resumes.

#### Queue Size

Optional

Environment Variable

MINIO_AUDIT_WEBHOOK_QUEUE_SIZE[](#envvar.MINIO_AUDIT_WEBHOOK_QUEUE_SIZE)

Configuration Setting

audit_webhook queue_size[](#mc-conf.audit_webhook.queue_size)

An integer value to use for the queue size for audit webhook targets.
The default is `100000` events.

## Kafka Audit Logs

The following section documents environment variables for configuring MinIO to publish audit logs to a Kafka broker.

audit_kafka[](#mc-conf.audit_kafka)

The top-level configuration key for defining a Kafka broker target for publishing [[operations/monitoring/minio-logging#minio-logging|MinIO audit logs]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update a Kafka audit target.
Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set audit_kafka \
   brokers="https://kafka-endpoint.example.net:9092" [ARGUMENTS=VALUE ...]
```

### Settings

#### Enable

Required

Environment Variable

MINIO_AUDIT_KAFKA_ENABLE[](#envvar.MINIO_AUDIT_KAFKA_ENABLE)

Set to `"on"` to enable the target.

Set to `"off"` to disable the target.

Configuration Setting

There is not a configuration setting for this value.
Use the environment variable to disable a configured audit webhook target.

#### Brokers

Required

Environment Variable

MINIO_AUDIT_KAFKA_BROKERS[](#envvar.MINIO_AUDIT_KAFKA_BROKERS)

Configuration Setting

audit_kafka brokers[](#mc-conf.audit_kafka.brokers)

A comma-separated list of Kafka broker addresses:

```
brokers="https://kafka-1.example.net:9092,https://kafka-2.example.net:9092"
```

At least one broker must be online and reachable by the MinIO server to initialize and send audit log events.
MinIO checks each specified broker in order of specification.

#### Topic

Required

Environment Variable

MINIO_AUDIT_KAFKA_TOPIC[](#envvar.MINIO_AUDIT_KAFKA_TOPIC)

Configuration Setting

audit_kafka topic[](#mc-conf.audit_kafka.topic)

The name of the Kafka topic to associate to MinIO audit log events.

#### TLS

Optional

Environment Variable

MINIO_AUDIT_KAFKA_TLS[](#envvar.MINIO_AUDIT_KAFKA_TLS)

Configuration Setting

audit_kafka tls[](#mc-conf.audit_kafka.tls)

Set to `"on"` to enable TLS connectivity to the specified Kafka brokers.

Defaults to `"off"`.

#### TLS Skip Verify

Optional

Environment Variable

MINIO_AUDIT_KAFKA_TLS_SKIP_VERIFY[](#envvar.MINIO_AUDIT_KAFKA_TLS_SKIP_VERIFY)

Configuration Setting

audit_kafka tls_skip_verify[](#mc-conf.audit_kafka.tls_skip_verify)

Set to `"on"` to direct MinIO to skip verification of the Kafka broker TLS certificates.

You can use this option for enabling connectivity to Kafka brokers using TLS certificates signed by unknown parties, such as self-signed or corporate-internal Certificate Authorities (CA).

MinIO by default uses the system trust store and the contents of the MinIO [[operations/network-encryption#minio-tls|CA directory]] for verifying remote client TLS certificates.

Defaults to `"off"` for strict verification of TLS certificates.

#### SASL

Optional

Environment Variable

MINIO_AUDIT_KAFKA_SASL[](#envvar.MINIO_AUDIT_KAFKA_SASL)

Requires specifying [`MINIO_AUDIT_KAFKA_SASL_USERNAME`](#envvar.MINIO_AUDIT_KAFKA_SASL_USERNAME) and [`MINIO_AUDIT_KAFKA_SASL_PASSWORD`](#envvar.MINIO_AUDIT_KAFKA_SASL_PASSWORD).

Configuration Setting

audit_kafka sasl[](#mc-conf.audit_kafka.sasl)

Requires specifying [`sasl_username`](#mc-conf.audit_kafka.sasl_username) and [`sasl_password`](#mc-conf.audit_kafka.sasl_password).

Set to `"on"` to direct MinIO to use SASL to authenticate against the Kafka brokers.

#### SASL Username

Optional

Environment Variable

MINIO_AUDIT_KAFKA_SASL_USERNAME[](#envvar.MINIO_AUDIT_KAFKA_SASL_USERNAME)

Requires specifying [`MINIO_AUDIT_KAFKA_SASL`](#envvar.MINIO_AUDIT_KAFKA_SASL) and [`MINIO_AUDIT_KAFKA_SASL_PASSWORD`](#envvar.MINIO_AUDIT_KAFKA_SASL_PASSWORD).

Configuration Setting

audit_kafka sasl_username[](#mc-conf.audit_kafka.sasl_username)

Requires specifying [`sasl`](#mc-conf.audit_kafka.sasl) and [`sasl_password`](#mc-conf.audit_kafka.sasl_password).

The SASL username MinIO uses for authentication against the Kafka brokers.

#### SASL Password

Optional

Environment Variable

MINIO_AUDIT_KAFKA_SASL_PASSWORD[](#envvar.MINIO_AUDIT_KAFKA_SASL_PASSWORD)

Requires specifying [`MINIO_AUDIT_KAFKA_SASL`](#envvar.MINIO_AUDIT_KAFKA_SASL) and [`MINIO_AUDIT_KAFKA_SASL_USERNAME`](#envvar.MINIO_AUDIT_KAFKA_SASL_USERNAME).

Configuration Setting

audit_kafka sasl_password[](#mc-conf.audit_kafka.sasl_password)

Requires specifying [`sasl`](#mc-conf.audit_kafka.sasl) and [`sasl_username`](#mc-conf.audit_kafka.sasl_username).

The SASL password MinIO uses for authentication against the Kafka brokers.

#### SASL Mechanism

Optional

Environment Variable

MINIO_AUDIT_KAFKA_SASL_MECHANISM[](#envvar.MINIO_AUDIT_KAFKA_SASL_MECHANISM)

Important

The `PLAIN` authentication mechanism sends credentials in plain text over the network.
Use [`MINIO_AUDIT_KAFKA_TLS`](#envvar.MINIO_AUDIT_KAFKA_TLS) or to enable TLS connectivity to the Kafka brokers and ensure secure transmission of SASL credentials.

Configuration Setting

audit_kafka sasl_mechanism[](#mc-conf.audit_kafka.sasl_mechanism)

Important

The `PLAIN` authentication mechanism sends credentials in plain text over the network.
Use [`tls`](#mc-conf.audit_kafka.tls) to enable TLS connectivity to the Kafka brokers and ensure secure transmission of SASL credentials.

The SASL mechanism MinIO uses for authentication against the Kafka brokers.

Defaults to `plain`.

#### TLS  Client Auth

Optional

Environment Variable

MINIO_AUDIT_KAFKA_TLS_CLIENT_AUTH[](#envvar.MINIO_AUDIT_KAFKA_TLS_CLIENT_AUTH)

Requires specifying [`MINIO_AUDIT_KAFKA_CLIENT_TLS_CERT`](#envvar.MINIO_AUDIT_KAFKA_CLIENT_TLS_CERT) and [`MINIO_AUDIT_KAFKA_CLIENT_TLS_KEY`](#envvar.MINIO_AUDIT_KAFKA_CLIENT_TLS_KEY).

Configuration Setting

audit_kafka tls_client_auth[](#mc-conf.audit_kafka.tls_client_auth)

Requires specifying [`client_tls_cert`](#mc-conf.audit_kafka.client_tls_cert) and [`client_tls_key`](#mc-conf.audit_kafka.client_tls_key).

Set to `"on"` to direct MinIO to use mTLS to authenticate against the Kafka brokers.

#### Client TLS Certificate

Optional

Environment Variable

MINIO_AUDIT_KAFKA_CLIENT_TLS_CERT[](#envvar.MINIO_AUDIT_KAFKA_CLIENT_TLS_CERT)

Configuration Setting

audit_kafka client_tls_cert[](#mc-conf.audit_kafka.client_tls_cert)

The path to the TLS client certificate to use for mTLS authentication.

#### Client TLS Key

Optional

Environment Variable

MINIO_AUDIT_KAFKA_CLIENT_TLS_KEY[](#envvar.MINIO_AUDIT_KAFKA_CLIENT_TLS_KEY)

Configuration Setting

audit_kafka client_tls_key[](#mc-conf.audit_kafka.client_tls_key)

The path to the TLS client private key to use for mTLS authentication.

#### Version

Optional

Environment Variable

MINIO_AUDIT_KAFKA_VERSION[](#envvar.MINIO_AUDIT_KAFKA_VERSION)

Configuration Setting

audit_kafka version[](#mc-conf.audit_kafka.version)

The version of the Kafka broker MinIO expects at the specified endpoints.

MinIO returns an error if the Kakfa broker version does not match those specified to this setting.

#### Comment

Optional

Environment Variable

MINIO_AUDIT_KAFKA_COMMENT[](#envvar.MINIO_AUDIT_KAFKA_COMMENT)

Configuration Setting

audit_kafka comment[](#mc-conf.audit_kafka.comment)

A comment to associate with the configuration.

#### Queue Directory

Optional

Environment Variable

MINIO_AUDIT_KAFKA_QUEUE_DIR[](#envvar.MINIO_AUDIT_KAFKA_QUEUE_DIR)

Configuration Setting

audit_kafka queue_dir[](#mc-conf.audit_kafka.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the Kafka service is offline and replays the stored events when connectivity resumes.

#### Queue Size

Optional

Environment Variable

MINIO_AUDIT_KAFKA_QUEUE_SIZE[](#envvar.MINIO_AUDIT_KAFKA_QUEUE_SIZE)

Configuration Setting

audit_kafka queue_size[](#mc-conf.audit_kafka.queue_size)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).