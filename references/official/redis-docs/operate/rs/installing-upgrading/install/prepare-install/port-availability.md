---
title: "Ensure port availability"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/prepare-install/port-availability/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/prepare-install/port-availability"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Ensure port availability"
---
# Ensure port availability

Make sure required ports are available.

            Redis Software
          

Before [[operate/rs/installing-upgrading/install|installing Redis Software]], make sure all required ports are available.

If ports that Redis assigns to the database are being used by the operating system or other processes, the installation fails.

Follow the relevant sections to configure required ports.

For recommended and optional port configuration, see [[operate/rs/networking/port-configurations|Network port configurations]].

## Update sysctl.conf to avoid port collisions

To avoid port collision, update `/etc/sysctl.conf` to include:

```sh
net.ipv4.ip_local_port_range = 30000 65535
```

## OS conflicts with port 53

If port 53 is in use, the installation fails. This issue can occur in default installations of certain operating systems in which `systemd-resolved` (DNS server) or `dnsmasq` is running.

To prevent this issue, change the system configuration to make this port available before installation.

To prevent `systemd-resolved` from using port 53:

Edit `/etc/systemd/resolved.conf`:

```sh
sudo vi /etc/systemd/resolved.conf
```

Add `DNSStubListener=no` as the last line in the file and save the file.

Rename the current `/etc/resolv.conf` file:

```sh
sudo mv /etc/resolv.conf /etc/resolv.conf.orig
```

Create a symbolic link for `/etc/resolv.conf`:

```sh
sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

Note:
  You might encounter a temporary name resolution error (`sudo: unable to resolve host {hostname}: Temporary failure in name resolution`), which should be fixed when you restart `systemd-resolved` in the next step.

Restart the DNS service:

```sh
sudo service systemd-resolved restart
```

To prevent `dnsmasq` from using port 53:

Stop the `dnsmasq` service if it's running:

```sh
sudo systemctl stop dnsmasq
```

Prevent `dnsmasq` from starting automatically at system boot:

```sh
sudo systemctl disable dnsmasq
```

Mask `dnsmasq` to prevent it from being started manually or by other services:

```sh
sudo systemctl mask dnsmasq
```

Verify `dnsmasq` is no longer active and won't start at system boot:

```sh
sudo systemctl status dnsmasq
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page