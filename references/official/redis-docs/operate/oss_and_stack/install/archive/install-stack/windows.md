---
title: "Install Redis Stack on Windows"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-stack/windows/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/archive/install-stack/windows"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Stack on Windows"
---
# Install Redis Stack on Windows

How to install Redis Stack on Windows

            Redis Open Source
          

These docs apply to Redis Community Edition and Redis Stack version 7.x.

## Run Redis on Windows using Memurai

Redis is now natively supported on Windows through [Memurai](https://www.memurai.com/), the official Redis partner for Windows compatibility.

## Run Redis on Windows using Docker

To install Redis Stack on Windows, you will need to have Docker installed. When Docker is up and running, open Windows PowerShell and follow the instructions described in [[operate/oss_and_stack/install/install-stack/docker|Run Redis Stack on Docker]]. Then, use Docker to connect with `redis-cli` as explained in that topic.

About using WSL and Ubuntu for Windows :

If you attempt to use Windows Subsystem for Linux (WSL) or Ubuntu for Windows to follow [Linux instructions](/docs/latest/operate/oss_and_stack/install/install-stack/apt/), you will get a `systemd` error telling you `System has not been booted with systemd as init system (PID 1). Can't operate.` Do not fret. Just use Docker.

`systemd` is a suite of basic building blocks for a Linux system. For more information about its function, see [System and Service Manager](https://systemd.io/). This becomes an issue due to the lack of support for Linux workflows on WSL. But, you can test the instructions listed in [Systemd support is now available in WSL!](https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/). Let us know how that worked for you.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page