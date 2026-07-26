---
title: "Examples"
source: "https://stalw.art/docs/sieve/examples"
source_format: official-html-to-markdown
language: "en"
doc_slug: "sieve/examples"
category: "sieve"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Examples"
---
# Examples

      

This section collects example Sieve scripts that can serve as a starting point for custom rules. Each example is stored as a [[ref/object/sieve-system-script|SieveSystemScript]] record (found in the WebUI under  Settings ›  Sieve › System Scripts) and invoked from the relevant SMTP stage by setting the stage’s `script` expression to the script’s [[ref/object/sieve-system-script#name|`name`]].

## Greylisting

[Section titled “Greylisting”](#greylisting)

The following script implements a greylisting filter backed by an SQL store. Install it as a system script and reference it from the [[ref/object/mta-stage-rcpt#script|`script`]] field on the [[ref/object/mta-stage-rcpt|MtaStageRcpt]] singleton (found in the WebUI under  Settings ›  MTA › Session › RCPT TO Stage) so that it runs at the `RCPT TO` stage.

```
{  "name": "greylist",  "description": "Greylisting filter backed by SQL",  "isActive": true,  "contents": "require [\"variables\", \"vnd.stalwart.expressions\", \"envelope\", \"reject\"];\n\nset \"triplet\" \"${env.remote_ip}.${envelope.from}.${envelope.to}\";\n\nif eval \"!query('sql', 'SELECT 1 FROM greylist WHERE addr = ? LIMIT 1', [triplet])\" {\n    eval \"query('sql', 'INSERT INTO greylist (addr) VALUES (?)', [triplet])\";\n    reject \"422 4.2.2 Greylisted, please try again in a few moments.\";\n}\n"}
```

The first argument of [[sieve/reference#query|`query`]] names the SQL store that holds the `greylist` table, `sql` in the script above; passing the empty string selects the default data store. The `?` placeholders in the queries use SQLite or MySQL syntax; on PostgreSQL use `$1`, `$2`, … instead, since the query is passed to the database driver verbatim.

The `script` field on MtaStageRcpt then selects the script by name:

```
{  "script": {"else": "'greylist'"}}
```

## Domain blocklisting

[Section titled “Domain blocklisting”](#domain-blocklisting)

The following script rejects messages whose `HELO`/`EHLO` domain is found in an SQL-backed list. Install it as a system script and reference it from the [[ref/object/mta-stage-ehlo#script|`script`]] field on the [[ref/object/mta-stage-ehlo|MtaStageEhlo]] singleton (found in the WebUI under  Settings ›  MTA › Session › EHLO Stage).

```
{  "name": "is-blocked",  "description": "Reject blocklisted HELO domains",  "isActive": true,  "contents": "require [\"variables\", \"extlists\", \"reject\"];\n\nif string :list \"${env.helo_domain}\" \"sql/blocked-domains\" {\n    reject \"551 5.1.1 Your domain '${env.helo_domain}' has been blocklisted.\";\n}\n"}
```

The referenced `sql/blocked-domains` list is defined as a [[ref/object/store-lookup|StoreLookup]] against the SQL store, using the query `SELECT 1 FROM blocked_domains WHERE domain=? LIMIT 1` (on PostgreSQL the placeholder would be `$1` instead of `?`).

The `script` field on MtaStageEhlo then selects the script:

```
{  "script": {"else": "'is-blocked'"}}
```

## Message modification

[Section titled “Message modification”](#message-modification)

The following script modifies the incoming message, replacing the contents of each HTML MIME part with its uppercase form and adding a custom header to every part. Install it as a system script and reference it from the [[ref/object/mta-stage-data#script|`script`]] field on the [[ref/object/mta-stage-data|MtaStageData]] singleton (found in the WebUI under  Settings ›  MTA › Session › DATA Stage).

```
{  "name": "modify-message",  "description": "Uppercase HTML parts and annotate each MIME part",  "isActive": true,  "contents": "require [\"envelope\", \"variables\", \"replace\", \"mime\", \"foreverypart\", \"editheader\", \"extracttext\"];\n\nif envelope :domain :is \"to\" \"example.net\" {\n    set \"counter\" \"a\";\n    foreverypart {\n        if header :mime :contenttype \"content-type\" \"text/html\" {\n            extracttext :upper \"text_content\";\n            replace \"${text_content}\";\n        }\n        set :length \"part_num\" \"${counter}\";\n        addheader :last \"X-Part-Number\" \"${part_num}\";\n        set \"counter\" \"${counter}a\";\n    }\n}\n"}
```

The `script` field on MtaStageData then selects the script:

```
{  "script": {"else": "'modify-message'"}}
```