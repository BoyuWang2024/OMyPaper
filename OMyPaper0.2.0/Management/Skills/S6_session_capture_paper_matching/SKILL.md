# S6 - Session Capture & Paper Matching

## 1. Skill Name

- ID: `S6`
- English: `Session Capture & Paper Matching`
- Chinese: `阅读会话沉淀与论文匹配`

## 2. Purpose / Goal

S6 distills the high-value parts of a discussion into standardized session notes that can later be consumed by S1, S3, S4, and S5.

It is the memory-capture layer between transient chat and durable writing.

The upgraded target types are:

- `paper session`
- `child-project session`
- `parent-project session`
- `hybrid session`

S6 must preserve only reusable insight, not raw chat transcript.

## 3. Typical Use Cases

- after a good paper discussion with the agent
- after clarifying a confusing method or experiment
- after forming a better interpretation during reading
- after discussing the structure or gap of a child or parent project
- when the user wants to save a discussion before moving on

## 4. Allowed Input Forms

S6 accepts:

- the current conversation context
- explicit `citekey`
- explicit `zotero_key`
- explicit title
- explicit `project_id`
- raw session summary supplied by the user

Preferred input pattern:

```text
执行 S6，把我们刚才关于这篇论文 / 这个项目的高价值讨论沉淀成 SessionNote 或 ProjectSessionNote；先保守匹配，匹配不稳就保存到对应 _unmatched/。
```

## 5. Must Read

S6 must read:

- the current conversation content or provided session summary
- `Management/SessionIndex.md`
- `Management/PaperRegistry.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/FrontmatterSpec.md`

When matching a target, S6 should also read as needed:

- candidate literature notes in `LiteratureNotes/`
- existing paper pages in `Notes/Papers/`
- candidate project manifests in `projects/`

## 6. Allowed Write Scope

S6 may write to:

- `Management/SessionNotes/{citekey}/`
- `Management/SessionNotes/_unmatched/`
- `Management/ProjectSessionNotes/{project_id}/`
- `Management/ProjectSessionNotes/_hybrid/`
- `Management/ProjectSessionNotes/_unmatched/`
- `Management/SessionIndex.md`
- `Management/log.md`

S6 must not write to:

- `LiteratureNotes/`
- formal wiki pages under `Notes/`
- any `project.yml`

## 7. Standard Execution Flow

1. Identify whether the current discussion is primarily about:
   - one paper
   - one child project
   - one parent project
   - both paper and project together
2. Resolve paper identity using the fixed paper priority:
   - citekey
   - zotero key
   - title
   - source note / attachment path
   - current-session guess
3. Resolve project identity using the fixed project priority:
   - project_id
   - exact project manifest path
   - exact title
   - parent-child context from the local tree
   - current-session guess
4. Assign `session_type`, `merge_status`, and `match_confidence`.
5. Route by target type:
   - paper session -> `Management/SessionNotes/{citekey}/`
   - project session -> `Management/ProjectSessionNotes/{project_id}/`
   - hybrid session -> `Management/ProjectSessionNotes/_hybrid/`
   - unstable paper / project match -> corresponding `_unmatched/`
6. Extract only high-value content from the discussion:
   - user questions worth preserving
   - Codex answers worth keeping where appropriate
   - updated understanding
   - still uncertain points
   - evidence anchors
   - candidate wiki or project updates
7. Create a standardized session note using the proper template and required metadata.
8. For project sessions, record at minimum:
   - `linked_project`
   - `parent_project`
   - `child_project`
   - `linked_citekeys`
   - `scope_level`
   - `updated_project_understanding`
   - `still_uncertain`
   - `candidate_project_updates`
9. Set `merge_status`:
   - usually `pending` for matched notes
   - `unmatched` for unstable identity
   - `needs_review` when useful but risky
10. Update `Management/SessionIndex.md`.
11. Append a log entry.
12. Report the created path and whether the note is ready for S1, S3, S4, or S5.

## 8. Output Requirements

S6 should produce:

- one standardized paper, project, or hybrid session note
- one session index update
- one log entry
- a clear statement of target type, match status, and remaining uncertainty

The note body should include the durable sections appropriate to its type.

For paper sessions this should still include:

- `My Questions`
- `Codex Answers Worth Keeping`
- `My Updated Understanding`
- `Still Uncertain`
- `Evidence Anchors`
- `Candidate Wiki Updates`

For project sessions this should include:

- `My Questions`
- `Updated Project Understanding`
- `Still Uncertain`
- `Evidence Anchors`
- `Candidate Project Updates`

## 9. Hard Constraints / Prohibitions

- Do not modify `LiteratureNotes/`.
- Do not save the raw chat transcript as-is.
- Only preserve high-value material.
- Do not formally modify `Notes/` inside S6.
- Do not formally modify `project.yml` inside S6.
- If matching is unstable, prefer the relevant `_unmatched/` path over forced assignment.
- Do not collapse paper sessions and project sessions into the same template by default.

## 10. Matching and Exception Rules

- `exact`: direct citekey or project_id match, or direct source-file match
- `high`: strong metadata match with little ambiguity
- `medium`: plausible match, but one anchor is missing
- `low`: guess only; store under `_unmatched/`
- `unmatched`: no reliable target

Additional handling rules:

- If multiple candidate papers are plausible, use `Management/SessionNotes/_unmatched/`.
- If multiple candidate projects are plausible, use `Management/ProjectSessionNotes/_unmatched/`.
- If the session genuinely covers both paper and project logic, keep it as `session_type: hybrid`.
- If the conversation is mostly logistics with little durable insight, S6 should say no high-value session note should be created.

## 11. Relationship With Other Skills

- S6 feeds S1 directly for paper ingest.
- S6 also feeds S3, S4, and S5 by preserving project-level reasoning and hybrid reasoning.
- S4 can benefit from session notes created by S6.
- S3 may summarize S6 outputs in the weekly report.
- S6 does not replace S1 or S5.

## 12. Call Examples

### Example 1

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论整理成标准化 paper session note，只保留高价值内容，并设为 merge_status: pending。
```

### Example 2

```text
执行 S6，把当前关于父项目 uncertainty 和子项目 uncertainty_llpr 的讨论沉淀下来。先尝试匹配项目；如果匹配不稳，就放到 Management/ProjectSessionNotes/_unmatched/，不要正式改 Notes/ 或 project.yml。
```

## 13. Success Criteria

S6 is complete only if:

- a session note was created or a justified refusal was given because the discussion lacked durable value
- the note contains standardized metadata
- the target type and match status are explicit
- `Management/SessionIndex.md` was updated consistently
- the user can later hand the note to S1, S3, S4, or S5 without additional cleanup
