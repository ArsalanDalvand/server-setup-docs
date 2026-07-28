---
title: "System Events"
source: "https://docs.nats.io/running-a-nats-service/configuration/sys_accounts.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/sys_accounts"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "System Events"
---
# System Events

NATS servers leverage [Accounts](/running-a-nats-service/configuration/securing_nats/accounts.md) support and generate events such as:

* account connect/disconnect
* authentication errors
* server shutdown
* server stat summary

In addition the server supports a limited number of requests that can be used to query for account connections, server stat summaries, and pinging servers in the cluster.

These events are enabled by configuring `system_account` and [subscribing/requesting](#available-events-and-services) using a *system account* user.

[Accounts](/running-a-nats-service/configuration/securing_nats/accounts.md) are used so that subscriptions from your applications, say `>`, do not receive system events and vice versa. Using accounts requires either:

* [Configuring authentication locally](#local-configuration) and listing one of the accounts in `system_account`
* Or by using decentralized authentication and authorization via [jwt](/running-a-nats-service/configuration/securing_nats/auth_intro/jwt.md) as shown in this [Tutorial](/running-a-nats-service/configuration/sys_accounts/sys_accounts.md). In this case `system_account` contains the account public key.

N.B. The default global account `$G` does not publish advisories.

## Available Events and Services

### System Account

The system account publishes messages under well known subject patterns.

Server initiated events:

* `$SYS.ACCOUNT.<id>.CONNECT` (client connects)
* `$SYS.ACCOUNT.<id>.DISCONNECT` (client disconnects)
* `$SYS.ACCOUNT.<id>.SERVER.CONNS` (connections for an account changed)
* `$SYS.SERVER.<id>.CLIENT.AUTH.ERR` (authentication error)
* `$SYS.SERVER.<id>.STATSZ` (stats summary)

In addition other tools with system account privileges, can initiate requests (Examples can be found [here](/running-a-nats-service/configuration/sys_accounts/sys_accounts.md#system-services)):

* `$SYS.REQ.SERVER.<id>.STATSZ` (request server stat summary)
* `$SYS.REQ.SERVER.PING` (discover servers - will return multiple messages)

[Monitoring endpoints](/running-a-nats-service/configuration/monitoring.md) as listed in the table below are accessible as system services using the following subject pattern:

* `$SYS.REQ.SERVER.<id>.<endpoint-name>` (request server monitoring endpoint corresponding to endpoint name.)
* `$SYS.REQ.SERVER.PING.<endpoint-name>` (from all server, request server monitoring endpoint corresponding to endpoint name - will return multiple messages)

| Endpoint                                                                                                     | Endpoint Name |
| ------------------------------------------------------------------------------------------------------------ | ------------- |
| [General Server Information](/running-a-nats-service/configuration/monitoring.md#general-information)        | `VARZ`        |
| [Connections](/running-a-nats-service/configuration/monitoring.md#connection-information)                    | `CONNZ`       |
| [Routing](/running-a-nats-service/configuration/monitoring.md#route-information)                             | `ROUTEZ`      |
| [Gateways](/running-a-nats-service/configuration/monitoring.md#gateway-information)                          | `GATEWAYZ`    |
| [Leaf Nodes](/running-a-nats-service/configuration/monitoring.md#leaf-nodes-information)                     | `LEAFZ`       |
| [Subscription Routing](/running-a-nats-service/configuration/monitoring.md#subscription-routing-information) | `SUBSZ`       |
| [JetStream](/running-a-nats-service/configuration/monitoring.md#jetstream-information)                       | `JSZ`         |
| [Accounts](/running-a-nats-service/configuration/monitoring.md#account-information)                          | `ACCOUNTZ`    |
| [Health](/running-a-nats-service/nats_admin/monitoring.md#health)                                            | `HEALTHZ`     |

* `"$SYS.REQ.ACCOUNT.<account-id>.<endpoint-name>`(from all server, request account specific monitoring endpoint corresponding to account id and endpoint name - will return multiple messages)

| Endpoint                                                                                                     | Endpoint Name |
| ------------------------------------------------------------------------------------------------------------ | ------------- |
| [Connections](/running-a-nats-service/configuration/monitoring.md#connection-information)                    | `CONNZ`       |
| [Leaf Nodes](/running-a-nats-service/configuration/monitoring.md#leaf-nodes-information)                     | `LEAFZ`       |
| [Subscription Routing](/running-a-nats-service/configuration/monitoring.md#subscription-routing-information) | `SUBSZ`       |
| [JetStream](/running-a-nats-service/configuration/monitoring.md#jetstream-information)                       | `JSZ`         |
| [Account](/running-a-nats-service/configuration/monitoring.md#account-information)                           | `INFO`        |

Servers like `nats-account-server` publish system account messages when a claim is updated, the nats-server listens for them, and updates its account information accordingly:

* `$SYS.ACCOUNT.<id>.CLAIMS.UPDATE`

With these few messages you can build useful monitoring tools:

* health/load of your servers
* client connects/disconnects
* account connections
* authentication errors

## Local Configuration

To make use of System events, just using accounts, your configuration can look like this:

```
accounts: {
    USERS: {
        users: [
            {user: a, password: a}
        ]
    },
    SYS: { 
        users: [
            {user: admin, password: changeit}
           ]
    },
}
system_account: SYS
```

Please note that applications now have to authenticate such that a connection can be associated with an account. In this example username and password were chosen for simplicity of the demonstration. Subscribe to all system events like this `nats sub -s nats://admin:changeit@localhost:4222 ">"` and observe what happens when you do something like `nats pub -s "nats://a:a@localhost:4222" foo bar`. Examples on how to use system services can be found [here](/running-a-nats-service/configuration/sys_accounts/sys_accounts.md#system-services).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/sys_accounts.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
