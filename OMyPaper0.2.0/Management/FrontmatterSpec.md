# Frontmatter and Schema Specification

This file defines the canonical metadata and schema conventions for `OMyPaper/`.

The system now supports two durable object families:

- global paper knowledge objects
- hierarchical project objects

It also supports two session families:

- paper sessions
- project / hybrid sessions

## 1. Path Conventions

### 1.1 Global Paper Layer

- `LiteratureNotes/{citekey}.md`
- `Notes/Papers/{citekey}.md`
- `Notes/Concepts/{concept-slug}.md`
- `Notes/Methods/{method-slug}.md`
- `Notes/Comparisons/{comparison-key}.md`
- `Notes/Topics/{topic-slug}.md`

### 1.2 Project Layer

- `projects/{project_id}/project.yml`
- `projects/{project_id}/文献地图.md`
- `projects/{project_id}/论文大纲.md`

Projects are stored as flat directories under `projects/`.
Hierarchy is logical, not physical, and is expressed by `parent_project` in `project.yml`.

### 1.3 Session and Report Layer

- `Management/SessionNotes/{citekey}/{session-id}.md`
- `Management/SessionNotes/_unmatched/{session-id}.md`
- `Management/ProjectSessionNotes/{project_id}/{session-id}.md`
- `Management/ProjectSessionNotes/_hybrid/{session-id}.md`
- `Management/ProjectSessionNotes/_unmatched/{session-id}.md`
- `Management/ReviewNotes/{citekey}-review-YYYYMMDD.md`
- `Management/ReviewNotes/project-{project_id}-review-YYYYMMDD.md`
- `Management/WeeklyReports/YYYY/YYYY-Www.md`
- `Management/LintReports/YYYY-MM-DD.md`
- `Management/BootstrapReports/YYYY-MM-DD-HHmm.md`

### 1.4 Control Files

- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`

## 2. Identity Rules

### 2.1 Paper Identity

Primary paper key: `citekey`

Matching priority:

1. `citekey`
2. `zotero_key`
3. `title`
4. `source note` or `attachment path`
5. current-session guess

### 2.2 Project Identity

Primary project key: `project_id`

Matching priority:

1. `project_id`
2. exact manifest path `projects/{project_id}/project.yml`
3. exact title in manifest
4. local parent-child context from `ProjectTree.md`
5. current-session guess

## 3. Claim Separation Rule

Whenever paper content is synthesized into durable notes, try to separate:

- `Paper claim`
- `My note`
- `LLM synthesis`

Project pages may also use these labels where helpful, but the main rule remains strongest for paper-derived content.

## 4. Status Fields

### 4.1 Merge Status

Allowed values:

- `pending`
- `merged`
- `skipped`
- `needs_review`
- `unmatched`

Meaning:

- `pending`: useful content exists but has not yet been safely absorbed
- `merged`: safely absorbed into the target wiki or reviewed project synthesis
- `skipped`: intentionally not absorbed
- `needs_review`: useful but risky / ambiguous
- `unmatched`: target paper or project could not be matched safely

### 4.2 Match Confidence

Allowed values:

- `exact`
- `high`
- `medium`
- `low`
- `unmatched`

### 4.3 Page / Project Status

Suggested values:

- `seed`
- `active`
- `needs_review`
- `stale`
- `paused`
- `archived`

### 4.4 Project Type

Allowed values:

- `parent`
- `child`
- `standalone`

Rules:

- `child` must have exactly one `parent_project`
- `parent` should not have `parent_project`
- `standalone` should not have `parent_project`
- child lists are derived, not manually maintained in `project.yml`

### 4.5 Session Type and Scope Level

Allowed `session_type` values:

- `paper`
- `parent-project`
- `child-project`
- `hybrid`

Allowed `scope_level` values:

- `paper`
- `parent`
- `child`
- `hybrid`

## 5. Canonical Schemas

### 5.1 Paper Session Note

Required frontmatter:

```yaml
type: session-note
session_type: paper
citekey: string
zotero_key: string | ""
title: string | ""
source_note: string
paper_wiki: string
session_date: YYYY-MM-DD
session_id: string
merge_status: pending | merged | skipped | needs_review | unmatched
match_confidence: exact | high | medium | low | unmatched
source_pdf: string | ""
tags: []
related_concepts: []
related_methods: []
related_topics: []
```

### 5.2 Project / Hybrid Session Note

Required frontmatter:

```yaml
type: project-session-note
session_type: parent-project | child-project | hybrid
linked_project: string | ""
project_type: parent | child | standalone
parent_project: string | ""
child_project: string | ""
linked_citekeys: []
scope_level: parent | child | hybrid
session_date: YYYY-MM-DD
session_id: string
merge_status: pending | merged | skipped | needs_review | unmatched
match_confidence: exact | high | medium | low | unmatched
project_path: string
tags: []
```

Recommended body sections:

- `Matching Summary`
- `My Questions`
- `Updated Project Understanding`
- `Still Uncertain`
- `Evidence Anchors`
- `Candidate Project Updates`

### 5.3 Paper Wiki Page

Required frontmatter:

```yaml
type: paper-wiki
citekey: string
zotero_key: string | ""
title: string
year: number | ""
authors: []
status: seed | active | needs_review | stale | archived
source_literature_note: string
source_pdf: string | ""
absorbed_sessions: []
related_concepts: []
related_methods: []
related_comparisons: []
related_topics: []
evidence_basis: []
last_compiled: YYYY-MM-DD
last_verified: YYYY-MM-DD
```

Important rule:

- do not store project membership in paper-wiki frontmatter

Recommended body addition:

- `Source Coverage`
  - raw-note coverage
  - PDF coverage
  - session coverage

### 5.4 Project Manifest (`project.yml`)

Canonical minimum fields:

```yaml
project_id: string
title: string
project_type: parent | child | standalone
parent_project: string | null
scope: string
objective: string
keywords: []
status: active | paused | archived
papers:
  core_refs:
    - citekey: string
      relevance: high | medium | low
      theme: string
      role: string
      priority: high | medium | low
      outline_sections: []
      note: string
  supporting_refs:
    - citekey: string
      relevance: high | medium | low
      theme: string
      role: string
      priority: high | medium | low
      outline_sections: []
      note: string
notes:
  sibling_boundaries: []
  exclusions: []
```

Rules:

- `papers.core_refs` and `papers.supporting_refs` are the canonical project-membership lists
- every `citekey` in `project.yml` must resolve in the global paper layer
- do not copy paper frontmatter into `project.yml`
- do not store `children` lists manually; derive them from `parent_project`

### 5.5 Project Literature Map

Required frontmatter:

```yaml
type: project-literature-map
project_id: string
project_type: parent | child | standalone
parent_project: string | ""
last_updated: YYYY-MM-DD
```

### 5.6 Project Outline

Required frontmatter:

```yaml
type: project-outline
project_id: string
project_type: parent | child | standalone
parent_project: string | ""
last_updated: YYYY-MM-DD
```

### 5.7 Review Note

Paper review note:

```yaml
type: review-note
review_target_type: paper
citekey: string
title: string
review_mode: one-minute | five-minute | oral-explanation | self-test
review_date: YYYY-MM-DD
source_paper_wiki: string
source_literature_note: string
source_sessions: []
```

Project review note:

```yaml
type: project-review-note
project_id: string
project_type: parent | child | standalone
parent_project: string | ""
review_mode: one-minute | five-minute | oral-explanation | self-test | quick-familiarization
review_date: YYYY-MM-DD
source_project_manifest: string
source_literature_map: string
source_outline: string
linked_project_sessions: []
linked_citekeys: []
```

### 5.8 Concept / Method / Comparison / Topic Pages

Recommended frontmatter:

```yaml
type: concept | method | comparison | topic
slug: string
title: string
aliases: []
status: active | needs_review | stale | archived
related_papers: []
last_updated: YYYY-MM-DD
```

Additional fields by type may be used as needed:

- concept: `related_methods`, `related_topics`
- method: `related_concepts`, `related_comparisons`
- comparison: `comparison_key`, `entities`
- topic: `related_concepts`, `related_methods`

### 5.9 Reports

Weekly report:

```yaml
type: weekly-report
week_id: YYYY-Www
week_start: YYYY-MM-DD
week_end: YYYY-MM-DD
generated_on: YYYY-MM-DD
```

Lint report:

```yaml
type: lint-report
report_date: YYYY-MM-DD
scope: string
auto_fixes_applied: []
high_risk_items: []
```

Bootstrap report:

```yaml
type: bootstrap-report
report_date: YYYY-MM-DD
scope: string
created_files: []
created_directories: []
conflicts_found: []
```

### 5.10 Control File Conventions

`ProjectIndex.md`

- key column: `project_id`
- derived from `project.yml`
- may include `project_type`, `parent_project`, `scope`, `objective`, `keywords`, `project_path`, and derived `paper_counts`

`ProjectTree.md`

- derived tree view of the current `parent_project` relations
- must not be treated as the authoritative source over `project.yml`

`ProjectAssignmentQueue.md`

Suggested columns:

- `queue_id`
- `citekey`
- `paper_wiki`
- `suggested_outcome`
- `suggested_path`
- `candidate_parent`
- `candidate_child`
- `suggested_bucket`
- `relevance`
- `theme`
- `role`
- `priority`
- `outline_sections`
- `status`
- `source`
- `notes`

Rules:

- created by `S1`
- default status should be `pending_user_confirmation`
- does not itself assign the paper; it records a recommendation
- `suggested_outcome` should be one of:
  - `parent_child`
  - `parent_only`
  - `none`
  - `needs_review`

## 6. SessionIndex Convention

`Management/SessionIndex.md` should index both paper and project/hybrid sessions.

Suggested columns:

- `session_id`
- `session_type`
- `primary_target`
- `citekey`
- `linked_project`
- `merge_status`
- `match_confidence`
- `path`

## 7. Skill Dependency Rules

- `S0` initializes and validates all baseline structures.
- `S1` writes global paper knowledge first, then project suggestions.
- `S2` maintains both the global wiki and the project tree.
- `S3` summarizes both the global and project layers.
- `S4` reviews papers and project nodes.
- `S5` retrieves from both the wiki and the project tree.
- `S6` captures paper, project, and hybrid discussion.

## 8. Write Safety

1. `LiteratureNotes/` is read-only for these skills.
2. Project membership is written only through `project.yml`.
3. Do not write project fields into `LiteratureNotes/` or `Notes/Papers/`.
4. When a match is unstable, prefer `_unmatched/`.
5. When a change is structurally safe but semantically risky, report before normalizing.
