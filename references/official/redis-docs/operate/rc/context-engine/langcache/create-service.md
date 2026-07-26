---
title: "Create a LangCache service"
source: "https://redis.io/docs/latest/operate/rc/context-engine/langcache/create-service/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/langcache/create-service"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create a LangCache service"
---
# Create a LangCache service

            Redis Cloud
          

LangCache on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

Redis LangCache provides vector search capabilities and efficient caching for AI-powered applications. This guide walks you through creating and configuring a LangCache service in Redis Cloud.

## Prerequisites

To create a LangCache service, you will need:

A Redis Cloud database. If you don't have one, see [Create a database](/docs/latest/operate/rc/databases/create-database/).

Note:

LangCache does not support the following databases during public preview:

Databases with a [[operate/rc/security/cidr-whitelist|CIDR allow list]]
[Active-Active](/docs/latest/operate/rc/databases/active-active/) databases
Databases with the [[operate/rc/security/access-control/data-access-control/default-user|default user]] turned off

An API key for your embedding provider, if not using the default Redis embedding provider. LangCache supports [OpenAI](https://platform.openai.com/api-keys) and the Redis embedding provider during public preview.

## Create a LangCache service

From the [Redis Cloud console](https://cloud.redis.io/), select LangCache from the left-hand menu.

If you have not already created a LangCache service, you'll see a page with an introduction to LangCache. Otherwise, select New service to go to the LangCache introduction page.

[](/docs/latest/images/rc/langcache-new-service.png)

From here:

Select Quick create to create a LangCache service with default settings using your Free 30MB database. If you haven't created a Free database yet, Redis Cloud will create one and set up the LangCache service for you.

[](/docs/latest/images/rc/langcache-quick-create.png)

After Redis Cloud creates your LangCache service, a window containing your LangCache service key will appear. Select Copy to copy the key to your clipboard.

[](/docs/latest/images/rc/langcache-service-key.png)

Warning:

This is the only time the value of the user key is available. Save it to a secure location before closing the dialog box.

If you lose the service key value, you will need to [[operate/rc/context-engine/langcache/view-edit-cache#replace-service-api-key|generate a new service key]] to be able to use the LangCache API.

After your cache is created, you can [[operate/rc/context-engine/langcache/use-langcache|use the LangCache API]] from your client app.

You can also [[operate/rc/context-engine/langcache/view-edit-cache|view and edit the cache]] and [[operate/rc/context-engine/langcache/monitor-cache|monitor the cache's performance]].

If you want to customize your LangCache service, select Create custom service.

[](/docs/latest/images/rc/langcache-custom-service.png)

This takes you to the Create LangCache service page. The page is divided into the following sections:

The [General settings](#general-settings) section defines basic properties of your service.
The [Embedding settings](#embedding-settings) section defines the embedding model used by your service.
The [Attributes settings](#attributes-settings) section allows you to define attributes for your service.

### General settings

The General settings section defines basic properties of your service.

[](/docs/latest/images/rc/langcache-general-settings.png)

Setting name
Description

Service name
Enter a name for your LangCache service. We recommend you use a name that describes your service's purpose.

Select database
Select the Redis Cloud database to use for this service from the list.

TTL
The time to live (TTL) for cache entries, in milliseconds. Default: `No expiration` - items in the cache will remain until manually removed.

User for this service
The [[operate/rc/security/access-control/data-access-control/role-based-access-control|database access user]] to use for this service. LangCache only supports the [[operate/rc/security/access-control/data-access-control/default-user|`default` user]] during public preview.

### Embedding settings

The Embedding settings section defines the embedding model used by your service.

[](/docs/latest/images/rc/langcache-embedding-settings.png)

Required settings depend on the embedding provider you select.

Setting name
Description

Embedding Provider
The embedding provider to use for your service. Select between `Redis`, `OpenAI`, and `Bring your own`. 
Any embedding provider you use must support the [OpenAI embeddings API](https://platform.openai.com/docs/api-reference/embeddings).

Embedding provider API key
Enter your embedding provider's API key (`OpenAI` and `Bring your own` embedding providers only)

Embedding provider URL
Enter your embedding provider's API URL (`Bring your own` embedding provider only)

Model
Select or enter the embedding model to use for your service.

Similarity threshold
Set the minimum similarity score required to consider a cached response a match. Range: `0.5` to `1.0`. Default: `0.85`

A higher value means more precise matches, but if it's too high, you will compromise on the number of matches and may lose relevant matches. A lower value means more matches, but may include less relevant matches. We recommend starting between `0.8` and `0.9` and then fine-tuning based on your results.

### Attributes settings

Attributes provide powerful scoping capabilities for your LangCache operations. Think of them as tags or labels that help you organize and manage your cached data with precision.

The Attributes settings section allows you to define attributes for your service. It is collapsed by default.

[](/docs/latest/images/rc/langcache-attribute-settings.png)

LangCache allows you to define up to 5 custom attributes that align with your specific use case. To add a new attribute:

Select Add attribute.

[](/docs/latest/images/rc/langcache-add-attribute.png)

Give your custom attribute a descriptive name and select the check mark button to save it.

[](/docs/latest/images/rc/langcache-custom-attributes.png)

After you save your custom attribute, it will appear in the list of custom attributes. Use the Delete button to remove it.

[](/docs/latest/images/rc/icon-delete-lb.png)

You can also select Add attribute again to add an additional attribute.

[](/docs/latest/images/rc/langcache-add-attribute.png)

### Create service

When you are done setting the details of your LangCache service, select Create to create it.

[](/docs/latest/images/rc/button-access-management-user-key-create.png)

A window containing your LangCache service key will appear. Select Copy to copy the key to your clipboard.

[](/docs/latest/images/rc/langcache-service-key.png)

Warning:

This is the only time the value of the user key is available. Save it to a secure location before closing the dialog box.

If you lose the service key value, you will need to [[operate/rc/context-engine/langcache/view-edit-cache#replace-service-api-key|generate a new service key]] to be able to use the LangCache API.

You'll be taken to your LangCache service's Configuration page. You'll also be able to see your LangCache service in the LangCache service list.

[](/docs/latest/images/rc/langcache-service-list.png)

If an error occurs, verify that:

Your database is active.
You have provided a valid OpenAI API key.
You have provided valid values for all the required fields.

For help, [contact support](https://redis.io/support/).

## Next steps

After your cache is created, you can [[operate/rc/context-engine/langcache/use-langcache|use the LangCache API]] from your client app.

You can also [[operate/rc/context-engine/langcache/view-edit-cache|view and edit the cache]] and [[operate/rc/context-engine/langcache/monitor-cache|monitor the cache's performance]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page