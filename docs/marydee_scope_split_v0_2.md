# MaryDee Scope Split v0.2 — Verification Hooks Seed

**Artifact:** `marydee_scope_split_v0_2`  
**Builds on:** `marydee_scope_split_v0_1`  
**Layer:** L0 Purpose Layer  
**Authority:** `false`  
**Status:** seed

## Purpose

Version 0.2 turns the MaryDee scope split from doctrine into policy-as-code hooks.

The public repository remains `MARYDEE_PUBLIC` only. Any artifact that belongs to PERSONAL or PRIVATE scope must be blocked before commit, push, verification, or execution.

Core rule:

> Verify scope before storage. Show the receipt, not the secret.

## Hook Targets

```text
pre_commit_scope_check
  -> scan staged files before commit

pre_push_scope_check
  -> scan outgoing diff before push

receipt_scope_check
  -> require scope metadata on MaryDee receipts

replay_machine_scope_gate
  -> reject verification if MaryDee scope is missing or invalid
```

## Policy Object

```json
{
  "policy_type": "marydee_scope_policy_v0_2",
  "authority": false,
  "public_repo_allowed_scope": "MARYDEE_PUBLIC",
  "blocked_public_scopes": [
    "MARYDEE_PERSONAL",
    "MARYDEE_PRIVATE"
  ],
  "required_checks": {
    "scope_lane_present": true,
    "public_safe_only": true,
    "private_material_excluded": true,
    "execution": false,
    "verification": false
  },
  "failure_state": "BLOCK_COMMIT"
}
```

## Minimal Scope Metadata

Every future MaryDee artifact should declare:

```json
{
  "scope_lane": "MARYDEE_PUBLIC",
  "public_safe_only": true,
  "personal_material_excluded": true,
  "private_material_excluded": true,
  "authority": false
}
```

## Pre-Commit Pseudocode

```text
for each staged file:
  if file path is public repo:
    require scope_lane == MARYDEE_PUBLIC
    require public_safe_only == true
    reject if wrong-scope markers appear as content rather than routing doctrine
    reject if secret-like patterns appear
    reject if authority != false
```

## Pre-Push Pseudocode

```text
read outgoing diff
verify changed files are public-safe doctrine, schema, receipt, or manifest
verify no wrong-scope material is introduced
emit PUBLIC_SAFE receipt
block push on uncertainty
```

## Replay Machine Link

Replay Machine should reject intent verification unless the linked MaryDee receipt has:

```json
{
  "scope_lane": "MARYDEE_PUBLIC",
  "authority": false,
  "execution": false,
  "verification": false,
  "private_material_excluded": true
}
```

Failure response:

```text
Execution blocked. MaryDee scope receipt missing or not PUBLIC_SAFE. No downstream verification or Bankr execution allowed.
```

## Receipt Status Phrases

```text
PUBLIC_SAFE_VERIFIED
SCOPE_METADATA_MISSING
WRONG_SCOPE_BLOCKED
PRIVATE_POINTER_ONLY
NO_PUBLIC_PUSH
```

## v0.2 Deliverables

- schema for `marydee_scope_policy_v0_2`
- pre-commit hook
- pre-push hook
- receipt validator
- Replay Machine parent-hash compatibility check
- test fixtures for PUBLIC, PERSONAL, PRIVATE routing outcomes

## Status

Version: `0.2.0-seed`  
Authority: `false`  
Execution: `false`  
Verification: `false`  
Default public lane: `MARYDEE_PUBLIC`
