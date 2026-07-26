---
title: "View and edit Redis Cloud Pro plan"
source: "https://redis.io/docs/latest/operate/rc/subscriptions/view-pro-subscription/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/subscriptions/view-pro-subscription"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "View and edit Redis Cloud Pro plan"
---
# View and edit Redis Cloud Pro plan

            Redis Cloud
          

To view the details of a Redis Cloud Pro subscription:

Sign in to the [Redis Cloud console](https://cloud.redis.io/#) and select Subscriptions.

If you have more than one subscription, select the target subscription from the subscription list.

[](/docs/latest/images/rc/subscription-list-select.png)

Your subscription details appear, along with a summary of your database details.

[](/docs/latest/images/rc/subscription-flexible-databases-tab-pending.png)

From here, you can:

Select Create database in this subscription to [add a database to your subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-existing/).

[](/docs/latest/images/rc/button-add-new-to-pro.png)

View the Status icon to learn the status of your subscription.  Active subscriptions display a green circle with a check mark. Pending subscriptions display an animated yellow circle.

[](/docs/latest/images/rc/icon-database-status-active.png#no-click)
 Â  

	

[](/docs/latest/images/rc/icon-subscription-status-pending.png#no-click)

Because subscriptions represent active deployments, there aren't many details you can change.  If your needs change, [create a new subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/) and then [migrate the existing data](/docs/latest/operate/rc/databases/migrate-databases/) to the new databases.

In addition, you can view and edit the following subscription details:

The Databases tab lists the databases in your subscription and summarizes their settings.

The Overview tab displays subscription settings for your Redis Cloud Pro subscription.

The Data integration tab displays your Data integration workspace and pipelines.

The Connectivity tab lets you limit access to the subscription by defining a VPC peering or other connectivity options.

The Security tab lets you set security settings for the databases in your subscription.

The Regions tab lets you manage the regions in your Active-Active subscription (Active-Active subscriptions only).

The following sections provide more info.

## Databases tab

The Databases tab summarizes the databases in your subscription.

[](/docs/latest/images/rc/subscription-flexible-databases-tab-pending.png)

The following details are provided:

Detail
Description

Status
An icon indicating whether the database is active (a green circle) or pending (yellow circle)

[](/docs/latest/images/rc/icon-database-detail-status-active.png#no-click)
Â 

	

[](/docs/latest/images/rc/icon-database-detail-status-pending.png#no-click)

Name
The database name

Endpoint
Use the Copy button to copy the endpoint URI to the Clipboard

Memory
Memory size of the database, showing the current size and the maximum size

Throughput
Maximum operations per second supported for the database

Capabilities
Identifies advanced capabilities attached to the database

Options
Icons showing options associated with the database

To view full details of a database, click its name in the list.

## Overview tab

The Overview summarizes the options used to create the subscription.

[](/docs/latest/images/rc/subscription-details-overview-flexible.png)

The general settings panel describes the cloud vendor, region, and high-availability settings for your subscription.

Select Edit to change the name of the subscription.

[](/docs/latest/images/rc/icon-edit-subscription-name.png)

Setting
Description

Cloud vendor
Your subscription cloud vendor

Plan description
Brief summary of subscription, including the plan type, cloud provider, and region

Auto Tiering
Checked when Auto Tiering is enabled

Multi-AZ
Checked when multiple availability zones are enabled

Active-Active Redis
Checked when Active-Active Redis is enabled for your subscription

Region
Describes the region your subscription is deployed to

Availability Zones
The availability zones your subscription is deployed in (Visible if you selected availability zones on creation)

AWS Resource tags
The tags applied to your resources in your BYOC account. Select the Edit button to manage your resource tags. See [Resource tags](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/resource-tags/) for more information.

(Available only if [Redis Cloud Bring your own Cloud](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/) is enabled)

The Price panel shows the monthly cost of your Redis Cloud Pro subscription.

The Payment Method panel shows the current payment details.

Select the Edit payment method button to change the credit card associated with this subscription.

[](/docs/latest/images/rc/icon-subscription-detail-change-payment-flexible.png)

Select Add credit card to add a new credit card.

The Maintenance Window panel shows your current [[operate/rc/subscriptions/maintenance/set-maintenance-windows|maintenance window settings]].

See [[operate/rc/subscriptions/maintenance|Maintenance]] for more information about subscription maintenance on Redis Cloud.

The Provisioned cloud resources panel shows the storage resources used by your subscription.

If your subscription is attached to a cloud account, the details appear in the panel header.

The Redis price panel breaks down your subscription price.

## Data Integration tab

The Data integration tab displays your Data integration workspace and pipelines. See [[operate/rc/rdi|Data integration]] for more information.

## Connectivity tab

The Connectivity tabs helps secure your subscription.

[](/docs/latest/images/rc/subscription-details-connectivity-tab-flexible.png)

Here, you can:

Set up a [[operate/rc/security/vpc-peering|VPC peering]] relationship between the virtual private cloud (VPC) hosting your subscription and another VPC.

Set up a [CIDR allow list](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/subscription-whitelist/) containing IP addresses or security groups permitted to access your subscription ([Bring your own Cloud](/docs/latest/operate/rc/subscriptions/bring-your-own-cloud/) only).

Set up [[operate/rc/security/private-service-connect|Private Service Connect]] (Google Cloud only), [[operate/rc/security/aws-transit-gateway|Transit Gateway]] (AWS only), or [[operate/rc/security/aws-privatelink|AWS PrivateLink]] (AWS only).

See the individual links to learn more.

## Security tab

The Security tab lets you set security settings for the databases in your subscription.

Here, you can [[operate/rc/security/database-security/block-public-endpoints|block public endpoints]] for all databases in the subscription.

## Regions tab

The Regions tab is only available for Active-Active subscriptions.  It lets you manage the regions in your Active-Active subscription.

[](/docs/latest/images/rc/subscription-details-regions-tab.png)

See [Manage regions for an Active-Active database](/docs/latest/operate/rc/databases/active-active/manage-regions/) for more information.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page