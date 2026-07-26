---
title: "Installing nginx"
source: "https://nginx.org/en/docs/install.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "install"
category: "nginx"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Installing nginx"
---
## Installing nginx

nginx can be installed differently, depending on the operating system.

#### Installation on Linux

For Linux, nginx [packages](../linux_packages.html)
from nginx.org can be used.

#### Installation on FreeBSD

On FreeBSD, nginx can be installed either from the [packages](https://docs.freebsd.org/en/books/handbook/ports/#pkgng-intro)
or through the
[ports](https://docs.freebsd.org/en/books/handbook/ports/#ports-using)
system.
The ports system provides greater flexibility, allowing selection among
a wide range of options.
The port will compile nginx with the specified options and install it.

#### Building from Sources

If some special functionality is required, not available with packages and
ports, nginx can also be compiled from source files.
While more flexible, this approach may be complex for a beginner.
For more information, see [[configure.html|Building nginx from Sources]].