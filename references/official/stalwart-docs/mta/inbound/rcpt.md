---
title: "RCPT stage"
source: "https://stalw.art/docs/mta/inbound/rcpt"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/inbound/rcpt"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "RCPT stage"
---
# RCPT stage

      

The `RCPT TO` command specifies the recipient of an email message during the SMTP message transfer. It is used in conjunction with the `MAIL FROM` command. After the command is issued, the SMTP server responds with a status code indicating whether the recipient is valid and the server is willing to accept the message. If the recipient is valid, the SMTP client proceeds with the `DATA` command.

RCPT-stage behaviour is configured on the [[ref/object/mta-stage-rcpt|MtaStageRcpt]] singleton (found in the WebUI under  Settings ›  MTA › Session › RCPT TO Stage).

## Directory

[Section titled “Directory”](#directory)

Handling of local accounts relies on a configured [[auth/backend|directory]], which performs the following roles:

Identifying local domains: messages destined for these domains are accepted for local processing and storage.
Validating recipients: the directory confirms whether a recipient address corresponds to a valid local account before the server accepts the message.
Verifying addresses: the SMTP `VRFY` command queries the directory to confirm that an address is valid.
Expanding addresses: the SMTP `EXPN` command retrieves the actual delivery addresses when a single address is aliased to a group.

Without a directory, Stalwart cannot accept mail for local delivery, except in the case where [relay functionality](#relay) is enabled so that messages can be forwarded to another server.

The directory that validates a given recipient is detected automatically from the recipient domain: Stalwart matches the RCPT address against the configured [[ref/object/domain|Domain]] objects and routes the lookup to the [[ref/object/domain#directoryid|`directoryId`]] associated with that domain, falling back to the internal directory when no explicit directory is set. MtaStageRcpt exposes no per-stage directory override.

### Split-delivery relaying

[Section titled “Split-delivery relaying”](#split-delivery-relaying)

In split-delivery setups, Stalwart sits in front of another server that hosts a subset of a domain’s mailboxes. Messages for recipients that are not present in the local directory can be forwarded upstream by setting [[ref/object/domain#allowrelaying|`allowRelaying`]] to `true` on the matching [[ref/object/domain|Domain]] object. With that flag set, recipients that do not resolve to a local account are accepted and relayed rather than rejected with an unknown-recipient error.

## Relay

[Section titled “Relay”](#relay)

Relaying is the process of transferring an email from one mail server to another. When relaying is enabled, Stalwart acts as an intermediary, accepting messages from sending clients or servers and forwarding them to their ultimate destination. This is useful when Stalwart is deployed as a front-end server in a larger email infrastructure, but it must be restricted to authorised senders or networks to prevent open-relay abuse.

The [[ref/object/mta-stage-rcpt#allowrelaying|`allowRelaying`]] field on MtaStageRcpt accepts an expression that determines whether the SMTP server accepts messages for non-local domains. The default policy only allows relaying when the session is authenticated:

```
{  "allowRelaying": {    "match": [{"if": "!is_empty(authenticated_as)", "then": "true"}],    "else": "false"  }}
```

This controls relaying for entirely non-local domains. Relaying to unknown recipients within a domain that is otherwise local is governed by the domain-level [[ref/object/domain#allowrelaying|`allowRelaying`]] flag described above.

## Subaddressing

[Section titled “Subaddressing”](#subaddressing)

Subaddressing, also known as plus addressing or detailed addressing, allows the creation of dynamic, disposable email addresses under a primary email address. By adding a `+` sign and any string of text to the local part of an address (for example `[email protected]`), messages sent to any of these variants are all delivered to the same mailbox. This makes it easier to filter and sort incoming mail, for example to separate subscriptions from personal correspondence.

Subaddressing is configured per domain on the [[ref/object/domain|Domain]] object (found in the WebUI under  Management ›  Domains › Domains) through the [[ref/object/domain#subaddressing|`subAddressing`]] field. The field is a multi-variant value with three variants:

`Enabled`: standard `+` subaddressing is applied. This is the default.
`Disabled`: the `+` separator is not treated as a subaddress marker; the full local part is used for delivery.
`Custom`: a custom rule is applied through the `customRule` expression, which receives the recipient local part as `rcpt` and returns the rewritten local part. The domain is preserved by the server and is not part of the value passed in or out. For example, to strip an alias prefix so that `[email protected]` delivers to `[email protected]`:

```
{  "subAddressing": {    "@type": "Custom",    "customRule": {      "match": [{"if": "matches('^([^.]+)\\.([^.]+)$', rcpt)", "then": "$2"}],      "else": "rcpt"    }  }}
```

## Catch-all addresses

[Section titled “Catch-all addresses”](#catch-all-addresses)

A catch-all address receives all messages sent to non-existent addresses within a domain, preventing loss of mail due to misspellings. The catch-all recipient is configured per domain on the [[ref/object/domain|Domain]] object through the [[ref/object/domain#catchalladdress|`catchAllAddress`]] field, which holds the destination email address that receives mail for unknown local recipients.

For example, to route all unmatched recipients on `example.org` to `[email protected]`:

```
{  "name": "example.org",  "catchAllAddress": "[email protected]"}
```

When `catchAllAddress` is left unset, messages to unknown local recipients are rejected.

## Address rewriting

[Section titled “Address rewriting”](#address-rewriting)

The [[ref/object/mta-stage-rcpt#rewrite|`rewrite`]] field accepts an expression that can modify the recipient address. Typical uses include correcting common misspellings, obfuscating the original recipient’s address for privacy, or redirecting mail traffic from one address to another. For background on the expression-based rewrite model see the [[mta/rewrite/address|address rewriting]] documentation.

For example, the following configuration rewrites `[email protected]` to `[email protected]` for local domains only:

```
{  "rewrite": {    "match": [{"if": "is_local_domain('', rcpt_domain) & matches('^([^.]+)\\.([^.]+)@(.+)$', rcpt)", "then": "$1 + '+' + $2 + '@' + $3"}],    "else": "false"  }}
```

## Sieve script

[Section titled “Sieve script”](#sieve-script)

Running Sieve at the RCPT stage opens up uses that the delivery-time Sieve runtime cannot satisfy. Recipients can be rejected based on criteria such as the recipient domain; [[mta/rewrite/address#sieve|address rewriting]] can correct common misspellings or redirect mail; and policies such as greylisting (temporarily rejecting mail from unknown sources and asking the sender to retry) can be implemented against the envelope before the message body is accepted.

The [[ref/object/mta-stage-rcpt#script|`script`]] field selects the [[sieve|Sieve script]] to run at this stage. A common use is greylisting, implemented by storing a `remote_ip.sender.recipient` triplet in a store and rejecting the first attempt with a `422` temporary failure; the client then retries, and legitimate senders succeed while most spam sources do not.

For example, the following expression runs a `greylist` Sieve script only for unauthenticated sessions:

```
{  "script": {    "match": [{"if": "is_empty(authenticated_as)", "then": "'greylist'"}],    "else": "false"  }}
```

A companion `greylist` Sieve script stores the triplet in a store and rejects the first attempt. The first argument of [[sieve/reference#query|`query`]] names the SQL store holding the `greylist` table, `sql` in the example below; passing the empty string selects the default data store. The `?` placeholders use SQLite or MySQL syntax; on PostgreSQL use `$1`, `$2`, … instead, since the query is passed to the database driver verbatim:

```
require ["variables", "vnd.stalwart.expressions", "envelope", "reject"];
set "triplet" "${env.remote_ip}.${envelope.from}.${envelope.to}";
if eval "!query('sql', 'SELECT 1 FROM greylist WHERE addr = ? LIMIT 1', [triplet])" {    eval "query('sql', 'INSERT INTO greylist (addr) VALUES (?)', [triplet])";    reject "422 4.2.2 Greylisted, please try again in a few moments.";}
```

## Limits

[Section titled “Limits”](#limits)

Recipient-handling limits are configured via [[ref/object/mta-stage-rcpt#maxrecipients|`maxRecipients`]] (maximum number of recipients allowed per message, default 100), [[ref/object/mta-stage-rcpt#maxfailures|`maxFailures`]] (maximum number of invalid-recipient errors allowed before the session is disconnected, default 5), and [[ref/object/mta-stage-rcpt#waitonfail|`waitOnFail`]] (amount of time to wait after an invalid recipient is received, default 5 seconds):

```
{  "maxRecipients": {"else": "25"},  "maxFailures": {"else": "5"},  "waitOnFail": {"else": "5s"}}
```