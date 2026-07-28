---
title: "Username/Password"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/username_password"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Username/Password"
---
# Username/Password

## Plain Text Passwords

You can authenticate one or more clients using username and passwords; this enables you to have greater control over the management and issuance of credential secrets.

## Single User

```
authorization: {
    user: a,
    password: b
}
```

You can also specify a single username/password by:

```
> nats-server --user a --pass b
```

## Multiple users

```
authorization: {
    users: [
        {user: a, password: b},
        {user: b, password: a}
    ]
}
```

## Bcrypted Passwords

Username/password also supports bcrypted passwords using the [`nats`](/using-nats/nats-tools/nats_cli.md) tool. Simply replace the clear text password with the bcrypted entries:

```
> nats server passwd
? Enter password [? for help] **********************
? Reenter password [? for help] **********************

$2a$11$V1qrpBt8/SLfEBr4NJq4T.2mg8chx8.MTblUiTBOLV3MKDeAy.f7u
```

And on the configuration file:

```
authorization: {
    users: [
        {user: a, password: "$2a$11$V1qrpBt8/SLfEBr4NJq4T.2mg8chx8.MTblUiTBOLV3MKDeAy.f7u"},
        ...
    ]
}
```

## Reloading a Configuration

As you add/remove passwords from the server configuration file, you'll want your changes to take effect. To reload without restarting the server and disconnecting clients, do:

```
> nats-server --signal reload
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/username_password.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
