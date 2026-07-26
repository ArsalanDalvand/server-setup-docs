---
title: "Redis Cloud Agent Memory REST quickstart"
source: "https://redis.io/docs/latest/operate/rc/context-engine/agent-memory/use-agent-memory/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/context-engine/agent-memory/use-agent-memory"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud Agent Memory REST quickstart"
---
# Redis Cloud Agent Memory REST quickstart

Create an Agent Memory service on Redis Cloud and make your first session-memory and long-term-memory REST API requests.

            Redis Cloud
          

Redis Agent Memory on Redis Cloud is currently available as a public preview. Features and behavior are subject to change.

Use this quickstart to create an Agent Memory service on Redis Cloud and make your first REST API requests.

Note:
  Redis Agent Memory on Redis Cloud is available as a public preview. Features and behavior can change before general availability.

## Before you begin

To complete this quickstart, you need:

A Redis Cloud account that can create Agent Memory services.
An eligible Redis Cloud database, or permission to create one.
A shell with `curl` installed.

An eligible database is active, uses a Pro or Essentials plan, has a public endpoint and Query Engine, runs a supported Redis version, and has the default user enabled.

Agent Memory doesn't support Flex, Active-Active, or AWS PrivateLink databases during public preview.

For the complete list, see [[operate/rc/context-engine/agent-memory/create-service#prerequisites-and-limitations|prerequisites and limitations]].

## Create an Agent Memory service

Sign in to the [Redis Cloud console](https://cloud.redis.io/).
Select Agent Memory from the navigation menu.
If Redis Cloud displays the public-preview terms, review and accept them.
Select Quick create to use the default settings, or select Create custom and choose an eligible database.
After Redis Cloud creates the service, copy the Agent Memory API key and store it securely.

Warning:
  Redis Cloud displays the Agent Memory API key only once. If you lose it, [[operate/rc/context-engine/agent-memory/view-service#replace-service-api-key|generate a new API key]].

For screenshots and configuration details, see [[operate/rc/context-engine/agent-memory/create-service|create an Agent Memory service]].

## Save the connection values

Open the Agent Memory service in the Redis Cloud console.

On the Configuration tab, copy the API Base URL and Store ID.

Export the values in your shell. Replace each placeholder with the value from Redis Cloud:

```sh
export AGENT_MEMORY_URL='<API_BASE_URL>'
export STORE_ID='<STORE_ID>'
export API_KEY='<API_KEY>'
export SESSION_ID='quickstart-session'
export OWNER_ID='quickstart-user'
export MEMORY_ID='quickstart-preference'
```

Use the complete API base URL returned by Redis Cloud. Don't add another URL scheme, such as `https://`, to `AGENT_MEMORY_URL`.

Send the API key as a bearer token in the `Authorization` header. Keep the key out of source control, application logs, and other unsecured locations.

## Add a session event

Set the event timestamp to the current Coordinated Universal Time (UTC):

```sh
export EVENT_CREATED_AT="$(date -u +'%Y-%m-%dT%H:%M:%SZ')"
```

Add a user event to session memory:

```sh
curl --fail-with-body --silent --show-error \
  --request POST \
  --header "Authorization: Bearer $API_KEY" \
  --header 'Content-Type: application/json' \
  --data @- \
  "$AGENT_MEMORY_URL/v1/stores/$STORE_ID/session-memory/events" <<JSON
{
  "sessionId": "$SESSION_ID",
  "actorId": "$OWNER_ID",
  "role": "USER",
  "content": [
    {
      "text": "I prefer vegetarian restaurants."
    }
  ],
  "createdAt": "$EVENT_CREATED_AT"
}
JSON
```

A successful request returns `201 Created`. The response contains the stored event and its server-generated event ID.

For request and response details, see [`AddSessionEvent`](/docs/latest/develop/ai/context-engine/agent-memory/api-reference/#tag/session-memory/operation/AddSessionEvent).

## Retrieve the session

Retrieve the session event that you added:

```sh
curl --fail-with-body --silent --show-error \
  --header "Authorization: Bearer $API_KEY" \
  "$AGENT_MEMORY_URL/v1/stores/$STORE_ID/session-memory/$SESSION_ID"
```

A successful request returns `200 OK`. The response contains the session ID, owner ID, and stored events.

For request and response details, see [`GetSessionMemory`](/docs/latest/develop/ai/context-engine/agent-memory/api-reference/#tag/session-memory/operation/GetSessionMemory).

## Understand automatic extraction

Agent Memory processes session events asynchronously and extracts relevant information into long-term memory. By default, extraction runs on a five-minute cadence, so extracted memories might not appear immediately.

The next step creates a long-term memory directly. This approach lets you verify long-term-memory search without waiting for automatic extraction.

## Create a long-term memory

Create a long-term memory for the same owner and session:

```sh
curl --fail-with-body --silent --show-error \
  --request POST \
  --header "Authorization: Bearer $API_KEY" \
  --header 'Content-Type: application/json' \
  --data @- \
  "$AGENT_MEMORY_URL/v1/stores/$STORE_ID/long-term-memory" <<JSON
{
  "memories": [
    {
      "id": "$MEMORY_ID",
      "text": "The user prefers vegetarian restaurants.",
      "memoryType": "semantic",
      "sessionId": "$SESSION_ID",
      "ownerId": "$OWNER_ID"
    }
  ]
}
JSON
```

A successful request returns `201 Created`. The `created` array in the response contains the value of `MEMORY_ID`.

For request and response details, see [`BulkCreateLongTermMemories`](/docs/latest/develop/ai/context-engine/agent-memory/api-reference/#tag/long-term-memory/operation/BulkCreateLongTermMemories).

## Search long-term memory

Search for the long-term memory by meaning and owner:

```sh
curl --fail-with-body --silent --show-error \
  --request POST \
  --header "Authorization: Bearer $API_KEY" \
  --header 'Content-Type: application/json' \
  --data @- \
  "$AGENT_MEMORY_URL/v1/stores/$STORE_ID/long-term-memory/search" <<JSON
{
  "text": "What food does the user prefer?",
  "filter": {
    "ownerId": {
      "eq": "$OWNER_ID"
    }
  },
  "limit": 5
}
JSON
```

A successful request returns `200 OK`. The `items` array contains matching long-term memories, including the memory that you created.

For request and response details, see [`SearchLongTermMemory`](/docs/latest/develop/ai/context-engine/agent-memory/api-reference/#tag/long-term-memory/operation/SearchLongTermMemory).

## Next steps

Review more [Agent Memory API examples](/docs/latest/develop/ai/context-engine/agent-memory/api-examples/).
Use the [Agent Memory API reference](/docs/latest/develop/ai/context-engine/agent-memory/api-reference/) for endpoint and schema details.
[[operate/rc/context-engine/agent-memory/view-service|View and manage the service]] to update configuration, manage API keys, review metrics, flush memories, or delete the service.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page