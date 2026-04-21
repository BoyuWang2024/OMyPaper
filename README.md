# OMyPaper Skills

### Overview

**OMyPaper Skills** is a local literature-management and knowledge-base workflow designed for researchers who read papers in **Zotero**, sync literature notes into **Obsidian** through **ZotLit**, and then use an **LLM Agent** to continuously organize, refine, retrieve, review, and summarize the accumulated knowledge.

Instead of treating an LLM as a temporary question-answering tool that re-reads raw documents from scratch every time, this project treats the LLM as a **persistent knowledge-base maintainer**. The Agent reads your literature notes, transforms them into structured wiki-style knowledge, maintains links across papers and concepts, generates weekly reports, supports paper review, and helps you quickly relocate the most relevant notes when you need to revisit a topic.

This repository contains a set of reusable **Skills** that can be used with **any Agent tool**. The workflow is not tied to a specific coding assistant or platform. As long as your Agent can read and write local files, these Skills can be adapted for your use.

---

### Compatibility

This Skills set is developed and organized around the following environment:

- **Zotero**: `7.0.32`
- **Obsidian**: `1.6.7`
- **ZotLit plugin**: used as the bridge between Zotero and Obsidian

> These versions are the recommended baseline for this repository.  
> Other versions may also work, but the current structure and workflow are designed and tested around the versions listed above.

---

### Required Obsidian Setup

To use this Skills system correctly:

1. Copy the folder **`OMyPaper`** into your Obsidian vault.
2. Install and enable the **ZotLit** plugin in Obsidian.
3. In ZotLit settings, set:

```text
Default location for new literature notes = OMyPaper/LiteratureNotes
```

This setting is important because the Skills assume that all raw literature notes imported from Zotero will be stored under:

```text
OMyPaper/LiteratureNotes
```

If this path is changed, the Agent-side workflows, note matching logic, and knowledge-base maintenance routines may fail or require manual modification.

For ZotLit installation and configuration details, please refer to the official documentation:

- [ZotLit Official Documentation (Chinese)](https://zotlit.aidenlx.top/zh-CN)

---

### Core Design Philosophy

This project follows a simple principle:

- **Zotero** is used for reading papers, annotating PDFs, and managing references.
- **ZotLit** is used to export literature notes into Obsidian.
- **Obsidian** is used as the local file-based knowledge container.
- **LLM Agents** are used to organize, synthesize, maintain, and retrieve knowledge.

The raw literature note is treated as the **source layer**, while the Agent-maintained wiki is treated as the **knowledge layer**.

This means the system is not just a note archive. It is intended to become a continuously evolving, locally stored, and cross-linked **personal paper wiki**.

---

### Repository Structure

A recommended folder layout is:

```text
OMyPaper/
├─ LiteratureNotes/          # Raw notes imported from Zotero via ZotLit
├─ Notes/
│  ├─ Papers/                # Per-paper wiki pages
│  ├─ Concepts/              # Concept pages
│  ├─ Methods/               # Method pages
│  ├─ Datasets/              # Dataset pages
│  ├─ Comparisons/           # Cross-paper comparison pages
│  ├─ Topics/                # Thematic pages
│  └─ Syntheses/             # Higher-level synthesis pages
├─ Management/
│  ├─ AGENTS.md              # Agent instructions / schema
│  ├─ CurrentQuestions.md    # Current research questions
│  ├─ index.md               # Global wiki index
│  ├─ log.md                 # Append-only operation log
│  ├─ PaperRegistry.md       # Paper mapping registry
│  ├─ WeeklyReports/         # Weekly reading reports
│  ├─ ReviewNotes/           # Paper review / recall notes
│  └─ LintReports/           # Knowledge-base health reports
└─ Attachments/              # Optional local attachments
```

---

### What These Skills Are Designed to Do

This Skills collection is designed to support the full lifecycle of academic paper reading and knowledge management:

- turn raw Zotero reading notes into structured wiki pages
- continuously maintain a local paper knowledge base
- organize concepts, methods, datasets, and comparisons across papers
- generate weekly reading summaries from local notes
- help recall and review a paper efficiently
- answer topic-based questions by searching the local knowledge base first
- tell you **which paper**, **which note**, and **which part** is most worth revisiting

The long-term goal is to help researchers move from scattered reading traces to a maintainable and queryable local research memory.

---

### Skills Description

#### Skill 0 — Structure Check & Initialization

This Skill checks whether the local knowledge-base workspace is properly initialized and follows the expected folder and naming conventions. It verifies the existence of key directories such as `LiteratureNotes`, `Notes`, and `Management`, checks paper identifiers such as `citekey`, and creates missing management files when needed. It serves as the foundation for all other Skills by ensuring that the repository remains stable, consistent, and machine-maintainable as it grows.

#### Skill 1 — Single-Paper Note Ingestion

This Skill processes one paper at a time. It reads the corresponding literature note imported from Zotero, optionally reads the PDF and related local files, and transforms the raw note into a structured wiki page under the local knowledge base. It extracts the paper’s problem setting, core method, important experiments, user concerns, limitations, and relations to existing notes. It may also update related concept pages, method pages, comparison pages, and registry records. Its purpose is not to produce a shallow summary, but to integrate one paper into a reusable knowledge network.

#### Skill 2 — Full Wiki Organization

This Skill performs maintenance over the entire `Notes` knowledge base. It identifies duplicated pages, inconsistent naming, missing links, orphan pages, stale conclusions, and missing cross-paper relationships. It can generate a lint-style report and optionally reorganize the wiki in a safe and structured way. Its goal is to prevent the local knowledge base from becoming another pile of fragmented notes and instead keep it navigable, coherent, and sustainable over time.

#### Skill 3 — Weekly Report Generation

This Skill reads the update history of the local knowledge base and generates a weekly markdown report summarizing what has been read, what has been updated, what new concepts or judgments emerged, what unresolved questions remain, and what should be prioritized next week. The generated report is intended to be viewed directly inside Obsidian. It is not merely a reading log, but a structured summary of intellectual progress.

#### Skill 4 — Paper Review / Recall

This Skill helps the user quickly revisit and reconstruct their understanding of a specific paper. It combines the original ZotLit-imported note, the structured paper wiki, and related concept/comparison pages to produce a recall-oriented review. It can be used for fast memory refresh, deeper re-understanding, meeting preparation, or self-testing. The emphasis is not on re-summarizing the paper from scratch, but on restoring the user’s previous reading context and highlighting what still deserves a second look.

#### Skill 5 — Local Knowledge Retrieval & Re-reading Navigation

This Skill answers topic- or method-based questions by searching the local knowledge base first. Instead of giving only a generic answer, it identifies which papers, notes, or sections are most relevant, and tells the user what to revisit and in what order. It is designed to turn the knowledge base into an actual research navigation system rather than a static note archive.

---

### Why This Workflow Matters

Most LLM-based paper workflows behave like temporary retrieval systems: they re-read documents every time and do not preserve the structure of prior learning. This project takes a different approach. It uses the Agent to continuously build and maintain a persistent local paper wiki.

As more papers are read, the knowledge base becomes:

- more structured
- more interconnected
- easier to search
- easier to review
- easier to reuse in future research

In short, the system is designed to help users accumulate research understanding over time rather than repeatedly restart from raw documents.

---

### Recommended Usage Pattern

A practical daily workflow looks like this:

1. Read and annotate papers in Zotero.
2. Sync/import literature notes into Obsidian via ZotLit.
3. Run the **Single-Paper Note Ingestion** Skill to compile the paper into the local wiki.
4. Periodically run **Full Wiki Organization** to keep the knowledge base healthy.
5. Generate weekly reports with **Weekly Report Generation**.
6. Use **Paper Review** and **Local Knowledge Retrieval** during study, meetings, and research planning.

---

### Scope

This project is designed for:

- academic paper reading
- literature management
- research note organization
- local personal knowledge base building
- concept/method comparison across papers
- long-term research memory support

It is especially suitable for users who prefer:

- local-first workflows
- markdown-based knowledge systems
- Obsidian-based note taking
- reproducible and inspectable file structures
- Agent-assisted but user-controlled knowledge management

---

### Notes

- This repository is **Agent-agnostic**: any Agent tool can use these Skills.
- The system assumes a **local file-based workflow**.
- The imported literature notes under `LiteratureNotes/` should generally be treated as the raw source layer.
- The Agent-maintained content should mainly live in `Notes/` and `Management/`.
- If you customize the folder structure, you should also update the corresponding Skill prompts and Agent instructions.

### 项目简介

**OMyPaper Skills** 是一套面向本地论文知识库工作流的 Skills 设计方案，适用于这样的阅读流程：你在 **Zotero** 中阅读和批注文献，通过 **ZotLit** 将文献笔记导入 **Obsidian**，再由 **LLM Agent** 持续完成笔记整理、知识沉淀、结构维护、主题检索、论文复盘与周报生成。

这套系统的核心思想不是把 LLM 当作一个“临时读 PDF、临时回答问题”的工具，而是把它当作一个**持续维护本地知识库的助手**。Agent 会读取你的文献笔记，将其转化为结构化的 wiki 内容，维护不同论文、概念与方法之间的联系，生成阶段性总结，并在你需要时帮助你快速定位“应该回看哪篇论文、哪条笔记、哪一部分内容”。

本仓库中的 Skills 设计为**适用于任意 Agent 工具**。它不绑定某一个特定平台，也不依赖某一个特定编程助手。只要你的 Agent 具备本地文件读写能力，就可以将本 Skills 体系应用到自己的论文管理流程中。

---

### 兼容环境

本 Skills 体系基于以下版本进行开发与组织：

- **Zotero**：`7.0.32`
- **Obsidian**：`1.6.7`
- **ZotLit 插件**：作为 Zotero 与 Obsidian 之间的桥接工具

> 上述版本是本仓库推荐的基准环境。  
> 其他版本未必不能使用，但当前目录结构、工作流约定与相关 Skills 均围绕这些版本进行设计。

---

### 使用前的必要配置

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

如果这个路径被修改，那么 Agent 在执行笔记匹配、单篇整理、知识库维护和检索导航时，可能无法正确找到原始笔记，或者需要你手动调整相应的提示词与规则。

关于 ZotLit 的安装与详细配置，请参考官方文档：

- [ZotLit 官方文档](https://zotlit.aidenlx.top/zh-CN)

---

### 核心设计理念

本项目遵循如下分工：

- **Zotero** 负责阅读论文、管理参考文献、批注 PDF。
- **ZotLit** 负责将文献笔记导入 Obsidian。
- **Obsidian** 负责承载本地文件化知识库。
- **LLM Agent** 负责整理、综合、维护和检索知识。

其中，原始文献笔记被视为**原始来源层**，而 Agent 维护的 wiki 页面被视为**知识层**。

也就是说，这套系统不是简单的笔记存档工具，而是一个会随着阅读不断演化、不断补充链接和结构的**本地论文 wiki 系统**。

---

### 推荐目录结构

建议采用如下目录结构：

```text
OMyPaper/
├─ LiteratureNotes/          # 由 ZotLit 从 Zotero 导入的原始文献笔记
├─ Notes/
│  ├─ Papers/                # 单篇论文 wiki 页面
│  ├─ Concepts/              # 概念页
│  ├─ Methods/               # 方法页
│  ├─ Datasets/              # 数据集页
│  ├─ Comparisons/           # 跨论文比较页
│  ├─ Topics/                # 主题页
│  └─ Syntheses/             # 高层综合页
├─ Management/
│  ├─ AGENTS.md              # Agent 说明 / schema
│  ├─ CurrentQuestions.md    # 当前重点问题
│  ├─ index.md               # 全局索引
│  ├─ log.md                 # 追加式日志
│  ├─ PaperRegistry.md       # 论文映射登记表
│  ├─ WeeklyReports/         # 周报
│  ├─ ReviewNotes/           # 论文复盘记录
│  └─ LintReports/           # 知识库整理报告
└─ Attachments/              # 可选的本地附件目录
```

---

### 这套 Skills 主要解决什么问题

本 Skills 体系旨在覆盖论文阅读与知识管理的完整过程，包括：

- 将 Zotero 中的原始阅读批注整理成结构化 wiki
- 持续维护本地论文知识库
- 整理跨论文的概念、方法、数据集和比较关系
- 根据本地知识库生成周报
- 支持对单篇论文进行快速复盘
- 当你提问某个方法或主题时，优先从本地知识库中检索答案
- 明确告诉你**应该重看哪篇论文、哪份笔记、哪一部分内容**

它的长期目标，是帮助研究者把零散的阅读痕迹，逐步转化为一个可维护、可复盘、可检索、可持续积累的本地研究记忆系统。

---

### Skills 详细说明

#### Skill 0 —— 结构检查与初始化

该 Skill 用于检查本地知识库目录是否完整、命名是否规范、关键索引文件是否存在，并在必要时自动补齐缺失的基础管理文件。它会校验 `LiteratureNotes`、`Notes`、`Management` 等核心目录，检查 `citekey` 等论文标识是否可用于后续映射，并保证整个仓库在规模增长后仍然具备清晰、稳定、可维护的结构。它是所有其他 Skills 正常工作的前提。

#### Skill 1 —— 单篇论文笔记整理

该 Skill 面向单篇论文的知识摄取过程。它会读取通过 ZotLit 导入的原始文献笔记，并在必要时结合 PDF 与相关本地文件，将该论文整理为结构化的本地 wiki 页面。整理内容包括论文问题定义、核心方法、关键实验、用户关注点、局限性以及与现有知识库内容的关联。同时，它还可以联动更新相关概念页、方法页、比较页和论文登记表。它的目标不是生成一个浅层摘要，而是把一篇论文真正纳入本地知识网络中。

#### Skill 2 —— 全库 Wikis 整理

该 Skill 负责对整个 `Notes` 知识库进行巡检与结构维护。它会识别重复页面、命名不统一、孤儿页面、缺失链接、陈旧结论以及尚未建立的跨论文关系，并以整理报告或安全更新的方式帮助用户优化知识库结构。它的目的，是避免知识库随着阅读量增加而重新变成另一堆分散笔记，而是始终保持可导航、可扩展和便于长期维护的形态。

#### Skill 3 —— 周报生成

该 Skill 会基于本地知识库的更新记录生成结构化的 Markdown 周报，总结本周阅读了哪些论文、更新了哪些判断、发现了哪些新概念、还有哪些未解决的问题，以及下一周值得优先推进的方向。生成结果可直接在 Obsidian 中查看。它并不是简单的“阅读流水账”，而是对研究认知推进过程的一次阶段性梳理。

#### Skill 4 —— 论文复盘

该 Skill 用于帮助用户快速回想和重建自己对某篇论文的理解。它会综合原始 ZotLit 笔记、结构化论文 wiki 页面以及相关概念页和比较页，生成面向记忆恢复和理解加深的复盘材料。它既可以用于快速回忆，也可以用于准备组会、答辩、自测或重新进入某一研究方向。它的重点不在于重新复述论文摘要，而在于恢复用户当时的阅读脉络，并指出哪些部分值得重新细看。

#### Skill 5 —— 本地知识库检索与重读导航

该 Skill 在你询问某个方法、概念、任务设定或研究问题时，优先检索本地知识库，而不是直接给出泛化性的模型回答。它不仅会告诉你有哪些相关论文，还会指出哪些页面、哪条笔记、哪一部分内容最值得优先回看，以及可能存在的冲突和不同视角。它的目标，是让本地知识库真正成为一个研究导航系统，而不只是一个静态的笔记存放区。

---

### 为什么这套工作流值得使用

大多数基于 LLM 的论文阅读流程，本质上仍然是“每次重新读文档、重新回答问题”的临时检索模式，知识不会真正沉淀。  
而这套系统的目标，是让 Agent 持续维护一个本地、持久、可演化的论文 wiki。

随着阅读的增加，这个知识库会变得：

- 更有结构
- 更有连接
- 更容易检索
- 更容易复盘
- 更容易服务于后续研究

换句话说，它帮助用户从“重复从原始文档开始”转向“在持续积累的研究理解上继续推进”。

---

### 推荐使用方式

一个比较自然的日常工作流如下：

1. 在 Zotero 中阅读并批注文献。
2. 通过 ZotLit 将文献笔记导入 Obsidian。
3. 运行 **单篇论文笔记整理** Skill，将论文编译进本地 wiki。
4. 定期运行 **全库 Wikis 整理** Skill，保持知识库结构健康。
5. 每周使用 **周报生成** Skill 形成阶段性总结。
6. 在复习、组会和研究规划时使用 **论文复盘** 与 **本地知识库检索** Skills。

---

### 适用场景

本项目适用于：

- 学术论文阅读
- 文献管理
- 研究笔记整理
- 本地个人知识库建设
- 跨论文概念与方法比较
- 长期研究记忆支持

尤其适合偏好以下工作流的用户：

- 本地优先
- Markdown 文件化管理
- 基于 Obsidian 的笔记系统
- 结构清晰、可追踪、可检查的文件组织
- Agent 辅助但仍由用户掌控的知识管理方式

---

### 说明

- 本仓库是 **Agent 无关（Agent-agnostic）** 的：任意 Agent 工具都可以使用这套 Skills。
- 本系统默认采用 **本地文件工作流**。
- `LiteratureNotes/` 中的内容应尽量作为原始来源层保留。
- Agent 维护的整理性内容应主要写入 `Notes/` 与 `Management/`。
- 如果你自行修改目录结构，请同步修改对应的 Skill 提示词与 Agent 说明文件。

