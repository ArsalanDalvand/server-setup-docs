---
title: "View and edit LangCache service"
source: "https://redis.io/docs/latest/operate/rc/context-engine/langcache/view-edit-cache/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/langcache/view-edit-cache"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "View and edit LangCache service"
---
# View and edit LangCache service

            Redis Cloud
          

LangCache on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

After you have [[operate/rc/context-engine/langcache/create-service|created your first LangCache service]], selecting LangCache from the Redis Cloud Console menu will take you to the LangCache Services page.

This page displays a list of all LangCache services associated with your account.

[](/docs/latest/images/rc/langcache-service-list.png)

Select your LangCache service from the list to view the service's details.

## Configuration tab

The Configuration tab lets you view the details of your LangCache service. It contains the following sections:

The Connectivity section provides the connection details for your LangCache service.
The General section provides the cache settings for your LangCache service.
The Actions section lets you flush or delete your LangCache service.

### Connectivity

The Connectivity section provides the connection details for your LangCache service.

[](/docs/latest/images/rc/langcache-view-connectivity.png)

Setting name
Description

API Key
The Bearer token for your LangCache API requests.

Cache ID
The unique ID of your LangCache service.

URL List
A list of base URLs for LangCache API requests. Use the closest URL for the best performance.

Select the Copy button next to the Cache ID and API Base URL to copy them to the clipboard. If you lost the API key value or need to rotate the key, you can [generate a new service API key](#replace-service-api-key) at any time.

See [[operate/rc/context-engine/langcache/use-langcache|use the LangCache API]] for more information on how to use these values.

#### Generate a new service API key

The API key is only available immediately after you create the LangCache service. If you lost this value, or need to rotate the key, you can generate a new service key at any time.

To generate a new service key:

Select Generate key.

[](/docs/latest/images/rc/langcache-replace-key.png)

A confirmation dialog will appear. Select Confirm to confirm.

The new key will appear in a dialog box. Select Copy to copy the key to the clipboard.

[](/docs/latest/images/rc/langcache-service-key.png)

Warning:

After you generate a new service key, calls to the LangCache API with the old key will fail.

This is the only time the value of the user key is available. Save it to a secure location before closing the dialog box.

If you lose the service key value, you will need to generate a new key again.

### General

The General section provides configuration details for your LangCache service.

[](/docs/latest/images/rc/langcache-view-general.png)

Setting name
Description

Service name
The name of the LangCache service.

Database
The database that stores your cache data.

Similarity threshold
The minimum similarity score required to consider a cached response a match. (Editable)

TTL
The number of seconds to cache entries before they expire. (Editable)

Embedding Provider
The embedding provider used for your service.

Embedding Model
The embedding model used for your service.

Embedding Model API key
The obfuscated API key for the embedding provider.

Some of these settings can be changed after cache creation. To do so, select the Edit button.

### Attributes

The Attributes section provides the custom attributes defined for your LangCache service.

[](/docs/latest/images/rc/langcache-view-attributes.png)

You can not edit custom attributes after cache creation.

### Actions

The Actions section lets you flush or delete your LangCache service.

[](/docs/latest/images/rc/langcache-view-actions.png)

#### Flush cache

Flushing the cache completely erases all cached data while preserving the service configuration and the search index used by the cache.

To flush the cache:

Select Flush.

A confirmation dialog will appear. Select Flush again to confirm.

Flushing the cache is permanent and cannot be undone, and will result in cache misses until the cache is repopulated.

#### Delete service

Deleting your LangCache service permanently deletes all associated cached data, the service configuration, and the LangCache search index. It also immediately terminates all API keys associated with the service. Data stored in other indexes within the same database will remain unaffected.

To delete your LangCache service:

Select Delete.

A confirmation dialog will appear. Select the checkbox to confirm that you want to delete the service.

Select Delete again to confirm.

Deleting the LangCache service is permanent and cannot be undone.

## Metrics tab

The Metrics tab provides a series of graphs showing performance data for your LangCache service. See [[operate/rc/context-engine/langcache/monitor-cache|Monitor a LangCache service]] for more information.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page