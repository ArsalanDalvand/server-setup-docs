---
title: "DKIM key rotation"
source: "https://stalw.art/docs/domains/dkim-rotation"
source_format: official-html-to-markdown
language: "en"
doc_slug: "domains/dkim-rotation"
category: "domains"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DKIM key rotation"
---
# DKIM key rotation

      

Periodic DKIM key rotation limits the impact of key compromise and is a recommended operational practice for any mail domain that signs outbound messages. When rotation is enabled, the server takes over the full lifecycle of DKIM keys for a domain: generating new keys on schedule, publishing them in DNS ahead of use, switching signing over to the new key, and retiring old keys once they are no longer required for verification of in-flight messages.

Signing of outgoing messages and verification of incoming signatures are not covered on this page. For signing configuration and behaviour see [[mta/authentication/dkim/sign|DKIM signing]]; for verification of inbound messages see [[mta/authentication/dkim/verify|DKIM verification]]. The Domain object owns only the rotation side of the lifecycle described below.

## Enabling automatic rotation

[Section titled “Enabling automatic rotation”](#enabling-automatic-rotation)

Rotation is controlled by the [[ref/object/domain#dkimmanagement|`dkimManagement`]] field on the [[ref/object/domain|Domain]] object. The `Manual` variant leaves [[ref/object/dkim-signature|DkimSignature]] (found in the WebUI under  Management ›  Domains › DKIM Signatures) objects under the operator’s direct control; new keys are not generated and DNS publication is handled as part of manual DNS management. The `Automatic` variant carries the rotation schedule:

[[ref/object/domain#algorithms|`algorithms`]] lists the signing algorithms to use when generating new keys. The default provisions both `Dkim1Ed25519Sha256` and `Dkim1RsaSha256`, so receiving verifiers that do not yet support Ed25519 still find an RSA key.
[[ref/object/domain#selectortemplate|`selectorTemplate`]] produces the DNS selector for each new key. The template supports `{algorithm}`, `{hash}`, `{version}`, `{date-<fmt>}` (with chrono strftime), `{epoch}`, and `{random}`; the default `v{version}-{algorithm}-{date-%Y%m%d}` yields selectors such as `v1-ed25519-20260101`.
[[ref/object/domain#rotateafter|`rotateAfter`]] sets how often a fresh key is generated, defaulting to `90d`.
[[ref/object/domain#retireafter|`retireAfter`]] sets how long the superseded key’s DNS record remains published after the cutover, defaulting to `7d`.
[[ref/object/domain#deleteafter|`deleteAfter`]] sets how long the retired key’s material is retained on the server before permanent deletion, defaulting to `30d`.

Automatic rotation requires the Domain to have automatic DNS management enabled, since the rotation logic both writes and later removes DKIM DNS records. See [[domains/dns-records|DNS records]] for the DNS-side setup.

## Rotation lifecycle

[Section titled “Rotation lifecycle”](#rotation-lifecycle)

Each [[ref/object/dkim-signature|DkimSignature]] carries a [[ref/object/dkim-signature#stage|`stage`]] field that tracks its position in the rotation lifecycle, together with a [[ref/object/dkim-signature#nexttransitionat|`nextTransitionAt`]] timestamp indicating when the next transition is due. The four stages are:

pending: the key has been generated and is scheduled for DNS publication, but is not yet visible to remote verifiers. Signing continues with the current active key.
active: the key is published in DNS and is used to sign outgoing messages. For each algorithm in [[ref/object/domain#algorithms|`algorithms`]], at most one signature is active at a time.
retiring: a newer key has taken over signing, but the DNS record for this key remains published so that messages signed before the cutover still verify. The retiring key no longer signs new messages.
retired: the DNS record has been removed. The key material is still retained on the server for diagnostic purposes until it is permanently deleted.

## Cutover sequence

[Section titled “Cutover sequence”](#cutover-sequence)

When the server determines that a rotation is due for a given algorithm, the sequence is:

A new DkimSignature is created in the pending stage and the corresponding public key is pushed to the DnsServer as part of the domain’s managed records.
The DnsServer polls until propagation of the new selector is confirmed; the transition from pending to active is driven by that confirmation, not by a fixed wall-clock delay. Once propagation is observed, the new key transitions to active and the previous active key transitions to retiring. Signing switches to the new selector at this point.
After [[ref/object/domain#retireafter|`retireAfter`]] elapses, the retiring key transitions to retired and the DNS record for its selector is removed.
After [[ref/object/domain#deleteafter|`deleteAfter`]] further elapses, the retired DkimSignature is deleted from the store.

The schedule for the next rotation is rearmed from [[ref/object/domain#rotateafter|`rotateAfter`]] relative to the activation of the new key.

## Triggering a rotation manually

[Section titled “Triggering a rotation manually”](#triggering-a-rotation-manually)

Rotation runs automatically on the schedule above, but it can also be driven on demand. Rotation is performed by the DKIM management task, which is the `DkimManagement` variant of the [[ref/object/task|Task]] object. Triggering the task for a domain evaluates the current keys against the configured schedule and advances any transition that is due (generation, cutover, retirement, and deletion).

Running the task on its own does not force a fresh key to be generated before [[ref/object/domain#rotateafter|`rotateAfter`]] has elapsed. To force an emergency rotation (for example, when a key is suspected to be compromised), first set [[ref/object/dkim-signature#nexttransitionat|`nextTransitionAt`]] on the DkimSignature to the current time, then trigger the `DkimManagement` task. On that run the task sees that a rotation is due and generates the replacement key immediately.