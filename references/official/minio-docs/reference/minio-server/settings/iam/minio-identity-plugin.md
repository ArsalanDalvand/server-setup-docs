---
title: "MinIO Identity Management Plugin Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/iam/minio-identity-plugin.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/iam/minio-identity-plugin"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Identity Management Plugin Settings — SILO (MinIO Object Storage)"
---
# MinIO Identity Management Plugin Settings

This page documents settings for enabling external identity management using the MinIO Identity Management Plugin.
See [[administration/identity-access-management/pluggable-authentication#minio-external-identity-management-plugin|MinIO External Identity Management Plugin]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Examples

When setting up the MinIO Identity Management Plugin, you must define at a minimum all of the required settings.
The examples here represent the minimum required settings.

Environment Variables

```
MINIO_IDENTITY_PLUGIN_URL="https://authservice.example.net:8080/auth"
MINIO_IDENTITY_PLUGIN_ROLE_POLICY="ConsoleUser"
```

Configuration Settings

identity_plugin[](#mc-conf.identity_plugin)

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to create or update the identity plugin configuration.
The `identity_plugin url` argument is required.
Specify additional optional arguments as a whitespace (” “)-delimited list.

```
mc admin config set identity_plugin                  \
   url="https://external-auth.example.net:8080/auth" \
   role_policy="consoleAdmin"                        \
   [ARGUMENT=VALUE] ...
```

## Settings

### URL

Required

Environment Variable

MINIO_IDENTITY_PLUGIN_URL[](#envvar.MINIO_IDENTITY_PLUGIN_URL)

Configuration Setting

identity_plugin url[](#mc-conf.identity_plugin.url)

The webhook endpoint for the external identity management service (`https://authservice.example.net:8080/auth`).

### Role Policy

Required

Environment Variable

MINIO_IDENTITY_PLUGIN_ROLE_POLICY[](#envvar.MINIO_IDENTITY_PLUGIN_ROLE_POLICY)

Configuration Setting

identity_plugin role_policy[](#mc-conf.identity_plugin.role_policy)

Specify a comma-separated list of MinIO [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to assign to authenticated users.

### Enable

Optional

Environment Variable

This setting does not have an environment variable option.

Configuration Setting

identity_plugin enabled[](#mc-conf.identity_plugin.enabled)

Set to `false` to disable the identity provider configuration.

Applications cannot generate STS credentials or otherwise authenticate to MinIO using the configured provider if set to `false`.

Defaults to `true` or “enabled”.

### Token

Optional

Environment Variable

MINIO_IDENTITY_PLUGIN_TOKEN[](#envvar.MINIO_IDENTITY_PLUGIN_TOKEN)

Configuration Setting

identity_plugin token[](#mc-conf.identity_plugin.token)

An authentication token to present to the configured webhook endpoint.

Specify a supported HTTP [Authentication scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#authentication_schemes) as a string value, such as `"Bearer TOKEN"`.
MinIO sends the token using the HTTP [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) header.

### Role ID

Optional

Environment Variable

MINIO_IDENTITY_PLUGIN_ROLE_ID[](#envvar.MINIO_IDENTITY_PLUGIN_ROLE_ID)

Configuration Setting

identity_plugin role_id[](#mc-conf.identity_plugin.role_id)

Specify a unique ID MinIO uses to generate an ARN for this identity manager.
MinIO automatically adds an `idmp-` prefix to the specified ID when generating the ARN.

If omitted, MinIO automatically generates the ID and prints the full ARN to the server log.

### Comment

Optional

Environment Variable

MINIO_IDENTITY_PLUGIN_COMMENT[](#envvar.MINIO_IDENTITY_PLUGIN_COMMENT)

Configuration Setting

identity_plugin comment[](#mc-conf.identity_plugin.comment)

Specify a comment to associate to the identity configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).