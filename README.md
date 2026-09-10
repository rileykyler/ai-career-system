# AI Career System

A simple, Markdown-based system for tracking your job search, researching companies, prepping for interviews, networking, and managing resume notes. Every file is plain Markdown, so it works in any text editor today and will work natively when you open this folder as an Obsidian vault.

## Folder Guide

| Folder | Purpose |
|---|---|
| `Jobs/` | One note per job application — status, dates, links, notes |
| `Companies/` | One note per company you're researching — mission, culture, news |
| `Interview-Prep/` | Question banks, STAR stories, and per-interview prep notes |
| `Networking/` | Contacts you meet and conversations you have |
| `Resume/` | Master list of achievements/bullets and a log of resume versions |

## How to Use This

1. **Starting a new job application?** Copy `Jobs/_Job-Template.md`, rename it to `Company - Role.md`, and fill it in.
2. **Researching a company?** Copy `Companies/_Company-Template.md`, rename it to the company name, and fill it in. Link to it from the Job note using `[[Company Name]]` (Obsidian) or just type the filename for now.
3. **Got an interview?** Add prep notes either directly in the Job note or in a new file under `Interview-Prep/`.
4. **Met someone new?** Add a row to `Networking/Contacts.md`.
5. **Update your resume?** Keep raw bullet points and achievements in `Resume/Resume-Notes.md`, and log which version you sent where in `Resume/Versions.md`.

## Linking Notes Together (once you're in Obsidian)

Obsidian turns `[[Note Name]]` into a clickable link automatically. For example, in a Job note you could write:

```
Company: [[Acme Corp]]
```

...and it will link straight to `Companies/Acme Corp.md`. You don't need to do anything special now — just use that syntax whenever you reference another note, and it'll "just work" once opened in Obsidian.

## Status Conventions

Use these consistent tags in your notes so you can scan folders quickly:

- `#status/applied`
- `#status/interviewing`
- `#status/offer`
- `#status/rejected`
- `#status/researching`

## Getting Started

This vault comes with example files (marked "EXAMPLE" in the title) so you can see the system in action. Delete or repurpose them once you're comfortable with the pattern.
