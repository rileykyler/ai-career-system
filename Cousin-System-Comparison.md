---
type: system-analysis
date: 2026-09-15
compares: ai-career-system (mine) vs Job-Search-Agent-Starter-Kit (cousin's)
supersedes: the first version of this file, written earlier today
---

# Cousin's System vs Mine: Full Analysis

---

## CORRECTION, 2026-09-15 evening

**An earlier version of this file said the daily scan was not live yet. That was wrong, and several conclusions built on it were wrong too.** The scan has run every day since 2026-09-10, six consecutive successful runs. What follows below has been corrected in place. Specifically, these claims were false:

- *"No run logging."* Wrong. `Jobs-Seen-Log.md` carries dated per-run data-quality notes for 9/11 through 9/15, including which source failed and why. That is a run log, and a better one than the cousin's Run-Log template.
- *"No dedupe memory, skipped roles resurface forever."* Wrong. The log's **Filtered Out** section holds roughly 60 entries, each with a specific `REJECT_REASON`. That is precisely his Dead-Companies file. The **Needs follow-up** sections are precisely his Maybe-List.
- *"No hard filters, no comp parsing rule."* Wrong. The **project** copy of `Job-Fit-Scoring-Rubric.md` has eight hard filters applied before scoring, including a comp floor and a commission-only rejection, plus a "do not over-filter stretch roles" guardrail the cousin's rubric does not have.
- *"No ground truth on what you have applied to."* Half wrong. The log has an **Applications Sent** section with 13 entries. It is self-reported rather than Gmail-verified, and it is already decaying (four entries read "exact filename not retained in this session"), so the Gmail gate still matters, but the claim as written was too strong.
- *"Governance for a process that is not running is theater."* The process is running. The governance gap is real and now urgent rather than premature.

**What caused the error:** I read the vault copies of the governing documents. The scan reads the project copies. They are different documents with the same filenames. That turns out to be the most important finding in this whole analysis, and it is covered in the new [Part 0](#part-0-the-split-brain-problem-read-this-first).

---

## Contents
- [Part 0: The split-brain problem, read this first](#part-0-the-split-brain-problem-read-this-first)
- [Part 1: What his system actually is, all of it](#part-1-what-his-system-actually-is-all-of-it)
- [Part 2: What to take, why, and how it works in practice](#part-2-what-to-take-why-and-how-it-works-in-practice)
- [Part 3: Synopsis, pros and cons, both systems](#part-3-synopsis-pros-and-cons-both-systems)
- [Part 4: What not to take, and the overhead warning](#part-4-what-not-to-take-and-the-overhead-warning)
- [Part 5: Order of operations](#part-5-order-of-operations)

---

# Part 0: The split-brain problem, read this first

This outranks everything else in this document, including anything borrowed from my cousin.

## What is happening

**Four governing documents exist twice, with the same filenames and different contents.** The daily scan reads the project copies. Obsidian shows me the vault copies. Anything I change in Obsidian, the scan never sees.

| Document | Vault copy | Project copy |
|---|---|---|
| `Job-Fit-Scoring-Rubric.md` | 1,253 bytes. Weighted table only. **No hard filters.** | ~6,500 bytes. Eight hard filters applied before scoring, PASS/STRETCH classification, over-filter guardrails, rejection logging |
| `Job-Description-Analysis-Template.md` | 9 fill-in sections | 13-phase workflow |
| `Master-Profile.md` | Last written 2026-09-09 | Updated 2026-09-14 |
| `Resume-Formatting-Spec.md` | **Not in the vault at all** (sitting loose in Downloads) | Updated 2026-09-15, today |
| `Interview-Story-Library.md` | **Not in the vault at all** (loose in Downloads) | In the project |
| `Jobs-Seen-Log.md`, `Greenhouse-Target-Boards.md` | Do not exist in the vault | The live operational memory of the whole scan |

**The vault copies of the Frameworks and Profile files froze on 2026-09-09 at 9:42pm.** They have not been touched since. The project copies have been evolving continuously for six days, including one edited an hour ago.

So the vault is not my system. **The vault is a six-day-old fork of my system**, and it happens to be the copy I look at.

## Why it happened, mechanically

The `Daily Applications` scheduled task has **no folder bound to it.** Its folder state is empty. It physically cannot write to the vault, so it writes everything into project docs, because that is the only persistent surface it has.

Compare the other task. `Inbox scan` **is** bound to `C:\Users\rkyle\ai-career-system`, and it writes into `Inbox-Digests/` successfully three times a day, every day.

That is not a coincidence, it is the whole explanation. It is also, exactly, the warning in my cousin's `Scheduled-Tasks.md`: *a cloud task can reach a folder on my computer only if the task is bound to that computer.* I assumed that warning was a problem I had not hit yet. It is a problem I have been living inside since 2026-09-10, and it is why `Jobs/` has no notes after 9/11 while the scan kept surfacing and applying to roles through 9/15.

## The second mechanical finding: the approval gate is eating the scan

The `Inbox scan` task runs with **automatic approval**. The `Daily Applications` task does not.

Today's run log records the consequence in its own words: every WebFetch to `boards-api.greenhouse.io` and `glassdoor.com` returned `PROVENANCE_REQUIRED`, described as *"an approval gate with no live user available to answer it in this unattended scheduled run."*

That is not an Anthropic bug to escalate, which is what the log recommends. **It is a setting on my own task.** An unattended run with approvals on will block on every fetch that needs one, forever, because nobody is there at 9am to click approve.

## What it cost, measured

Today's scan, 2026-09-15:
- **Indeed: zero listings.** The connector failed to dial entirely. Not a rate limit this time, a dead connection. It had been rate-limiting on every run since 9/11.
- **Glassdoor: zero listings.** Killed by the approval gate.
- **Greenhouse: partial.** The documented API endpoint was killed by the approval gate across all 29 boards. The run rebuilt the whole source on the fly using WebSearch plus individual page fetches, which half-worked.
- **Net result: 4 listings, all Track A, zero Track B.**

The run was honest about all of it, which is genuinely good and is more self-reporting than most systems do. But two of three sources were destroyed by a checkbox, and Track B went dark for a day partly as a result.

## What to do about it

**Today, about fifteen minutes:**

1. **Reconnect the Indeed connector.** It is dead, not throttled, and no future run can fix that from inside.
2. **Set `Daily Applications` to automatic approval.** This alone restores Glassdoor and the documented Greenhouse API path.
3. **Bind `Daily Applications` to this computer**, the same way `Inbox scan` is already bound.

**Then, the real decision: pick one home for each document.** Not both. My cousin's third firewall law is the right rule here and I violated it without noticing: *never write the same fact into two files, duplicates are how contradictions are born.*

Two coherent answers, and either beats what exists now:

- **Project-canonical.** The four contested governing docs live in the project, the scan keeps reading them, and the vault stops carrying copies. The vault keeps what the project is bad at: `Jobs/` notes, `Companies/`, `Interview-Prep/`, `Networking/`, resume PDFs, and the wikilinks and Dataview queries that connect them. Lowest effort, works with the grain of what already happens.
- **Vault-canonical.** Bind the task, move the governing docs into the vault, and have the scan read them from the bound folder. Gets me git history on every rule change, which is the thing the project cannot give me and which my cousin has no answer for either.

Either way, **delete the losing copy.** A stale duplicate that still opens in Obsidian is worse than no copy, because it looks authoritative.

My read: bind the task and go vault-canonical, because version control on the rules is worth real money over the next few months and the Frameworks files are exactly the kind of thing I will keep tuning. But project-canonical is defensible and faster, and the wrong move is leaving it as it is.

---

# Part 1: What his system actually is, all of it

28 files. Here is the whole inventory, including the parts I skipped the first time. I have marked each one: **[TAKE]**, **[LATER]**, **[SKIP]**, or **[FYI]** for things that are just worth knowing about.

## 1a. The governance layer (the part that makes it a system)

**`CLAUDE.md`, the router. [TAKE]**
Read automatically every session when the folder is connected. Deliberately short. Contains: session startup order, file-size rules, naming rules, the context firewall summary, style rules, and pointers to everything else. His rule for what belongs in it: only rules that can be broken *silently* inside any task.

**`00-Inbox/Task-Queue.md`, the dispatch file. [LATER]**
Read first every run, before anything else. Sections: Queued, In Progress, Blocked, Done. Fixed header that never moves, newest entry directly below it. Rotation: Done items older than 30 days go to an archive.
I left this out the first time because Obsidian Kanban plus Dataview already covers it for me, and better.

**`00-Inbox/Complaints.md`, live corrections. [TAKE]**
Everything under OPEN is a live instruction, not history. Rules: when I complain in chat, log it immediately, in my words, before doing anything else, and do not clean up the wording because the anger is data about severity. **A complaint that appears twice is a broken rule, not a mistake.** Find the file that should have prevented it and check whether the rule is actually there, actually findable, and actually unambiguous. Most repeats are a rule buried in the wrong file, written as a soft preference, or contradicted somewhere else.

**`00-Inbox/Context-Memory.md`, session-to-session memory. [SKIP]**
Preferences, decisions, open loops, stable facts, plus a dated session log. Critically, it carries its own limit: **this is a LOG, it never feeds words into any output.** It only points at where rules live.
I skipped it because Claude's own memory plus my project files already do this, and a second memory that is allowed to be read but not used is a foot-gun.

**`00-Inbox/Context-Firewall.md`, the authority map. [LATER]**
Three laws: (1) every output type has exactly one governing file, and a fact is usable only if it is in that file, never because I said it once in a session; (2) when chat memory and the file disagree, the file wins, and if I contradict the file in chat, the file gets updated in the same turn; (3) new facts get written into their one governing file before being used, never into two.
Then a table: resume is governed by RESUME-STANDARD, application answers by Answer-Bank, biographical facts by Profile, LinkedIn DMs by the playbook plus voice register B, email by voice register A plus the cold email playbook. Each row also names what is explicitly *not* a source.
The tell he gives for when the firewall is about to break: reaching for a phrase and being unable to point at which file it came from. "Sounding right from memory is exactly the failure mode this file exists to kill."

**`00-Inbox/Verify-Protocol.md`, four subagent gates. [LATER, one part TAKE]**
Covered in detail in Part 2.

**`00-Inbox/Voice-Edits.md`, the voice correction log. [TAKE]**
Every time he edits or rejects a draft: what the draft said, what he changed it to and why, and the rule that implies. Read the top 5 before drafting anything. Once a rule shows up twice, it gets folded into Voice-Corpus. Keeps 20 entries, rest archived.

**`00-Inbox/Scheduled-Tasks.md`, the automation register. [TAKE]**
The live task list and the execution-environment warning. Covered in Part 2.

**`00-Inbox/scripts/vault-lint.py`, the mechanical gate. [TAKE]**
Covered in Part 2.

**`01-Daily/`, daily notes. [SKIP]**
Dated notes with a "For Claude" section whose tasks get migrated into Task-Queue at session start. My Inbox-Digests already occupy this slot.

## 1b. The job-search engine

**`Job-Search/Job-Search-AGENT.md`, the workflow. [TAKE the shape, rewrite the content]**
23KB, by far his biggest file, and the actual operating manual. Nine steps, read top to bottom at the start of every run:

- **Step 0, liveness check.** Before sourcing anything new, open the live URL of every pending role queued 4+ days ago. Gone, 404, or redirected to a talent-community page means move it to Dead-Companies with a one-line reason. Never present a queue that has not been liveness-checked this run.
- **Step 1, source.** Pull from the configured boards, top-choice first. Read the req *body*, never score off the title. Dedupe against three surfaces. Parse comp mechanically.
- **Step 2, score.** 0 to 100 rubric, then a tier: Strong fit queues, Maybe goes to a bench list with no work done, Skip gets logged and dropped.
- **Step 3, attach the master.** No tailoring, ever.
- **Step 4, queue for approval.** Two places: one row in the index, full detail in a per-company file. Every field required, write `unknown` rather than omit.
- **Step 5, submit.** Only after approval. Move to Applied only on the Gmail confirmation, never on the agent's say-so.
- **Step 6, Gmail sync and follow-up.**
- **Step 7, multithreaded prospecting.**
- **Step 8, Glassdoor intel.**

**The submission-mode machinery. [FYI, mostly not applicable to me yet]**
This is the part I left out entirely, and it is the most hard-won section in his kit. All of it is scar tissue from browser automation:

- **Test `file_upload` once at the top of every run.** If it errors, the entire run is a "hand-off run" and the summary says so in its first line. Do not find out per-application.
- **One new tab per application, and never navigate a tab you have already filled.** Moving to the next form in the same tab wipes every earlier form.
- **Write a batch manifest** to a dated `Handoff-YYYY-MM-DD.md`: one row per form with company, role, exact URL, which tab it lives in, which master to attach, and every field left blank. If a tab dies, the manifest is how the form gets found again in one click.
- **The attach ladder.** `file_upload` with the master PDF, or stop and hand it over. Cloud-picker buttons (Google Drive, Dropbox) render inside cross-origin iframes the agent cannot click. His line: **"a route that renders is not a route that works, and a fallback is only real once it has attached a file end to end at least once."**
- **Never submit without the actual PDF.** Pasted plaintext into an "enter manually" box is never a route.
- **Every route except direct upload reads a copy of the resume stored somewhere else, and copies go stale silently.** LinkedIn Easy Apply attaches whatever LinkedIn has saved at `linkedin.com/jobs/application-settings/`, not the vault file.
- **Form mechanics, learned the hard way.** Ashby's React inputs ignore programmatic input; the native value setter plus a bubbling `input` event sticks. Ashby's Location combobox wants a *state*, not a city, and needs real keystrokes plus a click on the option. Greenhouse custom comboboxes need a click, then a click on the option. Verify the resume filename rendered in the field before claiming it attached.
- **Board APIs.** `boards-api.greenhouse.io` and `api.ashbyhq.com` are CORS-blocked from a job page. Navigate the tab to the API origin first, then fetch same-origin. A 404 against a board token usually means the wrong token, not a dead company.
- **LinkedIn lazy-load.** After navigating to a job view, wait 4 seconds, then read. If the body is missing, scroll down 5 ticks, wait 3, scroll back up, wait 4, read again. After 2 retries, log "JD unreadable" and move on.

That last one is directly relevant to me even without automation, because it is the failure mode behind my Macquarie note.

**The honeypot rule. [TAKE, on principle]**
Any application that explicitly asks whether an AI is filling it out, or contains an anti-bot honeypot, is never submitted *by the agent*. He is careful about the framing: this is not a block on the application, only on who clicks submit. Fill every other field, leave the honeypot blank, hand it over. His reasoning: when the agent is the thing clicking submit, typing "no" there is the agent making a false statement to a third party who asked plainly.

**Auto-submit conditions (a) through (g). [SKIP for now]**
Seven conditions that must all hold. Direct ATS only, never LinkedIn Easy Apply or Wellfound because their terms prohibit bot submission. Location clears, comp clears, target seat, not already in the tracker or dead list, no tie to the current employer, and no long-form work (video, timed assessment, take-home, writing sample, salary-history field, multi-part essays past ~150 words).
Notable carve-out: **short free-text fields are the agent's job, not a blocker.** Why this company, why this role, tell us about yourself, proudest win, desired comp. Structure is always **experience first, then interest**, from recorded facts only.

**`Job-Search/Search-Config.md`, targeting. [TAKE]**
Mode, seat tiers, comp gate vs comp target, buyer segment, org profile, top-choice list, scoring rubric, hard screens, locations, sources, keywords, blocklist, volume guardrails. All in one editable file with a rule against appending dated paragraphs to it: change the rule in place and note the date in the frontmatter.

**`Job-Search/Profile.md`, canonical facts. [ALREADY HAVE, better]**
My Master-Profile is a superset of this.

**`Job-Search/Answer-Bank.md`, reusable application answers. [TAKE]**
Form basics, comp rules, common screening questions, a "why this company" pattern, and a flag-for-me list of things the agent never answers. The important mechanic: **a new answer written for one application gets saved here, then reused from here**, so the bank grows as a byproduct of applying instead of as a separate chore.

**`Job-Search/Voice-Corpus.md`. [TAKE]**
Covered in Part 2.

**`Job-Search/Resume/RESUME-STANDARD.md`. [PARTIAL]**
Mostly the never-tailor rule, which I am rejecting. Three things in it are still worth having: record the md5 of each master after every rebuild so a run can tell a rebuilt master from a stale one; a "copies live elsewhere and go stale" section that tracks the date each external copy was last replaced; and a "standing facts that must not drift" list.

**`Job-Search/Tracker.md`, `Run-Log.md`, `Stats.md`, `Maybe-List.md`. [PARTIAL]**
Tracker is a hand-maintained index, capped at 200 lines, detail forbidden in it. Dataview does this better for me. Run-Log and Maybe-List I do want, covered later.

**`Job-Search/Outreach-Queue.md`, `Outreach-Log.md`, `Companies/<Company>/Outreach.md`. [LATER]**
The prospecting engine, covered in Part 2. Two rules from the queue file worth quoting now: **"Delivered is not sent."** And a draft not delivered to chat within one run of being written gets marked `skipped`, not carried forward. The live queue never grows past five.

**`Job-Search/Archive/Dead-Companies.md`, the dedupe memory. [TAKE]**
Two tables: companies applied to or interviewed with, which keep a folder; companies never touched, which get a name and a date and nothing else. Explicitly part of the dedupe surface, read during sourcing. Also: a company here is not blocked, only done. Truly never-again companies go in the blocklist instead.

## 1c. The playbooks

**`Playbooks/LinkedIn-Message-Playbook.md`. [LATER]**
Ten IF/THEN rules, derived from LinkedIn's own outreach courses. The core ones: target under 400 characters, not the email word band, because the shortest InMails get the highest response rates and only about 10% are under 400 characters, so short is rare and rare is the advantage. Cut anything the reader could disqualify on. **If the personalization is about the job posting, it is not personalization, it is what everyone else sent.** Intro to yourself is one sentence and it must connect to why you reached out. Make the ask 15 minutes or smaller, and on a first touch "want me to send it?" beats "can we talk?" Follow up 2 to 3 times every 2 to 4 days, then switch channel, then monthly. Silence usually means distracted, not uninterested.
Plus a four-part formula: personalize, one-sentence intro, give them a reason to be glad they opened it, one small ask.

**`Playbooks/Cold-Email-Structure.md`. [LATER]**
Derived from a LinkedIn Learning course on cold email prospecting. Three body structures (BAB, PAS, PPP) with PPP as the default for hiring managers. Single low-effort CTA phrased as interest, not calendar time. Chase cadence: day 1, +2 working days, then doubling (day 3, 7, 15, 30), because three or more chases returns roughly 27% response versus 9% for fewer. **"If the message is about me, it is wrong"** with a mechanical test: count sentence openings, if more than two of five start with "I," rewrite.
The most interesting thing in this file is the **conflict table**, where he documents exactly where the source material loses to his own voice file. Example: Croft says open with praise. His Rule Zero says a sentence doing rhetorical work is wrong. Voice wins, because **praise is exactly where AI drafts produce flattery**, and the safe version is a specific factual reference to something the person published with no adjective attached.

## 1d. Cross-cutting rules I left out

**The "motion" concept. [TAKE, adapted]**
Runs through the entire system. Name the motion, not the label: land/new logo, expansion/upsell, renewal, partner. What is being sold, to whom, against which incumbent. **If that cannot be stated in one sentence, there is not enough research to draft yet.** Then match the credential to the motion: outbound and dial volume are LAND credentials, do not spend them on an EXPANSION seat.

**The activity-metric rule. [TAKE]**
Never quote an industry cold-call connect rate, answer rate or dial benchmark in anything sent in his name or said in an interview. Vendor blogs disagree by an order of magnitude and none cite a primary source. Quote his own numbers only, **average first, then peak, and never "meetings set" without "attended" beside it.**

**Comp rules, nine of them. [TAKE]**
Covered in Part 2.

**The unpaid work-trial hard no. [TAKE]**
Unpaid or vague multi-day work trials, trial weeks, auditions and "working interviews" are a hard no. A single paid half-day or a scoped take-home is fine.

**Failure fallbacks. [TAKE]**
A dead source is skipped and noted, never fatal. If the browser is unavailable, run with search and fetch only and say so. If a step is ambiguous, do the conservative thing: bench it, flag instead of guess, never submit. Every run ends with a log entry even with zero fits, because **a run that left no trace did not happen.** Never build a scheduled task off an offhand remark, because a remark about the future is not an automation request. And: **when the agent makes a mistake, name it, fix it, stop talking. No impact assessment, no context, no spin.**

**Dead contacts list. [TAKE, trivially]**
Never draft, never suggest, never resurrect anyone on it.

---

# Part 2: What to take, why, and how it works in practice

## 2.1 A `CLAUDE.md` at the vault root

**Why.** Right now, every Cowork session I start begins from zero. Whatever Claude knows about my rules comes from whichever files happen to get read that session, or from me retyping "use Master-Profile, don't invent anything, no em dashes." My README.md is addressed to a human reader and does none of this work.

**How it works in practice.** When a folder is connected, Claude reads `CLAUDE.md` at its root automatically, at the start of the session, before doing anything. It is the only file with that property. So it is the one place where a rule applies to *every* session without me remembering to say it.

Concretely, what changes: I open a session, say "find me energy roles today," and Claude already knows to read Master-Profile for facts, to run the gap map before tailoring, that ERCOT operating experience is a never-claim, and that anything I need to act on comes to chat and not into a file.

**What goes in it.** Use his tier test, which is the genuinely useful idea here:

- **Tier 1, CLAUDE.md:** rules that can be broken *silently* inside any task. No em dashes. Never invent experience. Never infer an email address. Delivery, not storage. Master-Profile is the only source of biographical fact.
- **Tier 2, a Frameworks file:** anything scoped to a nameable situation. How to score a posting. How to build a resume. How to analyze a JD. These already exist for me.
- **Tier 3, the lint script:** anything a string match can check.

The failure mode this prevents is the one I would otherwise walk into: dumping every rule I have into CLAUDE.md until it is 400 lines, at which point it gets partially read and the rules at the bottom stop existing. His own note on that: **"when a rule gets missed because it lives in a file over 300 lines, that is an architecture defect, not a discipline failure. Fix the structure. Do not add another MANDATORY line."**

**Also worth knowing before I build anything:** Cowork does not load hooks, `.claude/agents/` or `.claude/skills/` from a connected folder. **The folder is data, never configuration.** Rules only work because CLAUDE.md tells Claude to follow them, not because any runtime enforces them.

## 2.2 Delivery, not storage

**Why.** This is the rule that fixes a failure already sitting in my vault. The 2026-09-15 inbox digest ends with a real action: apply to LCRA Energy Analyst II, closest fit to the ERCOT track, and it names Peyton Homann as a 1st-degree connection there to reference. That is a warm intro on a Track B role. It is in a file I have to remember to open.

**The rule.** Writing to the vault is storage. It is not delivery. A file I have to remember to open is not a notification. Anything I am expected to act on comes to me **in chat, in full, at the moment I can act on it.** Never "it's drafted," never "it's ready," never "it's in the queue," never a file path in place of the thing itself. Put the actual text in the message and say exactly what I have to click. Cap it at 3 items a day. Every person, posting, form or profile named in chat ships as a clickable link on the name, and if there is no verified URL, say so rather than writing a bare string.

**How it works in practice.** This is a change to *task prompts*, not just to a rules file. My inbox-digest task currently ends by appending to the dated digest file. The new ending is: also reply in chat with at most 3 items that need my hands, each with the full text inline and every link clickable, and say exactly what to click. If nothing needs me, say so in one line.

The cap matters more than it looks. Without it, a "deliver everything actionable" rule turns into a 20-item wall every morning, which is functionally the same as storage because I stop reading it.

## 2.3 Gmail as the dedupe source of truth

**Why.** The vault does not know what I have applied to. Downloads holds roughly 60 tailored resume PDFs. `Resume/Versions.md` has 2 rows. `Application-Record.md` has 5 rows with "(fill in)" in every date and outcome column. The `Jobs/` folder has 8 notes. Those four numbers should be one number.

His framing is the right one: **every ATS sends a confirmation, so the inbox knows what I have applied to before any file does.** And critically, it stays true even when I apply on my own without touching the vault, which is exactly what I do.

**How it works in practice, two separate uses:**

**Use one, the gate.** Immediately before building a resume or submitting to any company, search Gmail for that company name over the last 7 days. Any hit means stop, log it, move to the next candidate. One search per candidate. The cost is small; the saving is not building a tailored resume for something I applied to last Tuesday.

**Use two, and this is the one I should do first: a one-time backfill.** Search Gmail for all ATS confirmations over the last 60 days and reconstruct the true application history from them. That gives me a real Versions.md and a real set of Jobs notes, built from evidence rather than memory. Right now I could not tell you with confidence which of those 60 PDFs actually went out. The inbox can.

His related rule is worth taking too: **a row moves to "Applied" only when Gmail shows the confirmation, never on anyone's say-so.** Status becomes evidence-based rather than intention-based.

## 2.4 The scheduled-task execution warning

**Why.** I am about to turn on a daily scan. This is the thing that would have silently broken it.

**The warning.** A Cowork scheduled task **runs in the cloud by default, and a cloud session can reach a folder on my computer only while the desktop app is open AND the task is bound to that computer.** A task that is not bound fires on schedule, sends its completion notification saying it ran, and writes nothing to the vault. Silently. I would see "task completed" every morning and an empty Jobs folder, and I would spend a week debugging the prompt.

**How it works in practice:**
- Account level: Settings, Cowork, "Only on this computer."
- Task level: bind the folder in each individual task's setup.
- In the task prompt itself: **Step 0 is always "list the vault root; if it is not reachable, reply with one line saying so and STOP."** Never reconstruct from chat context, because a run that cannot read Master-Profile and the formatting spec is drafting blind.
- One more gotcha: a bound task's prompt is effectively immutable from chat. To change it, create a new task.
- And: never edit a task time from memory, list the live tasks and match.

That Step 0 pattern generalizes. His CLAUDE.md opens with the same check for interactive sessions: verify the vault is reachable before anything else, say so in one line if it is not, and never silently proceed on chat context. His reasoning is exactly my situation: **anything written in my name while the vault is unreachable is drafted blind, because the voice file, the profile and the tracker are all unavailable. Proceeding anyway is my call, not Claude's to make by omission.**

## 2.5 Voice-Corpus from real sent mail

**Why.** He calls this "the single biggest quality lever in the whole system," and I think he is right, for a reason that is specific to me. My current voice guidance is a *description*: articulate, good vocabulary, varied sentence structure, some personality, not stiff, not dumbed down, no em dashes. That description is accurate and it is nearly useless as a generation target, because every AI draft will claim to satisfy it. A description cannot be failed. A word count can.

**How it works in practice.**
1. Pull 3 to 5 messages I **actually sent** to a recruiter, hiring manager or stranger. Real ones, not ones I would like to have sent. I have material: roughly 15 cover letters in Downloads, plus any recruiter correspondence in Gmail.
2. Paste them verbatim under GOLD SAMPLES. Redact the other person's name if needed, keep every word of mine.
3. **Count them.** Total words per sample, number of paragraph blocks, words per sentence. Write the ranges down as bands.
4. Note the observable facts: greeting, sign-off, contractions or not, exclamation points, how I ask for things, how I concede a weak point.
5. Every time I edit a draft afterward, log it in Voice-Edits. Once a rule appears twice, fold it into the corpus.

The bands are what make it enforceable. "Sounds like Riley" is unfalsifiable. "Runs 95 to 160 words in 4 to 5 blocks" is a check the lint script can run.

**Two of his rules I want verbatim.** First, **Rule Zero: if a sentence is doing rhetorical work, it is wrong.** The test is whether I would type it into my phone in ten seconds without thinking about it. If it took craft to build, it reads as craft, and craft in a short message reads as AI. Second, **an intro is an intro, not a pitch.** One credibility line, not the stat block. Numbers are interview and reply material.

Note the interaction with my stated preference for articulate prose: these are not in conflict, but they apply to different things. Cover letters get the full articulate register. A first-touch LinkedIn note does not, and that is where Rule Zero earns its keep.

## 2.6 `vault-lint.py`

**Why.** I have a no-em-dash rule. I have a never-claim list in Master-Profile Section 14 (ERCOT operating experience, ETRM, pipeline scheduling, gas nominations, Salesforce, HubSpot, NetSuite, Python, SQL, DAX, enterprise sales, significant cold calling, DCF/NPV/IRR, engineering credentials). Both are currently enforced by hope. A string match enforces them for free.

His honest framing of why the script exists at all: **Cowork fires no lifecycle hooks, so there is no runtime layer that can block a bad draft.** The check has to be a script a session *chooses* to run. "The choosing is not deterministic. Everything after the choosing is."

**How it works in practice.** It is a Python script I run from bash inside a session:

```
python3 00-Inbox/scripts/vault-lint.py --file draft.md --email
```

Exit 0 clean, 1 warnings, 2 fail. Nothing that exits 2 ships. What it checks:
- **Em dashes and en dashes**, with line numbers and surrounding context.
- **Email provenance.** For every address in the draft, grep the whole vault for it. **Zero occurrences elsewhere is the tell for an inferred address**, which is a clever check: a real address got into the vault from a real message, an invented one has no history. One or two occurrences is a warning for thin provenance.
- **Banned strings.**
- **Naked LinkedIn URLs** that should be markdown links on a person's name.
- **Word band** against the Voice-Corpus numbers, with `--email`.

**My highest-value adaptation:** put the Section 14 never-claim list into `BANNED`. That converts my most important rule, the non-invention rule, from a policy into a mechanical check. If a draft or a tailored resume contains "Salesforce" or "ERCOT operations experience," the script fails it before I ever look at it.

**Second mode worth having:** `--pipeline`. It takes no draft. It scans company files for statuses the vault cannot actually vouch for, matching phrases like "awaiting confirmation," "not confirmed," "status unclear," "no reply logged," and flagging anything older than 21 days that still claims a live stage with no recorded outcome. The principle underneath it is one of his best lines: **"a status field that admits it does not know is a QUESTION FOR THE USER, never a fact."**

Side benefit: Python is a GAP on my Skills-Gap-Tracker. Adapting a 200-line script I understand the purpose of is a better first Python project than a tutorial.

## 2.7 Complaints.md and Voice-Edits.md

**Why.** My own working notes already say the iterative correction to immediate documentation loop is the core system-building method. I believe this and I have no file for it. Corrections currently live in chat transcripts, which means they evaporate.

**How it works in practice.** When I push back on something in a session, Claude logs it before continuing, in my words, not cleaned up. Over a few weeks that becomes the actual training data for the system, and the accumulation is what makes it improve rather than reset.

The heuristic that makes it more than a diary: **a complaint that appears twice is a broken rule, not a mistake.** The second occurrence is a signal to go find the file that should have prevented it and ask whether the rule is actually written there, actually findable, and actually unambiguous. Most repeats turn out to be a rule buried in the wrong file or written as a soft preference.

## 2.8 Dedupe memory and a real Search-Config

**Why dedupe.** My Jobs folder holds live work only. Nothing records what I skipped or why, so a daily scan re-surfaces every skipped company forever, and I re-evaluate the same 20 roles weekly.

**How it works.** Three surfaces checked during sourcing: live work (Jobs, via Dataview), done (`Dead-Companies.md`), benched (`Maybe-List.md`, pruned at 30 days). Plus a blocklist in Search-Config for never-again companies, where **a blocklist hit is absolute**. His archive hygiene rule: a company never applied to gets one name-and-date row and its folder is deleted; only applied-to or interviewed-with companies keep a folder. Never write a multi-paragraph tombstone.

**Why Search-Config.** My targeting parameters are currently spread across Master-Profile Section 2, the scoring rubric and project memory. His consolidates all of it into one file I edit to retarget, with a rule against appending dated paragraphs: change the rule in place, note the date in the frontmatter.

**Two mechanisms in it worth taking specifically:**

**A top-choice list that bypasses the rubric entirely.** Board-check those companies every run and surface any relevant seat they post, regardless of comp or seniority read. "I decide, not the rubric." A rejection does not remove a company from the list. For Track B this is how I get in front of ERCOT-adjacent shops that post rarely and would score mediocre when they do. His related note: warm humans on that list outrank any cold application, and never let one go quiet without saying so in the run summary.

**Comp parsing as a mechanical rule.** A stated "base" range clears the gate only if its *bottom* clears. **OTE is not base.** If a split is given, compute base as OTE minus commission; if not, base is unconfirmed. Contract, commission-only and no-base roles always fail. My rubric has a $50k floor and no rule for how to read a posted range, which is exactly how a "$70k OTE" role gets scored as if it cleared my bar. Fisher, for instance, posts "salary plus uncapped commission" with no base stated, and my Jobs note records that honestly but my rubric has no procedure for it.

## 2.9 Comp conversation rules

**Why now.** I have a comp floor. I have no rules for a comp *conversation*, and I have a live second round at Fisher.

**The ones that matter most:**
- **Band width is the read, not the number.** Before any comp conversation, pull the company's whole job board and compare the target req's spread against its other reqs. **A spread under about 8% of the minimum is a fixed pay grade the recruiter cannot override.** Stop negotiating base, move to variable, promotion timeline and start date.
- A posted range at a covered employer is a compliance artifact, not an opening position. Do not build a plan around breaking the posted max.
- **An unquantified "bonus component" is the first question, not the base.** Ask what it pays at target.
- **Judge a closing seat by OTE, never base alone.** SDR seats run roughly 70/30, AE seats roughly 50/50. A range labelled OTE is not base.
- Never quote a comp benchmark without its survey year.
- When the base gap to the floor is small and the next rung on the same board is much higher, spend the call on the promotion path instead.
- Check whether the state has a salary-history ban before coaching anything about "what do you make now." Where there is no protection the move is the same anyway: give no number, redirect to the posted range. **Never coach calling a question illegal unless verified.**
- Never call a req "the promotion path" because it is the only senior title on the board. If the actual next rung is not posted, say it is unknown.

## 2.10 The verification gates

**Why.** His argument for using a *subagent* rather than just re-reading the file is the non-obvious part, and it is correct: **by the time a draft exists, the session is too full to read the file that governs it.** A main session deep into a run will `head` the voice file and not mention that it did. A subagent that starts empty and has one job reads all of it. **"It is not smarter. It is emptier, and emptier is the whole advantage."** The subagent never rewrites; it returns a verdict and line-level objections, and the main session does the fixing.

Four prompts, each with a fixed return shape so the output cannot wander:
- **VOICE:** reads Voice-Corpus and Voice-Edits in full, checks length and block count against the bands, quotes the rule and date for every objection.
- **CLAIMS:** extracts every factual assertion and tags it VERIFIED with a source, UNVERIFIED with what was tried, or WRONG with a quote from the source. **Facts about me come from Profile and that is canon; facts about the outside world must come from a live fetch, never from memory.**
- **STATUS:** for every claim asserting a state (sent, submitted, booked, confirmed, replied), find the evidence. Acceptable: I said it in plain words and you quote me, a verbatim artifact in the vault, or a live fetch this run. Not acceptable, each a fail: an inference from a short aside, a prior file asserting the state without its own source, or **an action that was drafted or queued being read as an action that was taken.**
- **FIREWALL:** checks that every fact in a draft came from the governing file for that output type.

**The one to take first is CLAIMS**, because it would have caught a failure already in my vault. My Macquarie note records it in its own body: the Indeed connector hit a rate limit, so the role was scored 89 and a resume was tailored from **search-preview data plus general knowledge of Macquarie, not the full job description.** A CLAIMS gate asks "cite the source for each assertion, outside-world facts must come from a live fetch" and would have stopped the resume build rather than documenting the problem after the resume already existed.

**Practical cost note from his own file:** the VOICE gate reads the voice files in full and can burn 100k+ subagent tokens. Budget one per new person, not one per revision. Re-run only after a substantive rewrite. The CLAIMS gate is much cheaper and worth running more often.

## 2.11 The outreach engine

**Why.** `Networking/Contacts.md` has one row. And the LCRA warm contact is sitting unused in a digest file.

His framing is the part that should land for Track A specifically: **a sales application IS the first rep test. A single-threaded pursuit is the behavior you would coach a rep out of.** If I apply to an SDR role and do nothing else, I have demonstrated the exact behavior the job exists to not do.

**How it works in practice:**
- **Read the JD for an outreach invitation first.** Some postings literally say "we encourage you to prospect us." When that language exists, outreach is being scored, and a silent application is a failed test. Surface it before submitting so both go out together.
- **3 to 5 contacts per role:** the hiring manager the seat reports to; that manager's boss at a large enough company; the recruiter named on the posting or the confirmation email; **one or two reps already in the seat**, who are the lowest-friction reply and the best intel on ramp, quota and comp; founder at seed and Series A only.
- **Sequence it, do not blast.** Recruiter and hiring manager on the day of application. Peer rep two to three days later. Second-line leader only after a week of silence. **Vary the angle per contact, never the same paragraph twice inside one company.**
- **Draft by default on every channel.** The agent never sends on its own initiative, and scheduled runs are always draft-only. In a live session it sends only when told "send" on a specific named message, showing the final text and recipient first. **"Take the lead" never means send.**
- **Never infer an email address.** Real only when it came from a real message, a signature block, or a published page. Otherwise the entry reads "no verified email." A wrong address is a hard bounce that burns the company.
- **Check LinkedIn sent invitations first** (`linkedin.com/mynetwork/invitation-manager/sent/`), because I prospect in parallel and that page is the authoritative record of who has already been reached. Caveat he includes: that page lists pending invitations only, so check the profile or thread before concluding nobody has touched a contact.
- **Deliver it, cap 3 a day, full text in chat.** A draft in a file is not done.

---

# Part 3: Synopsis, pros and cons, both systems

## The one-line version

**His is an operating system for an unattended agent. Mine is a precision content engine for a human.** Neither is a better version of the other; they are solutions to different bottlenecks.

## Why they diverged

They diverged because the two of us are stuck on different things.

**His bottleneck is volume and consistency**, with a track record that already qualifies him. He needs to touch a lot of companies, never contradict himself, never let an agent invent something while he is asleep, and never lose track of what went where. So: standardize the output, forbid tailoring, gate everything, log everything, deliver to chat because he is not going to browse a vault.

**My bottleneck is qualification and framing.** I do not auto-clear keyword screens, and I am pointing at two unrelated tracks with one background. Volume is demonstrably not my problem: 60 resumes in Downloads. Precision is. So: gap mapping, track profiles, sub-variants, a verified format spec, a rubric that weights growth over comp.

Which means the merge is not a compromise. His governance layer does not touch my content engine, and my content engine does not need his constraints. They stack.

## His system

**Pros**
- **Governance.** A router, an authority map, a rule-tiering discipline, and an explicit theory of where a rule should live so it actually gets read.
- **Delivery discipline.** The hardest-won idea in the kit, and the one with the clearest immediate payoff for me.
- **Ground truth about state.** Gmail as dedupe source, status only on evidence, a linter that flags statuses the vault cannot vouch for.
- **A correction loop that compounds.** Complaints and Voice-Edits mean the system gets better from use rather than resetting.
- **Real operational scar tissue.** The browser and ATS mechanics, the stale-copy trap, the tab-reuse trap. Nobody derives these; you earn them.
- **A complete outreach engine**, which is a whole stage of the funnel I have nothing for.
- **Honest-agent ethics that are actually thought through.** The honeypot rule, the never-infer-an-address rule, the refusal to auto-submit where terms prohibit it. He is not just avoiding getting caught, he distinguishes filling a form from making a false statement to a person who asked plainly.
- **Verification that accounts for its own context limits**, which is a subtle and correct insight.

**Cons**
- **No content engine at all.** Two static resumes. For anyone whose background does not already match the target, that is fatal.
- **Non-invention is one sentence.** Fine when your history matches. Not enough for a career-changer.
- **Nothing after the application.** No interview prep, no story library. Glassdoor intel and then a cliff.
- **No version control.** Complaints.md is a hand-rolled approximation of a git log.
- **Single-track by design.** Search-Config assumes one seat family.
- **Not Obsidian-native.** Hand-maintained tables, a 200-line cap on the index, and a TOC-in-the-first-20-lines rule that is a workaround for a problem frontmatter and Dataview already solve.
- **Heavy.** 28 files, most append-only, each needing a rotation policy. That is real maintenance, and maintenance competes with applying.
- **Sales-specific.** Motions, OTE splits, dial volume, quota attainment. Roughly a third of the kit does not transfer to Track B.
- **Expensive.** The VOICE gate can burn 100k+ tokens per run.
- **It is a skeleton, not a system.** Every personal fact is `[FILL]`. Nothing works until Profile, Search-Config and Voice-Corpus are filled, and the quality ceiling is set entirely by how well I fill them.

## My system

**Pros**
- **A real content engine.** Verified formatting spec derived from actual PDFs rather than guessed, two track profiles with four sub-variants, a bullet bank with fixed numbers and variable selection and order.
- **Anti-fabrication as an architecture, not a rule.** DIRECT/ADJACENT/GAP per requirement, a per-tool gap tracker, never upgrade a gap to direct, plus an explicit never-claim list. This is the single strongest thing I have and it is what my situation actually requires.
- **Dual-track targeting**, solved, with routing from posting to the right resume profile.
- **A rubric matched to my goal.** Growth/learning potential at 20 and skills match at 15 are criteria he has no equivalent for, because he is optimizing comp and I am optimizing pathway.
- **Interview prep depth.** Per-interview notes, a question bank, a STAR story library. A whole stage he does not cover.
- **Obsidian-native.** Frontmatter, Dataview, Kanban, wikilinks, status tags. My index regenerates; his is maintained by hand.
- **Git.** Every rule change is a commit with a message and a diff.
- **A live inbox digest** already producing dated output three times a day.
- **Light.** Eight folders. Low maintenance cost, which means I am mostly applying rather than tending the system.

**Cons**
- **Split brain.** Four governing docs exist in two places with different contents, and the copy I read in Obsidian is not the copy the scan uses. See Part 0. This is the big one.
- **One of two scheduled tasks is unbound and not auto-approving**, which is the cause of the split brain and of today's two dead sources.
- **No router.** Every interactive session starts cold and I re-explain the rules by hand. The scheduled tasks carry their rules in a long task prompt instead, which works but cannot be edited from chat once bound.
- **No delivery contract for the vault side.** The scan does deliver its brief to chat, which is right. But the inbox digest writes actionable items into a file and they die there, which is demonstrated, not hypothetical.
- **Application history is self-reported, not verified.** The log's Applications Sent section is real and useful, but it is built from what a run remembers rather than from ATS confirmations, and it is already decaying: four entries read "exact filename not retained in this session."
- **Rules enforced by hope.** The no-em-dash rule and the entire Section 14 never-claim list have no mechanical check.
- **No correction loop.** Corrections live in chat transcripts and evaporate. The task prompt says to fold lessons back into the docs, which is the right instinct, but nothing captures the complaint itself in my words at the moment I make it.
- **Tracker fragmentation.** Application state lives in `Jobs/` frontmatter, `Versions.md`, `Application-Record.md`, the Dataview query, and now the log's Applications Sent section. Five places, none authoritative.
- **Resume sprawl outside the vault.** Generated PDFs land in Downloads, unversioned, and no external copy (LinkedIn saved resumes, Indeed, Drive) is tracked for staleness.
- **No outreach.** One contact row, and a warm LCRA intro currently going unused.
- **No comp conversation rules**, with a live second round.
- **A demonstrated data-quality failure mode.** The Macquarie note proves a resume can get built from preview data when a source degrades. Worth noting the system has since partly self-corrected: the 9/12 and 9/13 runs chose to withhold listings rather than score them on preview data, and wrote the reasoning down. That is the correction loop working.

---

# Part 4: What not to take, and the overhead warning

**1. The never-tailor rule.** Reject it. Tailoring is my edge and my formatting spec makes it repeatable in a way his system cannot do. But the reason *behind* his rule is still right, and it is showing up in my Downloads folder as 60 near-identical PDFs with no record of which went where. Take the discipline without the constraint: every generated resume gets a Versions.md row at the moment it is generated, PDFs live in the vault and not Downloads, and his stale-copy rule applies to every external copy.

**2. His numbers.** Word bands, banned strings, seat tiers, comp gates. The *method* transfers, the values are measurements of his life.

**3. The sales-motion machinery, for Track B.** Land vs expansion vs renewal, OTE splits, dial-volume credentials. Track A only. Energy analyst and operations roles need a different read and his kit has nothing for it, so that part I build myself. The underlying *idea* does generalize though: for an energy role the equivalent question is what the desk actually does (physical vs financial, real-time vs day-ahead vs settlements, which ISO), and "if you cannot state it in one sentence from the req body, you do not have enough to tailor yet" is exactly right.

**4. His confidentiality posture as written.** His whole system assumes a confidential search while employed in the same industry, with a blocklist containing his employer and its competitors. I am employed at Crunch, so a light version applies, but importing rules designed for someone hiding a search inside their own industry would add friction for no benefit.

**5. Archive sprawl.** He has archive files for run logs, outreach logs, voice edits, tasks and companies, because he has no version control. I have git. Adopt rotation only where a file would otherwise become unreadable.

**6. Task-Queue, Context-Memory and daily notes.** Kanban, Claude's own memory, and Inbox-Digests already occupy those slots.

## The overhead warning

His system is 28 files, most of them append-only, each needing a rotation policy, plus two verification gates and a linter. That is a real ongoing cost. He can carry it because the system replaced most of his manual search effort, and because he has been building it for months, one scar at a time.

**The failure mode to avoid is adopting it wholesale in a weekend and spending more time maintaining the vault than applying to jobs.**

The scan is live and has been for six days, so governance is warranted now rather than premature. But most of what I would have added is already there under other names: the log is a run log, Filtered Out is a dedupe memory, Needs follow-up is a maybe-list, and the project rubric's eight hard filters are stricter than his. **Adding his versions on top would create a second set of duplicates, which is the exact problem Part 0 is about.**

So the order is: fix the infrastructure, collapse the duplicates, and only then add the genuinely missing pieces, which are the Gmail verification gate, the voice corpus, the linter, and the correction log. Four things, not twenty-eight. His system got good by accumulating fixes to real failures, not by being designed. Mine is doing the same thing already.

---

# Part 5: Order of operations

**Tonight, before tomorrow's 9am run. Fifteen minutes, and it is the highest-value thing in this document.**

1. **Reconnect the Indeed connector.** Dead, not throttled. Zero listings today.
2. **Set `Daily Applications` to automatic approval.** Restores Glassdoor and the documented Greenhouse API path, both currently dying on an approval prompt nobody is awake to answer.
3. **Bind `Daily Applications` to this computer**, matching how `Inbox scan` is already set up.

**This week.**

4. **Collapse the duplicate governing docs.** Pick project-canonical or vault-canonical per Part 0, then delete the losing copy. Do not skip the delete.
5. **The Gmail backfill.** One session: search 60 days of ATS confirmations and rebuild the application history from evidence rather than recollection. Reconcile it against the log's Applications Sent section, then collapse `Versions.md`, `Application-Record.md` and that section into one.
6. **Add the delivery rule to the inbox-digest task prompt**, so actionable items come to chat instead of only into a dated file. The LCRA item proves the need.
7. **`CLAUDE.md` at the vault root**, for interactive sessions. Tier 1 rules only, under 80 lines. The scheduled tasks already carry their own rules.
8. **Housekeeping:** delete the two files named `....md`, and move `Resume-Formatting-Spec.md` and `Interview-Story-Library.md` out of Downloads into whichever home step 4 picks.

**Next, the genuinely missing pieces.**

9. `Voice-Corpus.md` from 3 to 5 real sent messages, with counted bands.
10. `vault-lint.py`, with Master-Profile Section 14 in `BANNED` and the bands from step 9.
11. `Complaints.md` and `Voice-Edits.md`, the correction loop.
12. Comp rules, which arguably jump the whole queue given Fisher is live.

**Later, when warranted.**

13. The CLAIMS verification prompt, used on any run where a source degraded. Today would have qualified.
14. `Answer-Bank.md`, grown as a byproduct of applying rather than written up front.
15. The outreach engine and playbooks, once the application side is running clean.

**Explicitly not doing:** adding his `Run-Log.md`, `Dead-Companies.md` or `Maybe-List.md`. The Jobs-Seen-Log already does all three jobs. Adding his versions would recreate the Part 0 problem on purpose.

## The honest summary

He has been at this longer and it shows in exactly one dimension: **he has been burned, and every scar is a file.** The Gmail dedupe gate, the delivery rule, the twice-means-a-broken-rule heuristic, the task binding warning, the tab-reuse trap, the attach-the-actual-PDF rule. None of them are clever. They are all things that went wrong once and got written down.

What he does not have is a content engine, and for someone whose background already matches the target that is a reasonable thing to not have. For a career-changer aiming at two tracks it would be disqualifying.

**Keep my content engine exactly as it is. Put his governance layer underneath it. Take the operational scars for free, because those are the expensive kind of knowledge and he already paid for them.**
