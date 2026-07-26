---
title: "Trusted Interpreter"
source: "https://stalw.art/docs/sieve/interpreter/trusted"
source_format: official-html-to-markdown
language: "en"
doc_slug: "sieve/interpreter/trusted"
category: "sieve"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Trusted Interpreter"
---
# Trusted Interpreter

      

The trusted interpreter runs Sieve scripts invoked by the SMTP server. These scripts are created by the system administrator and are considered privileged. Stalwart compiles all defined Sieve scripts at start-up and executes them on demand through the Sieve runtime.

## Configuration

[Section titled “Configuration”](#configuration)

Interpreter settings and resource limits are configured on the [[ref/object/sieve-system-interpreter|SieveSystemInterpreter]] singleton (found in the WebUI under  Settings ›  Sieve › System Interpreter). The main fields are:

[[ref/object/sieve-system-interpreter#defaultfromname|`defaultFromName`]]: default name used in the `From` header of email notifications sent from a Sieve script. Default `"'Automated Message'"`.
[[ref/object/sieve-system-interpreter#defaultfromaddress|`defaultFromAddress`]]: default address used in the `From` header of email notifications sent from a Sieve script. Default `"'MAILER-DAEMON@' + system('domain')"`.
[[ref/object/sieve-system-interpreter#defaultreturnpath|`defaultReturnPath`]]: default return path applied to email notifications sent from a Sieve script.
[[ref/object/sieve-system-interpreter#dkimsigndomain|`dkimSignDomain`]]: domain whose DKIM signatures are applied to email notifications sent from a Sieve script. Default `"system('domain')"`.
[[ref/object/sieve-system-interpreter#messageidhostname|`messageIdHostname`]]: local hostname used when generating the `Message-Id` header. When unset, the server hostname is used.
[[ref/object/sieve-system-interpreter#nocapabilitycheck|`noCapabilityCheck`]]: when `true`, language extensions can be used without being declared through a `require` statement. Default `true`.

The [[ref/object/sieve-system-interpreter#dkimsigndomain|`dkimSignDomain`]] expression resolves to a single domain name; all DKIM signatures associated with that domain are then applied to outgoing notifications. A domain can have one or multiple DKIM signatures associated through its [[ref/object/dkim-signature|DkimSignature]] records.

### Limits

[Section titled “Limits”](#limits)

Resource limits protect the server from scripts that exceed reasonable bounds. The relevant fields on SieveSystemInterpreter are:

[[ref/object/sieve-system-interpreter#maxredirects|`maxRedirects`]]: maximum number of `redirect` commands per script. Default `3`.
[[ref/object/sieve-system-interpreter#maxoutmessages|`maxOutMessages`]]: maximum number of outgoing messages a script may send. Default `5`.
[[ref/object/sieve-system-interpreter#maxreceivedheaders|`maxReceivedHeaders`]]: maximum number of `Received` headers allowed in a message. Default `50`.
[[ref/object/sieve-system-interpreter#maxcpucycles|`maxCpuCycles`]]: maximum number of instructions a script can execute. Default `1048576`.
[[ref/object/sieve-system-interpreter#maxnestedincludes|`maxNestedIncludes`]]: maximum number of nested `include` instructions. Default `5`.
[[ref/object/sieve-system-interpreter#duplicateexpiry|`duplicateExpiry`]]: default expiration time for identifiers stored by the `duplicate` extension. Default `"7d"`.
[[ref/object/sieve-system-interpreter#maxvarsize|`maxVarSize`]]: maximum size of a variable, in bytes. Default `52428800`.

### Example

[Section titled “Example”](#example)

```
{  "defaultFromName": {"else": "'Automated Message'"},  "defaultFromAddress": {"else": "'[email protected]'"},  "defaultReturnPath": {"else": "''"},  "messageIdHostname": "mx.example.org",  "dkimSignDomain": {"else": "system('domain')"},  "maxRedirects": 3,  "maxOutMessages": 5,  "maxReceivedHeaders": 50,  "maxCpuCycles": 10000,  "maxNestedIncludes": 5,  "duplicateExpiry": "7d"}
```

## Scripts

[Section titled “Scripts”](#scripts)

Trusted Sieve scripts are stored as [[ref/object/sieve-system-script|SieveSystemScript]] records (found in the WebUI under  Settings ›  Sieve › System Scripts). Each record carries a [[ref/object/sieve-system-script#name|`name`]], an optional [[ref/object/sieve-system-script#description|`description`]], an [[ref/object/sieve-system-script#isactive|`isActive`]] flag, and the script body in [[ref/object/sieve-system-script#contents|`contents`]].

A trusted script is invoked from an SMTP stage by setting the stage’s `script` expression to the script’s name. For example, the `script` field on [[ref/object/mta-stage-rcpt|MtaStageRcpt]], [[ref/object/mta-stage-ehlo|MtaStageEhlo]], or [[ref/object/mta-stage-data|MtaStageData]]. Trusted scripts can also import each other using the `include` command.

For example, a system script that rejects messages from blocklisted HELO domains:

```
{  "name": "script_one",  "description": "Reject blocklisted HELO domains",  "isActive": true,  "contents": "require [\"variables\", \"extlists\", \"reject\"];\n\nif string :list \"${env.helo_domain}\" \"list/blocked-domains\" {\n    reject \"551 5.1.1 Your domain '${env.helo_domain}' has been blocklisted.\";\n}\n"}
```

The [[ref/object/sieve-system-script#contents|`contents`]] field holds the full script text; loading the script body from an external file is not supported.