---
title: "Configure CIDR allow list"
source: "https://redis.io/docs/latest/operate/rc/security/cidr-whitelist/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/cidr-whitelist"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Configure CIDR allow list"
---
# Configure CIDR allow list

The CIDR allow list permits traffic between a range of IP addresses and the Redis Cloud VPC.

            Redis Cloud
          

The [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) [allow list](https://en.wikipedia.org/wiki/Whitelist) lets you restrict traffic to your Redis Cloud database. When you configure an allow list, only the [IP addresses](https://en.wikipedia.org/wiki/IP_address) defined in the list can connect to the database. Traffic from all other IP addresses is blocked.

Note:
  To use the CIDR allow list, you must be on either paid Redis Cloud Essentials or on Redis Cloud Pro.  This feature is not supported on free Redis Cloud Essentials plans.

## Define CIDR allow list

To define the CIDR allow list for a database:

Select Databases from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your database from the list.

From the database's Configuration screen, select the Edit database button.

In the Security section, turn on the CIDR allow list toggle.

Enter the first IP address (in CIDR format) you want to allow in the text box and then select the check mark to add it to the allow list:

[](/docs/latest/images/rc/database-details-configuration-tab-security-cidr-allowlist-add-first-ip.png)

To allow additional IP addresses:

Select Add CIDR.

Enter the new IP address in the text box and then select check to add it to the allow list.

[](/docs/latest/images/rc/database-details-configuration-tab-security-cidr-allowlist-add-more-ips.png)

The number of CIDR allow list entries that you can add is based on your Redis Cloud plan. Redis Cloud Essentials plans can have between 4 and 32 entries depending on the plan. Redis Cloud Pro plans can have up to 32 entries.

Select Save database to apply your changes.

Note:
  The database CIDR allow list applies to both the public endpoint and the private endpoint. If you use connectivity options such as [[operate/rc/security/vpc-peering|VPC Peering]] and [[operate/rc/security/aws-transit-gateway|Transit Gateway]] to connect to your database via the private endpoint, you must also add those IPs to your database's CIDR allow list.

## Continue learning with Redis University

See the [Redis Cloud security overview course](https://university.redis.io/course/0b1hkhejlitg3x) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page