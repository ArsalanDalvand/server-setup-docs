---
title: "Command-line parameters"
source: "https://nginx.org/en/docs/switches.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "switches"
category: "nginx"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Command-line parameters"
---
## Command-line parameters

nginx supports the following command-line parameters:

 

`-?` | `-h` — print help
for command-line parameters.

`-c file` — use an alternative
configuration `file` instead of a default file.

`-e file` — use an alternative
error log `file` to store the log
instead of a default file (1.19.5).
The special value `stderr` selects the standard error file.

`-g directives` — set
[[ngx_core_module.html|global configuration directives]],
for example,

```nginx
nginx -g "pid /var/run/nginx.pid; worker_processes `sysctl -n hw.ncpu`;"
```

`-l port` — enable
[nginx control REST API](https://docs.nginx.com/nginx/admin-guide/basic-functionality/runtime-control#control-api)
on a specified `port` or UNIX-domain socket (1.29.8).

 
This parameter is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

`-p prefix` — set nginx path prefix,
i.e. a directory that will keep server files
(default value is `/usr/local/nginx`).

`-q` — suppress non-error messages
during configuration testing.

`-s signal` — send a signal
to the master process.
The argument signal can be one of:

`stop` — shut down quickly

`quit` — shut down gracefully

`reload` — reload configuration,
start the new worker process with a new configuration,
gracefully shut down old worker processes.

`reopen` — reopen log files

`-t` — test the configuration file: nginx checks the
configuration for correct syntax, and then tries to open files
referred in the configuration.

`-T` — same as `-t`,
but additionally dump configuration files to standard output (1.9.2).

`-v` — print nginx version.

`-V` — print nginx version, compiler version,
and configure parameters.