---
title: "FAQ"
source: "https://stalw.art/docs/faq"
source_format: official-html-to-markdown
language: "en"
doc_slug: "faq"
category: "stalwart"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "FAQ"
---
# FAQ

      

## General

[Section titled “General”](#general)

### Can Stalwart be used in production?

[Section titled “Can Stalwart be used in production?”](#can-stalwart-be-used-in-production)

Stalwart is currently being utilized in various production systems globally. To date, there have been no reports of data loss or crashes attributed to Stalwart. This track record speaks to the stability and reliability of Stalwart as a production-level mail server solution.

Users who are considering Stalwart for their production environments are encouraged to join our Discord channel or Reddit. Here, they can directly engage with other users who are actively running Stalwart in their production environments. This community-driven approach provides real-world insights and experiences, offering valuable perspectives on the effectiveness and reliability of Stalwart in diverse operational scenarios.

However, it’s important to note that Stalwart is currently in version 0.x. As with any software in its early stages, certain aspects are still being refined and optimized. Before reaching the milestone of version 1.0, there may be changes to Stalwart’s data layout and configuration files. These changes could necessitate data migration or adjustments in setup and users should be prepared for these potential updates and the accompanying requirements.

### Is my data safe?

[Section titled “Is my data safe?”](#is-my-data-safe)

Stalwart is built upon well-established and proven database systems such as RocksDB, FoundationDB and PostgreSQL. The use of these trusted database systems ensures that Stalwart has a strong and reliable foundation for data storage and management. These databases are widely recognized for their stability, security, and performance, contributing significantly to the overall safety of data within Stalwart.

In addition to this, before each release, Stalwart undergoes extensive stress testing. This testing is crucial to identify and resolve any issues related to concurrency or other potential problems that could lead to data loss. The stress tests simulate various operational conditions, including high-load scenarios and concurrent access, to ensure that Stalwart can handle real-world demands without compromising data integrity or system stability.

### Where is my data stored?

[Section titled “Where is my data stored?”](#where-is-my-data-stored)

This depends on storage backends you are using. Please refer to the [[storage|storage documentation]] for detailed information.

### Can it handle large volumes of users and emails?

[Section titled “Can it handle large volumes of users and emails?”](#can-it-handle-large-volumes-of-users-and-emails)

Yes. When using [[storage/backends/foundationdb|FoundationDB]] as the backend, Stalwart can scale to support millions of users without sacrificing performance.

### Does it have a web interface?

[Section titled “Does it have a web interface?”](#does-it-have-a-web-interface)

Yes, Stalwart includes a web interface that allows users to manage their accounts, settings, and preferences. The web interface is accessible via a web browser and provides a user-friendly way to interact with the mail server.

## Management

[Section titled “Management”](#management)

### How do I create and manage users?

[Section titled “How do I create and manage users?”](#how-do-i-create-and-manage-users)

It depends on the directory backend you are using. If you are using the [[auth/backend/internal|internal directory]], you can create and manage users using the [[management/webui|web-admin]] interface. If you are using an external directory such as [[auth/backend/ldap|LDAP]] or [[auth/backend/sql|SQL]], you will need to use the tools provided by that directory server.

### How do I add a new domain?

[Section titled “How do I add a new domain?”](#how-do-i-add-a-new-domain)

Domains are managed independently of the directory backend. A new domain is created from the [[management/webui|web-admin]] interface (Management › Domains › Domains), which stores it as a [[ref/object/domain|Domain]] object and exposes the per-domain settings for recipient resolution, DKIM signing, TLS certificate issuance, and DNS record publication. The Domain record is required even when accounts are stored in an external [[auth/backend/ldap|LDAP]] or [[auth/backend/sql|SQL]] directory; those backends provide account authentication only.

Sending mail from a new domain does not require any additional configuration. To improve deliverability, a new DKIM key should be [[mta/authentication/dkim/sign#generating-keys|generated]], [[mta/authentication/dkim/sign#publishing-dkim-keys|published in DNS]], and [[mta/authentication/dkim/sign#multiple-domains|enabled for signing]] on the new domain.

### How can I migrate from another server?

[Section titled “How can I migrate from another server?”](#how-can-i-migrate-from-another-server)

Stalwart includes a command line interface to facilitate [[management/maintenance/migration|data migration]] from a previous version of the server or from third-party servers.

### How do I backup my data?

[Section titled “How do I backup my data?”](#how-do-i-backup-my-data)

The backup procedure depends on the storage backend in use. For external databases such as PostgreSQL, MySQL, or FoundationDB, follow the backup instructions provided by the respective database. For embedded databases such as RocksDB or SQLite, the `/var/lib/stalwart` directory can be copied to create a full backup.

## E-mail

[Section titled “E-mail”](#e-mail)

### How is anti-spam handled?

[Section titled “How is anti-spam handled?”](#how-is-anti-spam-handled)

Stalwart includes its own built-in spam filter that uses the FTRL-Proximal algorithm and feature hashing. It also supports DNS Blocklists (DNSBLs) and collaborative digest-based spam filtering tools like Pyzor. Additionally, Stalwart can easily integrate with popular anti-spam solutions such as SpamAssassin and RSPAMD using [[mta/filter/milter|milter]] as well as other [[mta/filter|filtering mechanisms]].

### Does it support relay hosts?

[Section titled “Does it support relay hosts?”](#does-it-support-relay-hosts)

Yes, [[mta/outbound/routing#relay-host|relay hosts]] are supported for sending emails to external domains.

### Is greylisting supported?

[Section titled “Is greylisting supported?”](#is-greylisting-supported)

Yes, greylisting can be implemented as a Sieve filter. You can find an example [[sieve|here]].

## Encryption

[Section titled “Encryption”](#encryption)

### What is encryption at rest?

[Section titled “What is encryption at rest?”](#what-is-encryption-at-rest)

Encryption at rest refers to the process of encrypting data when it is stored, or “at rest”, on the disk. In Stalwart, this feature provides the ability to automatically encrypt plain-text messages using either PGP or S/MIME before they are written to disk.

### Is encryption at rest enabled by default?

[Section titled “Is encryption at rest enabled by default?”](#is-encryption-at-rest-enabled-by-default)

Yes, encryption at rest is enabled by default and is activated when the user uploads their S/MIME certificate or PGP public key. However, it can be disabled by setting the `email.encryption.enable` attribute to `false`.

### How do I enable or disable encryption at rest?

[Section titled “How do I enable or disable encryption at rest?”](#how-do-i-enable-or-disable-encryption-at-rest)

Users can enable or disable encryption at rest using a web interface that is available under “/crypto” on the same port where the JMAP server is listening. The interface asks for the login name, password, encryption method to use (S/MIME or PGP), and to select the file containing the certificates or public keys. To disable encryption, select the “Disable Encryption” option from the dropdown and then click “Update”.

### What happens when encryption at rest is enabled?

[Section titled “What happens when encryption at rest is enabled?”](#what-happens-when-encryption-at-rest-is-enabled)

Once encryption at rest is enabled, all incoming emails are automatically encrypted before they are written to disk. Not even the system administrator is able to decrypt these messages as they are encrypted with the PGP public key or S/MIME certificate provided by the user.

### What encryption methods are supported?

[Section titled “What encryption methods are supported?”](#what-encryption-methods-are-supported)

Stalwart supports two encryption methods: PGP (using either AES256 or AES128 symmetric encryption) and S/MIME (using either AES256-CBC or AES128-CBC symmetric encryption).

### What format should the S/MIME certificates or PGP keys be in when uploaded?

[Section titled “What format should the S/MIME certificates or PGP keys be in when uploaded?”](#what-format-should-the-smime-certificates-or-pgp-keys-be-in-when-uploaded)

S/MIME certificates can be imported in DER or PEM formats. However, if importing multiple certificates, they must be in PEM format. PGP public keys can be imported in raw format or as an ASCII-armored text file containing one or multiple public keys.

### I have enabled encryption at rest. How can I test if it’s working?

[Section titled “I have enabled encryption at rest. How can I test if it’s working?”](#i-have-enabled-encryption-at-rest-how-can-i-test-if-its-working)

Once you have enabled encryption at rest, it is recommended that you send a test email to yourself. This allows you to confirm that your email client can properly decrypt the message.

## Spam & Phishing Filter

[Section titled “Spam & Phishing Filter”](#spam--phishing-filter)

### What makes Stalwart’s Spam filter different from other solutions?

[Section titled “What makes Stalwart’s Spam filter different from other solutions?”](#what-makes-stalwarts-spam-filter-different-from-other-solutions)

Stalwart’s spam filter is built-in and doesn’t require any third-party software. It’s designed for speed and efficiency, keeping messages within the server during the filtering process. It offers filtering rules on par with popular solutions like RSpamd and SpamAssassin.

### How does the spam classifier work?

[Section titled “How does the spam classifier work?”](#how-does-the-spam-classifier-work)

Our spam classifier uses the FTRL-Proximal algorithm and feature hashing. It automatically trains itself, ensuring improved accuracy over time.

### How does Stalwart handle phishing attempts?

[Section titled “How does Stalwart handle phishing attempts?”](#how-does-stalwart-handle-phishing-attempts)

Stalwart provides phishing protection by checking against databases like OpenPhish and PhishTank, identifying homographic URL attacks, sender spoofing, and other deceptive techniques. It also detects and flags suspicious URL patterns.

### What are DNSBLs, and why are they important?

[Section titled “What are DNSBLs, and why are they important?”](#what-are-dnsbls-and-why-are-they-important)

DNS Blocklists (DNSBLs) are databases that track IP addresses known for sending spam. Stalwart checks IP addresses, domains, and even hashes against several DNSBLs to filter out potential spam sources.

### How can I customize the spam filter settings?

[Section titled “How can I customize the spam filter settings?”](#how-can-i-customize-the-spam-filter-settings)

Administrators can easily customize settings from the [[management/webui|web-admin]] or, for advanced users, the `spam-filter` Sieve script.

### What is greylisting, and how does it help?

[Section titled “What is greylisting, and how does it help?”](#what-is-greylisting-and-how-does-it-help)

Greylisting temporarily defers emails from unknown senders. If the sender is legitimate, they’ll typically resend the email after a short delay, whereas most spammers won’t. This process helps in reducing spam.

### How can I set up spam traps?

[Section titled “How can I set up spam traps?”](#how-can-i-set-up-spam-traps)

You can set up decoy email addresses to catch and analyze spam. Any messages sent to these addresses are considered spam. The list of spamtrap email addresses can be managed from the [[management/webui|web-admin]].

### I’ve heard about sender reputation tracking. How does that work in Stalwart?

[Section titled “I’ve heard about sender reputation tracking. How does that work in Stalwart?”](#ive-heard-about-sender-reputation-tracking-how-does-that-work-in-stalwart)

Stalwart tracks the previous spam scores of senders based on their IP address, ASN, domain, and email address. It uses this data to adjust the spam score of incoming messages, helping in more accurate spam identification.

### Does Stalwart support collaborative spam filtering mechanisms like Pyzor?

[Section titled “Does Stalwart support collaborative spam filtering mechanisms like Pyzor?”](#does-stalwart-support-collaborative-spam-filtering-mechanisms-like-pyzor)

Yes! Stalwart integrates with collaborative digest-based spam filtering tools like Pyzor to enhance its spam detection capabilities.

## Setup & WebUI

[Section titled “Setup & WebUI”](#setup--webui)

### Why does /admin return 404 Not Found?

[Section titled “Why does /admin return 404 Not Found?”](#why-does-admin-return-404-not-found)

The WebUI is delivered as a downloadable [[management/applications|Application]] bundle that the server fetches from `https://github.com/stalwartlabs/webui/releases/latest/` on first start, then refreshes on a schedule. When that initial download fails (no outbound HTTPS, GitHub blocked, restrictive proxy), the bundle is never unpacked and every WebUI route returns 404. Verify the host can reach `github.com` and `objects.githubusercontent.com` on TCP/443. For air-gapped deployments, host the bundle on an internal HTTPS server and update the [[ref/object/application#resourceurl|`resourceUrl`]] field on the WebUI’s [[ref/object/application|Application]] record to point at it. After one successful download, subsequent failures are non-fatal: the previously installed bundle keeps serving until the next refresh succeeds.

### Why does the root URL / return 404?

[Section titled “Why does the root URL / return 404?”](#why-does-the-root-url--return-404)

There is no page mounted at the root. The WebUI lives at `/admin`, the per-user portal at `/account`, and the JMAP endpoint at `/jmap`. Configure the reverse proxy or browser bookmark to point at `/admin` directly, or add a redirect from `/`.

### Why is sign-in stuck in a loop or showing a basic-auth dialog?

[Section titled “Why is sign-in stuck in a loop or showing a basic-auth dialog?”](#why-is-sign-in-stuck-in-a-loop-or-showing-a-basic-auth-dialog)

Almost always one of the following:

The browser is on `http://` while the configured hostname is announced over `https://`. Open the page over HTTPS, or use `http://<host>:8080/admin` while TLS is being put in place.
The browser cached an earlier visit on the wrong scheme. Clear the site’s history or use a private window.
A reverse proxy is terminating HTTPS but talking HTTP upstream and the discovery URLs end up pointing at the wrong scheme or port. See [[server/reverse-proxy|Reverse proxy › Overview]] for the supported patterns and the [[configuration/environment-variables#public-urls|`STALWART_PUBLIC_URL`]] variable.

### Can I reach the WebUI by IP address?

[Section titled “Can I reach the WebUI by IP address?”](#can-i-reach-the-webui-by-ip-address)

The OAuth flow used by the WebUI binds to the configured `defaultHostname` over HTTPS. Loading the WebUI by IP, by container name, or over plain HTTP appears to load the page but fails at the OAuth callback. Use the configured hostname over HTTPS, or sign in at `http://<host>:8080/admin` (which is exempt from the hostname check) until TLS is in place.

### No log files appear under /var/log/stalwart. What’s wrong?

[Section titled “No log files appear under /var/log/stalwart. What’s wrong?”](#no-log-files-appear-under-varlogstalwart-whats-wrong)

Three causes, in descending order of frequency:

The directory does not exist or is not writable by the service user. On the binary install, `/var/log/stalwart` is created at install time and owned by `stalwart:stalwart`. On Docker, mount a host directory and `chown 2000:2000 <dir>` before starting the container.
The configured tracer points at Console (intentional in containers, so logs surface through the orchestrator’s log driver). Inspect Telemetry → Tracers in the WebUI to confirm the destination.
The tracer’s path was misconfigured. Reset by deleting the tracer and recreating it from defaults at Telemetry → Tracers → New.

### My reverse proxy’s IP keeps getting banned. What do I do?

[Section titled “My reverse proxy’s IP keeps getting banned. What do I do?”](#my-reverse-proxys-ip-keeps-getting-banned-what-do-i-do)

When the proxy does not forward the real client IP, every request appears to come from the proxy and Stalwart’s automatic banning then locks the proxy out, which in turn locks every user out. Either mark the proxy’s IP as a trusted network so banning is skipped for requests from that IP and configure the proxy to forward the real client IP via PROXY protocol or `X-Forwarded-For` (then enable the matching reader on the listener), or raise the auto-ban threshold for the HTTP listener. See [[server/reverse-proxy/proxy-protocol|Proxy Protocol]] for the recommended pattern.

### What outbound HTTPS does Stalwart need?

[Section titled “What outbound HTTPS does Stalwart need?”](#what-outbound-https-does-stalwart-need)

A fresh install reaches out to:

`github.com` and `objects.githubusercontent.com`: for the WebUI bundle on first start and on the periodic refresh, and for the spam-filter rule list and Apple Push Notification (APN) endpoint list, both fetched from the same release storage.
`acme-v02.api.letsencrypt.org` (or whichever ACME directory is configured): for TLS certificate issuance.
The DNS provider’s API: when automatic DNS management is enabled.
The OIDC provider’s well-known and userinfo endpoints: when an OIDC directory is configured.

In restricted-egress environments, allow at minimum the GitHub hosts above; otherwise the WebUI never appears, the spam classifier runs without its fetched rule lists, and APN-based push notifications stop working. See [[management/webui#outbound-network-requirement|WebUI overview]] for the full list and for hosting the bundle internally.