# S1 - Paper Ingest & Wiki Builder

## 1. Skill Name

- ID: `S1`
- English: `Paper Ingest & Wiki Builder`
- Chinese: `单篇论文笔记整理`

## 2. Purpose / Goal

S1 formally compiles **one paper** into the durable global wiki layer, and then produces a conservative project-placement suggestion.

S1 is the main bridge from `LiteratureNotes/` to `Notes/`.

Its upgraded behavior is a mandatory three-phase flow:

1. compile the paper into the global wiki
2. generate project-placement suggestions based on the global result
3. write the suggestion into the assignment queue and wait for user confirmation

## 3. Typical Use Cases

- after finishing a paper reading session
- after one or more good S6 paper sessions exist
- when a paper is important enough to deserve a durable global wiki page
- when a paper already has a thin wiki page that now needs real structure
- when the user also wants conservative guidance about which parent / child project the paper may belong to

## 4. Allowed Input Forms

S1 accepts:

- explicit `citekey`
- `zotero_key`
- exact paper title
- literature note path
- attachment path if it is the only stable anchor

Preferred input pattern:

```text
执行 S1，把 citekey=yourcitekey 正式编译进全局 wiki。优先读取 pending session notes，先完成全局整理，再给出项目归属建议，但不要直接改 project.yml。
```

## 5. Must Read

S1 must read:

- corresponding PDF if it can be confidently located
- `LiteratureNotes/{citekey}.md`
- newest relevant files under `Management/SessionNotes/{citekey}/` with `merge_status: pending`
- relevant project or hybrid sessions when they sharpen placement judgment
- `Management/CurrentQuestions.md`
- `Notes/` pages already related to this paper
- `Management/PaperRegistry.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`
- `Management/FrontmatterSpec.md`
- `projects/` manifests when generating project suggestions

When ambiguity exists, S1 should also read:

- relevant concept pages
- relevant method pages
- relevant comparison pages
- relevant topic pages

## 6. Allowed Write Scope

S1 may write to:

- `Notes/Papers/{citekey}.md`
- relevant files under:
  - `Notes/Concepts/`
  - `Notes/Methods/`
  - `Notes/Comparisons/`
  - `Notes/Topics/`
- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/ProjectAssignmentQueue.md`
- session files under `Management/SessionNotes/{citekey}/` when updating `merge_status`

S1 must **not** write to:

- `LiteratureNotes/`
- unrelated wiki pages
- any `project.yml` by default
- `_unmatched/` session notes unless it is only reading them for context and not reclassifying them

## 7. Standard Execution Flow

### Phase 1 - Global Paper Ingest (mandatory)

1. Resolve the target paper identity using the fixed priority:
   - citekey
   - zotero key
   - title
   - source note or attachment path
2. Confirm the raw literature note exists. If not, stop and report.
3. Locate the PDF if possible through:
   - literature note metadata
   - attachment paths
   - nearby matching filenames
4. Read the literature note carefully.
5. Read the latest pending paper session notes first. These represent high-value user understanding and should influence the final page.
6. Read `Management/CurrentQuestions.md` to capture the user's current focus.
7. Read existing relevant wiki pages to avoid duplicate pages and to link the new paper into the vault.
8. Build or update `Notes/Papers/{citekey}.md` using the paper template.
9. Ensure the paper page includes at least:
   - basic information
   - source coverage
   - one-sentence summary
   - research question
   - core method
   - key experiments
   - results and limitations
   - user focus
   - relationship to existing vault
   - suggested re-read locations
   - related links
10. Update related concept/method/comparison/topic pages only if there is enough local evidence and the new link genuinely improves navigation.
11. Update `Management/PaperRegistry.md`.
12. Mark safely absorbed paper session notes from `pending` to `merged`. If a session is useful but still ambiguous, use `needs_review` instead.
13. If the raw note is thin or mostly metadata, explicitly record the evidence basis of the ingest so later agents know the page leaned on PDF and / or session evidence.

### Phase 2 - Project Placement Suggestion (mandatory)

14. Use the saved global ingest result as the main evidence base for project judgment.
15. Scan the current project tree:
    - `Management/ProjectIndex.md`
    - `Management/ProjectTree.md`
    - relevant `projects/*/project.yml`
    - relevant `文献地图.md` and `论文大纲.md` when helpful
16. Produce conservative placement suggestions that may include:
    - `suggested_outcome`
    - candidate parent project
    - candidate child project
    - recommended path such as `Parent -> Child`
    - `relevance`
    - `suggested_bucket` such as `core_refs` or `supporting_refs`
    - `theme`
    - `role`
    - `priority`
    - `outline_sections`
    - reason
17. Support all three suggestion outcomes:
    - parent + child
    - parent only
    - no project placement for now
18. Do **not** decide placement directly from raw PDF guesswork when the global wiki result is still thin.

### Phase 3 - Queue and Wait For User Confirmation (mandatory)

19. Write the suggestion into `Management/ProjectAssignmentQueue.md`.
20. Default queue status should be `pending_user_confirmation`.
21. Do **not** write directly into any `project.yml` unless the user explicitly requests that as a separate follow-up action.
22. Update `Management/index.md` if the global navigation changed materially.
23. Append a log entry to `Management/log.md`.
24. Report created/updated files and unresolved items.

## 8. Output Requirements

S1 should return:

- created files
- updated files
- whether the paper wiki is new or updated
- what the recorded evidence basis was
- which session notes were absorbed
- which topics remain `needs_review`
- whether any new concept/method/comparison/topic pages were created
- one structured project-placement suggestion
- the queue path and queue row / entry that was added or updated

## 9. Hard Constraints / Prohibitions

- Never modify `LiteratureNotes/`.
- Always read pending paper session notes before finalizing the paper wiki.
- Do not write uncertain interpretations as paper facts.
- Do not over-create pages just to make the vault look complete.
- Do not force-match a paper when identity is unstable.
- Do not treat session speculation as a confirmed paper claim.
- Do not directly edit `project.yml` during the default S1 flow.
- Do not write project-membership fields into paper wiki frontmatter.

## 10. Matching and Exception Rules

- If citekey resolution is unstable, stop formal ingest and report the ambiguity.
- If the literature note exists but the PDF cannot be located, continue only if the literature note and session evidence are strong enough; note the missing PDF explicitly.
- If related concept pages appear duplicated, do not merge them inside S1 unless one duplicate is clearly a thin stub and the merge is trivial.
- If a session note contains good intuition but weak evidence, keep it separate from paper claims and mark it as `LLM synthesis` or `needs_review`.
- If project fit is weak or split across multiple plausible branches, queue a conservative `no placement yet` or `needs_review` suggestion instead of forcing a path.

## 11. Relationship With Other Skills

- S1 consumes paper-session outputs from S6.
- S1 may consult project-session outputs from S6 during Phase 2.
- S2 may later lint or lightly normalize pages created by S1 and the resulting project-tree navigation.
- S3 summarizes S1 outputs at the weekly level.
- S4 relies on S1 paper pages for recall.
- S5 often navigates through paper pages created by S1 and project suggestions created by S1.

## 12. Call Examples

### Example 1

```text
执行 S1，把 citekey=vaswani2017attention 正式编译进 Notes/Papers/vaswani2017attention.md，并在全局整理完成后给出项目归属建议，但不要直接改 project.yml。
```

### Example 2

```text
执行 S1，目标论文标题是 "Attention Is All You Need"。先保守匹配；如果论文身份不稳，不要正式写入；如果身份稳定，就完成全局整理，并把父项目/子项目建议写入 ProjectAssignmentQueue。
```

## 13. Success Criteria

S1 is complete only if all of the following are true:

- the target paper was matched confidently enough
- the literature note was read
- pending paper session notes were considered first
- the paper wiki exists or was meaningfully updated
- management files were updated consistently
- merged paper session notes were marked correctly
- one project-placement suggestion was written into the queue
- all remaining uncertainty is explicitly called out
