---
title: "Support for QUIC and HTTP/3"
source: "https://nginx.org/en/docs/quic.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "quic"
category: "nginx"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Support for QUIC and HTTP/3"
---
## Support for QUIC and HTTP/3

[Building from sources](#building)
[Configuration tips](#configuration)
[Troubleshooting](#troubleshooting)

Support for
[QUIC](https://datatracker.ietf.org/doc/html/rfc9000)
and
[HTTP/3](https://datatracker.ietf.org/doc/html/rfc9114)
protocols is available since 1.25.0, it is included in
Linux [binary packages](../linux_packages.html).
Please refer to the
[[http/ngx_http_v3_module.html|ngx_http_v3_module]]
documentation.

#### Building from sources

The build is configured using the `configure` command.
Please refer to [[configure.html|Building nginx from Sources]] for details.

The [OpenSSL](https://openssl.org) library
version 3.5.1 or higher is recommended to build nginx with QUIC support.
Otherwise, the [OpenSSL](https://openssl.org)
compatibility layer will be used that does not support
[[http/ngx_http_ssl_module.html#ssl_early_data|early data]].
Alternatively,
[BoringSSL](https://boringssl.googlesource.com/boringssl),
[LibreSSL](https://www.libressl.org), or
[QuicTLS](https://github.com/quictls/openssl)
prebuilt libraries can be used.

Use the following command to configure nginx with
[BoringSSL](https://boringssl.googlesource.com/boringssl):

 

```nginx
./configure
    --with-debug
    --with-http_v3_module
    --with-cc-opt="-I../boringssl/include"
    --with-ld-opt="-L../boringssl/build -lstdc++"
```

Alternatively, nginx can be configured with
[QuicTLS](https://github.com/quictls/openssl):

 

```nginx
./configure
    --with-debug
    --with-http_v3_module
    --with-cc-opt="-I../quictls/build/include"
    --with-ld-opt="-L../quictls/build/lib"
```

Alternatively, nginx can be configured with
[LibreSSL](https://www.libressl.org):

 

```nginx
./configure
    --with-debug
    --with-http_v3_module
    --with-cc-opt="-I../libressl/build/include"
    --with-ld-opt="-L../libressl/build/lib"
```

After configuration,
nginx is compiled and installed using `make`.

#### Configuration tips

The [[http/ngx_http_core_module.html#listen|listen]] directive in
[[http/ngx_http_core_module.html|ngx_http_core_module]]
module got a new parameter
[[http/ngx_http_core_module.html#quic|quic]]
which enables HTTP/3 over QUIC on the specified port.

Along with the `quic` parameter
it is also possible to specify the
[[http/ngx_http_core_module.html#reuseport|reuseport]]
parameter to make it work properly with multiple workers.

To [[http/ngx_http_v3_module.html#quic_retry|enable]]
address validation:

 

```nginx
quic_retry on;
```

To [[http/ngx_http_ssl_module.html#ssl_early_data|enable]]
0-RTT:

 

```nginx
ssl_early_data on;
```

To [[http/ngx_http_v3_module.html#quic_gso|enable]]
GSO (Generic Segmentation Offloading):

 

```nginx
quic_gso on;
```

To [[http/ngx_http_v3_module.html#quic_host_key|set]]
host key for various tokens:

 

```nginx
quic_host_key <filename>;
```

QUIC requires TLSv1.3 protocol version which is enabled by default
in the [[http/ngx_http_ssl_module.html#ssl_protocols|ssl_protocols]] directive.

By default,
[GSO Linux-specific optimization](http://vger.kernel.org/lpc_net2018_talks/willemdebruijn-lpc2018-udpgso-paper-DRAFT-1.pdf)
is disabled.
Enable it in case a corresponding network interface is configured
to support GSO.

#### Troubleshooting

Tips that may help to identify problems:

 

Ensure nginx is built with the proper SSL library.

Ensure nginx is using the proper SSL library in runtime
(the `nginx -V` shows what it is currently used).

Ensure a client is actually sending requests over QUIC.
It is recommended to start with a simple console client such as
[ngtcp2](https://nghttp2.org/ngtcp2)
to ensure the server is configured properly before trying
with real browsers that may be quite picky with certificates.

Build nginx with [[debugging_log.html|debug support]]
and check the debug log.
It should contain all details about the connection and why it failed.
All related messages contain the “`quic`” prefix
and can be easily filtered out.

For a deeper investigation, additional debugging can be enabled
using the following macros:
`NGX_QUIC_DEBUG_PACKETS`,
`NGX_QUIC_DEBUG_FRAMES`,
`NGX_QUIC_DEBUG_ALLOC`,
`NGX_QUIC_DEBUG_CRYPTO`.

 

```nginx
./configure
    --with-http_v3_module
    --with-debug
    --with-cc-opt="-DNGX_QUIC_DEBUG_PACKETS -DNGX_QUIC_DEBUG_CRYPTO"
```