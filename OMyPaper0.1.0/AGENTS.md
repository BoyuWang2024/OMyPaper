# OMyPaper Agent Control File

This file is the project-level control document for agents working inside `OMyPaper/`.

It defines:

- what this repository is
- what each top-level directory means
- what may or may not be modified
- how to route work across Skills `S0` to `S6`
- what to do when the user says `执行 S1`, `使用 S6`, or similar
- what safety rules override convenience

This file is a routing and governance layer, not a replacement for the detailed skill specs in `Management/Skills/.../SKILL.md`.

## 1. Project Overview

`OMyPaper/` is a local paper knowledge vault for the workflow:

`Zotero -> ZotLit -> LiteratureNotes -> SessionNotes -> Notes wiki -> Weekly / Review outputs`

The repository is designed for long-lived, traceable knowledge accumulation rather than one-off PDF Q&A.

There are three main knowledge layers:

- `LiteratureNotes/`: raw layer
  - imported from Zotero/ZotLit
  - contains original reading traces, highlights, attachment links, page notes
- `Notes/`: wiki layer
  - maintained by the agent as durable paper, concept, method, comparison, and topic pages
- `Management/`: control layer
  - contains rules, indices, templates, logs, reports, session notes, and skill specs

Core idea:

- raw notes preserve the source reading trace
- wiki pages preserve structured long-term knowledge
- management files preserve control, routing, traceability, and reporting

## 2. Directory Semantics

| Directory | Meaning | Default Agent Behavior | Writable? |
| --- | --- | --- | --- |
| `Attachments/` | PDFs, figures, supplementary files, local resources | treat as attachment/resource layer; read when needed for evidence | usually no direct edits |
| `LiteratureNotes/` | raw reading trace imported from ZotLit | treat as source layer; read carefully; do not rewrite | **No** for S0-S6 |
| `Notes/` | durable wiki layer | create/update paper, concept, method, comparison, and topic pages conservatively | **Yes** |
| `Management/` | control and reporting layer | update indices, reports, templates, session notes, logs, and skill-facing control files | **Yes** |

### Notes on Top-Level Semantics

- `LiteratureNotes/` is the canonical raw layer and is read-only by default.
- `Notes/` is the canonical derived knowledge layer and is writable by the agent.
- `Management/` is the canonical control layer and is writable by the agent.
- `Attachments/` should be treated as supporting evidence, not as the main writing target.

## 3. Global Hard Rules

These rules apply to all work in this repository unless the user explicitly overrides them and the override does not violate a stronger safety constraint.

1. Do not directly modify files under `LiteratureNotes/`.
2. Use `citekey` as the primary paper key whenever possible.
3. Paper matching priority is fixed:
   - `citekey`
   - `zotero-key`
   - `title`
   - source note / attachment
   - current-session guess
4. When matching is unstable, be conservative. Do not force ingest, merge, or archival.
5. Keep these three information classes distinct whenever summarizing paper content:
   - `Paper claim`
   - `My note`
   - `LLM synthesis`
6. Every write-capable task must be able to report:
   - created files
   - updated files
   - exact paths
   - unresolved or still-uncertain items
7. High-risk changes should be reported before execution whenever possible.
8. Do not use one Skill to do another Skill's job.
9. Do not fabricate completeness. Traceability is more important than polish.
10. Prefer local evidence over model memory.

## 4. Control Files and Canonical References

Agents should treat the following files as the main control surface for the repository:

- `Management/README_SKILLS.md`: skill handbook and workflow overview
- `Management/SkillCalls.md`: invocation patterns and example call forms
- `Management/FrontmatterSpec.md`: metadata and schema rules
- `Management/index.md`: management index and current vault status
- `Management/log.md`: append-style operational history
- `Management/PaperRegistry.md`: paper-level registry keyed by `citekey`
- `Management/SessionIndex.md`: index of captured session notes
- `Management/Templates/`: canonical markdown templates

Detailed behavior for each Skill lives in:

- `Management/Skills/S0_vault_bootstrap_consistency_check/SKILL.md`
- `Management/Skills/S1_paper_ingest_wiki_builder/SKILL.md`
- `Management/Skills/S2_wiki_lint_refactor/SKILL.md`
- `Management/Skills/S3_weekly_report_builder/SKILL.md`
- `Management/Skills/S4_paper_recall_review/SKILL.md`
- `Management/Skills/S5_local_retrieval_reread_navigator/SKILL.md`
- `Management/Skills/S6_session_capture_paper_matching/SKILL.md`

## 5. Skill Routing Table

This section is route-level guidance only. Do not treat it as the full operating procedure.

| Skill | Primary Job | Use It When | It Must Not Do | Relationship |
| --- | --- | --- | --- | --- |
| `S0` | bootstrap and consistency checking | first setup, after large imports, after structural changes, before major maintenance | organize paper content or rewrite raw notes | prepares the vault for all other Skills |
| `S1` | single-paper ingest into the wiki | one paper is ready to become a durable paper page | act like whole-vault maintenance, rewrite raw notes, force uncertain claims | consumes raw notes and usually benefits from S6 |
| `S2` | whole-vault structural health and low-risk refactor | weekly maintenance, after many ingests, before reporting | do deep semantic rewriting or risky merges | keeps `Notes/` navigable after S1 |
| `S3` | weekly synthesis and reporting | end-of-week review or progress checkpoint | invent conclusions or output a raw activity dump | summarizes outputs from S1/S2/S4/S6 |
| `S4` | recall and review of one paper | quick recall, meeting prep, interview prep, self-test | replace formal wiki writing or pretend recall is certainty | depends on paper wiki and optionally session notes |
| `S5` | local-first retrieval and re-read navigation | concept/method/comparison questions grounded in the local vault | answer without paths, hide evidence gaps, silently use outside knowledge as local evidence | read-only by default; may recommend S1 or S4 |
| `S6` | capture high-value discussion into session notes | a valuable paper discussion should be preserved for later ingest | save raw transcript or directly edit formal wiki | feeds S1 and later supports S4/S3 |

### Routing Principles

- If the task is about environment readiness, start with `S0`.
- If the task is about one paper becoming a formal wiki asset, use `S1`.
- If the task is about the whole wiki structure, use `S2`.
- If the task is about weekly synthesis, use `S3`.
- If the task is about remembering how the user understood one paper, use `S4`.
- If the task is about answering a knowledge question from local files first, use `S5`.
- If the task is about preserving a valuable discussion before it is lost, use `S6`.

## 6. Default Workflows

### A. Daily Reading Workflow

1. Read and annotate in Zotero.
2. Import raw notes into `LiteratureNotes/` via ZotLit.
3. If a high-value discussion with the agent happens during reading, use `S6`.
4. Once the paper is mature enough for durable storage, use `S1`.
5. If the user later wants a quick refresher, use `S4`.
6. If the user asks a concept or method question, use `S5`.

### B. Weekly Maintenance Workflow

1. Use `S2` to inspect `Notes/` for structural health.
2. Review the lint report and keep high-risk edits conservative.
3. Use `S3` to generate the weekly report.

### C. Bootstrap / Reorganization Workflow

1. Use `S0` before large-scale work.
2. Confirm reports, conflicts, and missing pieces.
3. Only then continue with `S1`, `S2`, or `S6` depending on the next task.

### D. Recall / Meeting Prep Workflow

1. Use `S4` first for recall-oriented output.
2. If the user then asks broader cross-paper questions, use `S5`.

### E. Discussion-to-Wiki Workflow

1. Use `S6` to capture the valuable discussion.
2. Use `S1` later to absorb matched `pending` session notes into the paper wiki.
3. Use `S2` afterward if the new paper created structural issues or new links.

## 7. Invocation Convention

Agents should assume this `AGENTS.md` is the local operating contract for the repository. The user does **not** need to explicitly say "read AGENTS.md first" every time.

Interpretation rules:

- `执行 S1，...` means: route the task to Skill `S1` and follow the detailed `S1` spec.
- `使用 S6，...` means: route the task to Skill `S6` and prioritize S6's scope and constraints.
- `执行 S5，问题是...` means: answer via local retrieval and re-read navigation, not via generic model recall.

Recommended user-facing call style:

- `执行 S0，...`
- `执行 S1，...`
- `执行 S2，...`
- `执行 S3，...`
- `执行 S4，...`
- `执行 S5，问题是...`
- `执行 S6，...`

Agents should not require the user to say:

- "先阅读 AGENTS.md"
- "再去读 Skill 说明"
- "按某个隐藏内部流程做"

That behavior is already expected by default inside this repository.

If the user explicitly says "先按 AGENTS.md 工作", treat that as a request for stricter adherence to this file's routing and safety model.

## 8. Conflict Handling, Safety, and Exceptions

When something is uncertain, conflicting, or under-specified, prefer conservative handling.

### Matching and Routing

- If a paper match is unstable during `S6`, store the session note under `Management/SessionNotes/_unmatched/`.
- If a paper identity is unstable during `S1`, stop formal ingest and report the ambiguity.
- If multiple Skills could be involved, split the task by responsibility instead of blending them into one vague operation.

### Merge and Review States

- Use `pending` when content is useful but not yet ingested.
- Use `merged` only after safe absorption into the formal wiki.
- Use `needs_review` when the material looks useful but is not safe to finalize automatically.
- Use `_unmatched/` or `unmatched` when paper identity is not trustworthy.

### Structural Conflicts

- If two pages may be duplicates, do not auto-merge unless the merge is obviously low risk.
- If a broken link has multiple plausible targets, report instead of guessing.
- If a change is structurally safe but semantically risky, prefer writing a report.

### Scope Control

- Do not expand `S1` into whole-vault cleanup.
- Do not expand `S2` into deep paper synthesis.
- Do not expand `S5` into an unconstrained model answer.
- Do not expand `S6` into formal wiki writing.

### Evidence Discipline

- Do not treat inference as source fact.
- Do not silently fill local evidence gaps with model common sense.
- If the local record is thin, say so explicitly.

## 9. Relationship to Skill Specs

This file is the global control layer. Each `SKILL.md` remains the detailed operational spec for that Skill.

Interpretation rule:

- `AGENTS.md` controls global routing, directory semantics, safety boundaries, and default workflow.
- each `SKILL.md` controls the detailed execution behavior of its own Skill.

If there is a local tension between documents:

- follow `AGENTS.md` for global constraints and repository-wide safety
- follow the corresponding `SKILL.md` for step-by-step execution details
- if the two appear meaningfully inconsistent, choose the more conservative interpretation and say so in the output

## 10. Output Expectations

After any write action, agents should report:

- what was created
- what was updated
- exact file paths
- what remains uncertain

For multi-file updates, agents should also explain why those files were touched.

For paper work, agents should:

- prefer `citekey` as the stable handle
- say when matching relied on weaker signals
- preserve the distinction between `Paper claim`, `My note`, and `LLM synthesis`

For knowledge-base answers, agents should:

- prioritize local files first
- provide concrete paths
- point to sections or re-read locations when possible
- explicitly say when the vault is too thin to support a strong answer

## 11. Practical Reminder for Future Agents

When entering this repository, assume the following:

- this is a maintained local paper knowledge system, not a scratch workspace
- `LiteratureNotes/` is source memory, not editable workspace
- `Notes/` and `Management/` are the main writable surfaces
- Skills `S0` to `S6` already exist and should be reused, not reinvented
- the safest default is conservative, traceable, citekey-first work

If unsure how to proceed, start by checking:

1. `Management/index.md`
2. `Management/README_SKILLS.md`
3. the relevant `Management/Skills/.../SKILL.md`

