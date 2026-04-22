# S3 - Weekly Report Builder

## 1. Skill Name

- ID: `S3`
- English: `Weekly Report Builder`
- Chinese: `周报生成`

## 2. Purpose / Goal

S3 generates a weekly synthesis from local vault activity.

It now summarizes both:

- global paper / wiki progress
- project-tree progress across parent and child projects

The weekly report is not a raw activity dump. It should highlight what changed in understanding, what is worth keeping, what remains uncertain, which project levels moved forward, and where to focus next.

## 3. Typical Use Cases

- end-of-week review
- project checkpoint
- lab meeting preparation
- self-tracking of reading and knowledge-base growth

## 4. Allowed Input Forms

S3 accepts:

- current week by default
- explicit week ID such as `2026-W17`
- explicit date range if needed

Preferred input pattern:

```text
执行 S3，基于本周的本地文件变化生成周报，不要写成流水账；要同时总结全局论文库和项目树推进，明确保留判断、不确定项和下周重点。
```

## 5. Must Read

S3 must read:

- `Management/log.md`
- `Notes/` files updated this week
- `LiteratureNotes/` files added or updated this week
- `Management/SessionNotes/` files from this week
- `Management/ProjectSessionNotes/` files from this week
- `projects/` files updated this week
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/CurrentQuestions.md`

It may also read:

- the latest lint report
- the latest review notes
- the latest assignment queue changes

## 6. Allowed Write Scope

S3 may write to:

- `Management/WeeklyReports/YYYY/YYYY-Www.md`
- `Management/index.md`
- `Management/log.md`

S3 should not modify existing content in `Notes/`, `LiteratureNotes/`, or `project.yml`.

## 7. Standard Execution Flow

1. Determine the target week boundaries.
2. Collect evidence from `Management/log.md`.
3. Scan this week's changed files in:
   - `Notes/`
   - `LiteratureNotes/`
   - `Management/SessionNotes/`
   - `Management/ProjectSessionNotes/`
   - `projects/`
4. Read `Management/CurrentQuestions.md` to capture open problems and ongoing themes.
5. Read `Management/ProjectTree.md` to understand which parent and child projects were active.
6. Synthesize a weekly report covering:
   1. new papers this week
   2. new core concepts this week
   3. important judgment updates
   4. focused parent projects
   5. focused child projects
   6. project progress
   7. parent-level new understanding
   8. current project gaps
   9. papers worth re-reading
   10. recommended focus for next week
   11. todo list
7. Explicitly label:
   - conclusions worth retaining
   - still-uncertain points
   - papers worth re-reading
   - parent projects worth prioritizing
   - child projects worth prioritizing
8. Write the report to `Management/WeeklyReports/YYYY/YYYY-Www.md`.
9. Update `Management/index.md` if useful.
10. Append a log entry.

## 8. Output Requirements

The weekly report must:

- be based on local files only
- include evidence-aware synthesis instead of a raw list
- mention why a paper, concept, or project mattered this week
- separate stable conclusions from unresolved uncertainty
- summarize parent-project and child-project movement distinctly
- suggest concrete next-week reading and project directions

## 9. Hard Constraints / Prohibitions

- Do not invent content not supported by local files.
- Do not write the report as a simple chronological log.
- Do not only list paper names without interpretation.
- Do not omit uncertainty when evidence is weak.
- Do not replace missing evidence with model common sense.

## 10. Matching and Exception Rules

- If a file changed this week but the change was minor or administrative, mention it only when it affects actual understanding or vault construction.
- If a paper was discussed heavily but not yet ingested, it may still appear in the weekly report if session notes support it.
- If a project was discussed heavily but not yet reflected in `project.yml`, it may still appear when project sessions support it.
- If evidence is too thin to claim a judgment update, move it under `still uncertain`.

## 11. Relationship With Other Skills

- S3 summarizes work created by S1, S2, S4, and S6.
- S2 often improves the structure S3 reads from.
- S5 is read-only and does not feed the weekly report unless the query resulted in file updates elsewhere.

## 12. Call Examples

### Example 1

```text
执行 S3，生成本周周报，重点突出哪些结论值得保留、哪些仍不确定、哪些父项目和子项目在推进、哪些论文值得二刷。
```

### Example 2

```text
执行 S3，为 2026-W17 生成周报，并根据 CurrentQuestions 给出下周建议重点推进的父项目、子项目和论文。
```

## 13. Success Criteria

S3 is complete only if:

- the weekly report file exists
- the report is evidence-based and not just a list
- stable conclusions, uncertainties, project priorities, and re-read targets are all explicit
- the user receives the exact report path and a short summary of the week
