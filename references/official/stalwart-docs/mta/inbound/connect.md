---
title: "Connect stage"
source: "https://stalw.art/docs/mta/inbound/connect"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/connect"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Connect stage"
---
# Connect stage

      

The connect stage is the initial stage of an SMTP session, where the server and client establish a connection. It determines the hostname and greeting the server advertises, and can run a Sieve script against the incoming connection.

Connect-stage behaviour is configured on the [[ref/object/mta-stage-connect|MtaStageConnect]] singleton (found in the WebUI under  Settings ›  MTA › Session › Connect Stage).

## Hostname

[Section titled “Hostname”](#hostname)

The [[ref/object/mta-stage-connect#hostname|`hostname`]] field is an expression that returns the hostname the server uses to identify itself during the SMTP session. By default it resolves to the system hostname via `system('hostname')`.

## Greeting

[Section titled “Greeting”](#greeting)

The [[ref/object/mta-stage-connect#smtpgreeting|`smtpGreeting`]] field is an expression that returns the greeting message sent to the client when the SMTP session begins. The default greeting combines the system hostname with `Stalwart ESMTP at your service`.

## Sieve script

[Section titled “Sieve script”](#sieve-script)

The [[ref/object/mta-stage-connect#script|`script`]] field selects a [[sieve|Sieve script]] to run before the SMTP session is allowed to proceed. This is typically used to filter connections by remote IP address: an expression selects the script name at runtime, and the script itself may reject the connection based on connection-level variables such as `remote_ip`.

For example, setting [[ref/object/mta-stage-connect#script|`script`]] to the expression `"'connect_filter'"` runs a Sieve script named `connect_filter` which can inspect `env.remote_ip` and reject unwanted sources:

```
require ["variables", "reject"];
if string "${env.remote_ip}" "192.0.2.88" {    reject "Connection from this IP is not accepted.";}
```