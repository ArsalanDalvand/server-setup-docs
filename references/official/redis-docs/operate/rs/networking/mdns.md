---
title: "Client prerequisites for mDNS"
source: "https://redis.io/docs/latest/operate/rs/networking/mdns/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/networking/mdns"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Client prerequisites for mDNS"
---
# Client prerequisites for mDNS

Requirements for using the mDNS protocol in development and testing environments.

            Redis Software
          

Note:
  mDNS is only supported for development and testing environments.

If you choose to use the mDNS protocol when [[operate/rs/networking/cluster-dns|you set the cluster name]],
make sure that the configurations and prerequisites for resolving database endpoints are met on the client machines.
If you have [Replica Of](/docs/latest/operate/rs/databases/import-export/replica-of/) databases on the cluster,
the configurations and prerequisites are also required for the Redis Software nodes.

To prepare a client or node for mDNS:

Make sure that the clients and cluster nodes are on the same physical network
or have the network infrastructure configured to allow multicasting between them.

Install these prerequisite packages:

For Ubuntu:

```sh
apt-get install libnss-mdns
```

For RHEL/CentOS 6.x:

```sh
$ rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
$ yum install nss-mdns
$ service avahi-daemon start
```

For RHEL/CentOS 7:

```sh
$ rpm -ivh https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-12.noarch.rpm
$ yum install nss-mdns
$ service avahi-daemon start
```

If you are using [[operate/rs/networking/multi-ip-ipv6|mDNS with IPv6 addresses]],
update the hosts line in `/etc/nsswitch.conf` to:

```yaml
hosts: files mdns4_minimal
\[NOTFOUND=return\] mdns
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)