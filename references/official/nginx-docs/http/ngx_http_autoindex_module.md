---
title: "Module ngx_http_autoindex_module"
source: "https://nginx.org/en/docs/http/ngx_http_autoindex_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_autoindex_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_autoindex_module"
---
## Module ngx_http_autoindex_module

[Example Configuration](#example)
[Directives](#directives)
     [autoindex](#autoindex)
     [autoindex_exact_size](#autoindex_exact_size)
     [autoindex_format](#autoindex_format)
     [autoindex_localtime](#autoindex_localtime)

The `ngx_http_autoindex_module` module processes requests
ending with the slash character (‘`/`’) and produces
a directory listing.
Usually a request is passed to the `ngx_http_autoindex_module`
module when the
[[http/ngx_http_index_module.html|ngx_http_index_module]] module
cannot find an index file.

#### Example Configuration

 

```nginx
location / {
    autoindex on;
}
```

#### Directives

            Syntax:
                

`autoindex on | off;`

            Default:
                

```nginx
autoindex off;
```

            Context:
                

`http`, `server`, `location`

Enables or disables the directory listing output.

            Syntax:
                

`autoindex_exact_size on | off;`

            Default:
                

```nginx
autoindex_exact_size on;
```

            Context:
                

`http`, `server`, `location`

For the HTML [format](#autoindex_format),
specifies whether exact file sizes should be output in the directory listing,
or rather rounded to kilobytes, megabytes, and gigabytes.

            Syntax:
                

`autoindex_format
html |
    xml |
    json |
    jsonp;`

            Default:
                

```nginx
autoindex_format html;
```

            Context:
                

`http`, `server`, `location`

This directive appeared in version 1.7.9.

Sets the format of a directory listing.

When the JSONP format is used, the name of a callback function is set
with the `callback` request argument.
If the argument is missing or has an empty value,
then the JSON format is used.

The XML output can be transformed using the
[[http/ngx_http_xslt_module.html|ngx_http_xslt_module]] module.

            Syntax:
                

`autoindex_localtime on | off;`

            Default:
                

```nginx
autoindex_localtime off;
```

            Context:
                

`http`, `server`, `location`

For the HTML [format](#autoindex_format),
specifies whether times in the directory listing should be
output in the local time zone or UTC.