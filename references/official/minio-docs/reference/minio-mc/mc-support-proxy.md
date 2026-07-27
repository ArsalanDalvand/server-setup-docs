---
title: "mc support proxy — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-proxy.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-proxy"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support proxy — SILO (MinIO Object Storage)"
---
# mc support proxy

## Description

Use the [`mc support proxy`](#command-mc.support.proxy) command to configure a proxy to use to communicate with [MinIO SUBNET](https://min.io/pricing?jmp=docs).

## Examples

### Set a Proxy URL

Define the proxy URL to use when the deployment `minio1` communicates to SUBNET.
The proxy URL in the example is `http://my.proxy`.

```
mc support proxy set minio1 http://my.proxy
```

### Remove the Proxy URL Configured for a Deployment

The following command removes the URL configured as the proxy for the alias `minio1`.

```
mc support proxy remove minio1
```

### Disable callhome Logs

The following command shows the URL configured as the proxy for the alias `minio1`.

```
mc support proxy show minio1
```

## Syntax

mc support proxy set[](#mc.support.proxy.set)

Create a proxy URL for the MinIO deployment to use when communicating with [MinIO SUBNET](https://min.io/pricing?jmp=docs).

```
mc support proxy set ALIAS PROXY_URL
```

mc support proxy show[](#mc.support.proxy.show)

Display the current proxy URL configured for communicating with [MinIO SUBNET](https://min.io/pricing?jmp=docs).

```
mc support proxy show ALIAS
```

mc support proxy remove[](#mc.support.proxy.remove)

Remove the proxy URL configured for communicating with [MinIO SUBNET](https://min.io/pricing?jmp=docs).

```
mc support proxy remove ALIAS
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).