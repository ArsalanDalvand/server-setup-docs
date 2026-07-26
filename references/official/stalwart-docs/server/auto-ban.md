---
title: "Auto-banning"
source: "https://stalw.art/docs/server/auto-ban"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/auto-ban"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Auto-banning"
---
# Auto-banning

      

Stalwart includes an automatic ban mechanism that monitors incoming connections for abusive patterns and blocks the offending IP address when configured thresholds are exceeded. Once an address is banned, the server drops every subsequent connection from that source, shutting down further abuse without operator intervention.

The system tracks several independent categories of suspicious activity: failed authentication attempts, SMTP relay or recipient-probing abuse, idle (loitering) sessions, and port or URL scanning. Each category has its own rate limit, and each rate is evaluated separately; reaching the threshold for any category produces a ban. Banned IP addresses are recorded as [[ref/object/blocked-ip|BlockedIp]] entries (found in the WebUI under  Settings ›  Security › Blocked IPs), alongside any entries created manually from the [[management/webui|WebUI]] or the [[management/cli|CLI]].

Authentication ban tracking keys on both the source IP and the login name, so distributed brute-force attempts against a single account are detected even when the attacker rotates IP addresses.

## Configuration

[Section titled “Configuration”](#configuration)

Auto-ban thresholds are grouped on the [[ref/object/security|Security]] singleton (found in the WebUI under  Settings ›  Security › Settings). Each category exposes a `*BanRate` field (count over period) and an optional `*BanPeriod` duration that sets how long resulting bans last; omitting `*BanPeriod` leaves the ban in place until it is removed manually. Leaving `scanBanPaths` at its default covers common exploit URLs (`*.php*`, `*.cgi*`, `*/wp-*`, `*xmlrpc*`, traversal patterns, and the names of popular CMS platforms).

Every resulting BlockedIp carries a [[ref/object/blocked-ip#reason|`reason`]] recording what triggered the ban (one of `authFailure`, `rcptToFailure`, `loitering`, `portScanning`, `manual`, or `other`) and an [[ref/object/blocked-ip#expiresat|`expiresAt`]] timestamp. When a category’s `*BanPeriod` is set, `expiresAt` is populated with the corresponding deadline and the entry is removed automatically once the time passes; leaving the period unset stores the ban indefinitely until an operator deletes it. Manual entries created through the WebUI or the CLI carry [[ref/object/blocked-ip#reason|`reason`]] `manual` and can set their own [[ref/object/blocked-ip#expiresat|`expiresAt`]] at creation time.

### Authentication failures

[Section titled “Authentication failures”](#authentication-failures)

Authentication failures across every service (JMAP, IMAP, SMTP, ManageSieve) feed into a single counter per source. The rate is set through [[ref/object/security#authbanrate|`authBanRate`]] with a default of `100` failures per day, and the resulting ban duration is controlled by [[ref/object/security#authbanperiod|`authBanPeriod`]].

### Abuse protection

[Section titled “Abuse protection”](#abuse-protection)

Relay attempts and brute-force RCPT TO probing against the SMTP server are tracked together. The rate is set through [[ref/object/security#abusebanrate|`abuseBanRate`]] with a default of `35` attempts per day, and the ban duration through [[ref/object/security#abusebanperiod|`abuseBanPeriod`]].

### Loitering connections

[Section titled “Loitering connections”](#loitering-connections)

Clients that repeatedly connect but never send meaningful traffic consume resources and are often part of SYN-flood-style denial-of-service attempts. The rate is set through [[ref/object/security#loiterbanrate|`loiterBanRate`]] with a default of `150` disconnections per day, and the ban duration through [[ref/object/security#loiterbanperiod|`loiterBanPeriod`]].

### Port and URL scanning

[Section titled “Port and URL scanning”](#port-and-url-scanning)

The server tracks attempts to probe TCP ports it is not listening on, as well as HTTP requests for exploit-style paths. The scan rate is set through [[ref/object/security#scanbanrate|`scanBanRate`]] (default `30` attempts per day), the ban duration through [[ref/object/security#scanbanperiod|`scanBanPeriod`]], and the list of glob patterns that trigger an immediate ban on first HTTP request through [[ref/object/security#scanbanpaths|`scanBanPaths`]]. The default patterns cover the most common web-application exploits:

```
{  "scanBanRate": {"count": 30, "period": "1d"},  "scanBanPaths": [    "*.php*",    "*.cgi*",    "*.asp*",    "*/wp-*",    "*/php*",    "*/cgi-bin*",    "*xmlrpc*",    "*../*",    "*/..*",    "*joomla*",    "*wordpress*",    "*drupal*"  ]}
```