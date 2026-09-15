# Snapshot: 2026-09-15, before the governance merge

**This folder is a frozen copy. Nothing here is live. Do not edit these files and do not let a scan read them.**

## Why it exists

The daily scan reads governing documents that live in the **Claude project**, not in this vault. Git cannot see those, so a git tag on this repo would only protect half the system. This folder pulls the project-side documents into the repo so the `pre-cousin-merge-2026-09-15` tag actually covers everything.

Captured 2026-09-15 evening, before any of the planned changes (Gmail backfill, banned-string check, hard filter 9, duplicate collapse, task rebinding).

## What is here

### `project-docs/`
The six project documents that either diverged from their vault copies or exist nowhere else:

| File | Why it was captured |
|---|---|
| `Job-Fit-Scoring-Rubric.md` | Vault copy is 1,253 bytes with no hard filters. This one has eight. |
| `Job-Description-Analysis-Template.md` | Vault copy is a 9-section form. This is a 13-phase workflow. |
| `Resume-Formatting-Spec.md` | Far more detailed than the Downloads copy: measured twip values, OOXML gotchas, verification snippets. |
| `Greenhouse-Target-Boards.md` | Exists only in the project. 29 board tokens discovered over time, painful to rebuild. |
| `Jobs-Seen-Log.md` | Exists only in the project. Six days of operational memory: what was surfaced, applied to, filtered out and why. **The single most irreplaceable file in the whole system.** |

### `scheduled-tasks/`
The full prompt text of both live scheduled tasks, plus their settings.

This matters because a task bound to a computer has an effectively immutable prompt. If the `Daily Applications` task has to be recreated in order to bind it, that long three-source workflow prompt would otherwise have to be rebuilt from memory.

## What is NOT here

- `Master-Profile.md`, `Resume-Profile-Sales.md`, `Resume-Profile-Energy.md`, `Skills-Gap-Tracker.md`, `Interview-Story-Library.md`. These exist in the project but also in the vault or in Downloads, so they are recoverable without this snapshot. The project copy of Master-Profile is newer than the vault's (2026-09-14 vs 2026-09-09), so reconcile the two rather than assuming either is correct.
- The five resume PDFs attached to the project.
- Anything about connector auth or account settings.

## How to use it

Do not restore a file from here by copying it back blindly. Read it, compare it to whatever is live, and merge deliberately. A stale governing document that looks authoritative is the exact problem this whole refactor exists to fix.
