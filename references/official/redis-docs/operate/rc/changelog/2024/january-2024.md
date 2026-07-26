---
title: "Redis Cloud changelog (January 2024)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2024/january-2024/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2024/january-2024"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (January 2024)"
---
# Redis Cloud changelog (January 2024)

New features, enhancements, and other changes added to Redis Cloud during January 2024.

            Redis Cloud
          

## New features

### Transit Gateway

You can now connect your Flexible subscriptions hosted on Amazon Web Services (AWS) to [AWS Transit Gateway](https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html). See [[operate/rc/security/aws-transit-gateway|Connect to Transit Gateway]] to learn how to connect your Flexible subscription to Transit Gateway.

### Billing Admin role

You can now add a user with the Billing Admin role in the [[operate/rc/security/access-control/access-management|Access Management]] screen. Billing Admins can view and edit settings related to billing and payments. They can add and remove payment methods and change the payment method for a subscription, but they cannot change any other subscription or database settings.

See [[operate/rc/security/access-control/access-management#team-management-roles|Team Management roles]] to see an overview of user roles and their permissions.

### Database tags

You can now add [tags](/docs/latest/operate/rc/databases/tag-database/) to your database.

Tags are key-value pairs that let you categorize your databases. You can create tags and add them to your databases to associate them with each other. Once you've added tags, you can filter your databases in the [database list](/docs/latest/operate/rc/databases/view-edit-database/#manage-the-database-list) by tag key or value.

See [Manage tags](/docs/latest/operate/rc/databases/tag-database/) to learn how to add tags to your database.

### Network data cost breakdowns

Invoices for customers with Flexible and Annual subscriptions now include a breakdown of network data costs for the subscription. To download an invoice, go to the [[operate/rc/billing-and-payments#download-invoice|Billing and Payments]] page on the [Redis Cloud console](https://cloud.redis.io/).

Customers with Annual subscriptions can also find a breakdown of network data costs in the consumption report.

## Deprecations

As of January 2024, new and upgraded Redis Cloud Essentials subscriptions are limited to one database per subscription. If you have a Redis Cloud Essentials subscription created before January 2024, you can add databases up to the previous plan limit. If you choose to upgrade your plan, you can keep your databases, but you will need to create a new subscription to add another database.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page