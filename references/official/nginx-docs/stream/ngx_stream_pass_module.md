---
title: "Module ngx_stream_pass_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_pass_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_pass_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_pass_module"
---
## Module ngx_stream_pass_module

[Example Configuration](#example)
[Directives](#directives)
     [pass](#pass)

The `ngx_stream_pass_module` module (1.25.5) allows
passing the accepted connection directly to any configured listening socket
in `http`, `stream`, `mail`,
and other similar modules.

#### Example Configuration

 

```nginx
http {
    server {
        listen 8000;

        location / {
            root html;
        }
    }
}

stream {
    server {
        listen 12345 ssl;

        ssl_certificate     domain.crt;
        ssl_certificate_key domain.key;

        pass 127.0.0.1:8000;
    }
}
```

In the example,
after terminating SSL/TLS in the `stream` module
the connection is passed to the `http` module.

#### Directives

            Syntax:
                

`pass address;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Sets server address to pass client connection to.
The address can be specified as an IP address
and a port:

 

```nginx
pass 127.0.0.1:12345;
```

or as a UNIX-domain socket path:

 

```nginx
pass unix:/tmp/stream.socket;
```

The address can also be specified using variables:

 

```nginx
pass $upstream;
```