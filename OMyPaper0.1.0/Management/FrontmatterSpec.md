# Frontmatter and Schema Specification

## Purpose

This file defines the canonical metadata and schema conventions for the OMyPaper vault. Future skills should follow this file before creating or updating pages.

## Global Conventions

### Date and Time

- Date format: `YYYY-MM-DD`
- Datetime format when needed: `YYYY-MM-DD HH:mm`
- Weekly ID: `YYYY-Www`
- Session file date fragment: `YYYYMMDD-HHmm`

### Path Conventions

- Paper wiki: `Notes/Papers/{citekey}.md`
- Concept page: `Notes/Concepts/{slug}.md`
- Method page: `Notes/Methods/{slug}.md`
- Comparison page: `Notes/Comparisons/{comparison-slug}.md`
- Topic page: `Notes/Topics/{slug}.md`
- Session note: `Management/SessionNotes/{citekey}/{session-id}.md`
- Unmatched session note: `Management/SessionNotes/_unmatched/{session-id}.md`
- Weekly report: `Management/WeeklyReports/YYYY/YYYY-Www.md`
- Review note: `Management/ReviewNotes/{citekey}-review-YYYYMMDD.md`
- Lint report: `Management/LintReports/YYYY-MM-DD.md`
- Bootstrap report: `Management/BootstrapReports/YYYY-MM-DD-HHmm.md`

### Citekey Rules

1. Use the imported citekey as the canonical paper key.
2. Do not rewrite a citekey just to fit a preferred style.
3. When a file stem and frontmatter citekey disagree, the skill must report the mismatch.
4. Matching priority is fixed:
   - `citekey`
   - `zotero_key`
   - `title`
   - source note / attachment path
   - current-session guess

### Claim Separation Rule

Whenever a page summarizes content from a paper, the writing should distinguish:

- `Paper claim`: what the paper itself claims
- `My note`: what the user highlighted, asked, doubted, or concluded in raw notes/session notes
- `LLM synthesis`: later synthesis or abstraction by the agent

The distinction can appear as headings, callouts, or labeled bullets, but it should remain explicit.

## Status Fields

### `merge_status`

Allowed values:

- `pending`: session note has not yet been absorbed into the formal wiki
- `merged`: content has been reviewed and incorporated by S1
- `skipped`: intentionally not merged because it was redundant, weak, or off-scope
- `needs_review`: potentially useful but not safe to merge automatically
- `unmatched`: paper identity not stable enough to assign

### `match_confidence`

Allowed values:

- `exact`: explicit citekey or exact source match
- `high`: very likely same paper, strong metadata support
- `medium`: likely match, but one important field is missing or indirect
- `low`: weak guess only; do not formalize into a paper wiki
- `unmatched`: no stable match

### `status` for wiki pages

Recommended values:

- `seed`: page exists but is still thin
- `active`: page is useful and currently maintained
- `needs_review`: content exists but confidence, freshness, or structure is insufficient
- `stale`: page is old and may lag behind newer notes
- `archived`: keep for traceability, do not extend by default

## Required Frontmatter: Session Notes

Every file under `Management/SessionNotes/` must include at least:

```yaml
---
type: session-note
citekey: ""
zotero_key: ""
title: ""
source_note: ""
paper_wiki: ""
session_date: 2026-04-21
session_id: 20260421-2030-example
merge_status: pending
match_confidence: high
---
```

Recommended additional fields:

```yaml
source_pdf: ""
tags: []
related_concepts: []
related_methods: []
related_topics: []
```

### Session Note Relationship Rule

- S6 creates session notes.
- S1 reads the newest relevant session notes with `merge_status: pending`.
- After safe integration, S1 may update those session notes to `merge_status: merged`.
- Session notes are not substitutes for paper wikis; they are intermediate structured memory.

## Required Frontmatter: Paper Wiki

```yaml
---
type: paper-wiki
citekey: ""
zotero_key: ""
title: ""
year:
authors: []
status: seed
source_literature_note: ""
source_pdf: ""
absorbed_sessions: []
related_concepts: []
related_methods: []
related_comparisons: []
related_topics: []
last_compiled: 2026-04-21
last_verified: 2026-04-21
---
```

Notes:

- `absorbed_sessions` should list session IDs or relative paths already merged into the page.
- `last_verified` means the page has been checked against local evidence, not against model memory.

## Recommended Frontmatter: Concept / Method / Topic / Comparison Pages

### Concept Page

```yaml
---
type: concept
slug: ""
title: ""
aliases: []
status: active
related_papers: []
related_methods: []
related_topics: []
last_updated: 2026-04-21
---
```

### Method Page

```yaml
---
type: method
slug: ""
title: ""
aliases: []
status: active
related_papers: []
related_concepts: []
related_comparisons: []
last_updated: 2026-04-21
---
```

### Comparison Page

```yaml
---
type: comparison
comparison_key: ""
title: ""
entities: []
status: active
related_papers: []
last_updated: 2026-04-21
---
```

### Topic Page

```yaml
---
type: topic
slug: ""
title: ""
aliases: []
status: active
related_papers: []
related_concepts: []
related_methods: []
last_updated: 2026-04-21
---
```

## Recommended Frontmatter: Reports

### Weekly Report

```yaml
---
type: weekly-report
week_id: 2026-W17
week_start: 2026-04-20
week_end: 2026-04-26
generated_on: 2026-04-21
---
```

### Review Note

```yaml
---
type: review-note
citekey: ""
title: ""
review_mode: one-minute
review_date: 2026-04-21
source_paper_wiki: ""
source_literature_note: ""
source_sessions: []
---
```

### Lint Report

```yaml
---
type: lint-report
report_date: 2026-04-21
scope: Notes/
auto_fixes_applied: []
high_risk_items: []
---
```

### Bootstrap Report

```yaml
---
type: bootstrap-report
report_date: 2026-04-21
scope: OMyPaper
created_files: []
created_directories: []
conflicts_found: []
---
```

## File Naming Rules

1. Paper pages must use the exact citekey as filename.
2. Session note filenames should follow:
   - `{YYYYMMDD-HHmm}-{citekey}-{short-slug}.md`
   - or `{YYYYMMDD-HHmm}-unmatched-{short-slug}.md`
3. Concept / method / topic / comparison pages should prefer stable kebab-case slugs.
4. Do not rename files casually once other notes link to them.

## Registry Rules

### Paper Registry

Each paper should ideally appear once in `Management/PaperRegistry.md` with:

- `citekey`
- `zotero_key`
- `title`
- `status`
- literature note path
- paper wiki path
- last updated

### Session Index

Each session note should appear once in `Management/SessionIndex.md` with:

- `session_id`
- `session_date`
- `citekey`
- `title`
- session note path
- `merge_status`
- `match_confidence`

## Skill Dependency Rules

1. S0 initializes and checks the environment. It does not organize paper content.
2. S6 creates session notes and paper-matching metadata. It does not modify formal wiki pages.
3. S1 consumes raw notes plus pending session notes and updates formal wiki pages.
4. S2 maintains structure across the whole wiki layer.
5. S3 summarizes recent activity based on local files.
6. S4 creates recall aids from existing paper material.
7. S5 answers questions from the local vault first and should remain read-only.

## Write Safety Rules

1. Never edit `LiteratureNotes/` in S0-S6.
2. Do not turn uncertain interpretation into fact.
3. Report unresolved conflicts instead of forcing merges.
4. If matching is unstable, route content to `_unmatched/`.
5. When a skill writes files, it must be able to report the exact changed paths.

