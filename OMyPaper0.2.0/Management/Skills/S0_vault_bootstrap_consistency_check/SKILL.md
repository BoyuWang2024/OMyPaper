# S0 - Vault Bootstrap & Consistency Check

## 1. Skill Name

- ID: `S0`
- English: `Vault Bootstrap & Consistency Check`
- Chinese: `规范检查 / 初始化`

## 2. Purpose / Goal

S0 is the environment and consistency guard for the whole vault.

Its job is to make sure the repository has the minimum viable structure, control files, project-tree files, and cross-directory consistency needed for the rest of the workflow.

S0 does **not** organize paper content. It only checks readiness, initializes missing baseline files, and reports problems conservatively.

## 3. Typical Use Cases

- first-time setup of the vault
- after moving files manually across directories
- after bulk-importing notes from ZotLit
- after creating or reorganizing project directories
- before starting systematic S1/S2/S3 work
- after suspected metadata drift, duplicate paper pages, or broken project hierarchy

## 4. Allowed Input Forms

S0 accepts any of the following inputs:

- no argument, meaning "check the whole vault"
- explicit scope such as `LiteratureNotes/`, `Notes/`, `projects/`, or `Management/`
- instruction to initialize missing management and project-tree files
- instruction to only report conflicts without auto-fixing

Preferred input pattern:

```text
执行 S0，对整个 OMyPaper vault 做 bootstrap 与一致性检查，补齐缺失的基础管理文件、项目索引、项目树和 session 目录，并输出保守报告。
```

## 5. Must Read

S0 must read these areas when they exist:

- repository root directories
- `LiteratureNotes/`
- `Notes/`
- `projects/`
- `Management/`
- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`
- `Management/CurrentQuestions.md`
- `Management/FrontmatterSpec.md`

It should also inspect file stems and metadata where necessary to detect duplicate citekeys, duplicate paper pages, invalid project manifests, and broken project hierarchy.

## 6. Allowed Write Scope

S0 may write only to:

- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`
- `Management/CurrentQuestions.md`
- `Management/BootstrapReports/*.md`
- missing baseline directories under `Management/`, `Notes/`, and `projects/`

S0 must **not** modify:

- any file under `LiteratureNotes/`
- substantive paper content under `Notes/`
- project scope or paper membership inside existing `project.yml`

## 7. Standard Execution Flow

1. Confirm that top-level directories exist:
   - `Attachments/`
   - `LiteratureNotes/`
   - `Notes/`
   - `projects/`
   - `Management/`
2. Confirm that the core management files exist. If one is missing, initialize it conservatively from the current vault standard.
3. Confirm that the expected support directories exist:
   - `Management/Templates/`
   - `Management/Skills/`
   - `Management/SessionNotes/`
   - `Management/SessionNotes/_unmatched/`
   - `Management/ProjectSessionNotes/`
   - `Management/ProjectSessionNotes/_hybrid/`
   - `Management/ProjectSessionNotes/_unmatched/`
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
6. Scan `projects/`:
   - confirm each project directory has `project.yml`
   - validate required project fields
   - confirm `parent_project` points to a real project
   - detect cycles in parent-child references
   - detect orphan child projects
   - confirm every citekey listed in project manifests resolves in the global paper layer
   - confirm `文献地图.md` and `论文大纲.md` are present when expected
7. Check for policy violations:
   - project-membership fields incorrectly written into `LiteratureNotes/`
   - project-membership fields incorrectly written into `Notes/Papers/`
8. Cross-check management indices:
   - `PaperRegistry.md` against `LiteratureNotes/` and `Notes/Papers/`
   - `SessionIndex.md` against `Management/SessionNotes/` and `Management/ProjectSessionNotes/`
   - `ProjectIndex.md` and `ProjectTree.md` against `projects/`
9. Apply only low-risk fixes:
   - create missing control files
   - create missing support directories
   - initialize confidently matched `raw-only` rows in `Management/PaperRegistry.md` when raw notes are present and no registry row exists yet
   - refresh high-level status lines in `Management/index.md`
   - initialize empty `ProjectIndex.md`, `ProjectTree.md`, and `ProjectAssignmentQueue.md`
10. Write a bootstrap report to `Management/BootstrapReports/YYYY-MM-DD-HHmm.md`.
11. Append an entry to `Management/log.md`.
12. Return a user-facing summary with created paths, updated paths, and unresolved conflicts.

## 8. Output Requirements

S0 should produce:

- one bootstrap report file
- a concise summary of environment and project-tree health
- a list of created files
- a list of updated files
- a list of conflicts that require manual review

The report should include:

- directory presence
- missing-file initialization
- duplicate citekey findings
- duplicate paper-page findings
- raw-note-to-registry initialization results
- invalid project manifest findings
- broken parent-child findings
- project manifest citekeys that do not resolve
- raw/wiki policy violations
- registry mismatches
- manual follow-up items

## 9. Hard Constraints / Prohibitions

- Do not modify any raw note under `LiteratureNotes/`.
- Do not delete paper pages or wiki pages.
- Do not auto-merge duplicate papers.
- Do not auto-rewrite `project.yml` scope, objective, or paper membership.
- Do not "repair" ambiguous conflicts by guessing.
- Do not rewrite substantive content in `Notes/` or `projects/`.

## 10. Matching and Exception Rules

- If a literature note filename and frontmatter citekey disagree, report it as a conflict.
- If two files share a title but not a citekey, mark as `possible duplicate`, not `confirmed duplicate`.
- If a registry row points to a missing file, mark it as `broken reference`.
- If a `parent_project` target is missing, mark the project as `broken hierarchy`.
- If the project tree contains a cycle, do not attempt automatic repair.
- If a citekey in `project.yml` cannot be resolved confidently, report it and keep the manifest unchanged.
- If a conflict cannot be resolved from local evidence, stop at reporting.

## 11. Relationship With Other Skills

- S0 is the entry-point sanity check before the other skills.
- S0 does not replace S1 or S2.
- S1 relies on S0's baseline structure.
- S2 may use S0's report as structural context, but S2 operates at the wiki and project-structure layer.

## 12. Call Examples

### Example 1

```text
执行 S0，对当前 vault 做首次 bootstrap，补齐缺失的基础管理文件、项目索引和项目树文件，并输出环境报告。
```

### Example 2

```text
执行 S0，只做一致性检查：重点看 duplicate citekey、重复论文页、project.yml 合法性、父子项目是否成环，以及项目 citekey 是否都能在主文献库中解析；高风险项只报告，不自动修复。
```

## 13. Success Criteria

S0 is complete only if all of the following are true:

- the baseline directory structure exists
- the required management and project-tree files exist
- a bootstrap report has been written
- `Management/log.md` records the run
- unresolved conflicts are explicitly listed instead of silently ignored
