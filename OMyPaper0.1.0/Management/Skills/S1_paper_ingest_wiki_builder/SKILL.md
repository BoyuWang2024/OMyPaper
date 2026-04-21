# S1 - Paper Ingest & Wiki Builder

## 1. Skill Name

- ID: `S1`
- English: `Paper Ingest & Wiki Builder`
- Chinese: `单篇论文笔记整理`

## 2. Purpose / Goal

S1 formally compiles **one paper** into the durable wiki layer. It turns a raw literature note plus pending high-value session captures into a stable paper page and, only when necessary, related concept/method/comparison/topic pages.

S1 is the main bridge from `LiteratureNotes/` to `Notes/`.

## 3. Typical Use Cases

- after finishing a paper reading session
- after one or more good S6 captures exist for a paper
- when a paper is important enough to deserve a durable wiki page
- when a paper already has a thin wiki page that now needs real structure

## 4. Allowed Input Forms

S1 accepts:

- explicit `citekey`
- `zotero_key`
- exact paper title
- literature note path
- attachment path if it is the only stable anchor

Preferred input pattern:

```text
执行 S1，把 citekey=yourcitekey 正式编译进本地 wiki。优先读取 pending session notes，并显式区分 paper claim / my note / LLM synthesis。
```

## 5. Must Read

S1 must read:

- corresponding PDF if it can be confidently located
- `LiteratureNotes/{citekey}.md`
- newest relevant files under `Management/SessionNotes/{citekey}/` with `merge_status: pending`
- `Management/CurrentQuestions.md`
- `Notes/` pages already related to this paper
- `Management/PaperRegistry.md`
- `Management/FrontmatterSpec.md`

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
- session files under `Management/SessionNotes/{citekey}/` when updating `merge_status`

S1 must **not** write to:

- `LiteratureNotes/`
- unrelated wiki pages
- `_unmatched/` session notes unless it is only reading them for context and not reclassifying them

## 7. Standard Execution Flow

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
5. Read the latest pending session notes first. These represent high-value user understanding and should influence the final page.
6. Read `Management/CurrentQuestions.md` to capture the user's current focus.
7. Read existing relevant wiki pages to avoid duplicate pages and to link the new paper into the vault.
8. Build or update `Notes/Papers/{citekey}.md` using the paper template.
9. Ensure the paper page includes at least:
   - basic information
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
12. Update `Management/index.md` if the global navigation changed materially.
13. Append a log entry to `Management/log.md`.
14. Mark safely absorbed session notes from `pending` to `merged`. If a session is useful but still ambiguous, use `needs_review` instead.
15. Report created/updated files and unresolved items.

## 8. Output Requirements

S1 should return:

- created files
- updated files
- whether the paper wiki is new or updated
- which session notes were absorbed
- which topics remain `待核实`
- whether any new concept/method/comparison/topic pages were created

## 9. Hard Constraints / Prohibitions

- Never modify `LiteratureNotes/`.
- Always read pending session notes before finalizing the paper wiki.
- Do not write uncertain interpretations as paper facts.
- Do not over-create pages just to make the vault look complete.
- Do not force-match a paper when identity is unstable.
- Do not treat session speculation as a confirmed paper claim.

## 10. Matching and Exception Rules

- If citekey resolution is unstable, stop formal ingest and report the ambiguity.
- If the literature note exists but the PDF cannot be located, continue only if the literature note and session evidence are strong enough; note the missing PDF explicitly.
- If related concept pages appear duplicated, do not merge them inside S1 unless one duplicate is clearly a thin stub and the merge is trivial.
- If a session note contains good intuition but weak evidence, keep it separate from paper claims and mark it as `LLM synthesis` or `待核实`.

## 11. Relationship With Other Skills

- S1 consumes S6 outputs.
- S2 may later lint or lightly normalize pages created by S1.
- S3 summarizes S1 outputs at the weekly level.
- S4 relies on S1 paper pages for recall.
- S5 often navigates through paper pages created by S1.

## 12. Call Examples

### Example 1

```text
执行 S1，把 citekey=vaswani2017attention 正式编译进 Notes/Papers/vaswani2017attention.md，并更新必要的 Concepts/Methods 页面。
```

### Example 2

```text
执行 S1，目标论文标题是 "Attention Is All You Need"。先保守匹配；如果论文身份不稳，不要正式写入，只把不确定点报告给我。
```

## 13. Success Criteria

S1 is complete only if all of the following are true:

- the target paper was matched confidently enough
- the literature note was read
- pending session notes were considered first
- the paper wiki exists or was meaningfully updated
- management files were updated consistently
- merged session notes were marked correctly
- all remaining uncertainty is explicitly called out

