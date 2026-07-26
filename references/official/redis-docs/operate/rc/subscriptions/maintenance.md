---
title: "Subscription and database maintenance"
source: "https://redis.io/docs/latest/operate/rc/subscriptions/maintenance/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/subscriptions/maintenance"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Subscription and database maintenance"
---
# Subscription and database maintenance

Describes maintenance that Redis performs on a Redis Cloud subscription.

            Redis Cloud
          

Redis will maintain your Redis Cloud subscriptions and databases as needed to ensure your databases are running the most stable and up-to-date version of Redis.

## Maintenance windows

Maintenance windows define when Redis can perform maintenance on your subscriptions and databases. Maintenance window options depend on your subscription type.

### Redis Cloud Pro

For Redis Cloud Pro plans, Redis will perform maintenance automatically while limiting service disruption as much as possible. If you want to control when we can perform maintenance for a Redis Cloud Pro subscription, you can [[operate/rc/subscriptions/maintenance/set-maintenance-windows#set-manual-maintenance-windows|set manual maintenance windows]].

### Redis Cloud Essentials

Redis Cloud Essentials databases have a set maintenance window based on the cloud provider region. The maintenance window for Redis Cloud Essentials databases is daily from 12 AM to 6 AM in the region's local time zone. We won't use more than one maintenance window per week unless we need to do [[operate/rc/subscriptions/maintenance#urgent-maintenance|urgent maintenance]], and we won't perform any maintenance that introduces breaking changes during these maintenance windows.

## Maintenance activities

During maintenance, Redis ensures the stability of your subscriptions and databases.

This includes, but is not limited to:

Upgrading Redis or an advanced capability to the latest version
Cluster optimization
Replacing a cluster node
Adding more memory to a node
Applying security patches

Redis will notify users by email when maintenance starts and ends. For more details, see [Notifications](#notifications).

During maintenance, your database will be operational, but you may notice some latency when connecting to your databases.

Your application may also disconnect from your database for a few seconds. Most Redis clients are set to refresh their DNS address when they reconnect to the database, and you will not be required to perform any further action. If you encounter connectivity problems for more than a minute during maintenance, please refresh your DNS entries.

Tip:
  To make sure your applications are set to reconnect after maintenance activity, see [[operate/rc/resilient-apps|Develop highly available and resilient apps with Redis Cloud]].

### Urgent maintenance

Urgent maintenance refers to any activity that could affect service and cannot wait for scheduling. This includes applying urgent security patches.

Redis can perform urgent maintenance at any time, even if you have set a manual maintenance window or have temporarily [[operate/rc/subscriptions/maintenance/set-maintenance-windows#skip-maintenance-temporarily|skipped maintenance]]. Redis will notify you by email when urgent maintenance starts and ends.

### Major upgrades

We won't schedule major upgrades or upgrades that might include breaking changes until you opt-in to those changes. You can control your Redis major version on the [[operate/rc/subscriptions/view-pro-subscription|subscription page]].

## Notifications

Redis will notify you by email when maintenance starts and ends. If Redis needs an action from you to start maintenance, we will notify you with a reasonable amount of time before planned maintenance.

If you want to receive advance maintenance notifications, you must [[operate/rc/subscriptions/maintenance/set-maintenance-windows#set-manual-maintenance-windows|set manual maintenance windows]] and [contact support](https://redis.io/support/) to request advance maintenance notifications.

To receive maintenance notifications by email:

Go to [[operate/rc/security/access-control/access-management|Access Management]] and select your account in the list.

Select the Edit button.

[](/docs/latest/images/rc/icon-edit.png)

Select Operational emails if it is not already turned on.

[](/docs/latest/images/rc/access-mgmt-edit-user-dialog.png)

Select Save user to save your changes.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page