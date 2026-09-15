# Scheduled task: "Inbox scan -> vault digest (8am / 1pm / 6pm)"

**Snapshot taken 2026-09-15 evening.** This task is the healthy one. It is included as the reference configuration for what `Daily Applications` should look like after rebinding.

## Settings as of this snapshot

| Setting | Value |
|---|---|
| Trigger ID | `trig_01QKeE7VfNM6hpyB3Ncipa5s` |
| Schedule | `0 13,18,23 * * *` UTC = 8:00am / 1:00pm / 6:00pm Central |
| Enabled | yes |
| Created | 2026-09-15 |
| **Bound folders** | `C:\Users\rkyle\ai-career-system` |
| **Permission mode** | `auto` |
| Connectors | Gmail, Google_Drive, Google_Calendar, Indeed, Context7, Claude_Code_Remote |
| Last run | 2026-09-15 1:02pm Central, SUCCEEDED |

## Prompt, verbatim

```
Scan Riley Kyler's Gmail inbox (rkyler11@gmail.com) for messages that need his attention, and append a digest entry to his Obsidian career vault at C:\Users\rkyle\ai-career-system.

This is READ-ONLY on Gmail. Do not send, reply, draft, label, archive, star, trash, or modify any message. Only search and read.

## 1. Determine the lookback window

Local time zone is America/Chicago. This task runs at 8:00am, 1:00pm, and 6:00pm local. Cover only mail that arrived since the previous run:
- 8:00am run -> since 6:00pm the previous day
- 1:00pm run -> since 8:00am today
- 6:00pm run -> since 1:00pm today

Use `date` in bash to get the current time, compute the window start, and search Gmail with an epoch `after:` filter, e.g. `in:inbox after:<epoch> -in:chats -category:promotions`. If the exact run time is ambiguous, fall back to `newer_than:1d` and rely on the dedupe step below.

## 2. Triage

Read enough of each message to judge it. Surface ONLY messages matching one or more of:

- **Job search / recruiters** — interview invitations, recruiter outreach, application status updates, ATS/careers-portal notifications, assessment or scheduling requests. Riley is job hunting: his interests are power/ERCOT market analysis, trading, operations/management, and sales/business development at tech, SaaS, and financial investment firms. He's in the Austin-San Marcos, TX area.
- **Needs a reply from him** — a real person is directly asking him something or waiting on him. Not newsletters, marketing, receipts, or no-reply automated mail.
- **Deadlines / time-sensitive** — anything with a date, deadline, RSVP, expiring window, or scheduled event in the next ~7 days.

Skip everything else. Promotional mail, newsletters, social notifications, and routine receipts do not belong in the digest. Be strict — a digest full of noise is worse than a short one.

## 3. Write the digest

Target file on the user's computer: `C:\Users\rkyle\ai-career-system\Inbox-Digests\<YYYY-MM-DD>.md` (today's local date). The `Inbox-Digests` folder may not exist yet on the first run — committing a file to that path creates it.

Steps:
1. Use `device_list_dir` on `C:\Users\rkyle\ai-career-system\Inbox-Digests` to see whether today's file exists. If it does, stage it with `device_stage_files` and read it.
2. **Dedupe:** skip any message already listed in today's file (match on sender + subject).
3. Build the full updated file content and write it to `/mnt/user-data/outputs/<YYYY-MM-DD>.md`, then commit it to the device path above with `device_commit_files`. Preserve all existing content — append the new run's section at the bottom, never overwrite earlier sections.

File format — if creating the file fresh, start with:

---
type: inbox-digest
date: <YYYY-MM-DD>
---

# Inbox Digest - <Weekday, Month D, YYYY>

Then append one section per run:

## <h:mm am/pm> scan

### <Category: Job Search | Needs Reply | Time-Sensitive>

- **<Subject>** — <Sender Name> (<sender@email>), <time received>
  - <One or two sentences: what it is and what it's asking of him.>
  - Action: <the specific next step, or "FYI — no action">
  - [Open in Gmail](https://mail.google.com/mail/u/0/#inbox/<messageId>)

Group items by category, most urgent first within each. Use Obsidian [[Company Name]] wiki-links when a message involves a company that plausibly has a note in the vault's `Companies/` folder.

If a run finds nothing worth surfacing, still append the section with a single line: `- No items needing attention.` — so Riley can see the scan ran.

Do NOT create or modify any other vault file. Only the digest file.

## 4. Report

End your run with a short plain-text summary in the chat: how many messages you scanned, how many you surfaced, and a one-line headline for each surfaced item — anything genuinely urgent named first. Say which file you wrote to.

If the computer is unreachable and you can't write to the vault, still produce the full digest in the run summary and say plainly that the vault write failed so the content isn't lost.
```

## Note on the fenced block above

The original prompt contains its own fenced code blocks for the file-format examples. Those inner fences were unwrapped here so this file renders correctly. The wording is otherwise verbatim. If you recreate this task, re-add fences around the frontmatter and section-format examples.
