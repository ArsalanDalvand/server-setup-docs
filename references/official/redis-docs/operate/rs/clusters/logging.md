---
title: "Logging events"
source: "https://redis.io/docs/latest/operate/rs/clusters/logging/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/logging"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Logging events"
---
# Logging events

Management actions performed with Redis Software are logged to make sure system management tasks are appropriately performed or monitored by administrators and for compliance with regulatory standards.

            Redis Software
          

Management actions performed with Redis Software are logged to make sure system management tasks are appropriately performed or monitored by administrators and for compliance with regulatory standards.

Log entries contain the
following information:

Who performed the action?
What exactly was the performed action?
When was the action performed?
Did the action succeed or not?

To get the list of logged events, you can use the REST API or
the Logs screen in the UI. The Logs screen displays the system and user
events regarding alerts, notifications, and configuration.

[](/docs/latest/images/rs/screenshots/cluster/cluster-logs.png)

You can use the Logs screen to review what actions a user has performed, such as editing a database's configuration.

[[operate/rs/clusters/logging/redis-slow-log|Redis slow
log]]
[[operate/rs/clusters/logging/rsyslog-logging|rsyslogÂ logging]]

## View logs in the UI

Redis Software provides log files for auditing cluster management actions and troubleshooting. You can view these logs in the UI and on the host operating system.

To view event logs in the new Cluster Manager UI, go to Cluster > Logs.

## View logs on the server

Server logs can be found by default in the directory `/var/opt/redislabs/log/`.

These log files are used by the Redis support team to troubleshoot issues. The logs you will most frequently interact with is 'event_log.log'. This log file is where logs of configuration actions within Redis are stored and is useful to determine events that occur within Redis Software.

## Configure log timestamps

Redis Software allows you to configure log timestamps. To configure log timestamps in the new Cluster Manager UI:

Go to Cluster > Configuration > General.

Change the Time zone for the logs based on your location.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page