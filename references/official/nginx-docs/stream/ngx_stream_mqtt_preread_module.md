---
title: "Module ngx_stream_mqtt_preread_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_mqtt_preread_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_mqtt_preread_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_mqtt_preread_module"
---
## Module ngx_stream_mqtt_preread_module

[Example Configuration](#example)
[Directives](#directives)
     [mqtt_preread](#mqtt_preread)
[Embedded Variables](#variables)

The `ngx_stream_mqtt_preread_module` module (1.23.4) allows
extracting information from the CONNECT message
of the Message Queuing Telemetry Transport protocol (MQTT) versions
[3.1.1](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)
and
[5.0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html),
for example, a username or a client ID.

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
mqtt_preread on;
return       $mqtt_preread_clientid;
```

#### Directives

            Syntax:
                

`mqtt_preread on | off;`

            Default:
                

```nginx
mqtt_preread off;
```

            Context:
                

`stream`, `server`

Enables extracting information from the MQTT CONNECT message at
the [[stream/stream_processing.html#preread_phase|preread]] phase.

#### Embedded Variables

 
`$mqtt_preread_clientid`

the `clientid` value from the CONNECT message

`$mqtt_preread_username`

the `username` value from the CONNECT message