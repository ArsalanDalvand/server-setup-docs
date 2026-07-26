---
title: "Network security"
source: "https://redis.io/docs/latest/operate/rc/security/database-security/network-security/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/database-security/network-security"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Network security"
---
# Network security

            Redis Cloud
          

Redis Cloud supports two types of network security: database-level CIDR allow lists and [VPC](#virtual-private-clouds)-wide CIDR allow lists.

These features are available in most Redis Cloud configurations, as shown here:

CloudÂ provider
VPC peering
IP restrictions

AWS
Redis Cloud Pro
Paid Redis Cloud Essentials and Redis Cloud Pro

Google Cloud
Redis Cloud Pro
Paid Redis Cloud Essentials and Redis Cloud Pro

Azure
Redis Cloud Pro
Redis Cloud Pro

## IP and subnet restrictions

You can restrict database access to a configurable
set of source IP addresses and subnets. This is roughly equivalent
to using [iptables](https://en.wikipedia.org/wiki/Iptables) to limit access to a host.

To restrict a database to a specific set of source IP addresses or subnets, see [[operate/rc/security/cidr-whitelist|Configure CIDR allow list]].

## Virtual private clouds

A [Virtual Private Cloud](https://en.wikipedia.org/wiki/Virtual_private_cloud) (VPC) is an isolated set of resources within a [public cloud](https://en.wikipedia.org/wiki/Cloud_computing#Public_cloud), usually having its own subnets and VLAN.

Redis Cloud Pro databases are almost always deployed in a Redis VPC. In most cases, you'll need to create a [[operate/rc/security/vpc-peering|VPC peering connection]] to access these databases. A VPC peering connection allows unrestricted network access between two VPCs.

Connection creation and supported features vary by public cloud provider. Learn more about VPC usage for [AWS](#vpcs-with-aws), [Google Cloud](#vpcs-with-google-cloud), and [Azure](#vpcs-with-azure).

### VPCs with AWS

Subscriptions that run on AWS support two VPC options. To ensure that that you can securely connect to your database, you need to [[operate/rc/security/vpc-peering#aws-vpc-peering|create a VPC peering connection]].

If you create a VPC peering connection, you can also [[operate/rc/security/cidr-whitelist|configure a CIDR allow list]] to allow connections only from specific IP address blocks or security groups.

### VPCs with Google Cloud

Subscriptions that run on Google Cloud require a VPC peering connection. See [[operate/rc/security/vpc-peering#gcp-vpc-peering|Google Cloud VPC peering]] to learn how to set up VPC peering for Google Cloud.

### VPCs with Azure

When you request a Redis Cloud Annual subscription, all databases will be deployed in your own Azure VPC.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page