---
title: "Module ngx_google_perftools_module"
source: "https://nginx.org/en/docs/ngx_google_perftools_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ngx_google_perftools_module"
category: "nginx"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_google_perftools_module"
---
## Module ngx_google_perftools_module

[Example Configuration](#example)
[Directives](#directives)
     [google_perftools_profiles](#google_perftools_profiles)

The `ngx_google_perftools_module` module (0.6.29) enables
profiling of nginx worker processes using
[Google Performance Tools](https://github.com/gperftools/gperftools).
The module is intended for nginx developers.

This module is not built by default, it should be enabled with the
`--with-google_perftools_module`
configuration parameter.

 
This module requires the
[gperftools](https://github.com/gperftools/gperftools) library.

#### Example Configuration

 

```nginx
google_perftools_profiles /path/to/profile;
```

Profiles will be stored as
`/path/to/profile.<worker_pid>`.

#### Directives

            Syntax:
                

`google_perftools_profiles file;`

            Default:
                

            
            —
        
                

            Context:
                

`main`

Sets a file name that keeps profiling information of
nginx worker process.
The ID of the worker process is always a part of the file name
and is appended to the end of the file name, after a dot.