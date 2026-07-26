---
title: "Headers"
source: "https://stalw.art/docs/mta/rewrite/headers"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/rewrite/headers"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Headers"
---
# Headers

      

Message header rewriting involves the process of adding, removing, or modifying the headers of an email message. Headers are crucial parts of an email, as they contain meta-information about the email, such as the sender, recipient, subject, and other technical details, including those required for proper routing and delivery.

There are various reasons why header rewriting might be useful:

Obfuscating internal details: Internal server names, IP addresses, or other specifics could be present in the headers. Removing or modifying these helps keep internal infrastructure private and improves the security posture.
Compliance and regulatory reasons: Certain industries or legal environments require specific information to be present in email headers. By adding or modifying headers, an organization can ensure it meets these requirements.
Message enhancement and organization: Headers can be added to include additional information about the email, such as categorization information, which can be used by email clients to sort or filter messages.
Troubleshooting and Monitoring: By adding custom headers, it’s possible to track the path an email took through the system, which can be invaluable for troubleshooting delivery issues or monitoring system performance.

Headers can be modified using [[sieve|Sieve scripts]], which is a language designed for filtering email messages. A Sieve script containing the specific add, delete, or alter commands is defined as a [[ref/object/sieve-system-script|SieveSystemScript]] object and referenced from the [[ref/object/mta-stage-data#script|`script`]] field on the [[ref/object/mta-stage-data|MtaStageData]] singleton (found in the WebUI under  Settings ›  MTA › Session › DATA Stage); see the [[mta/inbound/data#sieve|DATA stage]] documentation.

The Sieve language has a rich set of commands for precise header manipulation. The `addheader` action adds a new header, `deleteheader` removes a header, and combining the two effectively replaces a header’s value. Modifications can be applied conditionally based on attributes such as sender, recipient, subject line, or any other header.

## Examples

[Section titled “Examples”](#examples)

### Adding Headers

[Section titled “Adding Headers”](#adding-headers)

The `addheader` command is used to add a new header to the email. Below is an example Sieve script that adds a “X-Custom-Header” with a value “Custom Value”:

```
require ["editheader"];addheader "X-Custom-Header" "Custom Value";
```

### Deleting Headers

[Section titled “Deleting Headers”](#deleting-headers)

The `deleteheader` command is used to remove a header from the email. In the following example, any existing “X-Custom-Header” is removed:

```
require ["editheader"];deleteheader "X-Custom-Header";
```

### Replacing headers

[Section titled “Replacing headers”](#replacing-headers)

Sieve does not have a built-in replace command to modify a header. The `deleteheader` command removes the old header and the `addheader` command adds a new header with the updated value. The following example replaces the value of “X-Custom-Header”:

```
require ["editheader", "variables"];if header :matches "X-Custom-Header" "*" {    set "oldValue" "${1}";    deleteheader "X-Custom-Header";    addheader "X-Custom-Header" "New Value";}
```

In this script, the original value of “X-Custom-Header” is first stored in the variable “oldValue”. Then, the header is removed and a new header with the same name but a new value “New Value” is added.