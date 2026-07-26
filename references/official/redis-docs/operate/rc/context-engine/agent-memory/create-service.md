---
title: "Create an Agent Memory service"
source: "https://redis.io/docs/latest/operate/rc/context-engine/agent-memory/create-service/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/agent-memory/create-service"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Create an Agent Memory service"
---
# Create an Agent Memory service

Create an Agent Memory service in Redis Cloud to store working and long-term memory for AI agents.

            Redis Cloud
          

Redis Agent Memory on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

Redis Agent Memory provides a persistent, structured memory layer that AI agents can use to store, retrieve, and manage contextual data across interactions. This guide walks you through creating and configuring an Agent Memory service in Redis Cloud.

## Prerequisites and limitations

To create a Redis Agent Memory service, you will need a Redis Cloud database. If you don't have one, see [Create a database](/docs/latest/operate/rc/databases/create-database/).

Note:

Agent Memory does not support the following databases during public preview:

[Redis Flex](/docs/latest/operate/rc/databases/create-database/create-flex-database/) databases
Databases using [[operate/rc/security/aws-privatelink|AWS PrivateLink]] connectivity
[Active-Active](/docs/latest/operate/rc/databases/active-active/) databases
Databases with the [[operate/rc/security/access-control/data-access-control/default-user|default user]] turned off

## Create an Agent Memory service

From the [Redis Cloud console](https://cloud.redis.io/), select Agent Memory from the left-hand menu.

If you have not already created an Agent Memory service, you'll see a page with an introduction to Agent Memory. Otherwise, select New service to go to the Agent Memory introduction page.

[](/docs/latest/images/rc/langcache-new-service.png)

From here:

Select Quick create to create an Agent Memory service with default settings using your Free 30MB database. If you haven't created a Free database yet, Redis Cloud will create one and set up the Agent Memory service for you.

[](/docs/latest/images/rc/langcache-quick-create.png)

After Redis Cloud creates your Agent Memory service, a window containing your Agent Memory service key will appear. Select Copy to copy the key to your clipboard.

[](/docs/latest/images/rc/agent-memory-service-key.png)

Warning:

This is the only time the value of the user key is available. Save it to a secure location before closing the dialog box.

If you lose the service key value, you will need to [[operate/rc/context-engine/agent-memory/view-service#replace-service-api-key|generate a new service key]] to be able to use the Agent Memory API.

After Redis Cloud creates your service, [[operate/rc/context-engine/agent-memory/use-agent-memory|continue with the REST quickstart]].

If you want to customize your Agent Memory service, select Create custom.

[](/docs/latest/images/rc/agent-memory-custom-service.png)

This takes you to the Create Agent Memory Service page. This page is divided into the following sections:

The [General settings](#general-settings) section defines basic properties of your service.
The [Memory configuration](#memory-configuration) section allows you to define the time-to-live (TTL) of your agent's memories, set how often memories are extracted, and control automatic summarization of session memory.

### General settings

The General settings section defines basic properties of your service.

[](/docs/latest/images/rc/agent-memory-general-settings.png)

Setting name
Description

Service name
Enter a name for your Agent Memory service. We recommend you use a name that describes your service's purpose.

Select database
Select the Redis Cloud database to use for this service from the list.

User for this service
The [[operate/rc/security/access-control/data-access-control/role-based-access-control|database access user]] to use for this service. Agent Memory only supports the [[operate/rc/security/access-control/data-access-control/default-user|`default` user]] during public preview.

### Memory configuration

The Memory configuration section allows you to define the time-to-live (TTL) of your agent's memories, set how often memories are extracted, and control automatic summarization of session memory.

[](/docs/latest/images/rc/agent-memory-memory-configuration.png)

Setting name
Description

Short-term TTL
Defines the time-to-live (TTL) of your agent's short-term memory (also known as session memory). You can define this TTL in seconds, minutes, hours, or days. Default: 1 hour

Long-term TTL
Defines the time-to-live (TTL) of your agent's long-term memory. You can define this TTL in seconds, minutes, hours, or days. Default: 365 days

Extraction cadence
How often the extraction pipeline runs while a session is active. Leave this blank to use the default of 5 minutes, or set a value between 60 and 600 seconds to override it.

#### Automatic summarization

Automatic summarization keeps your agent sharp during long conversations by compressing older messages in a session into a summary, while keeping the most recent messages in full. This helps control the size of session memory without losing important context.

Use the Automatic summarization toggle to enable or disable this behavior. When it is enabled, you can configure the following settings:

Setting name
Description

Summarize after (messages)
The number of messages a session can hold before older messages are summarized. When a session exceeds this threshold, the oldest messages beyond the Keep most recent count are compressed into a summary.

Keep most recent (messages)
The number of most recent messages that are always kept in full and never summarized.

For example, with Summarize after set to 20 and Keep most recent set to 10, once a session reaches 20 messages, the oldest 10 messages are summarized automatically and the 10 most recent are kept in full.

### Create service

When you are done setting the details of your Agent Memory service, select Create to create it.

[](/docs/latest/images/rc/button-access-management-user-key-create.png)

A window containing your Agent Memory service key will appear. Select Copy to copy the key to your clipboard.

[](/docs/latest/images/rc/agent-memory-service-key.png)

Warning:

This is the only time the value of the user key is available. Save it to a secure location before closing the dialog box.

If you lose the service key value, you will need to [[operate/rc/context-engine/agent-memory/view-service#replace-service-api-key|generate a new service key]] to be able to use the Agent Memory API.

If an error occurs, verify that your database is active. For help, [contact support](https://redis.io/support/).

## Next steps

After Redis Cloud creates your service, [[operate/rc/context-engine/agent-memory/use-agent-memory|continue with the REST quickstart]].

You can also [[operate/rc/context-engine/agent-memory/view-service|view and edit the service]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page