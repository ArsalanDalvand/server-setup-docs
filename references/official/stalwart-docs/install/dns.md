---
title: "Setting up DNS"
source: "https://stalw.art/docs/install/dns"
source_format: official-html-to-markdown
language: "en"
doc_slug: "install/dns"
category: "install"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Setting up DNS"
---
# Setting up DNS

      

Correctly configured DNS records are essential for email delivery, client compatibility, and transport security. The records listed below direct incoming mail, authenticate outgoing messages, enforce TLS, advertise policy to remote senders, and allow mail clients to discover and connect to the server automatically.

## Automatic or manual publication

[Section titled “Automatic or manual publication”](#automatic-or-manual-publication)

Stalwart offers two ways to publish the records a domain needs.

The recommended path is automatic DNS management. When a domain is linked to a [[ref/object/dns-server|DnsServer]] object configured for a supported provider, Stalwart publishes the full record set against the provider’s API the first time the domain is created and keeps it in sync over time. Updates driven by events such as DKIM key rotation or a change to the MTA-STS policy propagate without manual intervention. The set of supported providers and the discovery protocol used is described under [[server/dns|DNS provider integration]]; the fields that link a specific domain to a provider are documented on [[domains/dns-records|DNS record setup]].

If the DNS provider is not supported, or when manual control over the zone is preferred, the same records can be obtained from the [[management/webui|WebUI]] under the domain’s management page, or exported as a zone file through the [[management/cli|CLI]]. The resulting records can then be pasted into the provider’s DNS control panel.

The remainder of this page describes each record type and its role. The examples assume the domain `example.org` and the host `mail.example.org`.

## Mail Exchange (MX) records

[Section titled “Mail Exchange (MX) records”](#mail-exchange-mx-records)

MX records specify which host is responsible for receiving email for a domain. These records direct incoming traffic to the mail server.

```
MX  example.org.  10 mail.example.org.
```

Email for `example.org` is directed to `mail.example.org` with a priority of `10`. When several MX records are published, the lowest priority value is tried first; higher values act as fallbacks.

## DKIM records

[Section titled “DKIM records”](#dkim-records)

[[mta/authentication/dkim|DomainKeys Identified Mail (DKIM)]] adds a digital signature to outgoing messages, proving that a message was sent by an authorised mail server for the domain and that it was not modified in transit. DKIM uses TXT records that publish the public keys used to verify those signatures.

```
TXT  202404e._domainkey.example.org.  v=DKIM1; k=ed25519; h=sha256; p=N/BkJD6xbEiMb39v7JW6AwdPHO5gKB3fcCnod4zQ31U=TXT  202404r._domainkey.example.org.  v=DKIM1; k=rsa; h=sha256; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqlddLN3BjInvBqI1KpdouG7feBsEt5t233jWQJW7FaY7sR/MfWNxuzTObLoZ3l76DFq3xPjVhmy/YYiOAnMOtq9hUFqgBVTSwUNHYPz1YUEcrI5+Ban7P7LV8kggvTAaWhAI3iSXJIFaUq78K8YYr/zrGyBlg5HCPpd+DMRAB8j1ID8bcWFaVebwAOrartXOO/f8Bn9jrRrLhjP3c8UlmkJLXkSncXPp69R9VpevrKJtpBjaFxKtx7DXGie821MHuWJ7pWMdU1Uf3z8UBKF9bnrCZ5v0SdiaFkPXR1Iiq/gR6bMwdlWvST9V6ePnqZqX+Iv4FA28byOot73/CIINFwIDAQAB
```

Each DKIM record corresponds to a cryptographic key and is referenced by a selector such as `202404e` or `202404r`. When keys are rotated, the new selector is published alongside the retiring one until the transition is complete.

## SPF records

[Section titled “SPF records”](#spf-records)

The [[mta/authentication/spf|Sender Policy Framework (SPF)]] tells receiving servers which hosts are permitted to send mail for a domain. SPF policies are published as TXT records.

```
TXT  mail.example.org.  v=spf1 a ra=postmaster -allTXT  example.org.  v=spf1 mx ra=postmaster -all
```

These policies indicate that mail may originate from the host matched by the domain’s `A` record (or MX record, for the apex domain), and that messages from any other source should be rejected.

## DMARC record

[Section titled “DMARC record”](#dmarc-record)

[[mta/authentication/dmarc|Domain-based Message Authentication, Reporting, and Conformance (DMARC)]] builds on SPF and DKIM, instructing receiving servers how to handle a message that fails authentication and where to send aggregate and forensic reports.

```
TXT  _dmarc.example.org.  v=DMARC1; p=reject; rua=mailto:[email protected]; ruf=mailto:[email protected]
```

The example tells receiving servers to reject unauthenticated messages and to deliver aggregate (`rua`) and forensic (`ruf`) reports to the postmaster address.

## MTA-STS records

[Section titled “MTA-STS records”](#mta-sts-records)

[[mta/transport-security/mta-sts|Mail Transfer Agent Strict Transport Security (MTA-STS)]] requires that inbound SMTP connections from compliant senders take place over TLS with a valid certificate matching a known host.

```
CNAME  mta-sts.example.org.  mail.example.org.TXT  _mta-sts.example.org.  v=STSv1; id=16561011793845132961
```

The CNAME record points to the host that serves the MTA-STS policy document; the TXT record carries the policy version and a stable identifier used by remote servers to detect changes.

## TLS Reporting (TLS-RPT) record

[Section titled “TLS Reporting (TLS-RPT) record”](#tls-reporting-tls-rpt-record)

[[mta/reports/tls|SMTP TLS Reporting]] lets remote servers deliver daily reports describing failed TLS negotiations to the target domain.

```
TXT  _smtp._tls.example.org.  v=TLSRPTv1; rua=mailto:[email protected]
```

The record specifies the address at which the domain is willing to receive such reports, making it easier to detect and diagnose TLS problems affecting inbound mail.

## SRV records for client services

[Section titled “SRV records for client services”](#srv-records-for-client-services)

SRV records advertise the hostnames and ports that mail clients should use for each protocol.

```
SRV  _imap._tcp.example.org.  0 1 143 mail.example.org.SRV  _imaps._tcp.example.org.  0 1 993 mail.example.org.SRV  _submissions._tcp.example.org.  0 1 465 mail.example.org.SRV  _submission._tcp.example.org.  0 1 587 mail.example.org.
```

These records let clients that support SRV-based discovery locate the server without the user entering host and port information manually.

## Autoconfiguration records

[Section titled “Autoconfiguration records”](#autoconfiguration-records)

[[server/autoconfig|Autoconfiguration]] allows a mail client to retrieve its settings from the domain without manual entry. The legacy Mozilla autoconfig and Microsoft Autodiscover formats expect to find their endpoints on dedicated subdomains, typically pointed at the mail host:

```
CNAME  autoconfig.example.org.  mail.example.org.CNAME  autodiscover.example.org.  mail.example.org.
```

The newer PACC protocol does not require its own subdomain; compliant clients fetch the configuration from `https://example.org/.well-known/user-agent-configuration.json` directly.

## Certificate Authority Authorization (CAA) records

[Section titled “Certificate Authority Authorization (CAA) records”](#certificate-authority-authorization-caa-records)

CAA records tell certificate authorities which of them are allowed to issue TLS certificates for a domain. When a CA (including those used by ACME clients) receives a certificate request, it consults the target domain’s CAA records and refuses the request if it is not listed. Publishing CAA records narrows the set of authorities that could legitimately issue a certificate for the domain, which reduces the risk of an unauthorised or mistakenly issued certificate.

```
CAA  example.org.  0 issue "letsencrypt.org"CAA  example.org.  0 iodef "mailto:[email protected]"
```

The first record authorises Let’s Encrypt to issue certificates for `example.org`. The second specifies the address at which violations or misissuance notifications can be reported. When Stalwart obtains certificates through an [[domains/tls-certificates|ACME provider]], the corresponding CAA record lists that provider’s authorisation identifier, for example `letsencrypt.org` for Let’s Encrypt. Additional `issue` entries can be added when more than one CA is used, and an `issuewild` entry can further restrict wildcard certificates.

## TLSA records (DANE)

[Section titled “TLSA records (DANE)”](#tlsa-records-dane)

[[mta/transport-security/dane|TLSA records]] are part of DNS-based Authentication of Named Entities (DANE). They cryptographically link the mail server’s TLS certificate to its DNS records, allowing remote servers to verify that the certificate presented during SMTP delivery matches the one published by the domain. Publishing TLSA records is optional but recommended for inbound transport security.

```
TLSA  _25._tcp.mail.example.org.  3 0 1 064dbc632f1ba7d0a2a3faeadeedfebd6f90f850dfb852c97ee9df9b6e5c5e7c...
```

One or more TLSA records can be published depending on the certificate management strategy in use. When keys are not rotated frequently, a single record with a usage of `3` (DANE-EE) is usually sufficient. For frequent rotations, publishing several TLSA records in parallel provides resilience and avoids service interruptions during key transitions.

Correct DNS configuration is central to secure and reliable mail server operation. When automatic DNS management is used, Stalwart keeps the full record set in sync with server state. When records are managed manually, propagation should be verified after every change using the standard DNS lookup tools.