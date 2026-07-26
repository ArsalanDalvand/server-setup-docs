---
title: "Certificates"
source: "https://stalw.art/docs/server/tls/certificates"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/tls/certificates"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Certificates"
---
# Certificates

      

When TLS is terminated on the server with manually provided certificates, Stalwart parses each certificate at load time and extracts its Subject Alternative Names. The extracted names drive certificate selection during the TLS handshake: the server matches the hostname from the client’s Server Name Indication (SNI) extension against the stored SAN list and presents the matching certificate. SNI is what makes it possible to host several domains on a single IP address and still serve the correct certificate for each connection.

For clients that do not send an SNI value, the server falls back to a single default certificate. This default is selected globally by [[ref/object/system-settings#defaultcertificateid|`defaultCertificateId`]] on the [[ref/object/system-settings|SystemSettings]] singleton, which points at one of the configured Certificate records.

## Configuration

[Section titled “Configuration”](#configuration)

Certificates are stored as [[ref/object/certificate|Certificate]] objects (found in the WebUI under  Settings ›  TLS › Certificates). Each record carries:

[[ref/object/certificate#certificate|`certificate`]]: the PEM-encoded certificate chain. Accepts direct text, an environment-variable reference, or a file reference.
[[ref/object/certificate#privatekey|`privateKey`]]: the PEM-encoded private key. A secret value; typically loaded from a file rather than stored inline.
[[ref/object/certificate#subjectalternativenames|`subjectAlternativeNames`]]: the SAN list parsed from the certificate. Server-set, so manual entry is not needed.
[[ref/object/certificate#notvalidbefore|`notValidBefore`]] / [[ref/object/certificate#notvalidafter|`notValidAfter`]]: the certificate’s validity window. Server-set.
[[ref/object/certificate#issuer|`issuer`]]: the issuing certificate authority. Server-set.

For example, a certificate pasted inline with its private key read from a file on disk:

```
{  "certificate": {    "@type": "Text",    "value": "-----BEGIN CERTIFICATE-----\nMIIFCTCCAvGgAwIBAgIUCgHGQYUqtelbHGVSzCVwBL3fyEUwDQYJKoZIhvcNAQEL\n...\n0fR8+xz9kDLf8xupV+X9heyFGHSyYU2Lveaevtr2Ij3weLRgJ6LbNALoeKXk\n-----END CERTIFICATE-----\n"  },  "privateKey": {    "@type": "File",    "filePath": "/opt/stalwart-smtp/etc/private/tls.key"  }}
```

## Reloading certificates

[Section titled “Reloading certificates”](#reloading-certificates)

When a certificate file is rotated on disk, the running server needs to be told to re-read it. This can be triggered without stopping the server from the [[management/webui|WebUI]] or the [[management/cli|CLI]].