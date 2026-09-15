# Scheduled task: "Daily Applications"

**Snapshot taken 2026-09-15 evening, before any settings change.**

## Settings as of this snapshot

| Setting | Value |
|---|---|
| Trigger ID | `trig_01DJWXAFv9Daij2Pjb9mGLRA` |
| Schedule | `CRON_TZ=America/Chicago 0 9 * * *` (9:00am Central, daily) |
| Enabled | yes |
| Created | 2026-09-10 |
| Last updated | 2026-09-15 |
| **Bound folders** | **NONE** — this is the split-brain cause |
| **Permission mode** | **not set to auto** — this is why WebFetch hits `PROVENANCE_REQUIRED` |
| Connectors | Indeed, Claude_Code_Remote |
| Last run | 2026-09-15 09:08am Central, SUCCEEDED |

The two bolded rows are what the planned change is meant to fix. Compare against `inbox-scan.md` in this folder, which has a bound folder and `permission_mode: auto` and writes to the vault successfully three times a day.

## Prompt, verbatim

```
You are running the daily job scan for this project. Do the following, in order. Never invent or hallucinate a posting — every listing in your output must have a real URL returned by a tool call or a real fetch.

## 1. Search — three sources, in this order

Draw search terms from Master-Profile.md Sections 6-9 (target roles across Track A-Sales and Track B-Energy/Analytics/Operations). Run separate searches per track — don't collapse them into one vague query.

**Source A — Indeed (connector).** As before: `search_jobs` for each track, then `get_job_details` for full posting text. This connector has hit a persistent rate limit on every run since 2026-09-11, so follow the pacing rule in Job-Description-Analysis-Template.md — space out single calls, cap retries at 2-3, and move unresolved items to "Needs follow-up" in Jobs-Seen-Log.md rather than burning the run or scoring on preview data.

**Source B — Greenhouse (direct public API, via WebFetch).** Read `claude/Greenhouse-Target-Boards.md` fresh each run — it is a living list of employer board tokens, and its "How the daily run uses this list" section governs this step. For each token, WebFetch `https://boards-api.greenhouse.io/v1/boards/<token>/jobs?content=true`. Plain `curl` from the container is blocked by the egress proxy; WebFetch works. The `content` field carries the full job description, so Greenhouse listings are always scored on full text and never suffer the Indeed rate limit — prioritize this source when Indeed is throttled. Keep only postings located in Texas (Austin, San Marcos, Houston, Dallas, San Antonio) or explicitly US-remote. A 404 means the company left Greenhouse or renamed its board: skip it silently and note it in the run summary.

Once per run, also do one discovery search — WebSearch restricted to `job-boards.greenhouse.io` and `boards.greenhouse.io` with a role-and-place query, rotating through the query list at the bottom of Greenhouse-Target-Boards.md. Read new board tokens out of the result URLs and append them to that file with a one-line note on why. Do not guess tokens by company name; guessing 404s more often than it hits.

**Source C — Glassdoor (WebFetch).** Search via `https://www.glassdoor.com/Job/jobs.htm?sc.keyword=<url-encoded role>&locT=C&locId=1139761` (locId 1139761 is Austin, TX). Run it for a handful of role terms per track. Glassdoor is owned by Indeed's parent and its index largely mirrors Indeed's, so expect heavy overlap — dedupe hard against both Jobs-Seen-Log.md and today's Indeed results, and only spend real time on postings that are genuinely new. Glassdoor detail pages are unreliable to fetch; when a new Glassdoor listing looks worth scoring, get its full text from the employer's own careers page or Greenhouse board instead, and cite the URL you actually read. If a Glassdoor fetch returns a CAPTCHA, a login wall, or an empty result set, do not retry more than twice — say so in the run summary and move on.

**Cross-source dedupe.** The same req routinely appears on two or three of these. Deduplicate on employer + title + location before scoring, not after. When a listing appears in more than one place, keep the version with the best URL for actually applying — the employer's own Greenhouse posting beats an Indeed or Glassdoor redirect — and note the other sources it appeared on in one short phrase. Record the source in Jobs-Seen-Log.md's Source column.

## 2. Hard filters (apply before scoring — a fail on any of these drops the listing, no exceptions, no scoring, no mention in the brief)

Apply the full "Hard Filters" section of Job-Fit-Scoring-Rubric.md — it covers compensation, licensing/credentials, relocation, duplicates (Jobs-Seen-Log.md), door-to-door/residential canvassing, degree requirements substantially outside Riley's verified education, missing-core-qualifications (5+ yrs experience, central technical expertise, multiple simultaneous mandatory gaps), and seniority-by-actual-requirements. Read that section fresh each run — it is a living document.

For every listing dropped by a hard filter, keep a short internal REJECT_REASON note and add it to Jobs-Seen-Log.md's "Filtered Out" section (format documented at the top of that section).

## 3. Job-description analysis and scoring

For every listing that survives the hard filters, run Job-Description-Analysis-Template.md's Phases 2-4 (Job Description Decomposition, ATS/Keyword Map, Candidate Gap Map — cross-check Skills-Gap-Tracker.md for known skill/tool status rather than re-deriving it fresh) before scoring. The Gap Map classification (DIRECT/ADJACENT/GAP) feeds Job-Fit-Scoring-Rubric.md's Skills Match criterion specifically — Growth/Learning Potential is a separate, forward-looking judgment (does the role teach Riley things he wants to learn, independent of whether he's already qualified) and should be scored on its own merits, not derived from the Gap Map. Full Phase 1 company research is optional for this tier (time-permitting) — required only for the top 5 (next step). Tag each surviving listing PASS or STRETCH per Job-Fit-Scoring-Rubric.md's guidance and fold that into the one-line rationale AND into Jobs-Seen-Log.md's PASS/STRETCH column. Drop anything scoring below 60 — do not surface it, do not mention it, do not pad the list to reach 15. Preferred (non-mandatory) qualifications should almost never drop a listing below the bar on their own.

## 4. Rank and select

Sort surviving listings by score, descending. Take the top 15 (fewer is fine and expected — 15 is a cap, not a quota. Never lower the bar to hit 15). Rank purely on score; do not give a listing an edge because of which source it came from.

## 5. Resume generation (tiered, not all-15)

**Top 5 by score:** run the FULL Job-Description-Analysis-Template.md workflow (all 13 phases — company research, JD decomposition, ATS keyword map, Gap Map, positioning strategy, section rebuild, dynamic subsection headings, bullet rewriting, language mirroring, ATS quality check, recruiter test, hiring-manager test, final tailoring report), then generate a fully tailored one-page resume per Resume-Formatting-Spec.md (exact font/layout rules) and the appropriate Resume-Profile-Sales.md or Resume-Profile-Energy.md sub-variant as source material — not a fixed bullet bank to copy verbatim; rewrite/reorder/re-emphasize per that posting's positioning strategy and language, while never inventing experience or upgrading ADJACENT to DIRECT. During Phase 5, cross-check Interview-Story-Library.md for a matching STAR story and note it for potential cover-letter/interview use. The table-based grid format is the confirmed, settled choice (see Phase 10's resolution note in Job-Description-Analysis-Template.md) — don't rebuild an ATS-safe alternate version unless Riley asks for one for a specific non-Indeed application channel.

**Remaining listings (6-15):** provide the score, PASS/STRETCH tag, one-line rationale, and link only — no full resume build.

## 6. Log and deliver

Append every surfaced listing (URL, company, title, score, PASS/STRETCH tag, source, date) to Jobs-Seen-Log.md so tomorrow's run doesn't repeat it, plus the "Filtered Out" entries for hard-filter rejections. If the log's main table doesn't have a Source column yet, add one and mark pre-2026-09-15 rows `Indeed` — every row before today came from the Indeed-only scan.

Deliver the morning brief as: ranked list with scores, PASS/STRETCH tags, source, and one-line rationale each, resume file links for the top 5, everything sorted highest score first. Close with one line per source saying how many listings it produced and how many survived dedupe, so it's visible over time which sources are actually earning their place.

If fewer than 3 listings clear the 60-point bar today, say so plainly rather than lowering the threshold to fill space.

## Tuning this over time

When a surfaced listing turns out to be a bad fit despite a good score, or a good listing gets filtered out, tell Claude why in a normal chat in this project. Ask for the reason to be folded into the Hard Filters or weighted-scoring table in Job-Fit-Scoring-Rubric.md, or into the tailoring workflow in Job-Description-Analysis-Template.md, or into Skills-Gap-Tracker.md if it's a skill/tool status change, or into Greenhouse-Target-Boards.md if it's a company worth adding or dropping — the same way this filter set, the tailoring workflow, and the formatting spec were captured after earlier sessions. The task prompt above and every project doc it references are living documents, not one-time setups. When in doubt about where a fact belongs: Skills-Gap-Tracker.md is the single source of truth for what Riley does/doesn't have (Master-Profile.md, both Resume-Profile files, and this rubric's hard filters all point to it rather than each keeping their own copy) — update it there first.
```
