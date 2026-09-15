<!-- SNAPSHOT 2026-09-15 of the PROJECT copy. Frozen. Not the live file. -->
# Job Fit Scoring Rubric

Score every posting 1-5 on each criterion, multiply by weight, sum for a total /100. Use to compare postings at a glance in each Job note.

Apply the **Hard Filters** section below FIRST, before scoring anything. A listing that fails any hard filter is a **REJECT** — dropped entirely, never scored, never surfaced, never mentioned in the brief (except a short internal `REJECT_REASON` note so Riley can see why, per the log format at the bottom of this doc). Only listings that survive the hard filters get scored against the weighted table.

---

## Hard Filters (reject before scoring — no exceptions)

### 1. Compensation
Unpaid, commission-only-with-no-base, or clearly below the comp floor in Master-Profile.md Section 2, with no stated exceptional development pathway.

### 2. Licensing / credentials Riley doesn't hold and can't obtain after hiring
Non-negotiable per the posting — e.g. active insurance license, PE license, active Series 7 at application, CDL, RN, CPA required, existing security clearance required. (This is the one and only licensing/credential filter — an earlier version of this rubric also restated it under Filter 7 as its own sub-point; that was a redundant duplicate and has been removed. Missing-license situations belong here, full stop.)

### 3. Relocation
Relocation required with no exceptional pathway or comp justification per Master-Profile.md Section 2.

### 4. Duplicate
Already present in Jobs-Seen-Log.md (check by URL or by company+title+date if URL varies) — skip, don't rescan.

### 5. Door-to-door / residential canvassing (added 2026-09-10)
Reject any role where door-to-door sales, residential canvassing, neighborhood canvassing, in-person cold-knocking, or field solicitation at private residences is a core or expected responsibility.

Reject examples: door-to-door sales representative, residential solar canvasser, roofing canvasser, home security door-knocker, pest-control field sales requiring neighborhood canvassing.

**Do NOT reject** normal outside sales, territory sales, client-site visits, trade shows, in-person B2B prospecting, or general field sales — only when the posting specifically requires door-to-door/residential canvassing as a core duty.

### 6. Degree completely outside candidate scope (added 2026-09-10)
Riley's verified education: B.S. Business Management & Administration; Associate of General Studies (see Master-Profile.md Section 3).

Reject only when a required degree is **all three** of: (a) explicitly mandatory, (b) substantially outside that scope, and (c) no equivalent-experience/substitution language is offered in the posting.

Reject examples: requires B.S. Electrical Engineering / Mechanical Engineering / Computer Science / Nursing (RN) / Chemistry or Biology for lab work / an Accounting degree specifically for CPA-track work.

**Do NOT reject** for: "Business, Finance, Economics, Analytics, Supply Chain, Logistics, or related field," "Bachelor's degree required/preferred," "analytical degree preferred," "engineering/business/related field," or "equivalent experience accepted." If Riley's business degree could reasonably satisfy the requirement, it's not a hard filter — let it through to scoring (the Skills Match criterion handles the rest).

### 7. Extremely low probability from missing CORE qualifications (added 2026-09-10)
Only a hard filter when the missing qualifications are fundamental to performing the role — not merely preferred. Reject if any apply:

- **A.** Requires 5+ years of directly relevant professional experience and the role is clearly not entry-level/associate/junior/graduate/development-track.
- **B.** Requires deep technical expertise that is the central function of the job and that Riley doesn't have — e.g. senior software engineer needing production Python/Java/SQL experience, electrical design engineer, advanced data scientist needing ML/programming, senior accountant needing GAAP close experience, senior power trader needing multiple years of direct trading experience. (Cross-check Skills-Gap-Tracker.md for current status rather than guessing.)
- **C.** Missing several mandatory requirements simultaneously with little or no adjacent experience — e.g. a role requiring 4+ years Salesforce administration AND SQL AND Python AND Tableau AND SaaS RevOps experience, none of which Riley has.

(Missing licenses/certifications are Filter 2, not repeated here.)

### 8. Seniority by actual requirements, not just title (added 2026-09-10)
Generally reject titles like Senior / Lead / Principal / Director / VP / Head of, and Manager roles that require prior people-management experience — **unless** the posting's actual body text clearly says it's open to early-career candidates (title alone is not sufficient evidence either way — read the requirements).

Generally retain: Graduate, Entry Level, Junior, Associate, Analyst, Coordinator, Assistant, Development Program, Management Trainee, Sales Development, Business Development — again, confirmed against the actual requirements, not the title alone.

---

## Do NOT over-filter stretch roles

Hard filters exist for **fundamental** disqualifiers only. A posting listing something as *preferred* (not required) should almost never cause a hard rejection, and a candidate missing 1-2 learnable tools should normally stay eligible. Examples that should NOT be hard-rejected: "Salesforce preferred," "SQL interest preferred," "1-2 years preferred" when Riley has adjacent experience, "Power BI preferred," "industry experience preferred."

After hard filters, classify every surviving listing as one of:

- **PASS** — Riley has a realistic chance and meets most core requirements.
- **STRETCH** — missing some requirements, but strong transferable experience makes an interview plausible.

Note PASS vs. STRETCH in the one-line rationale delivered in the brief (e.g. "STRETCH — requires 5+ yrs B2B sales Riley doesn't have, but comp/track/location are strong"), AND record it in Jobs-Seen-Log.md's PASS/STRETCH column so the tag survives past that day's brief. This is in addition to, not instead of, the numeric score below — STRETCH listings still get scored normally and still need to clear 60 to surface.

---

## Weighted Scoring (for everything that survives the hard filters)

| Criterion | Weight | Notes |
|---|---|---|
| Compensation fit | 25 | Meets or exceeds Master-Profile.md Section 2's comp target = 5. Below floor but exceptional dev pathway = 3-4. Well below with no clear upside = 1-2. |
| Career-track alignment | 20 | Track A (Sales) or Track B (Energy) core role = 5. Adjacent = 3. Unrelated = 1. |
| Growth/learning potential | 20 | A forward-looking judgment, distinct from Skills Match below — does the role teach Riley things he actually wants to learn (market intelligence, AI tools, trading exposure, AE track), regardless of whether he's already qualified for it today? Clear path to those = 5. |
| Location/relocation fit | 10 | Central Texas or remote = 5. Relocation required but exceptional pathway/comp = 3-4. Relocation required, no exceptional case = 1-2. |
| Skills match (Gap Map) | 15 | Straight from the Job-Description-Analysis-Template.md Gap Map for this posting — mostly DIRECT/ADJACENT requirements = 5. Heavy GAPs on must-haves = 1-2. |
| Company signal (stability/growth) | 10 | Funded/growing/reputable = 5. Unclear or declining = 2-3. |

**Total score guide:** 80-100 = strong apply now. 60-79 = apply, moderate priority. 40-59 = apply only if low effort or portfolio still thin. Below 40 = skip unless something else offsets it.

Record the score, PASS/STRETCH tag, and a one-line rationale in the Job note for each application.

---

## Rejection logging

For every listing dropped by a hard filter, keep a short internal note (not shown as a full entry in the brief, but summarized in Jobs-Seen-Log.md's "Filtered Out" section so Riley can audit the calls):

```
REJECT_REASON: "<specific mandatory requirement(s) causing rejection>"
```

Example: `REJECT_REASON: "Requires 5+ years direct enterprise SaaS RevOps plus Salesforce administration and SQL."`

Never reject on vague intuition ("candidate seems underqualified") — name the specific mandatory requirement(s).
