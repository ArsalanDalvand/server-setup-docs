---
title: "Enable the API"
source: "https://redis.io/docs/latest/operate/rc/api/get-started/enable-the-api/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/api/get-started/enable-the-api"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable the API"
---
# Enable the API

Use the Redis Cloud dashboard to enable the REST API.

            Redis Cloud
          

If you have a Redis Cloud account, you can use a REST API to manage your subscriptions and databases programmatically.

For security reasons, the Redis Cloud API is inactive by default.

To enable the API:

Sign in to your [Redis Cloud account](https://cloud.redis.io) as an account owner.

From the menu, choose Access Management.

When the Access Management screen appears, select the API Keys tab.

[](/docs/latest/images/rc/access-management-api-keys-tab.png)

If a Copy button appears to the right of the API account key, the API is enabled.  This button copies the account key to the Clipboard.

[](/docs/latest/images/rc/button-copy.png)

If you see an Enable API button, select it to enable the API and generate your API account key.

[](/docs/latest/images/rc/button-access-management-enable-api.png)

To authenticate REST API calls, you need to use both the API account key and an [[operate/rc/api/get-started/manage-api-keys#api-user-keys|API user key]] to make API calls.

Only account owners can see the access key in the account settings and give API access to other users.

Warning:
  Make sure that you keep your access keys secret. Anyone who sends an API request with a valid access key can make changes to your account.

To manage your API keys or to limit IP addresses for user keys, see [[operate/rc/api/get-started/manage-api-keys|Manage API keys]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)