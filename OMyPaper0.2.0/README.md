# OMyPaper README

This README is bilingual.

- English version comes first.
- 中文版本在后面。

---

# English Guide

## What OMyPaper Is

`OMyPaper` is a local paper-reading and knowledge-building vault built around:

- `Zotero` for reading and annotation
- `ZotLit` for exporting reading traces into Obsidian
- `Obsidian` for the local knowledge base
- `Agent / Skills` for maintaining durable wiki pages, project structure, reports, reviews, and retrieval paths

This system is no longer only about single-paper notes. It now supports:

- a unique global paper wiki
- a hierarchical project tree
- paper sessions, project sessions, and hybrid sessions

The core idea is:

- `LiteratureNotes/` keeps the raw reading trace
- `Notes/` keeps durable global paper knowledge
- `projects/` keeps project-level grouping and synthesis
- `Management/` keeps rules, templates, logs, reports, and routing files

## Repository Structure

| Path | Meaning | Usually Maintained By |
| --- | --- | --- |
| `Attachments/` | PDFs, figures, supplementary files, and other resources | Zotero / user |
| `LiteratureNotes/` | raw reading traces exported by ZotLit | Zotero / ZotLit, usually not edited directly |
| `Notes/` | global wiki layer for papers, concepts, methods, comparisons, and topics | Agent / Skills |
| `projects/` | project tree layer with `project.yml`, `文献地图.md`, and `论文大纲.md` | Agent / user after confirmation |
| `Management/` | rules, templates, indices, logs, reports, session notes, and skill specs | Agent / Skills |

## Project Tree Notes

Projects currently use a logical tree and a physically flat directory layout:

- each project lives under `projects/{project_id}/`
- parent-child relations are stored in `project.yml` through `parent_project`
- one project can have at most one `parent_project`
- paper membership is finally written into `project.yml` under `papers.core_refs` or `papers.supporting_refs`
- `S1` only creates suggestions in `Management/ProjectAssignmentQueue.md` by default
- `S1` does not directly edit `project.yml` unless you explicitly ask for that follow-up action

## Skills Overview

| Skill | Name | What It Does | Best Time To Use It |
| --- | --- | --- | --- |
| `S0` | Bootstrap / Consistency Check | checks whether the global vault, project tree, and session layers are complete and consistent | first setup, after large imports, after structural changes |
| `S1` | Paper Ingest | compiles one paper into the global wiki and produces project-placement suggestions | after finishing a paper and deciding it is worth keeping |
| `S2` | Whole-Vault Lint | maintains the structural health of `Notes/` and `projects/` | weekly maintenance, after batches of updates |
| `S3` | Weekly Report | generates a weekly report across papers and projects | end of week, before meetings |
| `S4` | Review / Recall | helps you recall one paper, one child project, or one parent project | before meetings, interviews, reviews, or revision |
| `S5` | Local Retrieval / Navigation | answers with local evidence and gives re-read paths | when asking about concepts, methods, phenomena, or project structure |
| `S6` | Session Capture / Matching | turns valuable discussion into paper sessions, project sessions, or hybrid sessions | during reading, when a discussion becomes worth preserving |

## Recommended Workflows

### 1. Daily Paper Reading

1. Read and annotate in Zotero.
2. Export the raw note into `LiteratureNotes/` through ZotLit.
3. If a valuable discussion happens during reading, run `S6` first.
4. When the paper is mature enough, run `S1`.
5. `S1` must go through three phases:
   - Phase 1: ingest into global `Notes/`
   - Phase 2: scan the project tree and generate placement suggestions
   - Phase 3: write suggestions into `Management/ProjectAssignmentQueue.md`
6. After your confirmation, write the final project placement into `project.yml`.
7. Use `S4` later for recall.
8. Use `S5` when you want an answer plus a re-read path.

### 2. Project Work

1. Keep each project under `projects/{project_id}/`.
2. Use `project.yml` to track `project_type`, `parent_project`, `scope`, `objective`, and paper membership.
3. Use `文献地图.md` to track project literature organization.
4. Use `论文大纲.md` to track project narrative or writing structure.
5. Let `S1` create project suggestions first.
6. Confirm manually before changing `project.yml`.
7. If the discussion is about a project rather than a single paper, use `S6`.

### 3. Weekly Maintenance

1. Run `S2` first.
2. `S2` checks:
   - orphan pages, duplicates, broken links, inconsistent naming
   - whether `project.yml` files are valid
   - whether parent-child relations are clear
   - whether sibling projects overlap too much
   - whether `文献地图.md` and `论文大纲.md` drift apart
3. Then run `S3` to generate a weekly report.

### 4. Initial Setup or Large Reorganization

1. Run `S0` first.
2. `S0` now checks:
   - whether `projects/` exists
   - whether each project has a valid `project.yml`
   - whether the project tree has broken parents or cycles
   - whether citekeys inside project manifests resolve in the global paper layer
   - whether anyone incorrectly wrote project fields back into raw notes or paper wiki files
3. Only continue after the structure is healthy enough.

## How To Use Each Skill

### S0: Bootstrap / Consistency Check

Use S0 when you want the vault checked, initialized, or sanity-checked.

Typical use cases:

- first setup
- after large ZotLit imports
- after adding or changing projects
- when you want to make sure the vault is still safe to use

Examples:

```text
Run S0 on the whole OMyPaper vault. Fill in missing management files, project-tree files, and session directories, then give me a conservative report.
```

```text
Run S0 in report-only mode. Focus on projects/, project.yml validity, parent-child relations, duplicate citekeys, and index mismatches.
```

### S1: Paper Ingest

S1 compiles one paper into the global wiki. It now has three mandatory phases:

1. ingest into global `Notes/`
2. generate project suggestions from the saved global result
3. write the suggestions into `Management/ProjectAssignmentQueue.md`

Good times to use S1:

- after finishing a paper
- when a paper already has a raw note and one or more useful paper sessions
- when you want both a durable paper wiki and a conservative project suggestion

Avoid S1 when:

- there is no stable raw note yet
- the paper identity is still unstable
- you only want a temporary answer and not durable writing

Examples:

```text
Run S1 for citekey=vaswani2017attention. Read pending paper sessions first, ingest it into the global wiki, and then generate project-placement suggestions without editing any project.yml.
```

```text
Run S1 for the paper titled "Attention Is All You Need". Match conservatively. If the match is stable, complete the global ingest and write the suggestion into ProjectAssignmentQueue. If not, only report the ambiguity.
```

### S2: Whole-Vault Lint

S2 is the structural maintainer for the whole vault. It covers both `Notes/` and `projects/`.

It checks:

- orphan pages
- duplicate pages
- broken links
- naming inconsistencies
- project-tree health
- `project.yml` consistency
- mismatch between `文献地图.md` and `论文大纲.md`

Examples:

```text
Run S2 on the whole Notes/ and projects/ tree. Apply only low-risk fixes and generate a lint report.
```

```text
Run S2 on one parent project and its children. Focus on project.yml, literature maps, outlines, and sibling overlap. Report high-risk issues only.
```

### S3: Weekly Report

S3 creates a weekly report across:

- papers read this week
- wiki updates this week
- project-tree progress this week
- stable conclusions
- unresolved uncertainty
- next priorities

Examples:

```text
Run S3 and generate this week's report. Summarize both global paper progress and project-tree progress. Do not write a raw activity log.
```

```text
Run S3 for 2026-W17 and tell me which parent projects, child projects, and papers I should prioritize next week.
```

### S4: Review / Recall

S4 supports three review targets:

- one paper
- one child project
- one parent project

Common modes:

- `one-minute`
- `five-minute`
- `oral-explanation`
- `self-test`
- `quick-familiarization`

Use it when you want to recover your own understanding, not when you want a fresh abstract rewrite.

Examples:

```text
Run S4 for citekey=he2016resnet in one-minute mode. Help me recover how I understood this paper.
```

```text
Run S4 for the child project uncertainty_llpr in quick-familiarization mode. Tell me where it sits under its parent, what its core papers are, how it differs from sibling projects, and what the fastest familiarization path is.
```

### S5: Local Retrieval / Navigation

S5 is for "answer + paths + re-read order".

You can ask it about:

- concepts
- methods
- experimental phenomena
- parent projects
- child projects
- sibling-project differences

Examples:

```text
Run S5 for the question: which papers in my local vault discuss exposure bias? Give me the answer, exact paths, and a suggested re-read order.
```

```text
Run S5 for the question: guide me through the project uncertainty. Tell me where it sits in the project tree, what child projects exist, what the core problem is, and which papers I should read first.
```

### S6: Session Capture / Matching

S6 stores valuable discussion as structured intermediate notes.

It now supports four session types:

- `paper`
- `child-project`
- `parent-project`
- `hybrid`

It feeds later work in `S1`, `S3`, `S4`, and `S5`.

Default destinations:

- paper session -> `Management/SessionNotes/{citekey}/`
- project session -> `Management/ProjectSessionNotes/{project_id}/`
- hybrid session -> `Management/ProjectSessionNotes/_hybrid/`
- unstable match -> the corresponding `_unmatched/`

Examples:

```text
Run S6 and turn our discussion about citekey=vaswani2017attention into a standardized paper session note with merge_status: pending.
```

```text
Run S6 and turn our discussion about the parent project uncertainty and the child project uncertainty_llpr into a project session. If the project match is unstable, store it conservatively under ProjectSessionNotes/_unmatched/.
```

## Practical Reminders

- `LiteratureNotes/` is the raw layer. Do not edit it directly unless you intentionally introduce a separate raw-note repair workflow.
- When working on papers, provide `citekey` whenever possible.
- When working on projects, provide `project_id` whenever possible.
- New papers usually follow `S6 -> S1`.
- `S1` creates project suggestions, but does not directly modify `project.yml` by default.
- Project placement should be written into `projects/{project_id}/project.yml` only after confirmation.
- Weekly maintenance usually follows `S2 -> S3`.
- Use `S5` for local knowledge retrieval instead of jumping directly to generic Q&A.
- Use `S4` when you want to recover your own understanding instead of asking the agent to rewrite an abstract.

---

# 中文指南

## OMyPaper 是什么

`OMyPaper` 是一个面向论文阅读、知识沉淀与项目组织的本地知识库仓库，围绕下面这套组合来工作：

- `Zotero` 负责阅读和批注
- `ZotLit` 负责把阅读痕迹导入 Obsidian
- `Obsidian` 负责承载本地知识库
- `Agent / Skills` 负责维护 wiki 页面、项目树、周报、复盘和检索导航

这套系统现在不只管理“单篇论文”，还支持：

- 唯一的全局论文 wiki
- 层级项目树
- paper session、project session 和 hybrid session

核心分层是：

- `LiteratureNotes/` 保存原始阅读痕迹
- `Notes/` 保存全局论文知识
- `projects/` 保存项目组织与项目级沉淀
- `Management/` 保存规则、模板、索引、日志、报告和路由文件

## 仓库结构

| 路径 | 含义 | 一般由谁维护 |
| --- | --- | --- |
| `Attachments/` | PDF、图片、补充材料等附件层 | Zotero / 用户 |
| `LiteratureNotes/` | ZotLit 导出的原始阅读痕迹层 | Zotero / ZotLit，通常不直接改 |
| `Notes/` | 全局 wiki 层，放论文页、概念页、方法页、对比页、主题页 | Agent / Skills |
| `projects/` | 项目树层，放 `project.yml`、`文献地图.md`、`论文大纲.md` | Agent / 用户确认后维护 |
| `Management/` | 规则、模板、索引、日志、周报、复盘、SessionNotes、Skill 规范 | Agent / Skills |

## 项目树补充说明

当前项目层采用“逻辑树、物理平铺”的组织方式：

- 每个项目都放在 `projects/{project_id}/`
- 父子关系通过 `project.yml` 中的 `parent_project` 表达
- 一个项目最多只有一个 `parent_project`
- 论文归属最终写到 `project.yml` 的 `papers.core_refs` 或 `papers.supporting_refs`
- `S1` 默认只会把建议写入 `Management/ProjectAssignmentQueue.md`
- `S1` 不会默认直接修改 `project.yml`

## 七个 Skills 总览

| Skill | 名称 | 它做什么 | 最适合什么时候用 |
| --- | --- | --- | --- |
| `S0` | 规范检查 / 初始化 | 检查全局库、项目树和会话层是否齐全且一致 | 初次搭建、大批量导入后、结构调整后 |
| `S1` | 单篇论文整理 | 把一篇论文正式编译进全局 wiki，并生成项目归属建议 | 一篇论文读完且值得沉淀时 |
| `S2` | 全库整理 | 维护 `Notes/` 和 `projects/` 的结构健康 | 每周维护、批量整理后 |
| `S3` | 周报生成 | 生成覆盖全局论文库和项目推进的周报 | 每周末、组会前 |
| `S4` | 复盘 | 复盘单篇论文、子项目或父项目 | 面试、组会、答辩、复习前 |
| `S5` | 本地检索 / 导航 | 基于本地知识库回答问题并给出重读路径 | 查概念、方法、现象或项目脉络时 |
| `S6` | 会话沉淀 / 匹配 | 把高价值讨论沉淀成结构化 session | 阅读过程中讨论出关键内容时 |

## 推荐工作流

### 1. 日常读论文

1. 在 Zotero 里阅读和批注。
2. 用 ZotLit 把原始笔记导入 `LiteratureNotes/`。
3. 如果阅读过程中和 agent 讨论出了高价值内容，先执行 `S6`。
4. 一篇论文读到足够成熟时，执行 `S1`。
5. `S1` 必须走三个阶段：
   - Phase 1：整理进全局 `Notes/`
   - Phase 2：扫描项目树并生成项目归属建议
   - Phase 3：把建议写入 `Management/ProjectAssignmentQueue.md`
6. 你确认之后，再把最终项目归属写入 `project.yml`。
7. 之后需要快速回顾时，用 `S4`。
8. 需要“答案 + 路径 + 重读顺序”时，用 `S5`。

### 2. 项目推进

1. 每个项目都放在 `projects/{project_id}/` 下。
2. 用 `project.yml` 记录 `project_type`、`parent_project`、`scope`、`objective` 和论文归属。
3. 用 `文献地图.md` 维护项目内的文献组织。
4. 用 `论文大纲.md` 维护项目叙事或写作结构。
5. 先让 `S1` 生成项目建议。
6. 确认之后再修改 `project.yml`。
7. 如果讨论对象是项目而不是单篇论文，就优先用 `S6`。

### 3. 每周维护

1. 先执行 `S2`。
2. `S2` 会检查：
   - 孤儿页、重复页、断链、命名不一致
   - `project.yml` 是否有效
   - 父项目和子项目关系是否清楚
   - 兄弟项目是否重叠过多
   - `文献地图.md` 和 `论文大纲.md` 是否脱节
3. 然后执行 `S3` 生成周报。

### 4. 初始搭建或大改结构

1. 先执行 `S0`。
2. `S0` 现在会检查：
   - `projects/` 是否存在
   - 每个项目是否有合法的 `project.yml`
   - 父子关系是否失效或成环
   - 项目 manifest 中的 citekey 能否在全局主库解析
   - 是否有人把项目字段错误写回原始层或论文 wiki
3. 结构确认没问题后，再继续其他 Skill。

## 各 Skill 怎么用

### S0：规范检查 / 初始化

S0 适合在你想检查仓库是否能稳定运行时使用。

典型场景：

- 第一次搭建
- 大量导入 ZotLit 笔记之后
- 新增或调整项目之后
- 想确认仓库状态是否健康时

示例：

```text
执行 S0，对整个 OMyPaper 做一次 bootstrap 和一致性检查，补齐缺失的管理文件、项目树文件和 session 目录，并输出保守报告。
```

```text
执行 S0，只检查不激进修复；重点看 projects/、project.yml、父子关系、重复 citekey 和索引失配。
```

### S1：单篇论文整理

S1 负责把一篇论文正式编译进全局主知识库。现在它有三个**强制阶段**：

1. 先整理进全局 `Notes/`
2. 再基于全局结果扫描项目树并生成归属建议
3. 把建议写入 `Management/ProjectAssignmentQueue.md`

适合在这些时候用：

- 一篇论文读完后想正式沉淀
- 已经有原始文献笔记和若干 paper session
- 想同时得到保守的项目归属建议

不适合在这些时候用：

- 还没有稳定的 raw note
- 论文身份还没匹配稳
- 你只想做临时问答，不想正式写入

示例：

```text
执行 S1，把 citekey=vaswani2017attention 正式整理进全局 wiki；先吸收 pending paper sessions，再生成项目归属建议，但不要直接改任何 project.yml。
```

```text
执行 S1，目标论文标题是 “Attention Is All You Need”。请先保守匹配；如果匹配稳定，就完成全局整理并把项目建议写入 ProjectAssignmentQueue；如果不稳定，只报告问题。
```

### S2：全库整理

S2 是整个知识库的结构维护器，既整理 `Notes/`，也整理 `projects/`。

它会检查：

- 孤儿页
- 重复页
- 断链
- 命名不一致
- 项目树健康度
- `project.yml` 一致性
- `文献地图.md` 和 `论文大纲.md` 是否脱节

示例：

```text
执行 S2，对整个 Notes/ 和 projects/ 做结构健康检查，只修复低风险问题，并生成 lint 报告。
```

```text
执行 S2，重点检查某个父项目及其子项目的 project.yml、文献地图和论文大纲；高风险问题只报告。
```

### S3：周报生成

S3 会生成覆盖这些内容的周报：

- 本周读了哪些论文
- 本周更新了哪些 wiki
- 项目树推进到了哪里
- 哪些结论比较稳定
- 哪些问题还不确定
- 下周优先级是什么

示例：

```text
执行 S3，生成本周周报；请同时总结全局论文库更新和项目树推进，不要写成流水账。
```

```text
执行 S3，为 2026-W17 生成周报，并告诉我下周最该推进的父项目、子项目和论文。
```

### S4：复盘

S4 支持三类复盘对象：

- 单篇论文
- 子项目
- 父项目

常用模式包括：

- `one-minute`
- `five-minute`
- `oral-explanation`
- `self-test`
- `quick-familiarization`

它适合“恢复你自己的理解”，而不是重新写一份 abstract。

示例：

```text
执行 S4，目标是 citekey=he2016resnet，模式用 one-minute，帮我快速恢复我当时是怎么理解这篇论文的。
```

```text
执行 S4，目标是子项目 uncertainty_llpr，模式用 quick-familiarization，告诉我它在父项目中的位置、核心论文、与兄弟项目的区别，以及最快熟悉路径。
```

### S5：本地知识库检索 / 导航

S5 是“答案 + 路径 + 重读顺序”这个类型的 Skill。

你可以问它：

- 某个概念
- 某种方法
- 某个实验现象
- 某个父项目
- 某个子项目
- 兄弟项目之间的差异

示例：

```text
执行 S5，问题是：在我的本地知识库里，哪些论文讨论过 exposure bias？请给我答案、精确路径和建议重看顺序。
```

```text
执行 S5，问题是：请导览项目 uncertainty，告诉我它在项目树中的位置、有哪些子项目、核心问题是什么、最先该读哪些论文。
```

### S6：阅读会话沉淀与匹配

S6 负责把高价值讨论保存成结构化中间文件。

它现在支持四种 session 类型：

- `paper`
- `child-project`
- `parent-project`
- `hybrid`

它会为后续的 `S1`、`S3`、`S4`、`S5` 提供输入。

默认落点大致是：

- paper session -> `Management/SessionNotes/{citekey}/`
- project session -> `Management/ProjectSessionNotes/{project_id}/`
- hybrid session -> `Management/ProjectSessionNotes/_hybrid/`
- 匹配不稳 -> 对应的 `_unmatched/`

示例：

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论沉淀成标准化 paper session note，并设为 merge_status: pending。
```

```text
执行 S6，把我们刚才关于父项目 uncertainty 和子项目 uncertainty_llpr 的讨论沉淀成 project session；如果无法稳定匹配项目，就保守放到 ProjectSessionNotes/_unmatched/。
```

## 常用提醒

- `LiteratureNotes/` 是原始层，不要直接改。
- 处理论文时尽量总是给出 `citekey`。
- 处理项目时尽量总是给出 `project_id`。
- 新论文通常先走 `S6 -> S1`。
- `S1` 会生成项目建议，但默认不会直接改 `project.yml`。
- 项目归属确认之后，才写入 `projects/{project_id}/project.yml`。
- 每周维护通常先走 `S2 -> S3`。
- 想查本地知识时优先用 `S5`，不要直接跳到泛化问答。
- 想恢复自己的理解时优先用 `S4`，不要直接让 agent 重写 abstract。
