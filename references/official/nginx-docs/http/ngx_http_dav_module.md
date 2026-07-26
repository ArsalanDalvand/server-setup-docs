---
title: "Module ngx_http_dav_module"
source: "https://nginx.org/en/docs/http/ngx_http_dav_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_dav_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_dav_module"
---
## Module ngx_http_dav_module

[Example Configuration](#example)
[Directives](#directives)
     [create_full_put_path](#create_full_put_path)
     [dav_access](#dav_access)
     [dav_methods](#dav_methods)
     [min_delete_depth](#min_delete_depth)

The `ngx_http_dav_module` module is intended for file
management automation via the WebDAV protocol.
The module processes HTTP and WebDAV
methods PUT, DELETE, MKCOL, COPY, and MOVE.

This module is not built by default, it should be enabled with the
`--with-http_dav_module`
configuration parameter.

 
WebDAV clients that require additional WebDAV methods to operate
will not work with this module.

#### Example Configuration

 

```nginx
location / {
    root                  /data/www;

    client_body_temp_path /data/client_temp;

    dav_methods PUT DELETE MKCOL COPY MOVE;

    create_full_put_path  on;
    dav_access            group:rw  all:r;

    limit_except GET {
        allow 192.168.1.0/32;
        deny  all;
    }
}
```

#### Directives

            Syntax:
                

`create_full_put_path on | off;`

            Default:
                

```nginx
create_full_put_path off;
```

            Context:
                

`http`, `server`, `location`

The WebDAV specification only allows creating files in already
existing directories.
This directive allows creating all needed intermediate directories.

            Syntax:
                

`dav_access users:permissions ...;`

            Default:
                

```nginx
dav_access user:rw;
```

            Context:
                

`http`, `server`, `location`

Sets access permissions for newly created files and directories, e.g.:

 

```nginx
dav_access user:rw group:rw all:r;
```

If any `group` or `all` access permissions
are specified then `user` permissions may be omitted:

 

```nginx
dav_access group:rw all:r;
```

            Syntax:
                

`dav_methods
off | method ...;`

            Default:
                

```nginx
dav_methods off;
```

            Context:
                

`http`, `server`, `location`

Allows the specified HTTP and WebDAV methods.
The parameter `off` denies all methods processed
by this module.
The following methods are supported:
`PUT`, `DELETE`, `MKCOL`,
`COPY`, and `MOVE`.

A file uploaded with the PUT method is first written to a temporary file,
and then the file is renamed.
Starting from version 0.8.9, temporary files and the persistent store
can be put on different file systems.
However, be aware that in this case a file is copied
across two file systems instead of the cheap renaming operation.
It is thus recommended that for any given location both saved files and a
directory holding temporary files, set by the
[[http/ngx_http_core_module.html#client_body_temp_path|client_body_temp_path]]
directive, are put on the same file system.

When creating a file with the PUT method, it is possible to specify
the modification date by passing it in the “Date”
header field.

            Syntax:
                

`min_delete_depth number;`

            Default:
                

```nginx
min_delete_depth 0;
```

            Context:
                

`http`, `server`, `location`

Allows the DELETE method to remove files provided that
the number of elements in a request path is not less than the specified
number.
For example, the directive

 

```nginx
min_delete_depth 4;
```

allows removing files on requests

 

```nginx
/users/00/00/name
/users/00/00/name/pic.jpg
/users/00/00/page.html
```

and denies the removal of

 

```nginx
/users/00/00
```