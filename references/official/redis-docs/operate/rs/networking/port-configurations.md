---
title: "Network port configurations"
source: "https://redis.io/docs/latest/operate/rs/networking/port-configurations/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/networking/port-configurations"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Network port configurations"
---
# Network port configurations

This document describes the various network port ranges and their uses.

            Redis Software
          

All Redis Software deployments span multiple physical/virtual nodes. You'll need to keep several ports open between these nodes. This document describes the various port ranges and their uses.

Note:
  Whenever you create a new database, you must verify that the ports assigned to the new database's endpoints are open. The cluster will not perform this verification for you.

## Ports and port ranges used by Redis Software

Redis Software's port usage falls into three general categories:

Internal: For traffic between or within cluster nodes
External: For traffic from client applications or external monitoring resources
Active-Active: For traffic to and from clusters hosting Active-Active databases

Protocol
Port
Configurable
Connection source
Description

TCP
8001
â No
Internal, External
Traffic from application to Redis SoftwareÂ [Discovery Service](/docs/latest/operate/rs/databases/durability-ha/discovery-service/)

TCP
8070
â No
External
Metrics exported and managed by the web proxy

TCP
3347-3349, 8000, 8071, 9091, 9125
â No
Internal
Internal metrics ports

TCP
8443
â Yes
Internal, External
Secure (HTTPS) access to the management web UI

TCP
9081
â Yes
Internal
CRDB coordinator for Active-Active management (internal)

TCP
9082
â No
Internal
Cluster API internal port

TCP
9443, 8080, 3346
â Yes
Internal, External, Active-Active
REST API traffic, including cluster management and node bootstrap

TCP
10050
â No
Internal
Zabbix monitoring

TCP
10000-10049, 10051-19999
â Yes
Internal, External, Active-Active
Database traffic

UDP
53, 5353
â No
Internal, External
DNS/mDNS traffic

TCP
1968
â No
Internal
Proxy traffic

TCP
3333-3345, 3350-3354, 36379
â No
Internal
Internode communication

TCP
3355
â Yes
Internal
Authentication service internal port

TCP
3357
â No
Internal
Internal communication

TCP
20000-29999
â No
Internal
Database shard traffic

TCP
8002, 8004, 8006
â Yes
Internal
Default system health monitoring (envoy admin, envoy management server, gossip envoy admin)

TCP
8444, 9080
â No
Internal
Traffic between web proxy and cnm_http/cm

## Change port configuration

### Reserve ports

Redis Software reserves some ports by default (`system_reserved_ports`). To reserve other ports or port ranges and prevent the cluster from assigning them to database endpoints, configure `reserved_ports` using one of the following methods:

[rladmin cluster config](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/config/)

```sh
rladmin cluster config reserved_ports <list of ports/port ranges>
```

For example:

```sh
rladmin cluster config reserved_ports 11000 13000-13010
```

[Update cluster settings](/docs/latest/operate/rs/references/rest-api/requests/cluster/#put-cluster) REST API request

```sh
PUT /v1/cluster
{ "reserved_ports": ["list of ports/port ranges"] }
```

For example:

```sh
PUT /v1/cluster
{ "reserved_ports": ["11000", "13000-13010"] }
```

### Change the Cluster Manager UI port

The Redis Software Cluster Manager UI uses port 8443, by default. You can change this to a custom port as long as the new port is not in use by another process.

To change this port, run:

```sh
rladmin cluster config cm_portÂ <custom-port>
```

After you change the Cluster Manager UI port, use the new port in the URL when accessing the UI on any node in the cluster. Example: `https://newnode.mycluster.example.com:<custom-port>`

### Change the envoy  ports

For system health monitoring, Redis uses the following ports by default:

Port 8002 for envoy admin

Port 8004 for envoy management server

Port 8006 for gossip envoy admin

You can change each envoy port to a custom port using the [`rladmin cluster config`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/config/) command as long as the new port is not in use by another process. When you change `envoy_admin_port`, expect a restart of envoy.

To change the envoy admin port, run:

```sh
$ rladmin cluster config envoy_admin_portÂ <new-port>
Updating envoy_admin_port... restarting now
```

To change the envoy management server port, run:

```sh
$ rladmin cluster config envoy_mgmt_server_portÂ <new-port>
Cluster configured successfully
```

To change the gossip envoy admin port, run:

```sh
$ rladmin cluster config gossip_envoy_admin_portÂ <new-port>
Cluster configured successfully
```

### Change the REST API port

For the REST API, Redis Software uses port 9443 (secure) and port 8080 (not secure), by default. You can change this to a custom port as long as the new port is not in use by another process.

To change these ports, run:

```sh
rladmin cluster config cnm_http_portÂ <new-port>
```

```sh
rladmin cluster config cnm_https_portÂ <new-port>
```

### OS conflicts with port 53

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

### Update sysctl.conf to avoid port collisions

To avoid port collision, update `/etc/sysctl.conf` to include:

```sh
net.ipv4.ip_local_port_range = 30000 65535
```

## Configure HTTPS

### Require HTTPS for API endpoints

By default, the Redis Software API supports communication over HTTP and HTTPS. However, you can turn off HTTP support to ensure that API requests are encrypted.

Before you turn off HTTP support, make sure you migrate any scripts or proxy configurations that use HTTP to the encrypted API endpoint to prevent broken connections.

To turn off HTTP support for API endpoints, run:

```sh
rladmin cluster config http_support disabled
```

After you turn off HTTP support, traffic sent to the unencrypted API endpoint is blocked.

### HTTP to HTTPS redirection

Starting with version 6.0.12, you cannot use automatic HTTP to HTTPS redirection.
To poll metrics from the `metrics_exporter` or to access the Cluster Manager UI, use HTTPS in your request. HTTP requests won't be automatically redirected to HTTPS for those services.

## Nodes on different VLANs

Nodes in the same cluster must reside on the same VLAN. If you can't
host the nodes on the same VLAN, then you must open [[operate/rs/networking/port-configurations|all ports]] between them.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page