---
title: "Redis Cloud changelog (October 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/october-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/october-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (October 2023)"
---
# Redis Cloud changelog (October 2023)

New features, enhancements, and other changes added to Redis Cloud during October 2023.

            Redis Cloud
          

## New features

### Cost report CSV download

You can now download shard cost reports in CSV format from the [[operate/rc/billing-and-payments|Billing and Payments]] and [[operate/rc/logs-reports/usage-reports|Usage Reports]] pages.

Select the Cost report button to download a report of all shard costs in CSV format.

[](/docs/latest/images/rc/button-cost-report-download.png)

The report gives a breakdown of all your Redis billing unit costs, per subscription and database, by month. Subscriptions under the minimum subscription price are broken down into actual billing unit prices and the difference from minimum price.

### SAML account linking tokens

The process for [[operate/rc/security/access-control/saml-sso#link-other-accounts|linking new Redis accounts]] to your [[operate/rc/security/access-control/saml-sso|SAML single sign-on]] configuration has changed to enhance security. Now, both accounts must use a token to ensure that the connection is legitimate.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page