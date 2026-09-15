<!-- SNAPSHOT 2026-09-15 of the PROJECT copy. Frozen. Not the live file. -->
# Resume Formatting Spec

Reference this before generating any new resume so formatting stays consistent
without re-deriving it each time. This spec is derived from direct measurement
(pdfplumber: char sizes, line-by-line vertical gaps, x0 indentation, rects) of
Riley's actual reference PDFs — not from guessing at a description of them.

**2026-09-10 — current canonical reference:** `Riley_Kyler_Voltus_Energy_Markets_Analyst_Resume_Refined.pdf`, as tuned across the same session (section order
flipped, headers de-capitalized, job-title line resized/unbolded, a real gap
added before a second job entry). Two earlier reference sets were superseded,
in order: the original 4 PDFs (SHI/CarbonBetter/Fisher/TC Energy/DR Horton,
all-caps section headers, bulleted coursework, small bullet indents) -> the
SHI-recalibrated pass (still all-caps headers, bulleted coursework) -> **this**
Voltus-modeled version, further hand-tuned per Riley's feedback, which is the
one to build from going forward. If a future session is unsure whether this
file is stale, re-derive from whatever PDF Riley says looks best, the same way
this one was: render it, then measure font sizes and line/rect positions with
pdfplumber rather than eyeballing it.

---

## 1. Page and Typography

- **Font:** Arimo (open-source Arial-equivalent) in the reference PDFs. Use
  **Arial** when building in docx — metrically compatible, renders identically.
- **Page:** US Letter (12240 x 15840 twips / 612 x 792 pt).
- **Margins:** top **560 twips (28pt)**, bottom **560 twips (28pt)**, left/right
  **800 twips (40pt)**. Top/bottom are intentionally smaller than left/right —
  this is what lets the page fill all the way down; do not "even them out" to
  800 on all sides, that reintroduces the old too-much-bottom-whitespace problem.
- **Name:** Bold, 25 half-points (~12.5pt), flush left at the margin (no extra
  indent — an earlier pass added an 8pt indent here; that was wrong).
- **Contact line:** Regular, 18 half-points (~9pt), flush left, pipe-separated
  (`|`). Exactly 3 fields, in order: phone `|` email `|` LinkedIn URL. **No
  location/city on the contact line** — location only ever appears under each
  school in Education.
- **Section header bars:** Bold, 21 half-points (~10.5pt), **Title Case — never
  all-caps** (e.g. "Core Competencies", "Professional Experience"; not "CORE
  COMPETENCIES"). Text sits flush at the left margin with no extra internal
  padding beyond the cell's own top/bottom padding. Gray fill = **`#EEEEEE`**.
- **School name (Education):** Bold, 18 half-points (~9pt).
- **Job title line (Professional Experience — `Company - Job Title`):** **Not
  bold**, 19 half-points (~9.5pt) — deliberately bigger than the surrounding
  16-17pt body/bullet text so each job reads as its own entry at a glance, but
  smaller than an earlier pass that used 22 (~11pt) and bold, which read as
  too heavy. This size/weight applies only to Professional Experience job
  titles, not the Education school-name line (which stays bold at 18).
  **2026-09-11 note:** Riley flagged this contrast (non-bold 19pt job title
  next to a bold 18pt school name and bold subheaders) as looking like a
  font change. Checked via `pdffonts` on the rendered PDF — confirmed it's a
  single font family throughout (only Regular/Bold/Italic variants), so this
  is the intended weight/size contrast described above, not a bug. Riley
  confirmed keeping it as-is; noted here so a future session doesn't
  "fix" it again without checking first.
- **Dates:** Italic, matches the size of whatever line it's on (18 for
  Education, 19 for Professional Experience job titles), right-aligned via a
  right tab stop at the content-width edge — never bold. **See section 9a for the
  exact tab-stop value to use when building in docx-js — an easy-to-repeat
  mistake overshoots this and pushes dates into/past the right margin.**
- **Body text (location, degree lines, grid items):** Regular, 17 half-points
  (~8.5pt).
- **Bullets:** Regular, 16 half-points (~8pt), standard round bullet character
  followed by two spaces.
- **Subheaders inside experience blocks** (e.g. "Analysis, Reporting &
  Operational Accuracy"): **Bold only, never italic**, and **flush left — no
  indent** (same left edge as the job title above them). Only the *bullets*
  underneath get indented, not the subheader line itself. (This flush-left
  rule is specific to Professional Experience subheaders — the "Relevant
  Coursework" / "Industry Interests" subheaders get their own indent rule,
  see section 5.)
- **Typographic characters:** use an **en dash**, not a hyphen, in (a) the
  Company-Title line ("Crunch Fitness - Personal Trainer Sales Consultant")
  and (b) date ranges ("July 2024 - Present", "May 2022 - August 2023"). Use a
  **curly apostrophe** in "Lowe's".

## 2. Section Order (confirmed against the Voltus reference, as tuned)

1. Name
2. Contact line
3. Core Competencies (grid)
4. Education
5. **Professional Experience** — comes before Coursework/Industry Interests so
   an employer hits work history first. (This was flipped from an earlier pass
   that put Coursework second; the current order is deliberate and final.)
6. Relevant Coursework and Industry Interests
7. Technical & Business Tools (grid)

Note: the Voltus reference itself replaces step 7 with a role-specific
"Market & Operational Focus" grid (e.g. "NYISO Capacity Markets", "DER Program
Operations") instead of a generic tools list. That section's *content* isn't
approved language anywhere in Master-Profile.md or the Resume-Profile files, so
it was **not** adopted — Technical & Business Tools stays as the closing
section by default. If Riley wants a themed closing section for a specific
posting, ask what belongs in it rather than inventing phrasing.

## 3. Grids — Core Competencies, Coursework, Industry Interests, Tools

All grids: **left-aligned** (not centered — an earlier pass had this wrong),
no borders, no bullet points. Use a **fixed table layout with explicit,
evenly-divided column widths** (not Word's automatic autofit) — autofit let
column widths drift unevenly in a 2026-09-15 build, which is also what
originally motivated checking grid alignment against the subheaders (see section 5).

- **Core Competencies: 3 columns**, always 6 items (2 rows x 3).
- **Relevant Coursework: 3 columns, always 6 items (2 rows x 3)** — changed
  2026-09-11 (previously "up to 6 items," which let the grid ship with only
  one row when fewer than 6 of the verified courses were relevant to a given
  posting; Riley asked for the grid to always be full). Selection order:
  1. First, use as many of Master-Profile.md's 7 verified courses (Business
     Analytics, Operations Management, Strategic Management, Project
     Management, Supply Chain Management, Energy Markets & Renewable Energy,
     Retailing) as are genuinely relevant to the posting.
  2. If that leaves fewer than 6, fill the remaining slots from the
     **McCoy Elective Pool** below — never an invented course name, and
     never a general-business-core course (see the exclusion list below).
     **2026-09-11 refinement:** Riley does not want entry-level/required-of-
     everyone core courses used as filler (his first examples: Business
     Communication, Principles of Marketing) — those are technically
     3000-level but every B.B.A. student takes them regardless of major, so
     they read as generic rather than as evidence of relevant background.
     Fill-in courses must be **either specific to the posting's job/industry,
     or an upper-division (3000-4000 level) major/concentration elective** —
     ideally both. Never pull from the general-business-core list below.
  3. These McCoy-catalog fill-ins are **not** part of the confirmed/verified
     7-course list in Master-Profile.md Section 3 — that list still only
     grows when Riley explicitly confirms a course. They're real courses
     that exist in his actual degree program, used here only to fill out the
     grid. List by course title only, matching the style of the verified
     list (no course numbers in the grid itself).

  **General-business-core courses — NEVER use these as filler** (required of
  every B.B.A. student regardless of major, so they don't read as
  job-specific even though several are numbered 3000+): Introduction to
  Business, Introduction to Microcomputer Applications in Business,
  Principles of Microeconomics, Principles of Macroeconomics, Business
  Statistics, Introduction to Financial Accounting, Introduction to
  Managerial Accounting, Professional Development I/II, Legal Environment of
  Business, Management of Organizations, Business Finance, Principles of
  Marketing, Business Communication, Enterprise Information Technology and
  Business Intelligence. (Strategic Management and Business Policy is also
  in this core list, but it's already one of the 7 verified courses, so it
  stays available as a verified pick — just never re-derive it as a "filler"
  pick from this pool.)

  **McCoy Elective Pool — upper-division, job/industry-specific fill-ins**
  (sourced from mycatalog.txstate.edu department course lists; pick whichever
  cluster matches the posting, re-verify against the live catalog if it's
  been a while since this was checked):

  | Cluster | Courses (course number - title) |
  |---|---|
  | Sales / Marketing (Track A) | MKT 3358 Professional Selling, MKT 3360 Sales Management, MKT 3350 Consumer Behavior, MKT 3370 Marketing Research, MKT 3387 Digital Marketing, MKT 4330 Promotional Strategy |
  | Finance / Commodities / Markets (Track B — trading, settlements, commodities-adjacent) | FIN 4318 Portfolio Management & Derivatives, FIN 4319 Financial Markets and Institutions, FIN 4320 Treasury and Working Capital Management, FIN 4327 Commercial Credit Analysis, ECO 3311 Money and Banking, ECO 3304 Environmental Economics for Decision Makers, ECO 3335 Managerial Economics |
  | Supply Chain / Operations (Track B — ops, logistics) | SCM 4310 Transportation and Distribution Management, SCM 4315 Purchasing and Supply Management, MGT 4340 Quality Management and Beyond, MGT 4344 Management of Teams and Groups |
  | Leadership / HR (use sparingly — only when the posting is people-management-adjacent) | MGT 4372 Effective Leadership, MGT 4373 Human Resource Management, MGT 4375 Organizational Behavior and Human Relations |

  Pick 2-3 courses from whichever cluster(s) best match the specific posting
  (a role can draw from more than one cluster — e.g. a commercial/customer-
  facing energy role can mix the Finance/Commodities and Sales/Marketing
  clusters). Don't reuse the exact same 3 across every resume in a batch just
  because two postings share a track — re-select per posting like any other
  tailoring decision, even if the end result overlaps.
- **Industry Interests: 3 columns**, 6 items (2 rows x 3).
- **Technical & Business Tools: 4 columns**, always 8 items (2 rows x 4).
- Keep phrases short enough to fit one line per cell without wrapping.

## 4. Education block

One block per school, no bullets:
- Line 1: School name (bold) - tab - graduation date (italic, right-aligned)
- Line 2: City, State (regular)
- Line 3: Degree name (regular)
- Texas State University always listed first, then Austin Community College.
- Tight line spacing within a block; a slightly larger gap between the two
  school blocks (see section 7 for the actual twip values).

## 5. Relevant Coursework and Industry Interests section

Header text is **"Relevant Coursework and Industry Interests"** (Title Case,
"and" spelled out — not "RELEVANT COURSEWORK & INDUSTRY FOCUS"). Inside the
gray-bar section:
- Bold, non-shaded subheader **"Relevant Coursework"**, then a 3-column grid
  (section 3) of course names — no bullets, no pipe-separated lines (that was the
  older, superseded style).
- Bold, non-shaded subheader **"Industry Interests"** (this label replaces the
  older "Industry Focus" — use "Industry Interests" universally now, for every
  sub-variant, not just non-standard ones), then a 3-column grid of the
  sub-variant's industry-focus phrases.

**Indent rule (added 2026-09-15, Riley's explicit instruction — applies to
every resume going forward):** unlike Professional Experience subheaders
(section 1, flush left, no indent), the **"Relevant Coursework" and "Industry
Interests" subheaders are indented one step in from the section bar** (200
twips), and the **grid directly below each one is indented to that exact
same 200-twip position** — so the grid's first column lines up vertically
with its subheader's bold text, not with the page margin or the gray section
bar above it. This is a visual sub-level: the section bar ("Relevant
Coursework and Industry Interests") sits at the true page margin like every
other section bar, while its two subheaders and their grids sit one step in
from that, showing they're nested under it.

**Implementation — three things must all be right, see section 9d.** The first
attempt at this rule (2026-09-15) looked correct in code and still rendered
misaligned, because two OOXML gotchas silently cancelled it. Do not re-derive
this by eye; section 9d has the exact mechanism and the verification command.
Summary: (1) set the subheader paragraph's `left_indent` to `SUB_INDENT`;
(2) set the grid table's `w:tblInd` to `SUB_INDENT + 108` — the +108 is
mandatory, see section 9d; (3) shrink each column's width to
`(content_width - SUB_INDENT) / 3`, since a table's own indent doesn't
resize its columns and skipping this pushes the grid's right edge past the
content area.

## 6. Professional Experience block

- Company and title share ONE line: `Company - Job Title`, not bold, 19
  half-points, en dash - tab - dates (italic, same 19 half-points,
  right-aligned, en dash for the range). See section 1 for why this isn't bold/18pt
  like the Education school-name line.
- A second (or third) job entry gets a real gap before its title line — **220
  twips** — so it doesn't read as just another bullet group under the
  previous job. Don't collapse this back to 0; that was the original mistake
  Riley flagged (Lowe's looked like it belonged to Crunch Fitness).
- Subheaders: bold, flush left (no indent — see section 1).
- Bullets: the glyph sits **18pt (360 twips)** in from the margin, and **all
  bullet text — the first line and every wrapped line — sits at 25pt (500
  twips)**, giving a hanging indent of 140 twips. Separate the glyph from the
  text with a **TAB to an explicit left tab stop at 500 twips, never with
  literal spaces.** **Corrected 2026-09-15 — see section 9e.** The previous version of
  this rule said wrapped text starts at 36pt/720 twips with a 360-twip hanging
  indent; building to that literally (glyph + two spaces, hanging indent 360)
  put the first line's text at ~25.2pt and its wrapped lines at 36pt, so every
  wrapped bullet hung ~10.8pt out to the right. Riley flagged it as bullets
  looking "warped." The tab-stop construction below makes the two positions
  identical by definition instead of by coincidence.
- Crunch Fitness entry: bold subheaders each followed by their bullets, pulled
  from the matching Resume-Profile bullet bank. Bullet counts per subheader can
  vary (3/3/3, or 3/3/2, etc.) — match whatever the target posting calls for,
  don't force a fixed count. Bullets may be lightly reworded per posting the
  way past sessions have demonstrated, but never alter the stated
  numbers/percentages, and never invent a new fact not already in
  Master-Profile.md.
- Lowe's entry: no subheader, plain bullets (typically 3), same indent
  treatment as Crunch's bullets, same real gap before its title line as above.

## 7. Measured spacing values (twips) — use these as the starting scheme

These were tuned so a full Crunch Fitness entry (3 subheaders x 3 bullets, one
of which wraps to 2 lines) plus a 3-bullet Lowe's entry, all 5 other sections,
and both grids still land on exactly one page. If new content is added/removed
and it no longer fits, adjust these proportionally rather than guessing new
numbers from scratch:

| Constant | Value (twips) | What it controls |
|---|---|---|
| `SECTION_SPACER` | 260 | Gap before every gray section-header bar |
| `HEADER_TO_CONTENT` | 230 | Gap from a header bar down to the first content line |
| `GRID_ROW_GAP` | 55 | Row-to-row gap inside any grid |
| `EDU_LINE_GAP` | 55 | School -> location -> degree, within one Education block |
| `EDU_ENTRY_GAP` | 140 | Between the two Education entries |
| `SUBSECTION_GAP` | 240 | "Relevant Coursework" grid -> "Industry Interests" subheader |
| `SUB_INDENT` | 200 | Left indent for the "Relevant Coursework"/"Industry Interests" subheaders and their grids (added 2026-09-15, see section 5) |
| `EXP_FIRST_SUBHEADER_GAP` | 230 | Job title line -> its first subheader |
| `EXP_SUBHEADER_GAP` | 260 | End of a bullet group -> the next subheader |
| `EXP_SUBHEADER_TO_BULLET` | 120 | Subheader -> its first bullet |
| `EXP_BULLET_GAP` | 25 | Extra "after" spacing on each bullet paragraph (on top of natural line height) |
| `EXP_NEXT_JOB_GAP` | 220 | End of one job's bullets -> the next job's title line — a real, visible gap (see section 6) |
| `BULLET_CHAR_POS` | 360 | Bullet glyph indent from margin (18pt) |
| `BULLET_TEXT_POS` | 500 | Where ALL bullet text sits — first line and wrapped lines alike (25pt). Reached by a tab stop, see section 9e |
| `BULLET_HANGING` | 140 | `BULLET_TEXT_POS - BULLET_CHAR_POS`; was wrongly 360 before 2026-09-15 |
| `CELL_EDGE_COMP` | 108 | Added to every grid's `w:tblInd` to cancel the cell-padding-box offset (section 9d) |
| `NAME_CONTACT_INDENT` | 0 | Name/contact are flush left, no indent |

Font sizes used (half-points, docx `size` property): name 25, section header
21, Education school title 18 (bold), Professional Experience job title 19
(not bold), contact 18, body/grid text 17, bullets 16.

If a resume's content is short enough that these values leave excess blank
space at the bottom, or long enough to spill to a second page, scale the whole
table up or down together (keep the relative proportions) rather than tweaking
one value in isolation — that's what caused visible inconsistency in earlier
passes. In practice, a resume that's one grid-row short of fitting (e.g. the
2026-09-15 energy-market-analysis master build) can be brought back to one
page by trimming `SECTION_SPACER`, `HEADER_TO_CONTENT`, `EXP_SUBHEADER_GAP`,
`EXP_NEXT_JOB_GAP`, and `SUBSECTION_GAP` down by roughly 30-40 twips each,
rather than cutting content — that 2026-09-15 build used 220/190/220/180/200
respectively and still reads with normal, uncrowded spacing.

**2026-09-11:** the Coursework grid moving from "up to 6" to "always 6" items
(section 3) adds one row of content versus earlier builds. Re-check page fit (still
fits, since Industry Interests was already a full 6-item grid at this same
size — Coursework just now matches it) rather than assuming these spacing
constants still leave the same bottom margin.

## 8. Representing AI / Git / GitHub / Obsidian Learning

Riley is actively building AI-enabled workflow skills (Master-Profile.md
Section 12) and Git/GitHub + Obsidian specifically (Skills-Gap-Tracker.md
status: IN-PROGRESS). These should be represented honestly — real tools in
active use, not claimed as professional/verified experience. Approved
language:

- **Core Competencies slot:** *"AI-Enabled Business Tools"* — already
  established/approved elsewhere in the system. Represents general
  AI-assisted workflow orientation (ChatGPT/Claude use), not claimed mastery
  of any specific technical AI skill.
- **Technical & Business Tools grid slot:** *"Git & GitHub"* — legitimate tool
  name, honestly reflects hands-on (if non-professional) use. Good filler if
  the Tools grid needs an 8th item.
- **Do not** list Obsidian directly as a "tool" in the grid — it reads as a
  personal system rather than an industry-recognized term. If Obsidian needs
  representation, frame it conceptually (e.g., "Knowledge Management &
  Documentation") rather than by product name.
- **Interview note:** If asked about Git/GitHub in an interview, Riley should
  describe it accurately as self-directed learning through the personal
  career-tracking project — not professional/work experience. Never let a
  tools-grid listing imply otherwise.

## 9. General reminder

Don't assume font/alignment/color/section-order/spacing choices when a real
reference PDF exists — render it and measure it (pdfplumber for char sizes and
line positions, pdftoppm to look at it) before building, rather than reusing a
memory of this spec that hasn't been checked against the current reference
recently. If Riley says a rebuilt resume still looks wrong, that means
re-open whatever PDF he says looks best and re-measure — don't re-guess. If
something looks like a font inconsistency, check the rendered PDF's embedded
font table (`pdffonts <file>.pdf`) before concluding it's a bug — a single
font family with Regular/Bold/Italic variants can look like "different fonts"
side by side when weight or size legitimately differs by design (see the
2026-09-11 note in section 1).

## 9a. docx-js build note: right tab-stop position (added 2026-09-10)

**Bug pattern to never repeat:** when building the Company-Title/dates and
Education school/date lines with a `TabStopType.RIGHT`, the tab stop's
`position` value must be set to the **content width** (page width minus left
margin minus right margin — currently `10640` twips), **not** margin + content
width (`11440`). OOXML measures tab-stop positions from the paragraph's own
margin origin (i.e., 0 = left margin), not from the physical left edge of the
page. Using `margin + content width` double-counts the left margin and pushes
the right-aligned date ~800 twips (0.5") too far right — enough to overshoot
the right margin and read as cut off / crowding the page edge.

Correct pattern:
```
const CONTENT_W = PAGE_W - (MARGIN_LR * 2); // e.g. 12240 - 1600 = 10640
const RIGHT_TAB = CONTENT_W;                // NOT MARGIN_LR + CONTENT_W
```
This applies to every right-aligned date line (both Education entries, both
Professional Experience job-title lines). Verify visually after any resume
build by rendering to PDF (pdftoppm) and confirming dates land flush with the
right edge of the gray section-header bars, not past them.

## 9b. docx-js build note: section-header bars and spacer paragraphs (added 2026-09-10)

Build the gray section-header bars as a single **Paragraph** with paragraph-
level `shading` (fill `#EEEEEE`) and an explicit `spacing.before`, not as a
one-cell **Table** wrapper. A Table can't carry `spacing.before` itself, which
forces an extra empty "spacer" paragraph in front of it — and an empty
paragraph with no runs still takes on the default Normal-style line height,
silently adding ~260-280 twips beyond the value passed to `spacing.before`.
Stacked across every section header and header-to-content gap, that phantom
height was enough to push an otherwise one-page resume onto two pages.

Rule of thumb: apply `spacing.before`/`spacing.after` directly on the real
content paragraph wherever possible (headers, subheaders, body lines, title/
date lines) instead of inserting a blank paragraph before it. The one place a
true spacer paragraph is unavoidable is immediately before a grid **Table**
(which has no spacing property of its own) — there, give the spacer a tiny
font size (e.g. `size: 2`) and `spacing: { line: <gapTwips>, lineRule: EXACT }`
so its height is exactly the intended gap rather than whatever the default
font's line height happens to be.

## 9c. Build tool note: python-docx used in place of docx-js (added 2026-09-11)

The 2026-09-11 run couldn't reach the npm registry from the sandbox (403 on
`npm install docx`), so that run built resumes with **python-docx** instead of
docx-js. python-docx exposes twips directly via `docx.shared.Twips(n)`, so all
the twip values in section 7 carry over unchanged. Equivalents worth noting for the
next session, whichever library ends up used:
- Right-aligned tab stops: `paragraph_format.tab_stops.add_tab_stop(Twips(n), WD_TAB_ALIGNMENT.RIGHT)` — same content-width-not-margin-width rule as section 9a
  applies (position is still measured from the paragraph's own margin origin).
- Gray section-header shading: python-docx has no high-level paragraph-shading
  API — set it via raw OOXML (`w:pPr/w:shd` with `w:fill="EEEEEE"`), same
  effect as the docx-js `shading` option in section 9b.
- Grids were built as borderless `Table` objects (border XML set to `nil` on
  all edges) rather than tab-stopped paragraph lines, to keep column alignment
  exact across rows — same visual result as section 3 describes either way. Use a
  fixed `w:tblLayout` with explicit `w:tblGrid` column widths (see section 3) rather
  than leaving `table.autofit` on — autofit can distribute width unevenly
  when cell text lengths vary a lot between columns.
- Verified fonts render correctly via LibreOffice (`soffice --headless --convert-to pdf`) on this sandbox — `pdffonts` on the output confirmed a
  single embedded font family (LibreOffice substitutes "Arial" with
  "Liberation Sans," which is metrically compatible and the reason this spec
  says Arial "renders identically" in section 1). `pdftoppm` + visual inspection (or
  cropping/zooming a rendered PNG) is still the fastest way to sanity-check
  a build before sending it to Riley.

## 9d. Table indentation: two gotchas that silently break alignment (added 2026-09-15)

Riley flagged twice that the Coursework/Industry Interests grids didn't line
up with their subheaders. The code *looked* right both times. Two separate
OOXML behaviours were responsible, and both fail silently — nothing errors,
the property just doesn't take effect. Anyone touching grid indentation needs
both of these:

**1. `w:tblPr` has a REQUIRED child order, and out-of-order children are
discarded.** The schema sequence is: `tblStyle, tblpPr, tblOverlap,
bidiVisual, tblStyleRowBandSize, tblStyleColBandSize, tblW, jc,
tblCellSpacing, tblInd, tblBorders, shd, tblLayout, tblCellMar, tblLook`.
python-docx's `table.autofit = False` appends `w:tblLayout`, and
`doc.add_table()` already emits `w:tblW`, `w:jc` and `w:tblLook` — so naively
`tblPr.append(tblInd)` lands `w:tblInd` *after* `w:tblLayout`/`w:tblLook`,
where Word and LibreOffice both ignore it entirely. Insert it positionally
(before the first existing child that sorts later in the list above), not with
`.append()`. Also don't hand-append a second `w:tblLayout` — `autofit = False`
already wrote one, and duplicates are invalid.

**2. `w:tblInd` positions the cell PADDING BOX, not the cell text, so table
text lands 108 twips (5.4pt) LEFT of where a paragraph at the same indent
would start.** Measured on this sandbox: with the text margin at x0 = 40.1pt,
an un-indented grid's text rendered at x0 = 34.7pt — exactly 108 twips left.
Zeroing the per-cell `w:tcMar` does *not* remove this; the offset is baked
into how the table's position is computed. **Always set
`w:tblInd = desired_text_indent + 108`.** With that compensation every grid
lands exactly where intended: `0 + 108` puts Core Competencies and Tools flush
on the margin, `200 + 108` puts the Coursework and Industry Interests grids
exactly under their subheaders.

**Verify by measuring, never by eye** — a 5.4pt offset is visible to Riley but
easy to talk yourself out of in a rendered PNG. After any build that touches
grid geometry, run pdfplumber and confirm the left edges match:

```python
import pdfplumber
page = pdfplumber.open("<resume>.pdf").pages[0]
rows = {}
for w in page.extract_words():
    rows.setdefault(round(w["top"], 1), []).append(w)
for top in sorted(rows):
    ws = sorted(rows[top], key=lambda x: x["x0"])
    print(f'{ws[0]["x0"]:8.3f}  {" ".join(x["text"] for x in ws)[:54]}')
```

Expected for a correct build: name, contact, every gray section bar,
Education lines, Experience lines, and the Core Competencies + Tools grids all
at **40.1**; the "Relevant Coursework" and "Industry Interests" subheaders
*and* all four of their grid rows all at **50.1**. Anything else means one of
the two gotchas above is back.

## 9e. Bullet wrap alignment: use a tab, not spaces (added 2026-09-15)

Riley flagged bullets looking "warped" — a wrapped continuation line sitting
~10.8pt to the RIGHT of its own first line. Same class of bug as section 9d: the code
matched the spec's stated numbers and still rendered wrong, because two
independent things set the two positions and nothing forced them to agree.

- The **first line's** text position = glyph position + whatever the literal
  separator (glyph plus two spaces) happens to render as. Measured:
  58.1pt + 7.23pt = 65.33pt. Font-dependent and not controllable.
- The **wrapped lines'** position = the paragraph's `left_indent`, which the
  old rule set to 720 twips = 76.1pt.

65.33 does not equal 76.1, hence the overhang. Zeroing or tweaking either number
independently just moves the mismatch around.

**The fix is structural:** put the glyph at `BULLET_CHAR_POS`, set
`left_indent = BULLET_TEXT_POS`, set
`first_line_indent = -(BULLET_TEXT_POS - BULLET_CHAR_POS)`, add an explicit
LEFT tab stop at `BULLET_TEXT_POS`, and write the run as the glyph, a tab, then the text. The
tab drives the first line's text to exactly `BULLET_TEXT_POS`, which is by
definition where wrapped lines start — so they align regardless of font,
glyph width, or renderer. python-docx:

```python
pf.left_indent = Twips(BULLET_TEXT_POS)
pf.first_line_indent = Twips(-(BULLET_TEXT_POS - BULLET_CHAR_POS))
pf.tab_stops.add_tab_stop(Twips(BULLET_TEXT_POS), WD_TAB_ALIGNMENT.LEFT)
run = p.add_run("•\t" + text)
```

**Verify with the section 9d pdfplumber snippet**, checking three numbers on any
resume that has at least one wrapping bullet: glyph x0 = **58.1**, first-line
text x0 = **65.1**, wrapped-line x0 = **65.1**. The last two must be equal —
if they differ, the tab stop is missing or the run went back to using spaces.
Note that a resume with no wrapping bullet can't exercise this check; the
2026-09-15 rebuild used the energy master resume (whose "Maintain accurate
customer activity..." bullet wraps) as the test case for all five builds.
