---
title: "Install, set up, and upgrade Redis Software"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install, set up, and upgrade Redis Software"
---
# Install, set up, and upgrade Redis Software

Learn how to install, set up, and upgrade Redis Software.

            Redis Software
          

You can run self-managed Redis Software in an on-premises data center or on your preferred cloud platform.

If you prefer a fully managed Redis database-as-a-service, available on major public cloud services, consider setting up a [[operate/rc|Redis Cloud]] subscription. You can [try Redis Cloud](https://redis.io/try-free/) for free.

## Quickstarts

If you want to try out Redis Software, see the following quickstarts:

[Redis Software quickstart](/docs/latest/operate/rs/installing-upgrading/quickstarts/redis-enterprise-software-quickstart/)

[[operate/rs/installing-upgrading/quickstarts/docker-quickstart|Docker quickstart for Redis Software]]

## Install Redis Software

To install Redis Software on a [[operate/rs/installing-upgrading/install/plan-deployment/supported-platforms|supported platform]], you need to:

[[operate/rs/installing-upgrading/install/plan-deployment|Plan your deployment]].

[[operate/rs/installing-upgrading/install/prepare-install|Prepare to install]].

[[operate/rs/installing-upgrading/install|Perform the install]].

Depending on your needs, you may also want to [customize the installation](#more-info-and-options).

## Upgrade existing deployment

If you already installed Redis Software, you can:

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster|Upgrade a cluster]]

[[operate/rs/installing-upgrading/upgrading/upgrade-database|Upgrade a database]]

[[operate/rs/installing-upgrading/upgrading/upgrade-active-active|Upgrade an Active-Active database]]

## Uninstall Redis Software

[[operate/rs/installing-upgrading/uninstalling|Uninstall existing deployment]]

## More info and options

More information is available to help with customization and related questions:

[[operate/rs/installing-upgrading/configuring/centos-rhel-firewall|CentOS/RHEL firewall configuration]]
[Change socket file location](/docs/latest/operate/rs/installing-upgrading/configuring/change-location-socket-files/)
[[operate/rs/networking/cluster-dns|Cluster DNS configuration]]
[[operate/rs/networking/cluster-lba-setup|Cluster load balancer setup]]
[[operate/rs/installing-upgrading/install/plan-deployment/file-locations|File locations]]
[[operate/rs/installing-upgrading/configuring/linux-swap|Linux swap space configuration]]
[[operate/rs/networking/mdns|mDNS client prerequisites]]
[[operate/rs/installing-upgrading/install/customize-user-and-group|User and group ownership]]

## Next steps

After you install Redis Software and set up your cluster, you can:

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