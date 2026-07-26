---
title: "Module ngx_stream_return_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_return_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_return_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_return_module"
---
## Module ngx_stream_return_module

[Example Configuration](#example)
[Directives](#directives)
     [return](#return)

The `ngx_stream_return_module` module (1.11.2) allows
sending a specified value to the client and then closing the connection.

#### Example Configuration

 

```nginx
server {
    listen 12345;
    return $time_iso8601;
}
```

#### Directives

            Syntax:
                

`return value;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Specifies a `value` to send to the client.
The value can contain text, variables, and their combination.