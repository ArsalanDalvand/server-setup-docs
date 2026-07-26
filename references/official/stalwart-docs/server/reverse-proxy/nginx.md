---
title: "NGINX"
source: "https://stalw.art/docs/server/reverse-proxy/nginx"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/reverse-proxy/nginx"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "NGINX"
---
# NGINX

      

[NGINX](https://nginx.org) is a web server that also functions as a reverse proxy, load balancer, and HTTP cache. The `stream` module extends it with TCP / UDP proxying, which is what Stalwart needs for the mail ports.

Stalwart can sit behind NGINX for both HTTP and mail traffic. The example below uses TCP-passthrough on every port, so NGINX hands the TLS connection to Stalwart untouched and the original client IP is preserved through the [[server/reverse-proxy/proxy-protocol|Proxy Protocol]]. HTTP-upstream patterns (NGINX terminating TLS for the WebUI and forwarding plain HTTP to Stalwart’s port `8080`) are also supported and produce identical OAuth, OIDC, and JMAP discovery responses; see [[server/reverse-proxy#how-discovery-urls-are-composed|How discovery URLs are composed]] for the full model and set the [[configuration/environment-variables#public-urls|`STALWART_PUBLIC_URL`]] variable to the URL clients reach NGINX on whenever it differs from `https://<defaultHostname>` on port `443`.

Stalwart accepts the Proxy Protocol header when the source address is listed in [[ref/object/system-settings#proxytrustednetworks|`proxyTrustedNetworks`]] on the [[ref/object/system-settings|SystemSettings]] singleton (found in the WebUI under  Settings ›  Network › Services,  Settings ›  Network › General) or in [[ref/object/network-listener#overrideproxytrustednetworks|`overrideProxyTrustedNetworks`]] on the matching [[ref/object/network-listener|NetworkListener]].

## Configuration

[Section titled “Configuration”](#configuration)

NGINX must be built with `--with-stream` for TCP proxying to be available.

/etc/nginx/nginx.conf

```
# Load the required modulesload_module modules/ngx_stream_module.so;
events {    worker_connections 1024;}
# Stream configurationstream {    # Proxy SMTP    server {        listen 25;        proxy_pass 127.0.0.1:10025;        proxy_protocol on;    }
    # Proxy IMAPS    server {        listen 993;        proxy_pass 127.0.0.1:10993;        proxy_protocol on;    }
    # Proxy SMTPS    server {        listen 465;        proxy_pass 127.0.0.1:10465;        proxy_protocol on;    }
    # Proxy HTTPS    server {        listen 443;        proxy_pass 127.0.0.1:10443;        proxy_protocol on;    }}
```

The `proxy_protocol on` directive emits a Proxy Protocol header toward Stalwart on every forwarded connection. The `proxy_protocol` flag is intentionally absent from each `listen` line: that flag tells NGINX to expect an incoming Proxy Protocol header from the client, which mail and HTTP clients do not send. Add it only when another Proxy-Protocol-aware proxy sits in front of NGINX.