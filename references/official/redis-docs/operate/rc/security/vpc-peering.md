---
title: "Enable VPC peering"
source: "https://redis.io/docs/latest/operate/rc/security/vpc-peering/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/vpc-peering"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable VPC peering"
---
# Enable VPC peering

VPC peering uses private IP addresses to route traffic between a Redis Cloud VPC and an application VPC.

            Redis Cloud
          

VPC peering uses private IP addresses to allow network connections between two [virtual private clouds](https://en.wikipedia.org/wiki/Virtual_private_cloud) (VPCs).

You can connect your VPC in the Redis Cloud subscription to the VPC of your application. This lets your application connect securely to your Redis Cloud database using VPC peering to optimize the performance of your application.

Note:
  VPC peering is available only with Redis Cloud Pro.  It is not supported for Redis Cloud Essentials.

VPC peering configuration requires you to initiate VPC peering on your Redis Cloud subscription and then accept the VPC peering request for the AWS VPC that you want to peer with.

## AWS VPC peering

If you want to peer a Redis Cloud VPC with an AWS VPC, you need to:

[Configure and initiate VPC peering](#config-aws-vpc-peering) for your Redis Cloud subscription.

[Approve the VPC peering request](#approve-aws-vpc-peering).

[Update the routes tables](#update-route-tables).

### Configure VPC peering

To set up VPC peering:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.

Select Connectivity > VPC Peering.

Select Add peering.

Enter VPC peering details:

Setting
Description

Consumer AWS Account
Your AWS account ID (see [Finding your AWS account ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html#FindingYourAWSId))

Consumer region
AWS VPC region

Consumer VPC ID
The VPC ID for the application that needs to access your Redis Cloud VPC (see [Finding a VPC ID](https://docs.aws.amazon.com/managedservices/latest/userguide/find-vpc.html))

Consumer VPC CIDRs
[CIDR-formatted](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) IP addresses for the AWS VPC that needs to access your Redis Cloud VPC; must not overlap with the Redis producer VPC CIDR (see [View your VPCs](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#view-vpc))

You can provide up to five VPC CIDRs.

To add multiple VPC CIDRs:

Select Add CIDR.

Enter additional CIDR-formatted IP address in the box:

[](/docs/latest/images/rc/subscription-connectivity-vpc-peering-add-multiple-cidrs.png)

Select Initiate peering.

Note the Peering ID of the VPC peering request.

[](/docs/latest/images/rc/subscription-connectivity-vpc-peering-aws.png)

### Approve VPC peering request

After you set up and intitiate VPC peering, you need to approve the VPC peering request:

Follow the AWS guide to [accept the VPC peering connection](https://docs.aws.amazon.com/vpc/latest/peering/accept-vpc-peering-connection.html).

After you accept the peering request, select Modify my route tables now. This button can be found in the green header once the peering connection is active.

[](/docs/latest/images/rc/modify_route_tables_aws.png)

### Update route tables

To finish VPC peering setup, [update your route tables for the peering connection](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-routing.html). There may be many different route tables assigned to different VPCs. It is important to choose the route table assigned to the VPC you specified in the [Configure VPC Peering](#config-aws-vpc-peering) section with the following details:

In the Destination field, enter the Redis Cloud VPC CIDR shown when you accepted the peering request.

This is the Redis Cloud VPC CIDR address, to which your application's VPC connects.

In the Target field, select Peering Connection and select the relevant Peering ID.

Once VPC peering is established, we recommend switching your application connection string to the private endpoint.

Note:
  If you've enabled the database's [[operate/rc/security/cidr-whitelist|CIDR allow list]], you must also [[operate/rc/security/cidr-whitelist#define-cidr-allow-list|add the VPC peered IP addresses to the CIDR allow list]] to connect to the database via the private endpoint.

## Google Cloud VPC peering

If you want to peer a Redis Cloud VPC with a Google Cloud VPC, you need to:

[Configure and initiate VPC peering](#config-gcp-vpc-peering) for your Redis Cloud subscription.

[Approve the VPC peering request](#approve-gcp-vpc-peering).

### Configure VPC peering

To set up VPC peering:

Select Subscriptions from the [Redis Cloud console](https://cloud.redis.io/) menu and then select your subscription from the list.

Select Connectivity > VPC Peering.

Select Add peering.

Enter the VPC peering details:

SettingÂ name
Description

Project ID
Google Cloud project ID (see [Identifying projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects#identifying_projects))

Network name
Google Cloud VPC network you want to peer with (see [View networks](https://cloud.google.com/vpc/docs/create-modify-vpc-networks#viewing-networks))

Copy the Google cloud command after you enter the other VPC peering settings. You need this command to accept the peering request later:

[](/docs/latest/images/rc/subscription-connectivity-vpc-peering-gcloud-command.png)

Select Initiate peering.

Note the Cloud peering ID of the VPC peering request.

[](/docs/latest/images/rc/subscription-connectivity-vpc-peering-gcp.png)

### Approve VPC peering request

To approve the VPC peering request between Redis Cloud and Google Cloud, use the [`gcloud` CLI](https://cloud.google.com/sdk/gcloud) to run the Google cloud command that you copied before you initiated VPC peering.

Once VPC peering is established, we recommend switching your application connection string to the private endpoint.

Note:
  If you've enabled the database's [[operate/rc/security/cidr-whitelist|CIDR allow list]], you must also [[operate/rc/security/cidr-whitelist#define-cidr-allow-list|add the VPC peered IP addresses to the CIDR allow list]] to connect to the database via the private endpoint.

## Continue learning with Redis University

See the [Redis Cloud networking course](https://university.redis.io/course/xkz1coqj9tsqjr) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page