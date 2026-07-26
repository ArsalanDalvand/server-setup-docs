---
title: "System logs"
source: "https://redis.io/docs/latest/operate/rc/logs-reports/system-logs/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/logs-reports/system-logs"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "System logs"
---
# System logs

You can view and export system logs to track any activity associated with your subscriptions and databases.

            Redis Cloud
          

The Logs page contains events, alerts, and logs from the activities, databases, and subscriptions associated with your account.

[](/docs/latest/images/rc/system-logs.png)

You can:

Sort the list by a specific field in descending or ascending order. Supported fields include Time, Originator, Database name, API key name, and Activity.

[](/docs/latest/images/rc/icon-list-sort-asc.png#no-click)
Â 

	

[](/docs/latest/images/rc/icon-list-sort-desc.png#no-click)

Select the arrow icon to change the sort order.  You can only sort by one field at a time.

Use the Export all button to export all logs as a comma-separated values (CSV) file for use in other systems and programs.

[](/docs/latest/images/rc/system-logs-export.png)

Use the refresh button to refresh the system logs.

[](/docs/latest/images/rc/system-logs-refresh.png)

Use the search bar to search for specific entries. Supported fields include Originator, Database name, API key name, Activity, and Description.

## Get system logs using REST API

Use [[operate/rc/api/api-reference#tag/Account/operation/getAccountSystemLogs|`GET /logs`]] to get the system logs using the REST API. See [Audit system logs](/docs/latest/operate/rc/api/examples/audit-system-logs/) for more information.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page