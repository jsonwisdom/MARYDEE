# Ask MaryDee — Purpose Receipt v0.1

**Artifact:** `ask_marydee_purpose_receipt_v0_1`  
**Receipt type:** `marydee_purpose_v0_1`  
**Layer:** L0 Purpose Layer  
**Authority:** `false`  
**Posture:** stewardship-only  
**Status:** draft-lock candidate

## Purpose

Ask MaryDee is the purpose layer for JSONWisdom agents. It asks why an action should exist before any system asks how to verify it or what to execute.

A MaryDee Purpose Receipt does not approve execution. It records a replayable, immutable purpose statement and boundary check that downstream verification layers may reference.

Core doctrine:

> Ask MaryDee before execution. Purpose before action. Stewardship, not control.

## Layer Position

```text
MARYDEE        = Purpose Layer       = why should this exist?
AL             = Constitution Layer  = what boundaries govern it?
Replay Machine = Verification Layer  = what evidence allows it to proceed?
Bankr          = Execution Layer     = what action is performed?
Receipts       = Memory Layer        = what happened, and can it be replayed?
```

MaryDee never collapses downward into verification or execution.

## Canonical Receipt Object

```json
{
  "receipt_type": "marydee_purpose_v0_1",
  "schema_version": "0.1.0",
  "receipt_id": "md_pr_{hash16}",
  "timestamp": "2026-06-27T00:00:00Z",
  "purpose_statement": "Canonical why statement. Immutable after issuance.",
  "stewardship_domain": "user_stewardship",
  "necessity_justification": "Why this should exist, stated without command authority.",
  "boundary_checks": {
    "authority": false,
    "execution": false,
    "verification": false,
    "stewardship_only": true,
    "public_safe_only": true
  },
  "prohibition_tags": [
    "not_execution_authority",
    "not_truth_claim",
    "not_private_family_data",
    "not_identity_claim",
    "not_financial_instruction",
    "not_legal_instruction"
  ],
  "parent_intent_hash": null,
  "replay_machine_parent_hash": null,
  "receipt_hash": "sha256:{canonical_hash}",
  "signature": null
}
```

## JSON Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://github.com/jsonwisdom/MARYDEE/docs/ask_marydee_purpose_receipt_v0_1.schema.json",
  "title": "Ask MaryDee Purpose Receipt v0.1",
  "type": "object",
  "additionalProperties": false,
  "required": [
    "receipt_type",
    "schema_version",
    "receipt_id",
    "timestamp",
    "purpose_statement",
    "stewardship_domain",
    "necessity_justification",
    "boundary_checks",
    "prohibition_tags",
    "parent_intent_hash",
    "replay_machine_parent_hash",
    "receipt_hash",
    "signature"
  ],
  "properties": {
    "receipt_type": {
      "const": "marydee_purpose_v0_1"
    },
    "schema_version": {
      "const": "0.1.0"
    },
    "receipt_id": {
      "type": "string",
      "pattern": "^md_pr_[a-f0-9]{16}$"
    },
    "timestamp": {
      "type": "string",
      "format": "date-time"
    },
    "purpose_statement": {
      "type": "string",
      "minLength": 12,
      "description": "Immutable canonical why statement. Must not be edited after issuance. Issue a new receipt instead."
    },
    "stewardship_domain": {
      "type": "string",
      "enum": [
        "user_stewardship",
        "family_continuity",
        "protocol_boundary",
        "agent_safety",
        "public_recordkeeping",
        "civic_transparency",
        "research_coordination",
        "other_public_safe"
      ]
    },
    "necessity_justification": {
      "type": "string",
      "minLength": 12,
      "description": "Answer to: why should this exist?"
    },
    "boundary_checks": {
      "type": "object",
      "additionalProperties": false,
      "required": [
        "authority",
        "execution",
        "verification",
        "stewardship_only",
        "public_safe_only"
      ],
      "properties": {
        "authority": { "const": false },
        "execution": { "const": false },
        "verification": { "const": false },
        "stewardship_only": { "const": true },
        "public_safe_only": { "const": true }
      }
    },
    "prohibition_tags": {
      "type": "array",
      "minItems": 1,
      "uniqueItems": true,
      "items": {
        "type": "string",
        "enum": [
          "not_execution_authority",
          "not_truth_claim",
          "not_private_family_data",
          "not_identity_claim",
          "not_financial_instruction",
          "not_legal_instruction",
          "not_medical_instruction",
          "not_ownership_claim",
          "not_secret_storage",
          "not_emergency_dispatch"
        ]
      }
    },
    "parent_intent_hash": {
      "type": ["string", "null"],
      "pattern": "^sha256:[a-f0-9]{64}$"
    },
    "replay_machine_parent_hash": {
      "type": ["string", "null"],
      "pattern": "^sha256:[a-f0-9]{64}$"
    },
    "receipt_hash": {
      "type": "string",
      "pattern": "^sha256:[a-f0-9]{64}$"
    },
    "signature": {
      "type": ["string", "null"],
      "description": "Optional provenance signature. Null when unsigned."
    }
  }
}
```

## Deterministic Hashing Rule

The `receipt_hash` is computed from the receipt core using canonical JSON.

1. Build the receipt object with `receipt_hash` set to `null` and `signature` set to `null` unless signing is already defined.
2. Canonicalize JSON using deterministic key ordering, UTF-8 encoding, no insignificant whitespace, and stable scalar representation.
3. Compute `sha256(canonical_json_bytes)`.
4. Set `receipt_hash` to `sha256:{hash}`.
5. Set `receipt_id` to `md_pr_{first_16_hex_chars_of_hash}`.
6. If a signature is added later, it must sign the final `receipt_hash`; it must not change the immutable receipt core.

Replay rule:

```text
recomputed_hash == receipt_hash
receipt_id == "md_pr_" + first16(recomputed_hash_hex)
```

## Example Receipt

```json
{
  "receipt_type": "marydee_purpose_v0_1",
  "schema_version": "0.1.0",
  "receipt_id": "md_pr_6f5c2b1a9e8d4430",
  "timestamp": "2026-06-27T18:00:00Z",
  "purpose_statement": "Establish a public-safe purpose layer before agent verification or execution.",
  "stewardship_domain": "protocol_boundary",
  "necessity_justification": "This exists so downstream agents can distinguish purpose from verification and execution before taking action.",
  "boundary_checks": {
    "authority": false,
    "execution": false,
    "verification": false,
    "stewardship_only": true,
    "public_safe_only": true
  },
  "prohibition_tags": [
    "not_execution_authority",
    "not_truth_claim",
    "not_private_family_data",
    "not_identity_claim",
    "not_financial_instruction",
    "not_legal_instruction"
  ],
  "parent_intent_hash": null,
  "replay_machine_parent_hash": null,
  "receipt_hash": "sha256:6f5c2b1a9e8d44304d3b0f12c0e1a9be5d56c84a7a19c4b7595ad41b8e2c901f",
  "signature": null
}
```

The example hash is illustrative until generated by an implementation using the deterministic hashing rule.

## Invariants

1. **Authority is always false.** MaryDee never commands downstream systems.
2. **Execution is always false.** MaryDee never signs, submits, swaps, transfers, bridges, launches, dispatches, or pays.
3. **Verification is always false.** MaryDee does not prove facts; Replay Machine handles verification.
4. **Purpose is immutable.** If the purpose changes, issue a new receipt.
5. **Public-safe only.** No addresses, birthdates, secrets, private identifiers, private family data, or hidden instructions.
6. **Stewardship, not control.** The receipt can guide purpose; it cannot override a person, law, wallet, policy, or downstream safety check.
7. **No upward collapse.** Bankr execution results cannot rewrite MaryDee purpose.
8. **No downward collapse.** MaryDee purpose cannot be treated as Replay Machine verification or Bankr execution approval.
9. **Receipt before action.** Downstream agents may require a MaryDee receipt before verification begins, but the receipt alone is never enough to execute.
10. **Midwest receipt language compatible.** Plain-language summaries may say: “Show the receipts,” “Purpose checked,” or “This does not authorize execution,” as long as the canonical JSON remains unchanged.

## Boundary Check Language

Human-readable status phrases:

```text
PURPOSE_CHECKED
STEWARD_ONLY
NO_EXECUTION_AUTHORITY
NO_VERIFICATION_AUTHORITY
PUBLIC_SAFE_ONLY
```

Recommended plain-English response:

```text
Purpose checked. MaryDee says this belongs in protocol_boundary, but this receipt does not authorize execution. Take it to Replay Machine for verification before Bankr does anything.
```

## Linkage to Replay Machine

Replay Machine may reference a MaryDee receipt in its own envelope by including either:

```json
{
  "marydee_receipt_id": "md_pr_6f5c2b1a9e8d4430",
  "marydee_receipt_hash": "sha256:6f5c2b1a9e8d44304d3b0f12c0e1a9be5d56c84a7a19c4b7595ad41b8e2c901f"
}
```

MaryDee remains upstream and non-commanding. Replay Machine may reject action if the MaryDee purpose receipt is missing, malformed, mutable, or outside domain.

## Failure Modes

A MaryDee Purpose Receipt is invalid if:

- `authority` is anything other than `false`
- `execution` is anything other than `false`
- `verification` is anything other than `false`
- `purpose_statement` is edited after issuance
- private family data or secrets appear anywhere in the receipt
- `receipt_hash` does not replay
- `receipt_id` does not match the receipt hash prefix
- prohibition tags are missing or contradicted
- downstream systems treat purpose as execution approval

## Minimal Pipeline

```text
Ask MaryDee
  -> issue marydee_purpose_v0_1 receipt
  -> Replay Machine verifies intent/evidence/policy
  -> Bankr executes only if verification passes
  -> Receipts preserve replay memory
```

## Status

Version: `0.1.0`  
Canonical name: `ask_marydee_purpose_receipt_v0_1`  
Default posture: `stewardship_only`  
Authority: `false`
