---
title: "Delete a subscription"
source: "https://redis.io/docs/latest/operate/rc/subscriptions/delete-subscription/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/subscriptions/delete-subscription"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Delete a subscription"
---
# Delete a subscription

            Redis Cloud
          

Note:
  Once a subscription is deleted, it cannot be recovered. We recommend [backing up your data](/docs/latest/operate/rc/databases/back-up-data/) before removing databases or subscriptions.

To delete a subscription:

[Delete all databases](/docs/latest/operate/rc/databases/delete-database/) from the subscription.

The number of databases for the subscription is shown in the subscription list. You cannot delete a subscription until there are zero databases in the subscription.

[](/docs/latest/images/rc/subscription-list-pro-no-databases.png)

View the subscription details.

If you have more than one subscription, select the target subscription from the subscription list.

Select the Overview tab.

[](/docs/latest/images/rc/subscription-details-overview-flexible.png)

Select the Delete subscription button.

[](/docs/latest/images/rc/button-subscription-delete.png)
[](/docs/latest/images/rc/subscription-delete-confirm-dialog.png)

Select the Delete subscription button to confirm your choice.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)