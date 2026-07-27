---
title: "Publish Server or Audit Logs to an External Service — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/monitoring/minio-logging.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/monitoring/minio-logging"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Publish Server or Audit Logs to an External Service — SILO (MinIO Object Storage)"
---
# Publish Server or Audit Logs to an External Service

MinIO publishes all [[reference/minio-server/minio-server#command-minio.server|`minio server`]] operations to the system console.
Reading these logs depends on how the server process is managed.
For example, if the server is managed through a `systemd` script,
you can read the logs using `journalctl -u SERVICENAME.service`. Replace
`SERVICENAME` with the name of the MinIO service.

MinIO also supports publishing server logs and audit logs to an HTTP webhook.

[Server logs](#minio-logging-publish-server-logs) contain the same
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] operations logged to the system console. Server logs
support general monitoring and troubleshooting of operations.

[Audit logs](#minio-logging-publish-audit-logs) are more granular
descriptions of each operation on the MinIO deployment. Audit logging
supports security standards and regulations which require detailed tracking
of operations.

MinIO publishes logs as a JSON document as a `PUT` request to each configured
endpoint. The endpoint server is responsible for processing each JSON document.
MinIO requires explicit configuration of each webhook endpoint and does not
publish logs to a webhook by default.

## Publish Server Logs to HTTP Webhook

You can configure a new HTTP webhook endpoint to which MinIO publishes
[[reference/minio-server/minio-server#command-minio.server|`minio server`]] logs using either environment variables or by setting
runtime configuration settings.

Environment Variables

MinIO supports specifying the [[reference/minio-server/minio-server#command-minio.server|`minio server`]] log HTTP webhook endpoint
and associated configuration settings using [[reference/minio-server/settings/metrics-and-logging#minio-server-envvar-logging-regular|environment variables]].

The following example code sets all environment variables related to
configuring a log HTTP webhook endpoint. The minimum required variables
are:

[[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_LOGGER_WEBHOOK_ENABLE|`MINIO_LOGGER_WEBHOOK_ENABLE`]]

[[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_LOGGER_WEBHOOK_ENDPOINT|`MINIO_LOGGER_WEBHOOK_ENDPOINT`]]

```
export MINIO_LOGGER_WEBHOOK_ENABLE_<IDENTIFIER>="on"
   export MINIO_LOGGER_WEBHOOK_ENDPOINT_<IDENTIFIER>="https://webhook-1.example.net"
   export MINIO_LOGGER_WEBHOOK_AUTH_TOKEN_<IDENTIFIER>="TOKEN"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
HTTP webhook endpoint. Use the same `<IDENTIFIER>` for all environment
variables related to the new log HTTP webhook.

If the specified `<IDENTIFIER>` matches an existing log endpoint,
the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get logger_webhook`]]
to review the currently configured log HTTP webhook endpoints.

Replace `https://webhook-1.example.net` with the URL of the HTTP
webhook endpoint.

Replace `TOKEN` with an authentication token of the appropriate type for the endpoint.
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

Consult the documenation for the desired service for more details.

Restart the MinIO server to apply the new configuration settings. You
must specify the same environment variables and settings on
all MinIO servers in the deployment.

Configuration Settings

MinIO supports adding or updating log HTTP webhook endpoints on a MinIO
deployment using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command and the
[[reference/minio-server/settings/metrics-and-logging#mc-conf.logger_webhook|`logger_webhook`]] configuration key. You must restart the
MinIO deployment to apply any new or updated configuration settings.

The following example code sets all settings related to configuring
a log HTTP webhook endpoint. The minimum required setting is
[[reference/minio-server/settings/metrics-and-logging#mc-conf.logger_webhook.endpoint|`logger_webhook endpoint`]]:

```
mc admin config set ALIAS/ logger_webhook:IDENTIFIER  \
   endpoint="https://webhook-1.example.net"           \
   auth_token="TOKEN"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
HTTP webhook endpoint. Use the same `<IDENTIFIER>` for all environment
variables related to the new log HTTP webhook.

If the specified `<IDENTIFIER>` matches an existing log endpoint,
the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get logger_webhook`]]
to review the currently configured log HTTP webhook endpoints.

Replace `https://webhook-1.example.net` with the URL of the HTTP
webhook endpoint.

Replace `TOKEN` with an authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example: for a Bearer token, prepend `Bearer`:

```
mc admin config set ALIAS/ logger_webhook    \
    endpoint="https://webhook-1.example.net"  \
    auth_token="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
mc admin config set ALIAS/ logger_webhook    \
   endpoint="https://webhook-1.example.net"  \
   auth_token="ServiceXYZ 1a2b3c4f5e"
```

Consult the documenation for the desired service for more details.

## Publish Audit Logs to HTTP Webhook

You can configure a new HTTP webhook endpoint to which MinIO publishes audit
logs using either environment variables or by setting runtime configuration
settings:

Environment Variables

MinIO supports specifying the audit log HTTP webhook endpoint and
associated configuration settings using [[reference/minio-server/settings/metrics-and-logging#minio-server-envvar-logging-audit|environment variables]].

The following example code sets all environment variables related to
configuring a audit log HTTP webhook endpoint. The minimum required
variables are:

[[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_AUDIT_WEBHOOK_ENABLE|`MINIO_AUDIT_WEBHOOK_ENABLE`]]

[[reference/minio-server/settings/metrics-and-logging#envvar.MINIO_AUDIT_WEBHOOK_ENDPOINT|`MINIO_AUDIT_WEBHOOK_ENDPOINT`]]

```
export MINIO_AUDIT_WEBHOOK_ENABLE_<IDENTIFIER>="on"
export MINIO_AUDIT_WEBHOOK_ENDPOINT_<IDENTIFIER>="https://webhook-1.example.net"
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_<IDENTIFIER>="TOKEN"
export MINIO_AUDIT_WEBHOOK_CLIENT_CERT_<IDENTIFIER>="cert.pem"
export MINIO_AUDIT_WEBHOOK_CLIENT_KEY_<IDENTIFIER>="cert.key"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
HTTP webhook endpoint. Use the same `<IDENTIFIER>` for all environment
variables related to the new audit log HTTP webhook.

If the specified `<IDENTIFIER>` matches an existing log endpoint,
the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get audit_webhook`]]
to review the currently configured audit log HTTP webhook endpoints.

Replace `https://webhook-1.example.net` with the URL of the HTTP
webhook endpoint.

Replace `TOKEN` with an authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example: for a Bearer token, prepend `Bearer`:

```
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_myendpoint="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
export MINIO_AUDIT_WEBHOOK_AUTH_TOKEN_xyz="ServiceXYZ 1a2b3c4f5e"
```

Consult the documenation for the desired service for more details.

Replace `cert.pem` and `cert.key` with the public and private key
of the x.509 TLS certificates to present to the HTTP webhook server.
Omit for endpoints which do not require clients to present TLS
certificates.

Restart the MinIO server to apply the new configuration settings. You
must specify the same environment variables and settings on
all MinIO servers in the deployment.

Configuration Settings

MinIO supports adding or updating audit log HTTP webhook endpoints on a
MinIO deployment using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command and the
[[reference/minio-server/settings/metrics-and-logging#mc-conf.audit_webhook|`audit_webhook`]] configuration key. You must restart the MinIO
deployment to apply any new or updated configuration settings.

The following example code sets all settings related to configuring
a audit log HTTP webhook endpoint. The minimum required setting is
[[reference/minio-server/settings/metrics-and-logging#mc-conf.audit_webhook.endpoint|`audit_webhook endpoint`]]:

```
mc admin config set ALIAS/ audit_webhook:IDENTIFIER  \
   endpoint="https://webhook-1.example.net"          \
   auth_token="TOKEN"                                \
   client_cert="cert.pem"                            \
   client_key="cert.key"
```

Replace `<IDENTIFIER>` with a unique descriptive string for the
HTTP webhook endpoint. Use the same `<IDENTIFIER>` for all environment
variables related to the new audit log HTTP webhook.

If the specified `<IDENTIFIER>` matches an existing log endpoint,
the new settings override any existing settings for that endpoint.
Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get audit_webhook`]]
to review the currently configured audit log HTTP webhook endpoints.

Replace `https://webhook-1.example.net` with the URL of the HTTP
webhook endpoint.

Replace `TOKEN` with an authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example: for a Bearer token, prepend `Bearer`:

```
mc admin config set ALIAS/ audit_webhook     \
    endpoint="https://webhook-1.example.net"  \
    auth_token="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
mc admin config set ALIAS/ audit_webhook     \
   endpoint="https://webhook-1.example.net"  \
   auth_token="ServiceXYZ 1a2b3c4f5e"
```

Consult the documenation for the desired service for more details.

Replace `cert.pem` and `cert.key` with the public and private key
of the x.509 TLS certificates to present to the HTTP webhook server.
Omit for endpoints which do not require clients to present TLS
certificates.

### Audit Log Structure

MinIO audit logs resemble the following JSON document:

The `api.timeToFirstByte` and `api.timeToResponse` fields are expressed
in nanoseconds.

For [[operations/concepts/erasure-coding#minio-erasure-coding|erasure coded setups]]
`tags.objectErasureMap` provides per-object details on the following:

The [[operations/concepts#minio-intro-server-pool|Server Pool]] on which the object
operation was performed.

The [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure set]] on which the object
operation was performed.

The list of drives in the erasure set which participated in the
object operation.

```
{
   "version": "1",
   "deploymentid": "8ca2b7ad-20cf-4d07-9efb-28b2f519f4a5",
   "time": "2024-02-29T19:39:25.744431903Z",
   "event": "",
   "trigger": "incoming",
   "api": {
      "name": "CompleteMultipartUpload",
      "bucket": "data",
      "object": "test-data.csv",
      "status": "OK",
      "statusCode": 200,
      "rx": 267,
      "tx": 358,
      "txHeaders": 387,
      "timeToFirstByte": "2096989ns",
      "timeToFirstByteInNS": "2096989",
      "timeToResponse": "2111986ns",
      "timeToResponseInNS": "2111986"
   },
   "remotehost": "127.0.0.1",
   "requestID": "17B86CB0ED88EBE9",
   "userAgent": "MinIO (linux; amd64) minio-go/v7.0.67 mc/RELEASE.2024-02-24T01-33-20Z",
   "requestPath": "/data/test-data.csv",
   "requestHost": "minio.example.net:9000",
   "requestQuery": {
      "uploadId": "OGNhMmI3YWQtMjBjZi00ZDA3LTllZmItMjhiMmY1MTlmNGE1LmU3MjNlNWI4LTNiYWYtNDYyNy1hNzI3LWMyNDE3NTVjMmMzNw"
   },
   "requestHeader": {
      "Accept-Encoding": "zstd,gzip",
      "Authorization": "AWS4-HMAC-SHA256 Credential=minioadmin/20240229/us-east-1/s3/aws4_request, SignedHeaders=content-type;host;x-amz-content-sha256;x-amz-date, Signature=ccb3acdc1763509a88a7e4a3d7fe431ef0ee5ca3f66ccb430d5a09326e87e893",
      "Content-Length": "267",
      "Content-Type": "application/octet-stream",
      "User-Agent": "MinIO (linux; amd64) minio-go/v7.0.67 mc/RELEASE.2024-02-24T01-33-20Z",
      "X-Amz-Content-Sha256": "d61969719ee94f43c4e87044229b7a13b54cab320131e9a77259ad0c9344f6d3",
      "X-Amz-Date": "20240229T193925Z"
   },
   "responseHeader": {
      "Accept-Ranges": "bytes",
      "Content-Length": "358",
      "Content-Type": "application/xml",
      "ETag": "1d9fdc88af5e74f5eac0a3dd750ce58e-2",
      "Server": "MinIO",
      "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
      "Vary": "Origin,Accept-Encoding",
      "X-Amz-Id-2": "dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8",
      "X-Amz-Request-Id": "17B86CB0ED88EBE9",
      "X-Content-Type-Options": "nosniff",
      "X-Xss-Protection": "1; mode=block"
   },
   "tags": {
      "objectLocation": {
            "name": "Mousepad Template-v03final.jpg",
            "poolId": 1,
            "setId": 1,
            "disks": [
               "/mnt/drive-1",
               "/mnt/drive-2",
               "/mnt/drive-3",
               "/mnt/drive-4"
            ]
      }
   },
   "accessKey": "minioadmin"
}
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).