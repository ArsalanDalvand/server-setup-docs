---
title: "Download and analyze cost report"
source: "https://redis.io/docs/latest/operate/rc/billing-and-payments/cost-report/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/billing-and-payments/cost-report"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Download and analyze cost report"
---
# Download and analyze cost report

Learn how to download and use the cost report for your Redis Cloud account.

            Redis Cloud
          

The Redis Cloud cost report gives you a detailed breakdown of your Redis Cloud subscription usage and associated charges. You can use it to track, audit, and optimize your Redis Cloud spending across Essentials and Pro subscription plans.

You can download the cost report from the [[operate/rc/billing-and-payments|Billing and payments]] and [[operate/rc/logs-reports/usage-reports|Usage reports]] pages. You can also use the [Redis Cloud REST API](#rest-api) to get a cost report in [FOCUS](https://focus.finops.org/) format.

Select the Cost report button to download a report of all shard costs in CSV format.

[](/docs/latest/images/rc/button-cost-report-download.png)

The report gives a breakdown of all your Redis billing unit costs, per subscription and database, by month. Subscriptions under the minimum subscription price are broken down into actual billing unit prices and the difference from minimum price.

The cost report helps you with:

Internal cost visibility
Usage trend analysis
Budget forecasting
Troubleshooting unexpected charges

Note:

Consider the following limitations when you view the cost report:

All pricing is based on list price. Account-specific discounts don't appear in the cost report.
Network costs for Pro subscriptions aren't available in the cost report before November 2025.
Network costs for Pro subscriptions appear for each subscription. Network costs for individual databases aren't available.
Monthly network costs for Pro subscriptions take up to 72 hours to appear in the cost report.

## Structure

The cost report CSV file has two sections: one for Redis Cloud Essentials subscriptions and one for Redis Cloud Pro subscriptions.

For more information about how to use the cost report effectively, see [How to download and visualize the cost report](https://support.redislabs.com/hc/en-us/articles/30042563097874-How-to-Download-and-Visualize-Redis-Cloud-Cost-Report) in the Redis knowledge base.

### Essentials subscriptions

In the Essentials subscription section, each row represents one month of usage for a subscription.

Column
Description

Start date
The start date of the usage period

End date
The end date of the usage period

Subscription ID
The subscription's unique ID

Subscription name
The subscription's name

Plan size
The size of the [[operate/rc/subscriptions/view-essentials-subscription/essentials-plan-details|subscription plan]], in MB

Plan cost
The cost of the subscription plan for the usage period

All columns after the Plan cost column show the key values for any [tags](/docs/latest/operate/rc/databases/tag-database/) you added to databases in all subscriptions. If a database has a tag with that key, the cell shows the tag value. You can use this information to filter and analyze your costs by tag.

Note:
  The cost report doesn't show tags for Essentials subscriptions created before January 2024 with multiple databases.

### Pro subscriptions

In the Pro subscription section, each subscription has multiple rows per usage period:

Each row represents each database's usage within the subscription over the usage period.

If you changed a database's configuration, the cost report shows additional rows for usage before and after the change.
For Active-Active databases, there are multiple rows for each region of the database.

Another row shows the monthly network costs for the subscription after November 2025.
If the Pro subscription doesn't meet the minimum price threshold, the cost report shows the actual cost of the subscription and another row shows the difference from the minimum price.

Column
Description

Start date
The start date of the usage period

End date
The end date of the usage period

Subscription ID
The subscription's unique ID

Subscription name
The subscription's name

Database ID
The database's unique ID

Database name
The database's name

Region
The database's region. For Active-Active databases, the cost report shows a row for each region of the database.

High availability
Whether the subscription is [highly available](/docs/latest/operate/rc/databases/configuration/high-availability/)

Memory limit
The database's memory limit, in MB

Throughput
The database's throughput, in ops/sec

RBU type
The [Redis Billing Unit (RBU)](#billing-unit-types) type of the database. This column shows "difference from min subscription price" for the row that shows the difference from the minimum subscription price, and "Network" for the row that shows the monthly network costs for the subscription.

RBU Count
The number of Redis Billing Units (RBUs) used by the database.

RBU Price/hr
The price per hour for the Redis Billing Unit (RBU) type of the database.

Hours
The number of hours the database was active during the usage period.

Total cost
The total cost of the database or network costs for the usage period

All columns after the Total cost column show the key values for any [tags](/docs/latest/operate/rc/databases/tag-database/) you added to databases in all subscriptions. If a database has a tag with that key, the column shows the value for that tag. You can use this information to filter and analyze your costs by tag.

#### Billing unit types

The Redis Billing Unit types associated with your Pro subscription depend on your database memory size and throughput requirements.

Billing unit type
Capacity (Memory/Throughput)

Pico
100MB / 100 ops/sec

Nano
500MB / 500 ops/sec

Micro
1GB / 1K ops/sec

High-throughput
2.5GB / 25K ops/sec

Small
12.5GB / 12.5K ops/sec

Large
25GB  / 25K ops/sec

Very large[1](#table-note-1)
50GB / 5K ops/sec

XLarge[2](#table-note-2)
50GB / 10K ops/sec

Flex-10[3](#table-note-3)
50GB / 5K ops/sec

Flex-20[3](#table-note-3)
50GB / 10K ops/sec

Flex-30[3](#table-note-3)
50GB / 15K ops/sec

Flex-40[3](#table-note-3)
50GB / 20K ops/sec

Flex-50[3](#table-note-3)
50GB / 25K ops/sec

Used for databases with Auto Tiering before Redis 7.2.

Used for hosted databases with Auto Tiering between Redis 7.2 and Redis 8.2.

Used for Redis Flex databases for Redis 8.2 or later. Available Flex billing units depend on the RAM percentage you set for your database.

Prices vary according to the cloud provider and region.  Minimum prices apply.

## Get FOCUS format using REST API

You can use the [[operate/rc/api|Redis Cloud REST API]] to get a cost report in [FinOps Open Cost and Usage Specification (FOCUS)](https://focus.finops.org/) compatible format.

To get the cost report using the REST API:

Use [[operate/rc/api/api-reference#tag/Account/operation/createCostReport|`POST /cost-report`]] to generate a cost report, with the request body containing the `startDate` and `endDate` for the report as well as any optional filters. Your account must have the Owner, Viewer, or Billing admin role to generate a cost report through this endpoint.

The response includes a `taskId` that you can use to track the status of the report generation.

Use [[operate/rc/api/api-reference#tag/Tasks/operation/getTaskById|`GET /tasks/{taskId}`]] to check report generation status. The report is ready when the `status` is `processing-completed` and the `response` field contains a `costReportId`.

Use [[operate/rc/api/api-reference#tag/Account/operation/getCostReport|`GET /cost-report/{costReportId}`]] to download the generated cost report.

The cost report returned from the Redis Cloud REST API contains fields from the [FOCUS column library](https://focus.finops.org/focus-columns/). See [Generate FOCUS-compliant cost report with REST API](/docs/latest/operate/rc/api/examples/generate-cost-report/) for more details.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page