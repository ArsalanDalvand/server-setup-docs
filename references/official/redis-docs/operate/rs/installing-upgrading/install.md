---
title: "Install Redis Software"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Software"
---
# Install Redis Software

Install Redis Software on Linux.

            Redis Software
          

After you [[operate/rs/installing-upgrading/install/plan-deployment|plan your deployment]], [[operate/rs/installing-upgrading/install/prepare-install/download-install-package|download a Redis Software installation package]], and finish [[operate/rs/installing-upgrading/install/prepare-install|installation preparation]]:

[[operate/rs/installing-upgrading/install/install-on-linux|Install the Redis Software package]] on one of the nodes in the cluster.

Repeat this process for each node in the cluster.

For installation on machines without an internet connection, see [[operate/rs/installing-upgrading/install/offline-installation|Offline installation]].

## Permissions and access

Redis Software installation creates the `redislabs:redislabs` user and group.

Assigning other users to the `redislabs` group is optional. Users belonging to the `redislabs` group have permission to read and execute (e.g. use the `rladmin` status command) but are not allowed to write (or delete) files or directories.

Redis Software is certified to run with permissions set to `750`, an industry standard.

Warning:
  Do not reduce permissions to `700`. This configuration has not been tested and is not supported.

Redis Software requires `umask` to be `0022` during installation to correctly set permissions for the installed directories and files.

## More info and options

If you've already installed Redis Software, you can also:

[[operate/rs/installing-upgrading/upgrading|Upgrade an existing deployment]].

[[operate/rs/installing-upgrading/uninstalling|Uninstall an existing deployment]].

To learn more about customization and find answers to related questions, see:

[[operate/rs/installing-upgrading/configuring/centos-rhel-firewall|CentOS/RHEL Firewall configuration]]
[Change socket file location](/docs/latest/operate/rs/installing-upgrading/configuring/change-location-socket-files/)
[[operate/rs/networking/cluster-dns|Cluster DNS configuration]]
[[operate/rs/networking/cluster-lba-setup|Cluster load balancer setup]]
[[operate/rs/installing-upgrading/install/plan-deployment/file-locations|File locations]]
[[operate/rs/installing-upgrading/install/plan-deployment/supported-platforms|Supported platforms]]
[[operate/rs/installing-upgrading/install/manage-installation-questions|Manage installation questions]]
[[operate/rs/networking/mdns|mDNS client prerequisites]]
[[operate/rs/installing-upgrading/install/customize-user-and-group|User and group ownership]]

## Next steps

After your cluster is set up with nodes, you can:

[[operate/rs/security/access-control/create-users|Add users]] to the cluster with specific permissions.  To begin, start with [[operate/rs/security/access-control|Access control]].
[Create databases](/docs/latest/operate/rs/databases/create/) to use with your applications.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page