---
title: "Connect to Amazon Web Services Transit Gateway"
source: "https://redis.io/docs/latest/operate/rc/security/aws-transit-gateway/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/aws-transit-gateway"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Connect to Amazon Web Services Transit Gateway"
---
# Connect to Amazon Web Services Transit Gateway

            Redis Cloud
          

[Amazon Web Services (AWS) Transit Gateway](https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html) acts as a Regional virtual router for traffic flowing between your virtual private cloud(s) (VPCs) and on-premises networks. You can attach different resources to your Transit Gateway which include:

One or more VPCs
One or more virtual private network (VPN) connections
One or more AWS Direct Connect gateways
One or more Transit Gateway Connect attachments
One or more transit gateway peering connections

You can connect your Redis Cloud Pro subscription to a Transit Gateway which is attached to the VPC of your application. This lets your application connect securely to your Redis Cloud database while optimizing performance.

Note:
  Transit Gateway is available only with Redis Cloud Pro.  It is not supported for Redis Cloud Essentials.

## Considerations

You can use Transit Gateway as an alternative to [[operate/rc/security/vpc-peering|VPC peering]], or you can enable both for your subscription.

Compared to VPC peering, Transit Gateway:

Supports complex network topologies, such as multiple VPCs or site-to-site VPNs.

Uses security groups and network ACLs to control traffic between VPCs.

Has a higher network latency and cost than VPC peering due to Transit Gateway infrastructure costs.

Consider using VPC peering and Transit Gateway in parallel for the following situations:

When migrating from one connectivity solution to the other.

If different applications need to connect to the same database but have different latency or security requirements.

## Prerequisites

Before you can set up Transit Gateway, you need to:

[Create a database with Redis Cloud Pro](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/) from the [Redis Cloud console](https://cloud.redis.io/#/).

[Create a transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/create-tgw.html) from the [AWS VPC console](https://console.aws.amazon.com/vpc/) in the same region as your database.

Note:
  If you use Redis Cloud Bring your own Cloud, you will need to set its IAM Instance Policy to include Transit Gateway. See [Create IAM resources using the AWS console](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/iam-resources/aws-console/) for more information.

## AWS Transit Gateway

To set up Transit Gateway:

[Associate your resource share with the Redis AWS account](#associate-resource-share).

[Accept the resource share and create an attachment](#accept-resource-share).

[Add consumer CIDRs](#add-consumer-cidrs) to the attachment.

[Update AWS route tables](#update-route-tables) with the Redis Cloud producer CIDRs.

### Associate resource share with Redis Cloud

In this step, you will associate your resource share with your subscription's AWS account. You can do this either in the [AWS console](#aws-console) or with the [AWS CLI](#aws-cli).

#### AWS Console

To use the AWS console to set up the resource share:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.

Select Connectivity > Transit Gateway to view the transit gateway settings.

In the Share Transit Gateway section, select Copy under AWS console to copy the Redis AWS Account number.

[](/docs/latest/images/rc/tgw-share-transit-gateway.png)

If you don't have a resource share for your transit gateway, follow the guide to [create a resource share](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing-create.html) in the [AWS resource access manager](https://console.aws.amazon.com/ram/home). If you do, follow the guide to [update](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing-update.html) the resource share to include the provided AWS account as an Allowed Principal.

During the Grant access to principals step, select AWS Account in the Select principal type field. Enter the copied AWS account number in the Enter an AWS Account ID field.

[](/docs/latest/images/rc/aws-tgw-add-principal.png)

After the principal is added, it may take some time before it is associated. You can see the status of the principals under Shared Principals in the resource share page.

#### AWS CLI

To use the AWS CLI to set up the resource share:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.

Select Connectivity > Transit Gateway to view the transit gateway settings.

In the Share Transit Gateway section, select Copy under AWS CLI Command to copy the Redis AWS Account number.

[](/docs/latest/images/rc/tgw-share-transit-gateway.png)

Enter the copied CLI command into a terminal shell. Replace `<TGW ARN>` with the Amazon resource name of your transit gateway.

### Accept resource share and create attachment

After you've associated the Redis AWS account with your resource share, you must accept the resource share in the Redis Cloud console.

In your Redis Cloud subscription's Transit Gateway settings, you should now see that a Resource Share is available. Select Resource Shares to view the resource share you initiated.

[](/docs/latest/images/rc/tgw-resource-shares-button.png)

Select Accept to associate the Resource Share with your Redis Cloud console account.

[](/docs/latest/images/rc/tgw-accept-resource-shares.png)

Select Close to close the Accept resource shares section.

You will now see your transit gateway in the Transit Gateways section. After the TGW status is Available, select Create Attachment under Attachment status.

[](/docs/latest/images/rc/tgw-create-attachment-button.png)

This will request a peering attachment representing Redis's AWS account to the Transit Gateway.

If your transit gateway does not automatically accept peering attachment requests, the attachment will be in Pending acceptance status. Follow the guide to [Accept a peering attachment request](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-peering.html#tgw-peering-accept-reject) from the [AWS VPC console](https://console.aws.amazon.com/vpc/).

### Add consumer CIDRs

In your Redis Cloud subscription's Transit Gateway settings, in the Transit Gateways section, select Add CIDRs under Consumer CIDRs.

[](/docs/latest/images/rc/tgw-add-cidrs-button.png)

Enter the IPv4 CIDR of the VPC you want to connect to that is also connected to your transit gateway. To find this, go to the [AWS VPC console](https://console.aws.amazon.com/vpc/) and select Your VPCs.

Select Add to add another CIDR if needed.

[](/docs/latest/images/rc/tgw-add-additional-cidrs-button.png)

Select Save to save your changes.

### Update AWS route tables

To finish Transit gateway setup, [update your route tables for the peering connection](https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-routing.html) with the following details:

In the Destination field, enter the producer deployment CIDRs.

You can find the producer deployment CIDRs on the Redis Cloud console in the Transit Gateway settings by selecting More actions > View Attachment in the Transit Gateway section.

[](/docs/latest/images/rc/tgw-attachment-more-actions-menu.png)
[](/docs/latest/images/rc/tgw-producer-cidr-copy.png)

In the Target field, select Transit Gateway and select the relevant Transit gateway ID.

After Transit gateway is established, we recommend switching your application connection string to the private endpoint.

Note:
  If you've enabled the database's [[operate/rc/security/cidr-whitelist|CIDR allow list]], you must also [[operate/rc/security/cidr-whitelist#define-cidr-allow-list|add the Transit Gateway's IP address to the CIDR allow list]] to connect to the database via the private endpoint.

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