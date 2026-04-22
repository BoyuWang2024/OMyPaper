---
type: management-index
last_updated: 2026-04-22
---

# Management Index

## Core Control Files

- [README_SKILLS.md](README_SKILLS.md)
- [FrontmatterSpec.md](FrontmatterSpec.md)
- [SkillCalls.md](SkillCalls.md)
- [PaperRegistry.md](PaperRegistry.md)
- [SessionIndex.md](SessionIndex.md)
- [ProjectIndex.md](ProjectIndex.md)
- [ProjectTree.md](ProjectTree.md)
- [ProjectAssignmentQueue.md](ProjectAssignmentQueue.md)
- [CurrentQuestions.md](CurrentQuestions.md)
- [log.md](log.md)

## Skill Specs

- [S0](Skills/S0_vault_bootstrap_consistency_check/SKILL.md)
- [S1](Skills/S1_paper_ingest_wiki_builder/SKILL.md)
- [S2](Skills/S2_wiki_lint_refactor/SKILL.md)
- [S3](Skills/S3_weekly_report_builder/SKILL.md)
- [S4](Skills/S4_paper_recall_review/SKILL.md)
- [S5](Skills/S5_local_retrieval_reread_navigator/SKILL.md)
- [S6](Skills/S6_session_capture_paper_matching/SKILL.md)

## Main Vault Areas

- `LiteratureNotes/`: raw imported reading trace; read-only for S0-S6
- `Notes/`: global paper wiki layer
- `projects/`: hierarchical project tree layer
- `Management/SessionNotes/`: paper-session layer
- `Management/ProjectSessionNotes/`: project / hybrid session layer

## Current Vault Status

- Skill system scaffolding is present and project-aware.
- Test-derived wiki pages, sessions, reports, and queue entries have been cleared for migration.
- `Notes/` currently has no retained wiki pages.
- `projects/` exists but still has no retained project manifests.
- `ProjectAssignmentQueue.md` is initialized and empty.
- No retained paper or project sessions are currently registered.
- `LiteratureNotes/` may still contain user-imported raw source notes, which were not treated as generated test artifacts in this cleanup.

## Next Recommended Actions

1. Import raw paper notes into `LiteratureNotes/` when needed.
2. Create or import project directories under `projects/{project_id}/` when project work begins.
3. Use `S6` to capture valuable paper or project discussion.
4. Use `S1` to compile important papers into the global wiki and queue project-placement suggestions.
5. After user confirmation, reflect final project placement in the relevant `project.yml`.
6. Use `S2` and `S3` for ongoing maintenance and reporting.
