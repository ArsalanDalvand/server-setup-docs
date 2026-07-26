---
title: "MIME types"
source: "https://stalw.art/docs/spamfilter/settings/mime-types"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/settings/mime-types"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MIME types"
---
# MIME types

      

File-extension classification lets the spam filter react to message attachments in an informed way, based on the expected MIME type of each extension and a small set of flags. Each classification is stored as a [[ref/object/spam-file-extension|SpamFileExtension]] object (found in the WebUI under  Settings ›  Spam Filter › Lists › File Extensions).

## Fields

[Section titled “Fields”](#fields)

Each SpamFileExtension instance carries:

[[ref/object/spam-file-extension#extension|`extension`]]: the file-name extension this entry applies to, without the leading dot.
[[ref/object/spam-file-extension#contenttypes|`contentTypes`]]: a list of MIME types that are expected for this extension. Leaving the list empty means no MIME type is required.
[[ref/object/spam-file-extension#isbad|`isBad`]]: marks the extension as risky or malicious. Messages carrying an attachment whose extension is flagged as bad receive an increased spam score.
[[ref/object/spam-file-extension#isarchive|`isArchive`]]: marks the extension as an archive format (such as `zip` or `rar`). The filter can then apply archive-specific logic, for example inspecting the archive contents or penalising nested archives.
[[ref/object/spam-file-extension#isnz|`isNz`]]: marks the extension as “not in archive”: a file with this extension should not appear inside another archive. If the filter finds such a file nested inside an archive, the spam score is adjusted accordingly.

The filter uses these flags to apply rule-based scoring. For example, finding an archive (`isArchive = true`) inside another archive increases the spam score, since nested archives are a common tactic for hiding malicious content.

## Examples

[Section titled “Examples”](#examples)

PDF files are expected to carry a PDF MIME type and should not appear inside archives:

```
{  "extension": "pdf",  "contentTypes": ["application/pdf", "application/x-pdf"],  "isArchive": false,  "isBad": false,  "isNz": true}
```

HTML files carried as attachments are treated as risky:

```
{  "extension": "htm",  "contentTypes": ["text/html"],  "isArchive": false,  "isBad": true,  "isNz": false}
```

Batch files are considered malicious regardless of the declared MIME type:

```
{  "extension": "bat",  "contentTypes": [],  "isArchive": false,  "isBad": true,  "isNz": false}
```

RAR files are archives:

```
{  "extension": "rar",  "contentTypes": [],  "isArchive": true,  "isBad": false,  "isNz": false}
```