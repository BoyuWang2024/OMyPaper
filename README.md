# OMyPaper Skills

This README is bilingual.

- English version comes first.
- 中文版本在后面。

---

# English Guide

## What OMyPaper Is

**OMyPaper Skills** is a local literature-management and knowledge-base workflow for researchers who read papers in **Zotero**, sync literature notes into **Obsidian** through **ZotLit**, and then use an **LLM Agent** to continuously organize, refine, retrieve, review, summarize, and project-manage the accumulated knowledge.

Instead of treating an LLM as a temporary question-answering tool that re-reads raw documents from scratch every time, this project treats the Agent as a **persistent local knowledge-base maintainer**.

The core workflow is:

- **Zotero** for reading, annotating, and reference management
- **ZotLit** for exporting literature notes into Obsidian
- **Obsidian** for the local markdown-based knowledge container
- **Agent / Skills** for turning raw notes into structured wiki pages, project structures, session traces, reviews, reports, and retrieval paths

This repository is **Agent-agnostic**. Any agent that can read and write local files can adapt these Skills.

---

## Compatibility

This Skills set is developed and organized around the following environment:

- **Zotero**: `7.0.32`
- **Obsidian**: `1.6.7`
- **ZotLit plugin**: used as the bridge between Zotero and Obsidian

> These versions are the recommended baseline for this repository.  
> Other versions may also work, but the current structure and workflow are designed and tested around the versions listed above.

---

## Required Obsidian Setup

To use this Skills system correctly:

1. Copy the folder **`OMyPaper`** into your Obsidian vault.
2. Install and enable the **ZotLit** plugin in Obsidian.
3. In ZotLit settings, set:

```text
Default location for new literature notes = OMyPaper/LiteratureNotes
```

This setting matters because the Skills assume that all raw literature notes imported from Zotero will be stored under:

```text
OMyPaper/LiteratureNotes
```

If this path is changed, the Agent-side workflows, note matching logic, and knowledge-base maintenance routines may fail or require manual modification.

For ZotLit installation and configuration details, please refer to the official documentation:

- [ZotLit Official Documentation (Chinese)](https://zotlit.aidenlx.top/zh-CN)

---

## Design Philosophy

OMyPaper follows a layered design:

- **Raw layer**: `LiteratureNotes/` stores raw reading traces imported from Zotero via ZotLit.
- **Global knowledge layer**: `Notes/` stores durable paper, concept, method, comparison, and topic pages.
- **Project layer**: `projects/` organizes literature into project-specific structures.
- **Control layer**: `Management/` stores rules, templates, indices, logs, reports, queues, and Skill specifications.

In other words:

- the raw literature note is the **source layer**
- the Agent-maintained wiki is the **knowledge layer**
- the project tree is the **research-organization layer**

The long-term goal is to turn scattered reading traces into a continuously evolving, locally stored, and cross-linked **personal paper wiki + project workspace**.

---

## Version Evolution

## v0.1.0

`0.1.0` established the first working version of the OMyPaper Skills system.

### Main characteristics of 0.1.0

- paper-centric local knowledge-base workflow
- `LiteratureNotes/` as the raw note layer
- `Notes/` as the global wiki layer
- `Management/` as the control and reporting layer
- core Skills focused on:
  - structure check
  - single-paper ingestion
  - whole-wiki cleanup
  - weekly report generation
  - paper recall
  - local knowledge retrieval

### What 0.1.0 already solved

- turning raw Zotero reading notes into structured wiki pages
- maintaining a local paper knowledge base
- organizing concepts, methods, datasets, and comparisons across papers
- generating weekly reading summaries
- recalling a paper efficiently
- answering topic-based questions from the local knowledge base first

### Limitation of 0.1.0

`0.1.0` was still primarily **paper-centric**.
It did not yet fully support:

- project-aware workflows
- project-level review and navigation
- project-level session capture
- hierarchical parent/child project structures
- conservative paper-to-project assignment queues

---

## v0.2.0

`0.2.0` upgrades OMyPaper from a paper-centric wiki workflow into a **paper + hierarchical project tree** workflow.

### Newly added content in 0.2.0

- `projects/` layer for project organization
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`
- `Management/ProjectSessionNotes/`
- project-aware templates such as:
  - `ProjectManifest.template.yml`
  - `ProjectLiteratureMap.template.md`
  - `ProjectOutline.template.md`
  - `ProjectReview.template.md`
  - `ProjectSessionNote.template.md`
  - `ProjectTree.template.md`
  - `ProjectIndex.template.md`
  - `ProjectAssignmentQueue.template.md`

### Newly implemented capabilities in 0.2.0

#### 1. Hierarchical project tree

`0.2.0` introduces a project tree with:

- parent projects
- child projects
- standalone projects

Project hierarchy is now represented through project metadata such as `project_type` and `parent_project`.

#### 2. Project-aware S0–S6

All seven Skills were upgraded from paper-only behavior into **paper + project-aware behavior**.

#### 3. S1 becomes a mandatory three-phase workflow

Skill 1 is no longer just a single-paper summarizer. It now follows three mandatory phases:

1. ingest into the global wiki
2. generate project-placement suggestions from the saved global result
3. write suggestions into `Management/ProjectAssignmentQueue.md` and wait for user confirmation

This means paper placement into projects is now:

- conservative
- reviewable
- user-confirmed

#### 4. S2 now lints both `Notes/` and `projects/`

Whole-vault maintenance now includes:

- project-tree health
- project manifest consistency
- parent-child coherence
- sibling overlap checks
- drift between `文献地图.md` and `论文大纲.md`

#### 5. S3 now reports paper progress and project progress together

Weekly reports are no longer only paper logs.
They now summarize:

- weekly paper reading progress
- global wiki progress
- parent-project progress
- child-project progress
- next project priorities

#### 6. S4 now supports project review

Review is no longer limited to one paper.
`0.2.0` adds support for:

- paper review
- child-project review
- parent-project review
- quick familiarization paths for projects

#### 7. S5 now supports project navigation

Local retrieval now supports:

- paper retrieval
- project-tree listing
- parent/child project explanation
- sibling-project comparison
- fast familiarization routes for one project

#### 8. S6 now supports paper sessions, project sessions, and hybrid sessions

High-value discussions can now be captured as:

- paper session
- child-project session
- parent-project session
- hybrid session

These sessions can later support S1, S3, S4, and S5.

---

## v0.1.0 vs v0.2.0 at a Glance

| Dimension                | v0.1.0                             | v0.2.0                                     |
| ------------------------ | ---------------------------------- | ------------------------------------------ |
| Main paradigm            | paper-centric                      | paper + hierarchical project tree          |
| Global paper wiki        | yes                                | yes                                        |
| Project organization     | very limited / not fully supported | fully introduced as a first-class layer    |
| Parent-child projects    | no                                 | yes                                        |
| Project assignment queue | no                                 | yes                                        |
| Project session capture  | no                                 | yes                                        |
| S1 flow                  | ingest one paper                   | ingest → project suggestion → confirmation |
| S2 scope                 | `Notes/` mainly                    | `Notes/` + `projects/`                     |
| S3 scope                 | weekly paper summary               | weekly paper + project summary             |
| S4 targets               | one paper                          | paper / child project / parent project     |
| S5 retrieval             | paper/topic retrieval              | paper + project-tree navigation            |
| S6 capture               | paper-centric                      | paper / project / hybrid                   |
| Templates                | paper/wiki oriented                | paper + project + queue + tree templates   |

---

## Repository Structure (v0.2.0)

A recommended folder layout is now:

```text
OMyPaper/
├─ LiteratureNotes/                  # Raw notes imported from Zotero via ZotLit
├─ Notes/
│  ├─ Papers/                        # Per-paper wiki pages
│  ├─ Concepts/                      # Concept pages
│  ├─ Methods/                       # Method pages
│  ├─ Datasets/                      # Dataset pages
│  ├─ Comparisons/                   # Cross-paper comparison pages
│  ├─ Topics/                        # Thematic pages
│  └─ Syntheses/                     # Higher-level synthesis pages
├─ Management/
│  ├─ AGENTS.md                      # Agent instructions / schema
│  ├─ CurrentQuestions.md            # Current research questions
│  ├─ index.md                       # Global wiki index
│  ├─ log.md                         # Append-only operation log
│  ├─ PaperRegistry.md               # Paper mapping registry
│  ├─ SessionIndex.md                # Session index
│  ├─ ProjectIndex.md                # Project summary index
│  ├─ ProjectTree.md                 # Parent-child project tree index
│  ├─ ProjectAssignmentQueue.md      # Pending project-placement suggestions
│  ├─ WeeklyReports/                 # Weekly reports
│  ├─ ReviewNotes/                   # Review / recall notes
│  ├─ LintReports/                   # Vault health reports
│  ├─ SessionNotes/                  # Paper sessions
│  ├─ ProjectSessionNotes/           # Project sessions / hybrid sessions
│  ├─ Templates/                     # Shared templates
│  └─ Skills/                        # Skill specifications
├─ projects/                         # Hierarchical project layer
└─ Attachments/                      # Optional local attachments
```

---

## Skills Overview (v0.2.0)

| Skill | Name                          | What It Does                                                 | Key Upgrade in 0.2.0                                         |
| ----- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `S0`  | Bootstrap / Consistency Check | checks whether the global vault, project tree, and session layers are complete and consistent | now checks `projects/`, tree cycles, manifest validity, and project-session readiness |
| `S1`  | Paper Ingest                  | compiles one paper into the global wiki and generates project-placement suggestions | now has mandatory three phases and writes suggestions into `ProjectAssignmentQueue.md` |
| `S2`  | Whole-Vault Lint              | maintains the structural health of `Notes/` and `projects/`  | now checks parent-child project coherence, sibling overlap, and map/outline drift |
| `S3`  | Weekly Report                 | generates a weekly report across papers and projects         | now includes parent-project and child-project progress       |
| `S4`  | Review / Recall               | helps you recall one paper, one child project, or one parent project | now supports project-level review and quick familiarization paths |
| `S5`  | Local Retrieval / Navigation  | answers with local evidence and gives re-read paths          | now supports project-tree listing, project explanation, and sibling comparison |
| `S6`  | Session Capture / Matching    | turns valuable discussion into paper sessions, project sessions, or hybrid sessions | now supports project-aware and hybrid session capture        |

---

## Recommended Usage Pattern (v0.2.0)

### Daily paper reading

1. Read and annotate papers in Zotero.
2. Sync/import literature notes into Obsidian via ZotLit.
3. If a discussion becomes worth preserving, run **S6** first.
4. Run **S1** to compile the paper into the global wiki.
5. Review the placement suggestions written into `Management/ProjectAssignmentQueue.md`.
6. After confirmation, write the final project placement into `project.yml`.
7. Use **S4** for recall.
8. Use **S5** for answer + path + re-read order.

### Weekly maintenance

1. Run **S2** for whole-vault lint.
2. Review the lint report.
3. Run **S3** to generate the weekly report.

### Project work

1. Keep paper knowledge unique in `Notes/`.
2. Keep project grouping and hierarchy inside `projects/`.
3. Use `project.yml` as the only project-membership interface.
4. Use **S4** and **S5** to revisit or navigate one project quickly.

---

## Scope

OMyPaper is designed for:

- academic paper reading
- literature management
- research note organization
- local personal knowledge-base building
- concept/method comparison across papers
- project-oriented research organization
- long-term research memory support

It is especially suitable for users who prefer:

- local-first workflows
- markdown-based knowledge systems
- Obsidian-based note taking
- reproducible and inspectable file structures
- Agent-assisted but user-controlled knowledge management

---

## Notes

- This repository is **Agent-agnostic**.
- The system assumes a **local file-based workflow**.
- `LiteratureNotes/` should still be treated as the raw source layer.
- Agent-maintained content should mainly live in `Notes/`, `projects/`, and `Management/`.
- If you customize the folder structure, you should also update the corresponding Skill prompts and Agent instructions.

---

# 中文指南

## OMyPaper 是什么

**OMyPaper Skills** 是一套面向本地论文知识库与项目组织工作流的 Skills 设计方案，适用于这样的阅读流程：你在 **Zotero** 中阅读和批注文献，通过 **ZotLit** 将文献笔记导入 **Obsidian**，再由 **LLM Agent** 持续完成笔记整理、知识沉淀、结构维护、主题检索、论文复盘、项目导航与周报生成。

它的核心思想不是把 LLM 当作一个“临时读 PDF、临时回答问题”的工具，而是把它当作一个**持续维护本地知识库与项目结构的助手**。

核心链路是：

- **Zotero** 负责阅读、批注与参考文献管理
- **ZotLit** 负责将阅读痕迹导入 Obsidian
- **Obsidian** 负责承载本地 Markdown 知识库
- **Agent / Skills** 负责将原始笔记转化为结构化 wiki、项目结构、session 沉淀、复盘、周报与导航路径

本仓库中的 Skills 设计为**适用于任意 Agent 工具**。只要 Agent 具备本地文件读写能力，就可以将本 Skills 体系应用到自己的论文管理流程中。

---

## 兼容环境

本 Skills 体系基于以下版本进行开发与组织：

- **Zotero**：`7.0.32`
- **Obsidian**：`1.6.7`
- **ZotLit 插件**：作为 Zotero 与 Obsidian 之间的桥接工具

> 上述版本是本仓库推荐的基准环境。  
> 其他版本未必不能使用，但当前目录结构、工作流约定与相关 Skills 均围绕这些版本进行设计。

---

## 使用前的必要配置

要正确使用本 Skills，请先完成以下设置：

1. 将仓库中的 **`OMyPaper`** 文件夹复制到你的 Obsidian 仓库中。
2. 在 Obsidian 中安装并启用 **ZotLit** 插件。
3. 在 ZotLit 设置中，将以下项设置为：

```text
Default location for new literature notes = OMyPaper/LiteratureNotes
```

这一设置非常关键，因为本 Skills 默认假设所有从 Zotero 导入的原始文献笔记都保存在：

```text
OMyPaper/LiteratureNotes
```

如果这个路径被修改，那么 Agent 在执行笔记匹配、单篇整理、知识库维护和项目导航时，可能无法正确找到原始笔记，或者需要你手动调整相应的提示词与规则。

关于 ZotLit 的安装与详细配置，请参考官方文档：

- [ZotLit 官方文档](https://zotlit.aidenlx.top/zh-CN)

---

## 设计理念

OMyPaper 采用分层设计：

- **原始层**：`LiteratureNotes/` 保存 Zotero / ZotLit 导入的原始阅读痕迹
- **全局知识层**：`Notes/` 保存稳定的论文页、概念页、方法页、比较页和主题页
- **项目层**：`projects/` 保存项目组织、项目文献地图与论文大纲
- **控制层**：`Management/` 保存规则、模板、索引、日志、报告、队列与 Skill 规范

换句话说：

- 原始文献笔记是**来源层**
- Agent 维护的 wiki 是**知识层**
- 项目树是**研究组织层**

长期目标，是把零散的阅读痕迹转化成一个不断演化的、本地存储的、可交叉链接的**个人论文 wiki + 项目工作区**。

---

## 版本演进

## v0.1.0

`0.1.0` 是 OMyPaper Skills 的第一版可用版本。

### 0.1.0 的主要特点

- 以单篇论文和全局知识库为中心
- `LiteratureNotes/` 作为原始笔记层
- `Notes/` 作为全局 wiki 层
- `Management/` 作为管理与报告层
- 核心 Skills 主要覆盖：
  - 结构检查
  - 单篇论文整理
  - 全库整理
  - 周报生成
  - 论文复盘
  - 本地检索导航

### 0.1.0 已解决的问题

- 将 Zotero 的原始阅读痕迹转为结构化 wiki 页面
- 持续维护本地论文知识库
- 组织跨论文的概念、方法、数据集与比较关系
- 生成周报
- 支持单篇论文复盘
- 优先从本地知识库中回答主题问题

### 0.1.0 的主要限制

`0.1.0` 仍然主要是**paper-centric**：

- 还没有完整的 project-aware 工作流
- 还不支持项目级复盘与导航
- 还不支持项目级 session 沉淀
- 还不支持父项目 / 子项目层级结构
- 还没有“项目归属建议 + 待确认队列”的保守机制

---

## v0.2.0

`0.2.0` 将 OMyPaper 从“论文中心的 wiki 工作流”升级为**论文 + 层级项目树**的工作流。

### 0.2.0 新加入的内容

- `projects/` 项目层
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`
- `Management/ProjectAssignmentQueue.md`
- `Management/ProjectSessionNotes/`
- 一组 project-aware 模板，例如：
  - `ProjectManifest.template.yml`
  - `ProjectLiteratureMap.template.md`
  - `ProjectOutline.template.md`
  - `ProjectReview.template.md`
  - `ProjectSessionNote.template.md`
  - `ProjectTree.template.md`
  - `ProjectIndex.template.md`
  - `ProjectAssignmentQueue.template.md`

### 0.2.0 新实现的能力

#### 1. 层级项目树

`0.2.0` 新增了项目树结构，支持：

- 父项目
- 子项目
- 独立项目

项目层级关系通过 `project_type` 与 `parent_project` 等元数据表达。

#### 2. S0–S6 全部升级为 project-aware

7 个 Skills 不再只是面向论文，而是升级为 **论文 + 项目** 双层能力。

#### 3. S1 升级为强制三阶段流程

Skill 1 不再只是“单篇论文整理器”，而变成：

1. 先整理进全局 wiki
2. 再根据全局整理结果生成项目归属建议
3. 把建议写入 `Management/ProjectAssignmentQueue.md`，等待用户确认

这样，论文进入项目的过程变成了：

- 保守的
- 可审核的
- 由用户最终确认的

#### 4. S2 同时整理 `Notes/` 和 `projects/`

全库整理现在会同时检查：

- 项目树健康度
- 项目清单一致性
- 父子项目逻辑
- 兄弟项目重叠
- `文献地图.md` 与 `论文大纲.md` 的漂移

#### 5. S3 现在会同时总结论文进展和项目进展

周报不再只是论文阅读流水，而会同时总结：

- 本周论文阅读进展
- 全局 wiki 更新
- 父项目推进
- 子项目推进
- 下周优先推进的项目

#### 6. S4 现在支持项目复盘

复盘不再只针对单篇论文，还支持：

- 单篇论文复盘
- 子项目复盘
- 父项目复盘
- 项目快速熟悉路径输出

#### 7. S5 现在支持项目树导航

本地检索现在支持：

- 论文检索
- 项目树列出
- 父项目 / 子项目说明
- 兄弟项目比较
- 项目快速熟悉路径

#### 8. S6 现在支持 paper / project / hybrid session

高价值讨论现在可以保存为：

- paper session
- child-project session
- parent-project session
- hybrid session

这些 session 后续可以被 S1、S3、S4、S5 复用。

---

## v0.1.0 与 v0.2.0 对比一览

| 维度               | v0.1.0            | v0.2.0                            |
| ------------------ | ----------------- | --------------------------------- |
| 主要范式           | paper-centric     | paper + hierarchical project tree |
| 全局论文 wiki      | 有                | 有                                |
| 项目组织层         | 很弱 / 未完整支持 | 成为正式一层                      |
| 父项目 / 子项目    | 无                | 有                                |
| 项目归属待确认队列 | 无                | 有                                |
| 项目级 session     | 无                | 有                                |
| S1 流程            | 整理一篇论文      | 整理 → 建议 → 确认                |
| S2 范围            | 主要是 `Notes/`   | `Notes/` + `projects/`            |
| S3 范围            | 论文周报          | 论文 + 项目周报                   |
| S4 复盘对象        | 单篇论文          | 论文 / 子项目 / 父项目            |
| S5 检索对象        | 论文 / 主题       | 论文 + 项目树                     |
| S6 沉淀对象        | 论文为主          | 论文 / 项目 / hybrid              |
| 模板体系           | 偏论文 / wiki     | 扩展到项目 / 队列 / 项目树        |

---

## 仓库结构（v0.2.0）

推荐目录结构现在是：

```text
OMyPaper/
├─ LiteratureNotes/                  # 由 ZotLit 从 Zotero 导入的原始文献笔记
├─ Notes/
│  ├─ Papers/                        # 单篇论文 wiki 页面
│  ├─ Concepts/                      # 概念页
│  ├─ Methods/                       # 方法页
│  ├─ Datasets/                      # 数据集页
│  ├─ Comparisons/                   # 跨论文比较页
│  ├─ Topics/                        # 主题页
│  └─ Syntheses/                     # 高层综合页
├─ Management/
│  ├─ AGENTS.md                      # Agent 说明 / schema
│  ├─ CurrentQuestions.md            # 当前重点问题
│  ├─ index.md                       # 全局索引
│  ├─ log.md                         # 追加式日志
│  ├─ PaperRegistry.md               # 论文映射登记表
│  ├─ SessionIndex.md                # Session 索引
│  ├─ ProjectIndex.md                # 项目索引
│  ├─ ProjectTree.md                 # 项目树
│  ├─ ProjectAssignmentQueue.md      # 项目归属待确认队列
│  ├─ WeeklyReports/                 # 周报
│  ├─ ReviewNotes/                   # 复盘记录
│  ├─ LintReports/                   # 整理报告
│  ├─ SessionNotes/                  # 论文级 session
│  ├─ ProjectSessionNotes/           # 项目级 / hybrid session
│  ├─ Templates/                     # 模板集合
│  └─ Skills/                        # Skills 规范
├─ projects/                         # 层级项目层
└─ Attachments/                      # 可选的本地附件目录
```

---

## Skills 总览（v0.2.0）

| Skill | 名称              | 它做什么                                            | 0.2.0 中的关键升级                                           |
| ----- | ----------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| `S0`  | 规范检查 / 初始化 | 检查全局库、项目树和会话层是否齐全且一致            | 现在会检查 `projects/`、树关系成环、manifest 合法性、project-session 就绪情况 |
| `S1`  | 单篇论文整理      | 把一篇论文编译进全局 wiki，并生成项目归属建议       | 现在有强制三阶段流程，并写入 `ProjectAssignmentQueue.md`     |
| `S2`  | 全库整理          | 维护 `Notes/` 和 `projects/` 的结构健康             | 现在检查父子项目一致性、兄弟项目重叠以及地图/大纲漂移        |
| `S3`  | 周报生成          | 生成覆盖论文与项目的周报                            | 现在同时总结父项目与子项目推进                               |
| `S4`  | 复盘              | 帮你复盘单篇论文、子项目或父项目                    | 现在支持项目复盘与快速熟悉路径                               |
| `S5`  | 本地检索 / 导航   | 基于本地证据回答问题并给出重读路径                  | 现在支持项目树列出、项目结构说明和兄弟项目比较               |
| `S6`  | 会话沉淀 / 匹配   | 把高价值讨论沉淀为 paper / project / hybrid session | 现在支持项目感知与 hybrid session                            |

---

## 推荐使用方式（v0.2.0）

### 日常读论文

1. 在 Zotero 中阅读并批注文献。
2. 通过 ZotLit 将文献笔记导入 Obsidian。
3. 如果讨论中出现值得保留的内容，先运行 **S6**。
4. 运行 **S1**，把论文编译进全局 wiki。
5. 查看 `Management/ProjectAssignmentQueue.md` 中的项目建议。
6. 确认后，再把最终项目归属写入 `project.yml`。
7. 需要快速回顾时用 **S4**。
8. 需要“答案 + 路径 + 重读顺序”时用 **S5**。

### 每周维护

1. 运行 **S2** 进行全库整理。
2. 查看 lint 报告。
3. 运行 **S3** 生成周报。

### 项目推进

1. 让论文知识保持唯一地写在 `Notes/` 中。
2. 让项目归类和层级关系保持在 `projects/` 中。
3. 始终把 `project.yml` 当作唯一项目归属接口。
4. 需要快速熟悉项目时，优先使用 **S4** 与 **S5**。

---

## 适用场景

本项目适用于：

- 学术论文阅读
- 文献管理
- 研究笔记整理
- 本地个人知识库建设
- 跨论文概念与方法比较
- 以项目为导向的研究组织
- 长期研究记忆支持

尤其适合偏好以下工作流的用户：

- 本地优先
- Markdown 文件化管理
- 基于 Obsidian 的笔记系统
- 结构清晰、可追踪、可检查的文件组织
- Agent 辅助但仍由用户掌控的知识管理方式

---

## 说明

- 本仓库是 **Agent 无关（Agent-agnostic）** 的。
- 本系统默认采用 **本地文件工作流**。
- `LiteratureNotes/` 中的内容应尽量作为原始来源层保留。
- Agent 维护的整理性内容应主要写入 `Notes/`、`projects/` 与 `Management/`。
- 如果你自行修改目录结构，请同步修改对应的 Skill 提示词与 Agent 说明文件。
