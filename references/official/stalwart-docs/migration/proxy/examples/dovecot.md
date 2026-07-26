---
title: "Migrating from Dovecot"
source: "https://stalw.art/docs/migration/proxy/examples/dovecot"
source_format: official-html-to-markdown
language: "en"
doc_slug: "migration/proxy/examples/dovecot"
category: "migration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Migrating from Dovecot"
---
# Migrating from Dovecot

      

This example fronts a legacy mail stack (Dovecot serving IMAP, POP3 and ManageSieve, with Postfix handling submission) and a new Stalwart server with a single proxy, migrating accounts from the legacy stack to Stalwart one at a time. Every account initially resolves to the legacy backend through the default destination, and as each mailbox is migrated it is given an explicit mapping to Stalwart. Clients keep their existing settings throughout. A ready-to-edit copy of the proxy configuration ships in the proxy repository as [`resources/dovecot-postfix-trust.conf`](https://github.com/stalwartlabs/proxy/blob/main/resources/config.dovecot-postfix.toml), and the backend changes are collected in [`resources/dovecot-postfix-trust.conf`](https://github.com/stalwartlabs/proxy/blob/main/resources/dovecot-postfix-trust.conf).

The defining characteristic of this scenario is that the legacy backend speaks the Dovecot and Postfix conventions: it expects the client’s real address to arrive through the XCLIENT command (and the IMAP ID command), not through a PROXY protocol header. The legacy destination therefore uses `forwarding = "xclient"`, while the Stalwart destination uses `forwarding = "proxy"`. The backends are reached over TLS on the classic ports, and the legacy server presents a self-signed certificate that the proxy either pins by fingerprint or, on a trusted internal network, accepts without verification.

## Proxy configuration

[Section titled “Proxy configuration”](#proxy-configuration)

```
[server]hostname = "mail.example.com"
[tls.certificate.default]cert = "/etc/proxy/tls/fullchain.pem"key = "/etc/proxy/tls/privkey.pem"default = true
[mapping]source = "file"normalize = "lowercase"
[mapping.file]path = "/etc/proxy/mappings.tsv"
[routing]default_destination = "legacy"smtp_passthrough_destination = "stalwart"
# Legacy Dovecot + Postfix backend, announced over XCLIENT and the IMAP ID command.[destination.legacy]host = "legacy.internal.example.com"tls_server_name = "legacy.internal.example.com"forwarding = "xclient"tls_allow_invalid_certs = true# Prefer pinning the legacy certificate instead of accepting any:# tls_pinned_cert_sha256 = "aa:bb:...:99"
[destination.legacy.protocol.imap]port = 143tls = "starttls"[destination.legacy.protocol.pop3]port = 110tls = "starttls"[destination.legacy.protocol.submission]port = 587tls = "starttls"[destination.legacy.protocol.managesieve]port = 4190tls = "starttls"
# New Stalwart backend, announced over the PROXY protocol.[destination.stalwart]host = "new.internal.example.com"tls_server_name = "new.internal.example.com"forwarding = "proxy"proxy_protocol = trueforwarded = true
[destination.stalwart.protocol.imap]port = 993tls = "implicit"[destination.stalwart.protocol.pop3]port = 995tls = "implicit"[destination.stalwart.protocol.submission]port = 587tls = "starttls"[destination.stalwart.protocol.managesieve]port = 4190tls = "starttls"[destination.stalwart.protocol.smtp]port = 25tls = "plain"
# Client-facing listeners on the familiar ports.[listener.imaps]protocol = "imap"bind = ["0.0.0.0:993", "[::]:993"]tls = "implicit"[listener.imap]protocol = "imap"bind = ["0.0.0.0:143", "[::]:143"]tls = "starttls"[listener.pop3s]protocol = "pop3"bind = ["0.0.0.0:995", "[::]:995"]tls = "implicit"[listener.pop3]protocol = "pop3"bind = ["0.0.0.0:110", "[::]:110"]tls = "starttls"[listener.submission]protocol = "submission"bind = ["0.0.0.0:587", "[::]:587"]tls = "starttls"[listener.submissions]protocol = "submission"bind = ["0.0.0.0:465", "[::]:465"]tls = "implicit"[listener.managesieve]protocol = "managesieve"bind = ["0.0.0.0:4190", "[::]:4190"]tls = "starttls"[listener.smtp]protocol = "smtp"bind = ["0.0.0.0:25", "[::]:25"]tls = "plain"
[admin]bind = "127.0.0.1:9443"tls = "implicit"bearer_token_file = "/etc/proxy/admin.token"
```

Dovecot and Postfix serve no HTTP or JMAP, so no HTTP listener is defined. Once accounts are on Stalwart, JMAP and the web interface are served by the new server; an HTTP listener routing to `stalwart` can be added at that point if a single hostname for web access is wanted.

## Trusting the proxy on the legacy backend

[Section titled “Trusting the proxy on the legacy backend”](#trusting-the-proxy-on-the-legacy-backend)

The XCLIENT mechanism only conveys the real client address if the legacy stack is told to trust the proxy. Without that trust, logins still succeed, but Dovecot and Postfix record the proxy’s address as the client rather than the real one. The proxy’s address is added to Dovecot’s trusted networks and to Postfix’s authorized XCLIENT hosts, scoped to the proxy alone so that other hosts cannot spoof a client address. The full set of changes is in [`resources/dovecot-postfix-trust.conf`](https://github.com/stalwartlabs/proxy/blob/main/resources/dovecot-postfix-trust.conf).

In Dovecot, in `dovecot.conf` or a file under `conf.d`:

```
login_trusted_networks = 203.0.113.10/32
```

In Postfix, applied with `postconf` or added to `main.cf`:

```
smtpd_authorized_xclient_hosts = 203.0.113.10/32
```

The address `203.0.113.10` is a placeholder and must be replaced with the proxy’s actual address. After the change, Dovecot is reloaded with `doveadm reload` and Postfix with `postfix reload`.

Whether the trust is in effect can be confirmed by signing in through the proxy and reading the Dovecot login line. The `rip=` field shows the real client address when XCLIENT is honoured, and the proxy’s address when it is not.

## Reading mailboxes during migration

[Section titled “Reading mailboxes during migration”](#reading-mailboxes-during-migration)

The migration tool, Vandelay, reads each legacy mailbox over IMAP and writes it to Stalwart. So that it can read any mailbox without each user’s password, Dovecot is given a master user, which Vandelay authenticates as on behalf of the account being migrated. The master user and the separator that joins it to the target login are configured as shown in [`resources/dovecot-postfix-trust.conf`](https://github.com/stalwartlabs/proxy/blob/main/resources/dovecot-postfix-trust.conf):

```
auth_master_user_separator = %
passdb {  driver = passwd-file  master = yes  args = scheme=PLAIN username_format=%u /etc/dovecot/master-users}
```

With a master user named `vmadmin`, Vandelay reads alice’s mailbox by authenticating as `[email protected]%vmadmin` with vmadmin’s password. Mail is read with `vandelay import imap` and sieve scripts with `vandelay import managesieve`; calendars and contacts are read with `vandelay import caldav` and `import carddav` when the deployment runs a CalDAV/CardDAV server. The archive is then written to Stalwart with `vandelay export`. Because Dovecot accepts ordinary direct connections as well as proxied ones, Vandelay can read the legacy stack directly rather than through the proxy. The complete, ordered procedure is in the [[migration/guide|migration guide]].

## Mapping file

[Section titled “Mapping file”](#mapping-file)

The mapping file starts empty, so every account falls through to `legacy`. As mailboxes are migrated to Stalwart, a line is added for each:

```
# identifier              destination[email protected]         stalwart[email protected]          stalwart
```

## Incoming mail

[Section titled “Incoming mail”](#incoming-mail)

The configuration routes incoming mail on port 25 to Stalwart through the pass-through destination, declared with `smtp_passthrough_destination = "stalwart"` and the `smtp` protocol on that destination. Stalwart performs split delivery: mail for an account already migrated is delivered locally, and mail for an account still on the legacy stack is relayed back to Postfix, which delivers it through its existing virtual transport. Because pass-through sessions are not authenticated, they are bridged as a stream and are not routed per account.

## Cutting an account over

[Section titled “Cutting an account over”](#cutting-an-account-over)

When an account’s mailbox has been copied to Stalwart, its mapping is set to `stalwart` and any live session is disconnected so that it reconnects to the new server:

Terminal window

```
TOKEN=$(cat /etc/proxy/admin.token)curl -sk -X PUT -H "Authorization: Bearer $TOKEN" \  "https://127.0.0.1:9443/[email protected]&destination=stalwart"curl -sk -X POST -H "Authorization: Bearer $TOKEN" \  "https://127.0.0.1:9443/connections/[email protected]"
```

When the mapping store is a flat file edited by hand rather than through the API, the file is reloaded and the account’s cache entry invalidated after the line is appended:

Terminal window

```
curl -sk -X POST -H "Authorization: Bearer $TOKEN" https://127.0.0.1:9443/mappings/reloadcurl -sk -X POST -H "Authorization: Bearer $TOKEN" \  "https://127.0.0.1:9443/cache/[email protected]"curl -sk -X POST -H "Authorization: Bearer $TOKEN" \  "https://127.0.0.1:9443/connections/[email protected]"
```

The end-to-end migration of an account, including copying its data, validating it and rolling it back, follows the same shape as the [[migration/guide|migration guide]], with the legacy-specific reads described above in place of the JMAP reads used between two Stalwart servers.