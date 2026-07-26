---
title: "Caddy"
source: "https://stalw.art/docs/server/reverse-proxy/caddy"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/reverse-proxy/caddy"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Caddy"
---
# Caddy

      

[Caddy](https://caddyserver.com) is an open-source, HTTP/2-enabled web server with automatic HTTPS. It covers static file serving, simple reverse proxies, and TLS management out of the box, and is often chosen for the conciseness of its Caddyfile syntax.

Stalwart can sit behind Caddy for HTTP traffic. Caddy handles HTTPS termination and certificate management, then forwards requests to Stalwart.

The example below forwards to Stalwart’s HTTPS listener with Proxy Protocol v2, but a plain HTTP-upstream pattern (Caddy forwarding plain HTTP to Stalwart’s port `8080`) works equally well and produces the same OAuth, OIDC, and JMAP discovery responses. See [[server/reverse-proxy#how-discovery-urls-are-composed|How discovery URLs are composed]] for the model that drives those responses, and set the [[configuration/environment-variables#public-urls|`STALWART_PUBLIC_URL`]] variable to the URL clients reach Caddy on whenever it differs from `https://<defaultHostname>` on port `443`.

## Note on Layer 4 support

[Section titled “Note on Layer 4 support”](#note-on-layer-4-support)

Caddy does not natively forward raw TCP streams, so it cannot forward the mail ports (SMTP, IMAP, POP3, ManageSieve) or preserve the [[server/reverse-proxy/proxy-protocol|Proxy Protocol]] on them. To carry client IPs into Stalwart’s mail listeners, either put [[server/reverse-proxy/haproxy|HAProxy]] or [[server/reverse-proxy/nginx|NGINX]] in front of Caddy, or build a Caddy binary that includes the community-maintained `caddy-l4` plugin. The plugin adds layer-4 forwarding and Proxy Protocol emission to Caddy.

For example, using `xcaddy`:

```
xcaddy build --with github.com/mholt/caddy-l4/modules/l4proxy \    --with github.com/mholt/caddy-l4/modules/l4tls \    --with github.com/mholt/caddy-l4/modules/l4proxyprotocol
```

## Configuration

[Section titled “Configuration”](#configuration)

The following Caddyfile forwards the mail ports (25, 465, 993, 4190) using `caddy-l4` with Proxy Protocol v2 and reverse-proxies HTTPS for `mail.example.com`:

```
{    layer4 {        0.0.0.0:25 {            route {                proxy {                    proxy_protocol v2                    upstream 127.0.0.1:10025                }            }        }
        0.0.0.0:993 {            route {                proxy {                    proxy_protocol v2                    upstream 127.0.0.1:10993                }            }        }
        0.0.0.0:465 {            route {                proxy {                    proxy_protocol v2                    upstream 127.0.0.1:10465                }            }        }
        0.0.0.0:4190 {            route {                proxy {                    proxy_protocol v2                    upstream 127.0.0.1:14190                }            }        }    }}
example.com {    redir https://www.example.com{uri}}
www.example.com {    root * /var/www/imkerei
    file_server}
mail.example.com {    reverse_proxy https://127.0.0.1:10443 {        transport http {            proxy_protocol v2            tls_server_name mail.example.com        }    }}
```

## Certificate management

[Section titled “Certificate management”](#certificate-management)

### Crontab

[Section titled “Crontab”](#crontab)

Automate copying Caddy-issued certificates into Stalwart’s certificate directory from cron:

Terminal window

```
0 3 * * * cat /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/example.com/example.com.crt > /opt/stalwart/cert/example.com.pem0 3 * * * cat /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/example.com/example.com.key > /opt/stalwart/cert/example.com.priv.pem
```

### Systemd

[Section titled “Systemd”](#systemd)

As an alternative to cron, a systemd path unit can watch the Caddy certificate file, copy the renewed files into place, and trigger a hot reload on Stalwart:

stalwart.path:

```
[Unit]Description=import certs from caddy to stalwart
[Path]PathModified=/var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/mail.example.com/mail.example.com.crt
[Install]WantedBy=multi-user.target
```

stalwart.service:

```
[Unit]Description=imports certs from caddy to stalwart
[Service]Type=oneshotExecStart=/usr/bin/cp -f /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/mail.example.com/mail.example.com.pem /opt/stalwart/cert/ExecStart=/usr/bin/cp -f /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/mail.example.com/mail.example.com.priv.pem /opt/stalwart/cert/ExecStart=/usr/local/bin/stalwart-cli --url https://mail.example.com --api-key <TOKEN> create action/ReloadTlsCertificates
[Install]WantedBy=multi-user.target
```

## Stalwart configuration

[Section titled “Stalwart configuration”](#stalwart-configuration)

Create a [[ref/object/certificate|Certificate]] object (found in the WebUI under  Settings ›  TLS › Certificates) pointing at the copied certificate and private-key files, and reference it from [[ref/object/system-settings#defaultcertificateid|`defaultCertificateId`]] on the [[ref/object/system-settings|SystemSettings]] singleton (found in the WebUI under  Settings ›  Network › Services,  Settings ›  Network › General) so that it is presented when clients do not send SNI. The Certificate fields look like:

```
{  "certificate": {    "@type": "File",    "filePath": "/opt/stalwart/cert/example.com.pem"  },  "privateKey": {    "@type": "File",    "filePath": "/opt/stalwart/cert/example.com.priv.pem"  }}
```