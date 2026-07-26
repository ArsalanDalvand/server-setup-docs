---
title: "Configure CentOS/RHEL firewall"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/configuring/centos-rhel-firewall/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/configuring/centos-rhel-firewall"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Configure CentOS/RHEL firewall"
---
# Configure CentOS/RHEL firewall

Configure firewall rules for Redis Software on CentOS or Red Hat Enterprise Linux (RHEL).

            Redis Software
          

CentOS and Red Hat Enterprise Linux (RHEL) distributions use [firewalld](https://firewalld.org/) by default to manage the firewall and configure [iptables](https://en.wikipedia.org/wiki/Iptables).
The default configuration assigns the network interfaces to the public zone and blocks all ports except port 22, which is used for [SSH](https://en.wikipedia.org/wiki/Secure_Shell).

When you install Redis Software on CentOS or RHEL, it automatically creates two firewalld system services:

A service named redislabs, which includes all ports and protocols needed for communication between cluster nodes.
A service named redislabs-clients, which includes the ports and protocols needed for external communication (outside of the cluster).

These services are defined but not allowed through the firewall by default.
During Redis Software installation, the [[operate/rs/installing-upgrading/install/manage-installation-questions|installer prompts]] you to confirm auto-configuration of a default (public) zone
to allow the redislabs service.

Although automatic firewall configuration simplifies installation, your deployment might not be secure if you did not use other methods to secure the host machine's network, such as external firewall rules or security groups.
You can use firewalld configuration tools such as firewall-cmd (command line) or firewall-config (UI)
to create more specific firewall policies that allow these two services through the firewall, as necessary.

Note:
  If databases are created with non-standard [[operate/rs/networking/port-configurations|Redis Software ports]],
you need to explicitly configure firewalld to make sure those ports are not blocked.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)