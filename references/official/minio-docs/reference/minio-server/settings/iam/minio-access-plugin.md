---
title: "MinIO Access Management Plugin Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/iam/minio-access-plugin.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/iam/minio-access-plugin"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Access Management Plugin Settings — SILO (MinIO Object Storage)"
---
# MinIO Access Management Plugin Settings

This page documents settings for enabling external authorization management using the MinIO Access Management Plugin.
See [[administration/identity-access-management/pluggable-authorization#minio-external-access-management-plugin|MinIO External Access Management Plugin]] for a tutorial on using these settings.

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

When setting up the MinIO Access Management plugin, you must define at minimum all required settings.
The examples here represent the minimum required setting.

Environment Variables

```
MINIO_POLICY_PLUGIN_URL="https://authzservice.example.net:8080/authz"
```

Configuration Settings

policy_plugin[](#mc-conf.policy_plugin)

Use the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command to create or update the access management plugin configuration.
The `policy_plugin url` argument is required.
Specify additional optional arguments as a whitespace (” “)-delimited list.

```
mc admin config set policy_plugin                     \
   url="https://authzservice.example.net:8080/authz"  \
   [ARGUMENT=VALUE] ...
```

## Settings

### URL

Required

Environment Variable

MINIO_POLICY_PLUGIN_URL[](#envvar.MINIO_POLICY_PLUGIN_URL)

Configuration Setting

policy_plugin url[](#mc-conf.policy_plugin.url)

The webhook endpoint for the external access management service (`https://authzservice.example.net:8080/authz`).

### Auth Token

Optional

Environment Variable

MINIO_POLICY_PLUGIN_AUTH_TOKEN[](#envvar.MINIO_POLICY_PLUGIN_AUTH_TOKEN)

Configuration Setting

policy_plugin auth_token[](#mc-conf.policy_plugin.auth_token)

An authentication token to present to the configured webhook endpoint.

Specify a supported HTTP [Authentication scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#authentication_schemes) as a string value, such as `"Bearer TOKEN"`.
MinIO sends the token using the HTTP [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) header.

### HTTP2

Optional

Environment Variable

MINIO_POLICY_PLUGIN_ENABLE_HTTP2[](#envvar.MINIO_POLICY_PLUGIN_ENABLE_HTTP2)

Configuration Setting

policy_plugin enable_http2[](#mc-conf.policy_plugin.enable_http2)

Enable experimental HTTP2 support for connecting to the configure webhook service.

Defaults to off

### Comment

Optional

Environment Variable

MINIO_POLICY_PLUGIN_COMMENT[](#envvar.MINIO_POLICY_PLUGIN_COMMENT)

Configuration Setting

policy_plugin comment[](#mc-conf.policy_plugin.comment)

Specify a comment to associate to the external access management configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).