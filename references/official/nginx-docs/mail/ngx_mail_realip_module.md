---
title: "Module ngx_mail_realip_module"
source: "https://nginx.org/en/docs/mail/ngx_mail_realip_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mail/ngx_mail_realip_module"
category: "mail"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_mail_realip_module"
---
## Module ngx_mail_realip_module

[Example Configuration](#example)
[Directives](#directives)
     [set_real_ip_from](#set_real_ip_from)

The `ngx_mail_realip_module` module is used
to change the client address and port
to the ones sent in the PROXY protocol header (1.19.8).
The PROXY protocol must be previously enabled by setting the
[[mail/ngx_mail_core_module.html#proxy_protocol|proxy_protocol]] parameter
in the `listen` directive.

#### Example Configuration

 

```nginx
listen 110 proxy_protocol;

set_real_ip_from  192.168.1.0/24;
set_real_ip_from  192.168.2.1;
set_real_ip_from  2001:0db8::/32;
```

#### Directives

            Syntax:
                

`set_real_ip_from
address |
    CIDR |
    unix:;`

            Default:
                

            
            —
        
                

            Context:
                

`mail`, `server`

Defines trusted addresses that are known to send correct
replacement addresses.
If the special value `unix:` is specified,
all UNIX-domain sockets will be trusted.