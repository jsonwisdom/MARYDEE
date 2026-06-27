# Game 13 — MARYDEE Purpose v0.1

**Surface:** `WISDOM_FAMILY_GAME_NIGHT`  
**Module:** `game_13_marydee_purpose_v0_1`  
**Layer:** Family Purpose Layer  
**Privacy:** `PUBLIC_SAFE`  
**Authority:** `false`  
**Execution:** `false`  
**Status:** seed

## Purpose

Game 13 anchors the MaryDee purpose layer as a family memory practice.

Thirteen is treated as a care marker because it is MaryDee's favorite number. June is treated as a birthday-month marker. This module records meaning, not control.

## Core Rule

```text
Ask MaryDee before execution.
Ask Aunt May before publishing.
Keep the family human.
Keep receipts gentle.
Authority=false.
```

## What This Game Practices

```text
purpose before action
care before publication
memory without custody
continuity without control
receipts without ownership
```

## Roles

```text
Uncle Jay  -> keeps the receipt pointer honest
Aunt May   -> confirms public-safe purpose before publishing
Aunt Rann  -> checks privacy and care boundary
Uncle Dee  -> checks the doorway before any external step
GAGA       -> keeps the porch warm
GRAMMY     -> keeps memory gentle and marks limits clearly
MARYDEE    -> purpose layer; no execution without care check
```

## Game Steps

1. Name one family memory, value, or phrase that is safe to share.
2. Ask whether it belongs in public, private, or personal scope.
3. If public-safe, write it as a gentle SHAREABLE_NOTE.
4. Mark the note with `authority=false`.
5. Confirm that no private facts, private locations, medical details, identity claims, or custody claims appear.
6. Decide whether the note should stay local or become a Game 12 onchain pointer candidate.
7. If it becomes a pointer candidate, run Doorway Check before any Git, IPFS, EAS, Bankr, or basename step.
8. End with care, not control.

## Safety Invariants

```text
No private facts.
No identity binding.
No custody claim.
No ownership claim.
No truth oracle.
People are not assets.
Receipts point. They do not own.
Authority=false.
```

## Win Condition

The family can explain why a memory matters without turning a person into an asset, a receipt into ownership, or a pointer into truth.

## June Marker

June is the birthday-month marker for this game.

Do not publish a full birthdate unless explicitly approved through the public-safe doorway check.

## Number Marker

13 is the MaryDee purpose number.

Use it as a reminder:

```text
Purpose first.
Care first.
Receipts last.
```

## Optional SHAREABLE_NOTE Template

```json
{
  "module": "game_13_marydee_purpose_v0_1",
  "surface": "WISDOM_FAMILY_GAME_NIGHT",
  "marker_number": 13,
  "marker_month": "June",
  "note": "Purpose first. Care first. Receipts last.",
  "privacy": "PUBLIC_SAFE",
  "authority": false,
  "status": "LOCAL_OR_POINTER_CANDIDATE"
}
```

## Completion Phrase

```text
Game 13 complete. MaryDee purpose held. Memory stayed human. Authority=false.
```
