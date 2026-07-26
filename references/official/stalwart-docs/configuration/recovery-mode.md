---
title: "Recovery mode"
source: "https://stalw.art/docs/configuration/recovery-mode"
source_format: official-html-to-markdown
language: "en"
doc_slug: "configuration/recovery-mode"
category: "configuration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Recovery mode"
---
# Recovery mode

      

Recovery mode is intended for situations where a running deployment requires maintenance and the normal services must not be active. Typical reasons include a misconfigured external directory that is preventing any administrator from signing in, a faulty setting that is causing the MTA to loop, a pre-deployment provisioning step where a large batch of objects must be loaded before any mail traffic reaches the server, or a freshly [[configuration/bootstrap-mode|bootstrapped]] installation where HTTPS on the configured hostname is not yet reachable and the WebUI must remain available over HTTP to finish the setup.

In this mode Stalwart starts with every background service disabled. The MTA does not accept or process messages, task workers do not run, cluster services do not join the coordinator, and scheduled jobs remain paused. The only active listener is the HTTP management endpoint on port `8080` (configurable with `STALWART_RECOVERY_MODE_PORT`), which serves the WebUI and the full JMAP management API. From there, administrators can use the WebUI or the [[management/cli|CLI]] to inspect the configuration, correct the problem, and then restart the server without the recovery flag to return it to normal operation.

Stalwart never enters recovery mode automatically. It is activated by setting the environment variable `STALWART_RECOVERY_MODE` to `1` or `true` before starting the server. When the variable is unset, or set to any other value, Stalwart starts normally.

## Recovery administrator

[Section titled “Recovery administrator”](#recovery-administrator)

Recovery mode still requires a valid sign-in. If the regular administrator account is locked out, or if the external directory that normally handles authentication is unreachable, the ordinary credentials will not work. The recovery administrator is a built-in account that bypasses the directory. It is honoured whenever `STALWART_RECOVERY_ADMIN` is set in the environment, including while the server is running normally, so emergency tasks such as promoting a directory account to administrator can be performed without taking services offline; [[configuration/bootstrap-mode|bootstrap]] and recovery mode also rely on the same variable when the directory is not reachable.

The recovery administrator is configured with the `STALWART_RECOVERY_ADMIN` environment variable, in the form `username:password`. For example:

Terminal window

```
STALWART_RECOVERY_ADMIN=admin:correcthorsebatterystaple
```

The same variable is used during bootstrap to pin a credential instead of relying on the randomly generated one, and it serves as the mechanism for regaining access after an incident.