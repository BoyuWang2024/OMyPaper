# S2 - Wiki Lint & Refactor

## 1. Skill Name

- ID: `S2`
- English: `Wiki Lint & Refactor`
- Chinese: `全库 Wikis 整理`

## 2. Purpose / Goal

S2 maintains the structural health of the whole `Notes/` layer. It checks for link quality, naming consistency, duplicate or orphaned pages, stale summaries, and explicit contradiction labeling. It may apply **low-risk** fixes, but it should avoid semantic overreach.

## 3. Typical Use Cases

- weekly maintenance of the vault
- after many S1 ingests
- before generating a weekly report
- after reorganizing topic or concept pages

## 4. Allowed Input Forms

S2 accepts:

- whole-vault scope: `Notes/`
- narrowed scope such as `Notes/Concepts/` or `Notes/Methods/`
- special focus such as `only duplicates` or `only broken links`

Preferred input pattern:

```text
执行 S2，对整个 Notes/ 做 lint 和低风险修复，生成 lint 报告；高风险项只写报告，不直接执行。
```

## 5. Must Read

S2 must read:

- `Notes/`
- `Management/index.md`
- `Management/PaperRegistry.md`
- `Management/FrontmatterSpec.md`

When helpful, S2 may read:

- recent `Management/WeeklyReports/`
- recent `Management/ReviewNotes/`

## 6. Allowed Write Scope

S2 may write to:

- `Management/LintReports/YYYY-MM-DD.md`
- `Management/index.md`
- low-risk fixes inside `Notes/`

Low-risk fixes include:

- adding missing related links when target is unambiguous
- fixing clearly broken internal links where the correct target is obvious
- standardizing obvious frontmatter omissions
- refreshing a top-level index page or navigation section

S2 must **not**:

- delete user-authored core content
- rewrite large semantic sections
- merge two substantive pages based on guesswork

## 7. Standard Execution Flow

1. Inventory pages under `Notes/` by type.
2. Build a light link graph:
   - incoming links
   - outgoing links
   - pages with no visible connections
3. Detect issues:
   - orphan pages
   - duplicate concept pages
   - duplicate method pages
   - inconsistent naming
   - broken links
   - missing backlinks / related links
   - likely missing comparison pages
   - stale summaries
   - contradictions not explicitly labeled
4. Classify each issue by risk:
   - low risk: safe to fix automatically
   - medium risk: report with a suggested action
   - high risk: report only
5. Apply only low-risk fixes.
6. Generate a lint report in `Management/LintReports/YYYY-MM-DD.md`.
7. Update `Management/index.md` if helpful for navigation.
8. Append a log entry.
9. Return the report path plus fixes applied and follow-up items.

## 8. Output Requirements

The lint report should include:

- summary counts
- issue categories
- exact file paths
- fixes applied
- medium/high-risk items requiring review

The user-facing response should include:

- report path
- files updated automatically
- items left for manual review

## 9. Hard Constraints / Prohibitions

- Structural maintenance only; do not do deep single-paper synthesis here.
- Do not delete core content written by the user.
- Do not perform large semantic rewrites.
- Do not auto-merge pages when the overlap is interpretive rather than exact.
- Do not touch `LiteratureNotes/`.

## 10. Matching and Exception Rules

- If two pages share a similar title but contain materially different scopes, report as `possible duplicate`, not `merge now`.
- If a broken link has multiple plausible targets, do not auto-fix it.
- If a page is stale but still structurally valid, report it under `stale summaries` without rewriting the summary.
- If contradictions are subtle and evidence is weak, flag them instead of normalizing them away.

## 11. Relationship With Other Skills

- S2 follows S1; it does not replace S1.
- S3 often benefits from the cleaner structure after S2.
- S5 navigation quality improves when S2 keeps links and naming healthy.
- S4 can also benefit from better backlinks and comparison coverage.

## 12. Call Examples

### Example 1

```text
执行 S2，对整个 Notes/ 做结构健康检查，自动修复低风险断链与索引问题，并生成今天的 lint 报告。
```

### Example 2

```text
执行 S2，只检查 Concepts 和 Methods，重点找重复页、命名不一致和缺失 comparison 页；高风险项只报告。
```

## 13. Success Criteria

S2 is complete only if:

- a lint report was written
- all applied fixes were low-risk and traceable
- unresolved medium/high-risk items were listed clearly
- the response tells the user which paths changed and which problems remain

