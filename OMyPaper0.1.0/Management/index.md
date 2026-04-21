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

- Initial scaffolding completed on `2026-04-21`.
- Latest bootstrap report: `Management/BootstrapReports/2026-04-21-2058-initial-bootstrap.md`
- `LiteratureNotes/` currently has no imported note files.
- `Notes/` currently has no paper or topic pages yet.
- No session notes have been registered yet.

## Next Recommended Actions

1. Import raw paper notes into `LiteratureNotes/`.
2. Use S6 to capture high-value reading discussions.
3. Use S1 to compile the first paper wiki page.
4. Use S2 and S3 on a weekly cadence.
