---
title: "Masked email"
source: "https://stalw.art/docs/email/management/masked-email"
source_format: official-html-to-markdown
language: "en"
doc_slug: "email/management/masked-email"
category: "email"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Masked email"
---
# Masked email

      

A masked email is a disposable address that forwards to an existing account without revealing the account’s real address. Each mask is bound to one account and can be disabled or destroyed independently, so end users can hand out a different address to every external service and cut any of them off without affecting the others. The typical use case is signing up for newsletters, shopping sites, or third-party applications where the receiving party should not learn the user’s primary address.

Masks are represented by the [[ref/object/masked-email|MaskedEmail]] object (found in the WebUI under  Account ›  Masked Addresses). Each instance belongs to exactly one [[ref/object/account|Account]] through the read-only [[ref/object/masked-email#accountid|`accountId`]] reference, and the server fills in the generated address in the [[ref/object/masked-email#email|`email`]] field.

## Lifecycle

[Section titled “Lifecycle”](#lifecycle)

End users create masks from the account-management area of the WebUI or through a password-manager integration that speaks the JMAP API. At creation time, the user may supply a short [[ref/object/masked-email#description|`description`]] to remember what the mask is for and a [[ref/object/masked-email#fordomain|`forDomain`]] value naming the site the address was issued to. Password managers typically fill these in automatically when storing a new login. A [[ref/object/masked-email#url|`url`]] can also be attached to deep-link back into the integrator that owns the entry.

The server records the creation time in [[ref/object/masked-email#createdat|`createdAt`]] and, when the request comes in with recognisable client credentials, the originating application’s name in [[ref/object/masked-email#createdby|`createdBy`]]. An optional [[ref/object/masked-email#expiresat|`expiresAt`]] can be set at creation time; once set, it is read-only because the expiry is encoded in the generated address itself and cannot be changed after the fact. The mask stops resolving once that time has passed.

Masks are toggled through [[ref/object/masked-email#enabled|`enabled`]]. Disabling a mask leaves the record in place but causes the server to reject incoming mail for that address, which is useful when a service starts misbehaving without losing the record of where the address was handed out. Destroying the mask removes it outright.

## Address generation

[Section titled “Address generation”](#address-generation)

By default the server picks the local part of a new mask and chooses a domain from those the account already has access to. Integrators that need a specific shape can pass [[ref/object/masked-email#emailprefix|`emailPrefix`]] to require the generated address to start with a given string (up to 64 characters, limited to lowercase letters, digits, and underscore) or [[ref/object/masked-email#emaildomain|`emailDomain`]] to force a particular domain. Both fields are honoured only at creation time and are ignored on update.

The pool of domains available through [[ref/object/masked-email#emaildomain|`emailDomain`]] is not configured separately: a mask can be requested under any domain or alias the owning account is linked to. Requesting a domain the account has no access to, or a domain that does not exist, returns an error.

## Managing masks

[Section titled “Managing masks”](#managing-masks)

Three surfaces share the same JMAP methods:

WebUI. The account manager shows every mask owned by the signed-in user, with controls to disable, re-enable, or destroy each one. Administrators with the corresponding `sysMaskedEmail*` permissions can list and manage masks across accounts for support purposes.
CLI. `stalwart-cli` covers the same operations for automation. For example, `stalwart-cli create masked-email --field description='Newsletter signup' --field forDomain=example.com` creates a mask, `stalwart-cli update masked-email <id> --field enabled=false` disables it, and `stalwart-cli delete masked-email --ids <id>` removes it. The full command list is on the [[ref/object/masked-email#cli|MaskedEmail object page]].
JMAP API. Password managers and other integrators call `x:MaskedEmail/get`, `x:MaskedEmail/query`, and `x:MaskedEmail/set` under the `urn:stalwart:jmap` capability. The methods follow the standard RFC 8620 shapes.

A minimal creation request, as a password manager might issue it:

```
{  "methodCalls": [    [      "x:MaskedEmail/set",      {        "create": {          "new1": {            "description": "Newsletter signup",            "forDomain": "https://example.com",            "createdBy": "ACME Password Manager",            "enabled": true          }        }      },      "c1"    ]  ],  "using": ["urn:ietf:params:jmap:core", "urn:stalwart:jmap"]}
```

Listing the masks that belong to a given user is done with `x:MaskedEmail/query` filtered by `accountId`; updating or destroying a mask uses the `update` and `destroy` arguments of `x:MaskedEmail/set`.

## Permissions and quotas

[Section titled “Permissions and quotas”](#permissions-and-quotas)

Each operation is gated by a dedicated permission: `sysMaskedEmailGet`, `sysMaskedEmailQuery`, `sysMaskedEmailCreate`, `sysMaskedEmailUpdate`, and `sysMaskedEmailDestroy`. End-user roles grant these permissions for masks owned by the signed-in account, while administrative roles can be configured to manage masks on behalf of other accounts.

The total number of masks an account may hold is bounded by the `maxMaskedAddresses` entry in the account’s [[ref/object/account#quotas|`quotas`]] map. Leaving the quota unset allows an unlimited number of masks; setting it to zero disables the feature for that account even when the Enterprise license is active.