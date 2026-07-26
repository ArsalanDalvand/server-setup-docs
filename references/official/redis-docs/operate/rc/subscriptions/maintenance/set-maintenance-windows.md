---
title: "Set maintenance windows"
source: "https://redis.io/docs/latest/operate/rc/subscriptions/maintenance/set-maintenance-windows/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/subscriptions/maintenance/set-maintenance-windows"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Set maintenance windows"
---
# Set maintenance windows

Shows how to set manual maintenance windows and skip maintenance.

            Redis Cloud
          

By default, Redis will maintain your Redis Cloud subscriptions and databases as needed. During maintenance, you may notice some latency when connecting to your databases. For more information about the types of maintenance and how Redis maintains your subscriptions, see [[operate/rc/subscriptions/maintenance|Maintenance]].

For Redis Cloud Pro plans, Redis will perform maintenance automatically while limiting service disruption as much as possible. If you want to control when Redis performs maintenance for a Redis Cloud Pro subscription, you can [set manual maintenance windows](#set-manual-maintenance-windows) to ensure non-urgent maintenance will occur at set times. Configuring or altering the maintenance window will not have any impact on your subscription or databases.

Note:
  You can only set manual maintenance windows for Redis Cloud Pro plans. A Redis Cloud Essentials database has a set maintenance window based on the region where it is located. See [[operate/rc/subscriptions/maintenance#redis-cloud-essentials|Essentials maintenance]] for more information.

## Set manual maintenance windows

To set manual maintenance windows for a single Redis Cloud Pro subscription:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.

Select the Overview tab.

In the Maintenance Windows panel, select Manual.

Click Activate to confirm your selection.

Enter the maintenance window time frame details.

[](/docs/latest/images/rc/subscriptions-set-maintenance-window.png)

Select the days that Redis can perform maintenance in the Days drop-down.
Select the time that Redis can start performing maintenance on those days in the From drop-down.
Select the amount of time that Redis can perform maintenance in the Duration drop-down.
Select + Time Frame to add another time frame for maintenance.
Select Advance notification if you want to be notified of maintenance in advance.

Note:
  You must [contact support](https://redis.io/support/) to request and be approved for advance maintenance notifications, otherwise skip this step as the this option does not appear by default.

Redis recommends allowing maintenance on at least two different days for 8 hours on each day.

Click Save.

## Skip maintenance temporarily

To skip maintenance temporarily for a subscription:

From the [Redis Cloud console](https://cloud.redis.io/), select the Subscriptions menu and then select your subscription from the list.

Select the Overview tab.

In the Maintenance Windows panel, select Skip the next 14 days.

Click Continue to confirm your selection.

You will only be allowed to skip maintenance once per month.

During the skipped maintenance period, Redis will not perform any minor or major upgrades. Redis may perform [[operate/rc/subscriptions/maintenance#urgent-maintenance|urgent maintenance]] on your subscription, but only if it is absolutely necessary.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page