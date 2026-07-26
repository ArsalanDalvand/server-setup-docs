---
title: "Untrusted Interpreter"
source: "https://stalw.art/docs/sieve/interpreter/untrusted"
source_format: official-html-to-markdown
language: "en"
doc_slug: "sieve/interpreter/untrusted"
category: "sieve"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Untrusted Interpreter"
---
# Untrusted Interpreter

      

The untrusted interpreter runs Sieve scripts created by end-users. Stalwart supports [JMAP for Sieve Scripts](https://www.rfc-editor.org/rfc/rfc9661.html) and [ManageSieve](https://datatracker.ietf.org/doc/html/rfc5804) for uploading and managing user scripts.

Interpreter settings, resource limits, and the list of globally available scripts are configured through the [[ref/object/sieve-user-interpreter|SieveUserInterpreter]] singleton (found in the WebUI under  Settings ›  Sieve › User Interpreter) and the [[ref/object/sieve-user-script|SieveUserScript]] object (found in the WebUI under  Settings ›  Sieve › User Scripts).

## Limits

[Section titled “Limits”](#limits)

User scripts execute in a sandbox that bounds their use of server resources. The limits are expressed as fields on the SieveUserInterpreter singleton:

[[ref/object/sieve-user-interpreter#maxscriptsize|`maxScriptSize`]]: maximum size of a Sieve script. Default `"100kb"`.
[[ref/object/sieve-user-interpreter#maxstringlength|`maxStringLength`]]: maximum size of a constant string. Default `4096`.
[[ref/object/sieve-user-interpreter#maxnestedblocks|`maxNestedBlocks`]]: maximum depth of nested `if` / `elsif` / `else` blocks. Default `15`.
[[ref/object/sieve-user-interpreter#maxnestedtests|`maxNestedTests`]]: maximum depth of nested tests. Default `15`.
[[ref/object/sieve-user-interpreter#maxnestedforevery|`maxNestedForEvery`]]: maximum depth of nested `foreverypart` loops. Default `3`.
[[ref/object/sieve-user-interpreter#maxmatchvars|`maxMatchVars`]]: maximum number of match and regex variables that can be captured. Default `30`.
[[ref/object/sieve-user-interpreter#maxlocalvars|`maxLocalVars`]]: maximum number of local variables in scope at any point. Default `128`.
[[ref/object/sieve-user-interpreter#maxheadersize|`maxHeaderSize`]]: maximum length of an RFC 822 header value. Default `1024`.
[[ref/object/sieve-user-interpreter#maxincludes|`maxIncludes`]]: maximum number of `include` instructions per script. Default `3`.
[[ref/object/sieve-user-interpreter#maxnestedincludes|`maxNestedIncludes`]]: maximum depth of nested `include` instructions. Default `3`.
[[ref/object/sieve-user-interpreter#maxcpucycles|`maxCpuCycles`]]: maximum number of instructions a script can execute, counted across any included scripts. Default `5000`.
[[ref/object/sieve-user-interpreter#maxvarnamelength|`maxVarNameLength`]]: maximum length of a variable name. Default `32`.
[[ref/object/sieve-user-interpreter#maxvarsize|`maxVarSize`]]: maximum size of a variable; contents beyond this point are truncated. Default `4096`.
[[ref/object/sieve-user-interpreter#maxredirects|`maxRedirects`]]: maximum number of message redirections per execution. Default `1`.
[[ref/object/sieve-user-interpreter#maxreceivedheaders|`maxReceivedHeaders`]]: maximum number of `Received` headers before a message is treated as looping. Default `10`.
[[ref/object/sieve-user-interpreter#maxoutmessages|`maxOutMessages`]]: maximum number of outgoing messages (including vacation responses, notifications, and redirects) a script can send. Default `3`.
[[ref/object/sieve-user-interpreter#maxscriptnamelength|`maxScriptNameLength`]]: maximum length of a script name. Default `512`.
[[ref/object/sieve-user-interpreter#maxscripts|`maxScripts`]]: default maximum number of Sieve scripts a single account may create. Default `100`.

Example limits:

```
{  "maxScriptNameLength": 512,  "maxScriptSize": "100kb",  "maxStringLength": 4096,  "maxVarNameLength": 32,  "maxVarSize": 4096,  "maxNestedBlocks": 15,  "maxNestedTests": 15,  "maxNestedForEvery": 3,  "maxMatchVars": 30,  "maxLocalVars": 128,  "maxHeaderSize": 1024,  "maxIncludes": 3,  "maxNestedIncludes": 3,  "maxCpuCycles": 5000,  "maxRedirects": 1,  "maxReceivedHeaders": 10,  "maxOutMessages": 3}
```

## Extensions

[Section titled “Extensions”](#extensions)

Stalwart supports [all registered Sieve extensions](https://www.iana.org/assignments/sieve-extensions/sieve-extensions.xhtml) and enables them by default. Administrators often want to disable extensions that let a user send outgoing email from a script (for example `enotify`) or modify message content (for example `editheader`, `replace`, `enclose`).

Disabled extensions are listed in [[ref/object/sieve-user-interpreter#disablecapabilities|`disableCapabilities`]] on SieveUserInterpreter. The accepted values are documented on the [[ref/object/sieve-user-interpreter#sievecapability|SieveCapability]] enum.

```
{  "disableCapabilities": ["editheader", "replace", "enclose", "enotify"]}
```

## Notification URIs

[Section titled “Notification URIs”](#notification-uris)

The allowed URI schemes for the `notify` extension are configured through [[ref/object/sieve-user-interpreter#allowednotifyuris|`allowedNotifyUris`]]. The default is `["mailto"]`.

```
{  "allowedNotifyUris": ["mailto"]}
```

## Protected Headers

[Section titled “Protected Headers”](#protected-headers)

Headers that cannot be added or removed by the `editheader` extension are listed in [[ref/object/sieve-user-interpreter#protectedheaders|`protectedHeaders`]]. The default list is `["Original-Subject", "Original-From", "Received", "Auto-Submitted"]`.

```
{  "protectedHeaders": ["Original-Subject", "Original-From", "Received", "Auto-Submitted"]}
```

## Vacation defaults

[Section titled “Vacation defaults”](#vacation-defaults)

Defaults for the `vacation` extension are exposed on SieveUserInterpreter through:

[[ref/object/sieve-user-interpreter#defaultsubject|`defaultSubject`]]: default subject of vacation responses. Default `"Automated reply"`.
[[ref/object/sieve-user-interpreter#defaultsubjectprefix|`defaultSubjectPrefix`]]: prefix prepended to vacation response subjects. Default `"Auto: "`.

```
{  "defaultSubject": "Automated reply",  "defaultSubjectPrefix": "Auto: "}
```

## Expiration defaults

[Section titled “Expiration defaults”](#expiration-defaults)

The default lifetimes for identifiers stored by the `vacation` and `duplicate` extensions are controlled by:

[[ref/object/sieve-user-interpreter#defaultexpiryvacation|`defaultExpiryVacation`]]: default expiration for vacation response tracking. Default `"30d"`.
[[ref/object/sieve-user-interpreter#defaultexpiryduplicate|`defaultExpiryDuplicate`]]: default expiration for identifiers stored by the `duplicate` extension. Default `"7d"`.

```
{  "defaultExpiryVacation": "30d",  "defaultExpiryDuplicate": "7d"}
```

## Global Scripts

[Section titled “Global Scripts”](#global-scripts)

Global user scripts are published as [[ref/object/sieve-user-script|SieveUserScript]] records. Each record carries a [[ref/object/sieve-user-script#name|`name`]], an optional [[ref/object/sieve-user-script#description|`description`]], an [[ref/object/sieve-user-script#isactive|`isActive`]] flag, and the script body in [[ref/object/sieve-user-script#contents|`contents`]]. Users reference these scripts from their own scripts with `include :global`.

For example, a global script that rejects messages larger than 100 KB:

```
{  "name": "global_one",  "description": "Reject oversized messages",  "isActive": true,  "contents": "# Declare the extensions used by this script.\n#\nrequire [\"reject\"];\n\n# Messages bigger than 100K will be rejected with an error message\n#\nif size :over 100K {\n    reject \"I'm sorry, I do not accept mail over 100kb in size. Please upload larger files to a server and send me a link. Thanks.\";\n}\n"}
```

The [[ref/object/sieve-user-script#contents|`contents`]] field holds the full script text; loading the script body from an external file is not supported.