# Replay Machine v0.2.0 — Onchain Receipt Pointer Wiring

**Artifact:** `replay_machine_v0_2_0_onchain_pointer_wiring`  
**Schema:** `schemas/receipts/ONCHAIN_RECEIPT_POINTER_SCHEMA_V0_1.json`  
**Layer:** PUBLIC_SAFE memory continuity  
**Authority:** `false`  
**Execution:** `false`  
**Status:** wiring spec

## Purpose

Replay Machine v0.2.0 validates public-safe onchain receipt pointers for existence and sequence only.

It does not validate truth, identity, ownership, custody, control, or intent.

## Input Contract

Replay Machine accepts one JSON receipt matching:

```text
schemas/receipts/ONCHAIN_RECEIPT_POINTER_SCHEMA_V0_1.json
```

Required validation surfaces:

```text
schema
version
module
surface
artifact_type
git.repository
git.commit
artifact.sha256
artifact.ipfs_cid
attestation.network
attestation.eas_schema_uid
attestation.eas_uid
pointer.basename
pointer.text_record
metadata.privacy
policy.doorway_check
policy.preflight
policy.authority
limits.proves
limits.does_not_prove
replay_checklist
```

## Validation Pipeline

```text
load_receipt
  -> validate_json_schema
  -> enforce_authority_false
  -> enforce_public_safe_privacy
  -> enforce_limits_block
  -> verify_git_commit_pointer
  -> verify_sha256_matches_artifact_bytes
  -> verify_ipfs_cid_resolves_to_expected_bytes
  -> verify_eas_attestation_references_hash_and_cid
  -> verify_basename_text_record_points_to_receipt
  -> emit_replay_result
```

## Hard Gates

```json
{
  "schema": "ONCHAIN_RECEIPT_POINTER_V0_1",
  "version": "0.1",
  "attestation.network": "base",
  "metadata.privacy": "PUBLIC_SAFE",
  "policy.doorway_check": "PASS",
  "policy.preflight": "PASS",
  "policy.authority": false,
  "replay_checklist.authority_false_end_to_end": true
}
```

## Limits Enforcement

The receipt must declare exactly:

```json
{
  "proves": [
    "artifact existence",
    "artifact ordering",
    "pointer continuity"
  ],
  "does_not_prove": [
    "truth",
    "identity",
    "ownership",
    "custody",
    "intent"
  ]
}
```

Any added truth, custody, identity, ownership, or control language fails validation.

## Replay Result States

```text
REPLAY_POINTER_VALID
SCHEMA_INVALID
AUTHORITY_FALSE_MISSING
PUBLIC_SAFE_MISSING
LIMITS_BLOCK_INVALID
GIT_POINTER_INVALID
HASH_MISMATCH
IPFS_POINTER_INVALID
EAS_POINTER_INVALID
BASENAME_POINTER_INVALID
PRIVATE_MATERIAL_BLOCKED
TRUTH_CLAIM_BLOCKED
```

## Valid Result Language

```text
Replayable provenance verified. The pointer chain is internally consistent and demonstrates artifact existence and ordering. No conclusion is made about factual truth, identity, ownership, custody, control, or intent.
```

## Invalid Result Language

```text
Replay blocked. Receipt pointer failed public-safe provenance validation. No downstream Bankr execution or basename update should occur.
```

## Game Night End-to-End Replay Test

Pilot target:

```text
surface: WISDOM_FAMILY_GAME_NIGHT
module: onchain_receipt_pointer_v0_1
privacy: PUBLIC_SAFE
authority: false
```

Test path:

```text
Game Night SHAREABLE_NOTE
  -> Doorway Check PASS
  -> JSON receipt
  -> SHA-256
  -> Git commit
  -> IPFS CID
  -> EAS UID on Base
  -> jaywisdom.base.eth latest_receipt text record
  -> Replay Machine v0.2.0 validation
```

## Non-Goals

Replay Machine v0.2.0 must not:

```text
claim truth
bind human identity
claim custody
claim ownership
claim intent
convert family memory into an asset
execute Bankr writes without PASS preflight
```

## Status

Version: `0.2.0-wiring`  
Authority: `false`  
Execution: `false`  
Verification scope: `pointer consistency only`
