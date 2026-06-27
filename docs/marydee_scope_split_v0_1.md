# MaryDee Scope Split v0.1

**Artifact:** `marydee_scope_split_v0_1`  
**Repo:** `jsonwisdom/MARYDEE`  
**Layer:** L0 Purpose Layer  
**Authority:** `false`  
**Status:** boundary split

## Purpose

MaryDee must separate public doctrine, personal continuity, and private life material so the Purpose Layer remains useful without leaking sensitive information.

Core rule:

> Public can describe purpose. Personal can preserve continuity. Private must stay protected.

## Three-Lane Split

```text
MARYDEE_PUBLIC
  -> public-safe doctrine, receipts, schemas, boundary rules

MARYDEE_PERSONAL
  -> non-secret continuity notes, preferences, names already public or user-approved

MARYDEE_PRIVATE
  -> secrets, addresses, birthdates, legal/medical details, family-sensitive records
```

## Lane 1 — Public

**Storage:** public GitHub repo `jsonwisdom/MARYDEE`

Allowed:

- purpose doctrine
- public-safe receipts
- schemas and manifests
- boundary rules
- non-sensitive role descriptions
- links to public repos
- generic emergency process templates
- public-safe continuity language

Disallowed:

- addresses
- birthdates
- phone numbers
- private emails
- account recovery data
- legal/medical details
- private family facts
- secrets or keys
- anything that could identify or endanger a person

Public posture:

```json
{
  "lane": "MARYDEE_PUBLIC",
  "authority": false,
  "public_safe_only": true,
  "execution": false,
  "verification": false
}
```

## Lane 2 — Personal

**Storage:** controlled personal notes or a future limited-access repo. Not this public repo unless explicitly sanitized.

Allowed:

- user-approved continuity notes
- non-secret preferences
- project memory pointers
- relationship roles without private identifiers
- purpose interpretations meant for the operator
- private-to-user reminders that do not expose sensitive data

Disallowed:

- secrets
- passwords
- seed phrases
- private keys
- exact addresses
- birthdates
- SSNs or government identifiers
- medical/legal case details
- financial account details

Personal posture:

```json
{
  "lane": "MARYDEE_PERSONAL",
  "authority": false,
  "public_safe_only": false,
  "private_data_allowed": false,
  "operator_context_allowed": true,
  "execution": false,
  "verification": false
}
```

## Lane 3 — Private

**Storage:** never in public GitHub. Use an encrypted vault, local encrypted file, or explicitly private storage controlled by the operator.

Allowed only in private storage:

- legal records
- medical records
- exact addresses
- phone numbers
- birthdates
- account recovery information
- identity documents
- financial documents
- family-sensitive facts
- emergency contact details

Private posture:

```json
{
  "lane": "MARYDEE_PRIVATE",
  "authority": false,
  "public_safe_only": false,
  "private_data_allowed": true,
  "requires_explicit_operator_control": true,
  "never_commit_to_public_repo": true,
  "execution": false,
  "verification": false
}
```

## Routing Rule

Every MaryDee artifact must be routed before storage.

```text
Does it contain secrets, addresses, private identifiers, legal, medical, financial, or family-sensitive facts?
  -> yes: MARYDEE_PRIVATE. Do not commit publicly.
  -> no: continue.

Does it contain personal continuity details not meant for the public?
  -> yes: MARYDEE_PERSONAL. Store only in controlled context.
  -> no: continue.

Is it public-safe doctrine, schema, manifest, or boundary language?
  -> yes: MARYDEE_PUBLIC.
```

## Public Repo Invariant

The public `MARYDEE` repo may only contain `MARYDEE_PUBLIC` material.

If a file needs `MARYDEE_PERSONAL` or `MARYDEE_PRIVATE`, it must not be committed here.

## Receipt Compatibility

A MaryDee purpose receipt may include a scope lane:

```json
{
  "scope_lane": "MARYDEE_PUBLIC",
  "private_material_excluded": true,
  "personal_material_excluded": true
}
```

For Personal or Private lanes, the public receipt may include only a redacted pointer:

```json
{
  "scope_lane": "MARYDEE_PRIVATE",
  "private_material_excluded": true,
  "redacted_pointer_hash": "sha256:{hash_of_private_pointer_metadata_only}",
  "note": "Private contents not stored in public repo."
}
```

The hash may prove that a pointer existed without revealing the private content.

## Failure Modes

Invalid public artifact if it contains:

- private family data
- exact location details
- private identifiers
- hidden instructions
- execution authority
- verification authority
- secrets or credentials
- personal material presented as public doctrine

## Midwest Receipt Language

Plain-language statuses:

```text
PUBLIC_SAFE
PERSONAL_HOLD
PRIVATE_DO_NOT_POST
SHOW_THE_RECEIPTS_NOT_THE_SECRETS
```

Recommended response:

```text
MaryDee split checked. This belongs in PRIVATE, not GitHub. Show the receipt, not the secret.
```

## Minimal Operating Rule

```text
MaryDee Public = doctrine people can inspect.
MaryDee Personal = continuity the operator controls.
MaryDee Private = protected life material that never goes public.
```

## Status

Version: `0.1.0`  
Canonical name: `marydee_scope_split_v0_1`  
Default public lane: `MARYDEE_PUBLIC`  
Authority: `false`
