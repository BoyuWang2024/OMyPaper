# S0 - Vault Bootstrap & Consistency Check

## 1. Skill Name

- ID: `S0`
- English: `Vault Bootstrap & Consistency Check`
- Chinese: `规范检查 / 初始化`

## 2. Purpose / Goal

S0 is the environment and consistency guard for the whole vault. Its job is to make sure the repository has the minimum viable structure, control files, and cross-directory consistency needed for the rest of the workflow.

S0 does **not** organize paper content. It only checks readiness, initializes missing baseline files, and reports problems conservatively.

## 3. Typical Use Cases

- first-time setup of the vault
- after moving files manually across directories
- after bulk-importing notes from ZotLit
- before starting systematic S1/S2/S3 work
- after suspected metadata drift or duplicate paper pages

## 4. Allowed Input Forms

S0 accepts any of the following inputs:

- no argument, meaning "check the whole vault"
- explicit scope such as `LiteratureNotes/`, `Notes/`, or `Management/`
- instruction to initialize missing management files
- instruction to only report conflicts without auto-fixing

Preferred input pattern:

```text
执行 S0，对整个 OMyPaper vault 做 bootstrap 与一致性检查，补齐缺失基础文件，并输出保守报告。
```

## 5. Must Read

S0 must read these areas when they exist:

- repository root directories
- `LiteratureNotes/`
- `Notes/`
- `Management/`
- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/CurrentQuestions.md`
- `Management/FrontmatterSpec.md`

It should also inspect file stems and frontmatter where necessary to detect duplicate citekeys and duplicate paper pages.

## 6. Allowed Write Scope

S0 may write only to:

- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/CurrentQuestions.md`
- `Management/BootstrapReports/*.md`
- missing baseline directories under `Management/` and `Notes/`

S0 must **not** modify:

- any file under `LiteratureNotes/`
- substantive paper content under `Notes/`

## 7. Standard Execution Flow

1. Confirm that top-level directories exist:
   - `Attachments/`
   - `LiteratureNotes/`
   - `Notes/`
   - `Management/`
2. Confirm that the core management files exist. If one is missing, initialize it conservatively from the current vault standard.
3. Confirm that the expected support directories exist:
   - `Management/Templates/`
   - `Management/Skills/`
   - `Management/SessionNotes/`
   - `Management/SessionNotes/_unmatched/`
   - `Management/WeeklyReports/`
   - `Management/ReviewNotes/`
   - `Management/LintReports/`
   - `Management/BootstrapReports/`
   - `Notes/Papers/`
   - `Notes/Concepts/`
   - `Notes/Methods/`
   - `Notes/Comparisons/`
   - `Notes/Topics/`
4. Scan `LiteratureNotes/` for duplicate or suspicious paper identity:
   - duplicate filenames with same citekey stem
   - conflicting frontmatter citekeys
   - repeated zotero keys
   - suspicious same-title notes with different keys
5. Scan `Notes/Papers/` for duplicate paper pages:
   - duplicate citekey pages
   - same-title pages pointing to different citekeys
   - registry entries pointing to missing files
6. Cross-check management indices:
   - `PaperRegistry.md` against `LiteratureNotes/` and `Notes/Papers/`
   - `SessionIndex.md` against `Management/SessionNotes/`
7. Apply only low-risk fixes:
   - create missing control files
   - create missing support directories
   - refresh high-level status lines in `Management/index.md`
8. Write a bootstrap report to `Management/BootstrapReports/YYYY-MM-DD-HHmm.md`.
9. Append an entry to `Management/log.md`.
10. Return a user-facing summary with created paths, updated paths, and unresolved conflicts.

## 8. Output Requirements

S0 should produce:

- one bootstrap report file
- a concise summary of environment health
- a list of created files
- a list of updated files
- a list of conflicts that require manual review

The report should include:

- directory presence
- missing-file initialization
- duplicate citekey findings
- duplicate paper-page findings
- registry mismatches
- manual follow-up items

## 9. Hard Constraints / Prohibitions

- Do not modify any raw note under `LiteratureNotes/`.
- Do not delete paper pages or wiki pages.
- Do not auto-merge duplicate papers.
- Do not "repair" ambiguous conflicts by guessing.
- Do not rewrite substantive content in `Notes/`.

## 10. Matching and Exception Rules

- If a literature note filename and frontmatter citekey disagree, report it as a conflict.
- If two files share a title but not a citekey, mark as `possible duplicate`, not `confirmed duplicate`.
- If a registry row points to a missing file, mark it as `broken reference`.
- If a conflict cannot be resolved from local evidence, stop at reporting.

## 11. Relationship With Other Skills

- S0 is the entry-point sanity check before the other skills.
- S0 does not replace S1 or S2.
- S1 relies on S0's baseline structure.
- S2 may use S0's report as structural context, but S2 operates at the wiki-content layer.

## 12. Call Examples

### Example 1

```text
执行 S0，对当前 vault 做首次 bootstrap，补齐缺失管理文件，并输出环境报告。
```

### Example 2

```text
执行 S0，只做一致性检查：重点看 duplicate citekey、重复论文页和 PaperRegistry 是否失配；高风险项只报告，不自动修复。
```

## 13. Success Criteria

S0 is complete only if all of the following are true:

- the baseline directory structure exists
- the required management files exist
- a bootstrap report has been written
- `Management/log.md` records the run
- unresolved conflicts are explicitly listed instead of silently ignored

