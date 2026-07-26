---
title: "Module ngx_stream_set_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_set_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_set_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_set_module"
---
## Module ngx_stream_set_module

[Example Configuration](#example)
[Directives](#directives)
     [set](#set)

The `ngx_stream_set_module` module (1.19.3) allows
setting a value for a variable.

#### Example Configuration

 

```nginx
server {
    listen 12345;
    set    $true 1;
}
```

#### Directives

            Syntax:
                

`set $variable value;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Sets a `value` for the specified `variable`.
The `value` can contain text, variables, and their combination.