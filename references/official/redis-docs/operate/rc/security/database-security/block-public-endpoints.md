---
title: "Block public endpoints"
source: "https://redis.io/docs/latest/operate/rc/security/database-security/block-public-endpoints/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/database-security/block-public-endpoints"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Block public endpoints"
---
# Block public endpoints

Learn how to block the public endpoints of your databases.

            Redis Cloud
          

By default, you can connect to Redis Cloud databases through the database's public endpoint, or through the database's private endpoint with a private connectivity method.

Public endpoints are accessible from the public internet and don't require a private connectivity method. While this makes Redis Cloud databases convenient to use, it also exposes the databases to potential unauthorized access or brute force attacks, even with a database password in place. Some organizations may want to block public access to their databases to comply with security policies or to better meet stringent compliance standards.

Users with Redis Cloud Pro databases can choose to block public endpoints for all databases in their subscription.

Note:
  After you block your database's public endpoint, any connection from an IP address that is not part of the private address space defined in [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918#section-3) will be rejected. Ensure that all connections to your database are made through a private connectivity method before blocking the public endpoint.

## Block public endpoints

You can block public endpoints for a [new subscription](#new-subscription) or an [existing subscription](#existing-subscription).

### New subscription

To block the public endpoints when you [create a new Pro subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/):

Follow the instructions to [create a Pro database with custom settings](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/#custom-settings).
On the Setup tab, go to Advanced options > Security to select persistent storage encryption options.
Select Block public endpoint to block the public endpoint for all databases on the subscription.
Select Continue to go to the [Sizing tab](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/#sizing-tab). Follow the instructions to provision your database(s).

After you block the public endpoints for a new subscription, you will need to set up a [private connectivity method](#private-connectivity-methods) to connect to your databases.

### Existing subscription

For existing subscriptions, we recommend setting up a [private connectivity method](#private-connectivity-methods) to connect to your databases before blocking the private endpoint and migrating all connections to the private endpoint.

To block the public endpoints of an existing Pro subscription:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.
Open the Security tab to view security settings.
In the Endpoint section, select Edit.
Select Block public endpoint.
Select Save to save your changes.
A window will appear asking you to confirm that blocking the public endpoint will reject clients connecting to the public endpoint. Select I understand and then Block to confirm.

After your changes are saved, any incoming connections to the public endpoint of your database will be rejected, and only connections through a private connectivity method will be allowed.

## Turn on passwordless authentication for the default user

If you have blocked the public endpoint for your Redis Cloud Pro subscription, you can turn on passwordless authentication for the [[operate/rc/security/access-control/data-access-control/default-user|default user]] for any database in that subscription.

To turn on passwordless authentication:

From the database Configuration tab, select Edit.
In the Security section, select Use passwordless auth to turn on passwordless authentication.

    
    	
    
    [](/docs/latest/images/rc/database-configuration-security-passwordless.png)

Select Save database to save your changes.

## Private connectivity methods

Redis Cloud supports the following private connectivity options:

[[operate/rc/security/vpc-peering|VPC peering]]
[[operate/rc/security/private-service-connect|Google Cloud Private Service Connect]] (Google Cloud only)
[[operate/rc/security/aws-transit-gateway|AWS Transit Gateway]] or [[operate/rc/security/aws-privatelink|AWS PrivateLink]] (AWS only)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page