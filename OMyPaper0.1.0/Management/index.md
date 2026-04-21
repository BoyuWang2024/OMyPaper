---
type: management-index
last_updated: 2026-04-21
---

# Management Index

## Core Control Files

- [README_SKILLS.md](./README_SKILLS.md)
- [SkillCalls.md](./SkillCalls.md)
- [FrontmatterSpec.md](./FrontmatterSpec.md)
- [CurrentQuestions.md](./CurrentQuestions.md)
- [PaperRegistry.md](./PaperRegistry.md)
- [SessionIndex.md](./SessionIndex.md)
- [log.md](./log.md)

## Skill Specs

- [S0_vault_bootstrap_consistency_check/SKILL.md](./Skills/S0_vault_bootstrap_consistency_check/SKILL.md)
- [S1_paper_ingest_wiki_builder/SKILL.md](./Skills/S1_paper_ingest_wiki_builder/SKILL.md)
- [S2_wiki_lint_refactor/SKILL.md](./Skills/S2_wiki_lint_refactor/SKILL.md)
- [S3_weekly_report_builder/SKILL.md](./Skills/S3_weekly_report_builder/SKILL.md)
- [S4_paper_recall_review/SKILL.md](./Skills/S4_paper_recall_review/SKILL.md)
- [S5_local_retrieval_reread_navigator/SKILL.md](./Skills/S5_local_retrieval_reread_navigator/SKILL.md)
- [S6_session_capture_paper_matching/SKILL.md](./Skills/S6_session_capture_paper_matching/SKILL.md)

## Main Vault Areas

- `LiteratureNotes/`: raw imported notes, read-only for S0-S6
- `Notes/Papers/`: canonical paper pages keyed by citekey
- `Notes/Concepts/`: concept pages
- `Notes/Methods/`: method pages
- `Notes/Comparisons/`: comparison pages
- `Notes/Topics/`: thematic topic pages
- `Management/SessionNotes/`: structured session capture
- `Management/WeeklyReports/`: weekly reports
- `Management/ReviewNotes/`: recall notes
- `Management/LintReports/`: whole-vault lint outputs
- `Management/BootstrapReports/`: bootstrap / consistency reports

## Operating Rules Snapshot

- Raw layer is read-only.
- Paper identity uses citekey first.
- Write conservatively and keep traceability.
- Distinguish `Paper claim`, `My note`, and `LLM synthesis`.
- Use S6 before S1 when a valuable discussion happened during reading.

## Current Vault Status

- Skill system scaffolding is present.
- Runtime and test outputs have been cleared for migration.
- `LiteratureNotes/` currently has no retained raw note files.
- `Notes/` currently has no retained paper, concept, method, comparison, or topic pages.
- No session notes have been registered yet.

## Next Recommended Actions

1. Import raw paper notes into `LiteratureNotes/`.
2. Use S6 when a substantive reading discussion produces reusable insight.
3. Use S1 to compile a paper into the wiki when ready.
4. Use S2 and S3 on a weekly cadence after real content accumulates.
