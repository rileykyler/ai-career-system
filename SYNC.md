---
type: sync-arrangement
decided: 2026-09-15
supersedes: the vault-canonical plan briefly adopted earlier the same evening
---

# Where each document actually lives

## The arrangement

**The Claude project is canonical for the four operational documents the daily scan reads.** The vault carries a git-tracked mirror of each so rule changes have version history. The mirror is never authoritative and editing it does nothing.

| Document | Live copy | Mirror in this vault |
|---|---|---|
| Job-Fit-Scoring-Rubric.md | Claude project | `Frameworks/` |
| Job-Description-Analysis-Template.md | Claude project | `Frameworks/` |
| Greenhouse-Target-Boards.md | Claude project | `Frameworks/` |
| Jobs-Seen-Log.md | Claude project | `Jobs/` |
| Resume-Formatting-Spec.md | Claude project | `Profile/` |

Every mirror file opens with a warning callout saying so. If you want to change a rule, change it in the project (or ask Claude to), then re-sync the mirror and commit.

**The vault is canonical for everything else** and always has been: `Jobs/` application notes, `Companies/`, `Interview-Prep/`, `Networking/`, `Resume/`, `Inbox-Digests/`, `Profile/Master-Profile.md`, `Profile/Skills-Gap-Tracker.md`, `Templates/`.

## Why it is this way, and not the other way

The earlier plan was vault-canonical: move the governing docs into the vault and have the scan read them from a bound folder. That was reversed the same evening for one reason.

**A scheduled task bound to this computer can only reach the vault while this computer is awake and online.** The job scan fires once a day at 9:00am. If the laptop is asleep, closed, or offline at 9:00am, that run cannot read or write the vault, and with the Step 0 backlog check it now needs `Jobs-Seen-Log.md` before it can do anything — so it would fail outright rather than degrade.

Unbound, the scan runs in the cloud and has worked every single day since 2026-09-10. Six for six, through an Indeed outage and a fetch-gate failure.

The `Inbox scan` task gets away with being bound because it fires three times daily, so a missed run is covered by the next one. The job scan has no such cushion. For a job search optimizing for speed to hire, a scan that runs every morning is worth more than version history on rule changes — and this arrangement gets the version history anyway.

## The problem this still solves

The original failure was never "the docs are in the project." It was that **two copies existed with different contents and nothing said which was real.** The vault copies of the rubric and the tailoring workflow had been frozen since 2026-09-09 while the project copies evolved for six days, and anyone reading the vault in Obsidian was reading a stale fork without knowing it.

That is fixed. One authoritative copy, one clearly-labeled mirror, and a header on every mirror file saying which is which.

## Re-syncing the mirror

Ask Claude in this project to re-export the project docs into the vault, then commit. Worth doing whenever a rule changes, so the git diff captures it. The mirror going stale between syncs is fine and expected — that is what the header warns about.

## Still outstanding

- **Task settings.** Automatic approval is set. Completion notifications for both tasks are still worth turning on, so a brief reaches your phone rather than an unwatched chat window. Computer binding is deliberately **not** being done, per the reasoning above.
- **Inbox scan prompt.** Not yet changed. The plan is to have it reconcile job-search mail against the Applications Sent list so confirmations and rejections connect back to what was applied to. That task is bound, so editing its prompt needs an approval.
- **Gmail backfill.** A one-time pass to rebuild the true application history from ATS confirmations rather than from what a run happened to remember. Four entries in Applications Sent currently read "exact filename not retained in this session."
- **Master-Profile.md.** Vault copy last written 2026-09-09; project copy updated 2026-09-14, and they have not been diffed. Under this arrangement the vault copy should win, but not before someone checks what the project version added.

## Rollback

Checkpoint tag `pre-cousin-merge-2026-09-15`, pushed to origin. Frozen copies of every project document and both scheduled task prompts, exactly as they stood before any of tonight's changes, are in `_snapshots/2026-09-15-pre-merge/`.
