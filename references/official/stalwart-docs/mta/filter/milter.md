---
title: "Milter filters"
source: "https://stalw.art/docs/mta/filter/milter"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/filter/milter"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Milter filters"
---
# Milter filters

      

Milter, short for “mail filter”, is an extension to mail servers based on the Sendmail protocol. Milters allow third-party software to access mail messages as they are being processed in order to filter, modify, or annotate them. Through milter, an MTA can add functionality such as spam filtering, virus scanning, and other kinds of message processing. Milters operate at the SMTP protocol level, so they have access to both the SMTP envelope and the message contents.

When a message reaches the [[mta/inbound/data|DATA stage]] (or any earlier configured stage), Stalwart calls the configured milter filters. Each filter can inspect and potentially modify the message, adding, changing, or removing headers, altering the body, or rejecting it outright. The modifications requested by each milter are merged. Once all milters have been processed, the message enters the queue and proceeds through the rest of the delivery process.

## Configuration

[Section titled “Configuration”](#configuration)

Each milter is defined as an [[ref/object/mta-milter|MtaMilter]] object (found in the WebUI under  Settings ›  MTA › Filters › Milters). The relevant fields are:

[[ref/object/mta-milter#enable|`enable`]]: expression that determines whether this milter is active. Defaults to `true`, but can be set to conditionally enable a milter based on session state (for example only for the SMTP listener).
[[ref/object/mta-milter#hostname|`hostname`]]: hostname or IP address of the milter server.
[[ref/object/mta-milter#port|`port`]]: network port on the milter server. Default 11332.
[[ref/object/mta-milter#stages|`stages`]]: list of SMTP stages at which this milter is invoked. Possible values are `connect`, `ehlo`, `auth`, `mail`, `rcpt`, and `data`. Default `["data"]`.
[[ref/object/mta-milter#usetls|`useTls`]]: whether to use TLS for the connection. Default `false`, since milters usually run on the same host as the MTA.
[[ref/object/mta-milter#allowinvalidcerts|`allowInvalidCerts`]]: whether Stalwart should connect to a milter that presents an invalid TLS certificate. Default `false`.

A typical deployment runs Rspamd and ClamAV as separate milters. For example, filtering messages received on the SMTP listener through both, each running on the same host on a distinct port:

```
[  {    "hostname": "127.0.0.1",    "port": 11332,    "stages": ["connect", "ehlo", "mail", "rcpt", "data"],    "enable": {      "match": [{"if": "listener == 'smtp'", "then": "true"}],      "else": "false"    },    "useTls": false,    "allowInvalidCerts": false  },  {    "hostname": "127.0.0.1",    "port": 15112,    "stages": ["data"],    "enable": {      "match": [{"if": "listener == 'smtp'", "then": "true"}],      "else": "false"    },    "useTls": false,    "allowInvalidCerts": false  }]
```

### Timeouts

[Section titled “Timeouts”](#timeouts)

Timeouts for milter communication are set with [[ref/object/mta-milter#timeoutconnect|`timeoutConnect`]] (maximum time to establish a connection with the milter, default 30 seconds), [[ref/object/mta-milter#timeoutcommand|`timeoutCommand`]] (maximum time for a command sent to the milter, default 30 seconds), and [[ref/object/mta-milter#timeoutdata|`timeoutData`]] (maximum time to wait for a response, default 60 seconds):

```
{  "timeoutConnect": "30s",  "timeoutCommand": "30s",  "timeoutData": "60s"}
```

### Options

[Section titled “Options”](#options)

Additional options include [[ref/object/mta-milter#tempfailonerror|`tempFailOnError`]] (whether to respond with a temporary failure, typically 4xx, when an error occurs while talking to the milter so that the sender retries later, default `true`), [[ref/object/mta-milter#maxresponsesize|`maxResponseSize`]] (maximum response size accepted from the milter, in bytes, default 52428800, 50 MB), [[ref/object/mta-milter#protocolversion|`protocolVersion`]] (milter protocol version to use, `v2` or `v6`, default `v6`), and [[ref/object/mta-milter#flagsaction|`flagsAction`]] / [[ref/object/mta-milter#flagsprotocol|`flagsProtocol`]] (optional action and protocol flags advertised during option negotiation):

```
{  "tempFailOnError": true,  "maxResponseSize": 52428800,  "protocolVersion": "v6",  "flagsAction": 255,  "flagsProtocol": 66}
```

## Development

[Section titled “Development”](#development)

The following sections are intended for developers creating milter filters to interact with Stalwart. They describe the actions Stalwart can take in response to instructions from a milter, and the macros Stalwart uses to communicate session or message information to the milter. General users of Stalwart can skip this section.

### Supported actions

[Section titled “Supported actions”](#supported-actions)

Stalwart supports the following actions and modifications that can be requested by a milter filter:

CodeNameDescriptionSMFIR_ADDRCPT (`+`)Add RecipientAdds a recipient to the emailSMFIR_DELRCPT (`-`)Delete RecipientRemoves a recipient from the emailSMFIR_ADDRCPT_PAR (`2`)Add Recipient (with ESMTP args)Adds a recipient to the email, including ESMTP argumentsSMFIR_SHUTDOWN (`4`)ShutdownReturn a 421 shutdown codeSMFIR_ACCEPT (`a`)AcceptAccepts the emailSMFIR_REPLBODY (`b`)Replace BodyReplaces the body of the emailSMFIR_CONTINUE (`c`)ContinueContinues with the emailSMFIR_DISCARD (`d`)DiscardDiscards the emailSMFIR_CHGFROM (`e`)Change FromChanges the envelope sender of the emailSMFIR_CONN_FAIL (`f`)Connection FailureCauses a connection failureSMFIR_ADDHEADER (`h`)Add HeaderAdds a header to the emailSMFIR_INSHEADER (`i`)Insert HeaderInserts a header in the emailSMFIR_SETSYMLIST (`l`)Set Symbol ListSets a list of symbols or macrosSMFIR_CHGHEADER (`m`)Change HeaderChanges a header in the emailSMFIR_PROGRESS (`p`)ProgressIndicates progressSMFIR_QUARANTINE (`q`)QuarantineQuarantines the emailSMFIR_REJECT (`r`)RejectRejects the emailSMFIR_SKIP (`s`)SkipSkip rest of body, send EOBSMFIR_TEMPFAIL (`t`)Temporary FailureReturn a temporary failureSMFIR_REPLYCODE (`y`)Reply CodeReply with a given SMTP reply code

### Supported macros

[Section titled “Supported macros”](#supported-macros)

Macros are variables used to communicate specific session or message information between the MTA and the milter. When the MTA starts a milter, it sends an initial set of predefined macros. The specific macros sent change throughout the SMTP transaction, providing different context at each stage (connection, helo, mail, rcpt, data, end-of-header, and end-of-message).

Stalwart supports the following macros:

Macro NameExplanation`i`Queue ID`j`Local hostname`_`Validated client name`{auth_authen}`SASL login name`{auth_author}`SASL sender`{auth_type}`SASL method`{client_addr}`Client address`{client_connections}`Number of client connections`{client_name}`Client name`{client_port}`Client port`{client_ptr}`Client pointer`{cert_issuer}`Certificate issuer`{cert_subject}`Certificate subject`{cipher_bits}`Cipher bits`{cipher}`Cipher`{daemon_addr}`Daemon address`{daemon_name}`Daemon name`{daemon_port}`Daemon port`{mail_addr}`Mail address`{mail_host}`Mail host address`{mail_mailer}`Mail mailer`{rcpt_addr}`Recipient address`{rcpt_host}`Recipient host`{rcpt_mailer}`Recipient mailer`{tls_version}`TLS version`{v}`Version