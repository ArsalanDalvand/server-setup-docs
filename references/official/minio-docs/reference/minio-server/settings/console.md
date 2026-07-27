---
title: "MinIO Console Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/console.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/console"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Console Settings — SILO (MinIO Object Storage)"
---
# MinIO Console Settings

Changed in version RELEASE.2025-05-24T17-08-30Z: The Console now presents only object browser capabilities similar to those available through the [[reference/minio-mc#command-mc|`mc`]] tool.
For administrative interactions, such as user management, use the [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] command.

Some of the settings on this page may no longer be relevant for newer deployments.

This page covers settings that manage access and behavior for the MinIO Console.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Browser Settings

The following settings control behavior for the embedded MinIO Console.

### MinIO Console

Optional

Environment Variable

MINIO_BROWSER[](#envvar.MINIO_BROWSER)

Specify `off` to disable the embedded MinIO Console.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Animation

Optional

Environment Variable

MINIO_BROWSER_LOGIN_ANIMATION[](#envvar.MINIO_BROWSER_LOGIN_ANIMATION)

New in version MinIO: Server RELEASE.2023-05-04T21-44-30Z

Specify `off` to disable the animated login screen for the MinIO Console.
Defaults to `on`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Browser Redirect

Optional

Environment Variable

MINIO_BROWSER_REDIRECT[](#envvar.MINIO_BROWSER_REDIRECT)

New in version MinIO: Server RELEASE.2023-09-16T01-01-47Z

Specify whether requests from a web browser automatically redirect to the Console address.
Defaults to `true`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Browser Redirect URL

Optional

Environment Variable

MINIO_BROWSER_REDIRECT_URL[](#envvar.MINIO_BROWSER_REDIRECT_URL)

Specify the Fully Qualified Domain Name (FQDN) the MinIO Console listens for incoming connections on.

If you want to host the MinIO Console exclusively from a reverse-proxy service, you must specify the hostname managed by that service.

For example, consider a reverse proxy configured to route `https://example.net/minio/` to the MinIO Console.
You must set this environment variable to match that hostname for the Console to both listen and respond to requests using that hostname.

If you omit this variable, the Console listens and responds to all IP addresses or hostnames associated to the host machine on which the MinIO Server runs.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Session Duration

Optional

Environment Variable

MINIO_BROWSER_SESSION_DURATION[](#envvar.MINIO_BROWSER_SESSION_DURATION)

New in version MinIO: Server RELEASE.2023-08-23T10-07-06Z

Specify the duration of a browser session for working with the MinIO Console.

MinIO supports the following units of time measurement:

`s` - seconds, “60s”

`m` - minutes, “60m”

`h` - hours, “24h”

`d` - days, “7d”

Defaults to `12h`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Log Query URL

Optional

Environment Variable

MINIO_LOG_QUERY_URL[](#envvar.MINIO_LOG_QUERY_URL)

Specify the URL of a PostgreSQL service to which MinIO writes [[operations/monitoring/minio-logging#minio-logging-publish-audit-logs|Audit logs]].
The embedded MinIO Console provides a Log Search tool that allows querying the PostgreSQL service for collected logs.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Content Security Policy

Optional

Configure MinIO Console to generate a [Content-Security-Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) header in HTTP responses.
Defaults to `default-src 'self' 'unsafe-eval' 'unsafe-inline';`

Environment Variable

MINIO_BROWSER_CONTENT_SECURITY_POLICY[](#envvar.MINIO_BROWSER_CONTENT_SECURITY_POLICY)

```
export MINIO_BROWSER_CONTENT_SECURITY_POLICY="default-src 'self' 'unsafe-eval' 'unsafe-inline';"
```

Configuration Setting

browser csp_policy[](#mc-conf.browser.csp_policy)

```
mc admin config set browser \
   csp_policy="default-src 'self' 'unsafe-eval' 'unsafe-inline';" \
   [ARGUMENT=VALUE ...]
```

### Strict Transport Security

Optional

Configure MinIO console to generate a [Strict-Transport-Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) header in HTTP responses.

To generate the header, you must set a duration using either [`MINIO_BROWSER_HSTS_SECONDS`](#envvar.MINIO_BROWSER_HSTS_SECONDS) or [`hsts_seconds`](#mc-conf.browser.hsts_seconds).
Other HSTS settings are optional.

Environment Variables

MINIO_BROWSER_HSTS_SECONDS[](#envvar.MINIO_BROWSER_HSTS_SECONDS)

The `max_age` the configured policy remains in effect, in seconds.
Defaults to `0`, disabled.
You must configure a non-zero duration to enable the `Strict-Transport-Security` header.

```
export MINIO_BROWSER_HSTS_SECONDS=31536000
```

MINIO_BROWSER_HSTS_INCLUDE_SUB_DOMAINS[](#envvar.MINIO_BROWSER_HSTS_INCLUDE_SUB_DOMAINS)

Set to `on` to also apply the configured HSTS policy to all MinIO Console subdomains.
Defaults to `off`.

```
export MINIO_BROWSER_HSTS_INCLUDE_SUB_DOMAINS="on"
```

MINIO_BROWSER_HSTS_PRELOAD[](#envvar.MINIO_BROWSER_HSTS_PRELOAD)

Set to `on` to direct the client browser to add the MinIO Console domain to its HSTS preload list.
Defaults to `off`.

```
export MINIO_BROWSER_HSTS_PRELOAD="on"
```

Configuration Settings

The following configuration settings require a service restart to take effect.
To restart the service, use [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]].

browser hsts_seconds[](#mc-conf.browser.hsts_seconds)

The `max_age` the configured policy remains in effect, in seconds.
Defaults to `0`, disabled.
You must configure a non-zero duration to enable the `Strict-Transport-Security` header.

```
mc admin config set browser \
   hsts_seconds="31536000" \
   [ARGUMENT=VALUE ...]
```

browser hsts_include_subdomains[](#mc-conf.browser.hsts_include_subdomains)

Set to `on` to also apply the configured HSTS policy to all MinIO Console subdomains.
Defaults to `off`.

```
mc admin config set browser \
   hsts_include_subdomains="on" \
   hsts_seconds="31536000" \
   [ARGUMENT=VALUE ...]
```

browser hsts_preload[](#mc-conf.browser.hsts_preload)

Set to `on` to direct the client browser to add the MinIO Console domain to its HSTS preload list.
Defaults to `off`.

```
mc admin config set browser \
   hsts_preload="on" \
   hsts_seconds="31536000" \
   [ARGUMENT=VALUE ...]
```

#### Examples

The following examples show the rendered header for the given configuration settings.
The equivalent environment variables generate the same result.
All examples use a value of `31536000`, which is the number of seconds in a calendar year (365 days).

`hsts_seconds`

```
mc admin config set ALIAS browser hsts_seconds=31536000
```

```
Strict-Transport-Security: max-age=31536000
```

`hsts_include_subdomains`

```
mc admin config set ALIAS browser hsts_seconds=31536000 hsts_include_subdomains=on
```

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

`hsts_preload`

```
mc admin config set ALIAS browser hsts_seconds=31536000 hsts_include_subdomains=on hsts_preload=on
```

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

### Referrer Policy

Optional

Configure MinIO Console to generate a [Referrer-Policy](https://www.w3.org/TR/referrer-policy/) header in HTTP responses.
Defaults to `strict-origin-when-cross-origin`.

Environment Variable

MINIO_BROWSER_REFERRER_POLICY[](#envvar.MINIO_BROWSER_REFERRER_POLICY)

```
export MINIO_BROWSER_REFERRER_POLICY="strict-origin-when-cross-origin"
```

Configuration Setting

browser referrer_policy[](#mc-conf.browser.referrer_policy)

```
mc admin config set browser \
   referrer_policy="strict-origin-when-cross-origin" \
   [ARGUMENT=VALUE ...]
```

## Prometheus Settings

The following settings manage how MinIO interacts with your Prometheus service.

### Prometheus URL

Optional

Environment Variable

MINIO_PROMETHEUS_URL[](#envvar.MINIO_PROMETHEUS_URL)

Specify the URL for a Prometheus service configured to [[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|scrape MinIO metrics]].

The MinIO Console populates the Dashboard with cluster metrics using the `minio-job` Prometheus scraping job.

If you are using a standalone MinIO Console process, this variable corresponds with `CONSOLE_PROMETHEUS_URL`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Prometheus Job ID

Optional

Environment Variable

MINIO_PROMETHEUS_JOB_ID[](#envvar.MINIO_PROMETHEUS_JOB_ID)

Specify the custom Prometheus job ID used for [[operations/monitoring/collect-minio-metrics-using-prometheus#minio-metrics-collect-using-prometheus|scraping MinIO metrics]].

MinIO defaults to `minio-job`.

If you are using a standalone MinIO Console process, this variable corresponds with `CONSOLE_PROMETHEUS_JOB_ID`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

### Prometheus Auth Token

Optional

Environment Variable

MINIO_PROMETHEUS_AUTH_TOKEN[](#envvar.MINIO_PROMETHEUS_AUTH_TOKEN)

Specify the [basic auth token](https://prometheus.io/docs/guides/basic-auth/) the Console should use to connect to a Prometheus service.

For example, a basic auth token you might use could resemble the following:

```
eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJwcm9tZXRoZXVzIiwic3ViIjoibWluaW8iLCJleHAiOjQ4NTAwMzg0MDJ9.GZCKR3d0FH2TCvNHSd39HaVfSuQVVV0s8glICBDmhT51V6CQ_hw8gTYlKHJmcpR8aHkqiJwCqcYJhaMmqwe00XY
```

If you are using a standalone MinIO Console process, this variable corresponds with `CONSOLE_PROMETHEUS_AUTH_TOKEN`.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).