# OMyPaper Guide

## Project Overview

`OMyPaper` is a local paper knowledge base system designed to gradually turn the “raw traces left while reading papers” into a “continuously maintainable knowledge base.”

The problem it solves is not “temporarily throw a PDF to a model and ask a few questions,” but rather:

- to let each reading session leave behind long-term reusable assets
- to give paper understanding, concept organization, method comparison, review, and weekly reporting a fixed entry point
- to make later retrieval prioritize your own local knowledge base instead of relying on the model’s temporary improvisation

This repository is intended to work with the following workflow by default:

`Read and annotate in Zotero -> Import raw notes through ZotLit -> Store the local knowledge base in Obsidian -> Use an Agent with Skills to maintain Notes/ and Management/`

You can understand it as three layers:

- `LiteratureNotes/`: the raw reading-trace layer
- `Notes/`: the organized knowledge base / wiki layer
- `Management/`: the management layer for indexes, logs, templates, weekly reports, reviews, SessionNotes, and more

The reason `S0` to `S6` are split into seven Skills is that they separate different kinds of work:

- some Skills are responsible for “checking the environment”
- some are responsible for “organizing a single paper”
- some are responsible for “maintaining the whole knowledge base”
- some are responsible for “weekly reports / review / retrieval / session accumulation”

The advantage of this design is that responsibilities are clear, the Skills are less likely to be mixed up, and the system is easier to maintain over the long term.

---

## Brief Explanation of the Repository Structure

### `Attachments/`

Stores PDF files, images, supplementary materials, and other attachment resources.

You usually do not need to organize this folder directly. Most of the time, it serves as the attachment layer that Skills may reference when reading evidence.

### `LiteratureNotes/`

This is the raw note layer, usually imported from Zotero through ZotLit.

The contents here generally include:

- citekey
- zotero-key
- title
- page-level annotations
- PDF attachment links

This layer is positioned as the “raw reading trace” layer. It is usually not edited directly, and by default the Skills will not modify it directly either.

### `Notes/`

This is the knowledge-base wiki layer, mainly used to store organized long-term pages such as:

- `Notes/Papers/`: per-paper pages
- `Notes/Concepts/`: concept pages
- `Notes/Methods/`: method pages
- `Notes/Comparisons/`: comparison pages
- `Notes/Topics/`: topic pages

If you have finished reading a paper and want to formally write it into the knowledge base, this is the layer where the main outputs will be produced.

### `Management/`

This is the control and management layer, used to store:

- Skills specifications
- invocation templates
- indexes
- logs
- SessionNotes
- weekly reports
- review notes
- lint reports
- template files

You can think of it as “the dispatch center and runtime record area of this repository.”

---

## Overview Table of the Seven Skills

| Skill | Name                                                   | One-sentence role                                            | Most common usage scenario                                   |
| ----- | ------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S0    | Specification Check / Initialization                   | Checks whether the repository structure and basic files are complete | Initial setup, after bulk import, after major structural changes |
| S1    | Single-Paper Note Organization                         | Formally compiles one paper into the local wiki              | After finishing a paper and preparing to formally ingest it  |
| S2    | Full Wiki Organization                                 | Checks and maintains the structural health of the whole knowledge base | Periodic maintenance, after batch organization               |
| S3    | Weekly Report Generation                               | Generates a weekly report based on this week’s knowledge-base changes | End of the week, before meetings, during stage summaries     |
| S4    | Paper Review                                           | Helps you recall how you understood a paper                  | Before interviews, meetings, revision, or defense            |
| S5    | Local Knowledge Base Retrieval / Re-reading Navigation | Answers based on the local knowledge base first and gives revisit paths | When checking concepts, methods, or experimental phenomena   |
| S6    | Reading Session Accumulation and Paper Matching        | Turns high-value discussion into SessionNotes                | When important understanding emerges during reading          |

If you only want to remember one sentence:

- Want to check the system: `S0`
- Want to organize one paper: `S1`
- Want to maintain the whole knowledge base: `S2`
- Want to generate a weekly report: `S3`
- Want to quickly review one paper: `S4`
- Want to find answers from the local knowledge base: `S5`
- Want to save discussion content first: `S6`

---

## Detailed Description of Each Skill

### S0: Specification Check / Initialization

**What it does**

S0 can be understood as the “environment inspector.” It is responsible for checking whether the current repository structure is complete, whether the basic management files exist, whether indexes are in place, and whether there are obvious consistency problems.

**Which files / directories it mainly handles**

- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/CurrentQuestions.md`
- `Management/BootstrapReports/`

**When you should call it**

- When setting up this repository for the first time
- After importing a large batch of paper notes
- After manually moving files or changing the structure
- When you want to confirm whether the current repository can continue to run stably

**When it is not suitable**

- It is not suitable for organizing the content of a single paper
- It should not be used as a substitute for S1 to ingest a paper into the knowledge base

**How to understand it**

If you say “Run S0,” it means asking the agent to first check whether this system is healthy, rather than to write paper wiki pages.

**Invocation templates**

```text
Run S0 to perform a bootstrap and consistency check on the current OMyPaper vault, fill in missing base files, and output a report.
```

```text
Run S0 to check only the structural consistency of LiteratureNotes / Notes / Management, make no aggressive fixes, and write conflicts into the report.
```

---

### S1: Single-Paper Note Organization

**What it does**

S1 is one of the most core Skills. It is responsible for formally compiling one paper into the local wiki.

You can understand it as:

- Input: raw paper notes + related SessionNotes + existing wiki
- Output: a formal paper page, plus necessary concept pages / method pages / comparison pages / topic pages

**Which files / directories it mainly handles**

- reads `LiteratureNotes/{citekey}.md`
- reads `Management/SessionNotes/{citekey}/`
- reads `Management/CurrentQuestions.md`
- updates `Notes/Papers/{citekey}.md`
- when necessary updates `Notes/Concepts/`, `Notes/Methods/`, `Notes/Comparisons/`, `Notes/Topics/`
- updates `Management/PaperRegistry.md`, `Management/log.md`, and related files

**When you should call it**

- When a paper has been finished or at least read to a stage where it can be formally ingested
- When a raw note imported by ZotLit already exists
- Ideally when part of the reading discussion has already been accumulated through S6

**When it is not suitable**

- When there is no raw `LiteratureNotes` note yet
- When the paper identity is still uncertain
- When you only want to ask a temporary question

**How to understand it**

If you say “Run S1,” it means: formally compile this paper into the knowledge base, not just generate a temporary summary.

**Invocation templates**

```text
Run S1 and formally compile citekey=vaswani2017attention into the local wiki. Prioritize reading pending session notes, and explicitly distinguish paper claim / my note / LLM synthesis.
```

```text
Run S1 for the target paper titled "Attention Is All You Need." If the citekey is uncertain, match conservatively first; if the match is unstable, do not write it formally—only explain why.
```

---

### S2: Full Wiki Organization

**What it does**

S2 is the maintainer of the whole knowledge base. Its main job is to check structural health and perform lightweight maintenance over the entire `Notes/`.

Problems it is good at handling include:

- orphan pages
- duplicate pages
- broken links
- inconsistent naming
- missing backlinks
- missing comparison pages
- lightweight normalization

**Which files / directories it mainly handles**

- reads the entire `Notes/`
- reads `Management/index.md`, `Management/PaperRegistry.md`
- outputs `Management/LintReports/YYYY-MM-DD.md`
- performs low-risk fixes when appropriate

**When you should call it**

- During periodic maintenance of the knowledge base
- After continuously ingesting many papers
- When you want to check whether the structure of the knowledge base is healthy

**When it is not suitable**

- It should not be used as a substitute for S1 to organize a single paper
- It is not suitable for large-scale semantic rewriting

**How to understand it**

If you say “Run S2,” it means asking the agent to scan the structure of the whole knowledge base rather than deeply rewriting the content of a specific paper.

**Invocation templates**

```text
Run S2 to lint and apply low-risk fixes across the whole Notes/, generate today’s lint report, and update the management index.
```

```text
Run S2 to check only Concepts and Methods, focusing on duplicate pages, broken links, inconsistent naming, and missing comparison pages; write high-risk items to the report only.
```

---

### S3: Weekly Report Generation

**What it does**

S3 is responsible for generating a weekly report based on this week’s knowledge-base updates.

It does not simply list paper titles, but tries to organize:

- new papers read this week
- new concepts added this week
- important judgment updates this week
- unresolved questions that remain
- progress in knowledge-base construction
- key priorities recommended for next week

**Which files / directories it mainly handles**

- reads `Management/log.md`
- reads `Notes/` updated this week
- reads `LiteratureNotes/` newly added this week
- reads this week’s `SessionNotes/`
- reads `Management/CurrentQuestions.md`
- outputs to `Management/WeeklyReports/`

**When you should call it**

- At the end of each week
- Before a group meeting
- During stage-level review

**When it is not suitable**

- It is not suitable for forcing a “rich” weekly report when the repository has almost no local updates
- It should not be treated as a simple流水账 exporter

**How to understand it**

If you say “Run S3,” it means asking the agent to generate a weekly report based on local files that contains real conclusions and unresolved questions.

**Invocation templates**

```text
Run S3 and generate a weekly report based on this week’s log, SessionNotes, Notes updates, and CurrentQuestions. Do not write it as a chronological list.
```

```text
Run S3 to generate the weekly report for 2026-W17, and clearly state which conclusions are worth keeping, which are still uncertain, and which papers deserve a second read.
```

---

### S4: Paper Review

**What it does**

S4 helps you recall “how you yourself understood this paper.”

The key is not to rewrite the abstract, but to answer:

- what points you focused on at the time
- what parts you still had not fully understood
- if you were to revisit it, which part you should read first

**Which files / directories it mainly handles**

- reads `LiteratureNotes/{citekey}.md`
- reads `Notes/Papers/{citekey}.md`
- reads related concept pages / method pages / comparison pages
- reads related `SessionNotes`
- outputs to `Management/ReviewNotes/`

**When you should call it**

- Before interviews
- Before meetings
- During revision
- Before a thesis defense
- When you want to quickly restore your understanding after some time has passed

**When it is not suitable**

- It should not be used as a substitute for formal ingestion
- It should not be treated as a tool for “rewriting the abstract for me”

**Common supported modes**

- one-minute recall version
- five-minute in-depth version
- oral explanation version
- self-test question version

**How to understand it**

If you say “Run S4,” you are asking the agent to reactivate “your own understanding at the time.”

**Invocation templates**

```text
Run S4 with citekey=he2016resnet in one-minute recall mode, focusing on my own understanding and the parts I still had not fully grasped.
```

```text
Run S4 with citekey=vaswani2017attention and generate an oral explanation version suitable for a group meeting, including suggested revisit locations.
```

---

### S5: Local Knowledge Base Retrieval / Re-reading Navigation

**What it does**

S5 answers questions by prioritizing the local knowledge base, and provides “answer + page path + suggested revisit order.”

It is suitable for questions such as:

- which papers mention a certain concept
- what variants of a method exist
- what explanations exist for a certain experimental phenomenon
- how two things are distinguished within the local knowledge base

**Which files / directories it mainly handles**

- prioritizes reading `Notes/`
- reads `Management/index.md`, `Management/PaperRegistry.md`
- traces back to `LiteratureNotes/` when necessary
- may also refer to `WeeklyReports/` and `ReviewNotes/`

**When you should call it**

- When you want to find answers from the local knowledge base first
- When you want to know “which pages should I revisit first”
- When you want to connect concepts, methods, or phenomena with papers already in your archive

**When it is not suitable**

- It is not suitable if you only want a generic conversational answer
- It is not suitable to skip local evidence and let the model improvise directly

**How to understand it**

If you say “Run S5, and the question is...,” it means: search my local knowledge base first, then answer me—do not answer based only on general knowledge.

**Invocation templates**

```text
Run S5. The question is: In my local knowledge base, which papers have discussed exposure bias, and how does each of them explain it? Please give me page paths and a suggested revisit order.
```

```text
Run S5. The question is: Why do some works show retrieval saturation on long-context tasks? Please answer based on the local knowledge base first, and clearly state where evidence is insufficient.
```

---

### S6: Reading Session Accumulation and Paper Matching

**What it does**

S6 accumulates high-value discussion that occurs during reading into `SessionNotes`, for later use by S1.

Its role is roughly:

- first save the key understanding that emerged from this discussion
- do not let valuable discussion remain only in the chat window

**Which files / directories it mainly handles**

- creates `Management/SessionNotes/{citekey}/...`
- or, when matching is unstable, writes to `Management/SessionNotes/_unmatched/`
- updates `Management/SessionIndex.md`
- updates `Management/log.md`

**When you should call it**

- When key understanding emerges during paper reading
- When a difficult point has just been clarified
- When you want to save this round of discussion first and hand it to S1 for formal absorption later

**When it is not suitable**

- It is not suitable for directly modifying the formal wiki
- It is not suitable for dumping raw chat transcripts as-is

**How to understand it**

S6 is the preparatory input stage for S1. In general, you use S6 first, then S1.

**Invocation templates**

```text
Run S6 and turn our discussion about citekey=vaswani2017attention into a SessionNote, keeping only high-value content, and set merge_status: pending.
```

```text
Run S6 and organize the current discussion into a standardized SessionNote. First try to match the paper; if the match is unstable, save it to _unmatched/ and do not formally modify Notes/.
```

---

## Recommended Workflows

### A. Daily Paper Reading Workflow

1. Read the PDF in Zotero and make annotations.
2. Use ZotLit to import the notes into `LiteratureNotes/`.
3. If key understanding emerges while discussing with the agent, use `S6` first to accumulate the high-value discussion.
4. After finishing a paper, use `S1` to formally compile it into the wiki layer under `Notes/`.
5. Later, when you want to quickly review this paper, use `S4`.
6. If you want to ask where a concept, method, or experimental phenomenon appears in the local knowledge base, use `S5`.

One-sentence memory aid: **for a new paper, usually use S6 first, then S1.**

### B. Weekly Maintenance Workflow

1. Use `S2` to perform a health check on the entire knowledge base.
2. Read the lint report and confirm whether there are duplicate pages, broken links, orphan pages, inconsistent naming, and similar issues.
3. Then use `S3` to generate the weekly report.

One-sentence memory aid: **for weekly reports, usually use S2 first, then S3.**

### C. Workflow After Initial Setup or Major Restructuring

1. First use `S0` for bootstrap / consistency checking.
2. Confirm that the basic directories, indexes, logs, and registries are all working properly.
3. Then continue with content work using S1, S2, S6, and others.

### D. Workflow Before Review / Interview / Group Meeting

1. Use `S4` first for paper review.
2. If this leads to cross-paper concept questions, then use `S5` for local retrieval and navigation.

---

## Most Common Invocation Templates

The templates below can be copied directly and used after changing a few keywords.

### S0

```text
Run S0 to perform a bootstrap and consistency check on the current OMyPaper vault, fill in missing management files, and output a report.
```

### S1

```text
Run S1 and formally compile citekey=vaswani2017attention into the local wiki. Prioritize reading pending session notes, and explicitly distinguish paper claim / my note / LLM synthesis.
```

```text
Run S1. The target paper title is "Attention Is All You Need." If the citekey is uncertain, match conservatively first; if the match is unstable, do not formally write it in—only explain why.
```

### S2

```text
Run S2 to lint the whole Notes/ and apply low-risk fixes, generate today’s lint report, and write high-risk items to the report only without executing them directly.
```

### S3

```text
Run S3 to generate a weekly report based on this week’s log, SessionNotes, Notes updates, and CurrentQuestions, and clearly indicate which conclusions are worth keeping and which remain uncertain.
```

### S4

```text
Run S4 with citekey=he2016resnet in one-minute recall mode, focusing on my own understanding and the parts I still had not fully grasped.
```

```text
Run S4 with citekey=vaswani2017attention and generate an oral explanation version suitable for a group meeting, including suggested revisit locations.
```

### S5

```text
Run S5. The question is: In my local knowledge base, which papers mention local attention and global attention respectively? What are the main differences? Please give me page paths and a suggested revisit order.
```

```text
Run S5. The question is: Why do some works show retrieval saturation on long-context tasks? Please answer based on the local knowledge base first. If evidence is insufficient, say so explicitly.
```

### S6

```text
Run S6 and organize our discussion about citekey=vaswani2017attention into a standardized SessionNote. Keep only high-value content and set merge_status: pending.
```

```text
Run S6 and accumulate the current discussion. First try to match the paper; if the match is unstable, place it under Management/SessionNotes/_unmatched/ and do not formally modify Notes/.
```

---

## Usage Suggestions / Notes

1. Do not directly modify `LiteratureNotes/`.
   - This layer is the raw note layer and is mainly used to preserve reading traces.

2. Whenever possible, always specify the `citekey`.
   - This makes matching the most stable and later updates the easiest.

3. If matching is unstable, be conservative.
   - Do not force archive, force merge, or force writing a formal page.

4. For a new paper, generally use `S6` first, then `S1`.
   - Save high-value discussion first, then formally ingest it.

5. For weekly reports, generally use `S2` first, then `S3`.
   - It is more stable to organize the structure before summarizing.

6. If you want to retrieve knowledge from the local archive, prioritize `S5`.
   - Do not skip the local knowledge base and directly ask a broad question.

7. If you want to review “how I understood this paper at the time,” use `S4`.
   - Do not let the agent simply rewrite the abstract as a substitute for review.

8. S1 is the formal ingestion entry point, and S2 is the full-knowledge-base maintenance entry point.
   - Do not mix them up.

9. If you only want to save a discussion first, do not use S1 directly.
   - Use S6 first, then hand it over to S1 later.

10. If you are not sure which Skill to use, you can first ask:

   - “In my current scenario, is S1, S4, or S5 more appropriate?”

---

## One-Sentence Closing

If you only want to remember the most commonly used combinations, remember this first:

- **While reading papers: S6 -> S1**
- **For weekly maintenance: S2 -> S3**
- **For review: S4**
- **For retrieval: S5**
- **For system checking: S0**

# OMyPaper 使用说明

## 项目简介

`OMyPaper` 是一个本地论文知识库系统，用来把“读论文时的原始痕迹”逐步沉淀成“可持续维护的知识库”。

它解决的问题不是“临时把 PDF 扔给模型问几个问题”，而是：

- 让每次阅读留下长期可复用的资产
- 让论文理解、概念整理、方法对比、复盘和周报都有固定入口
- 让后续检索优先基于你自己的本地知识库，而不是模型临时发挥

这个仓库默认配合下面的工作流使用：

`Zotero 阅读与批注 -> ZotLit 导入原始笔记 -> Obsidian 存放本地知识库 -> Agent 用 Skills 维护 Notes/ 和 Management/`

你可以把它理解成三层：

- `LiteratureNotes/`：原始阅读痕迹层
- `Notes/`：整理后的知识库 / wiki 层
- `Management/`：索引、日志、模板、周报、复盘、SessionNotes 等管理层

之所以需要 `S0` 到 `S6` 这七个 Skills，是因为它们把不同工作拆开了：

- 有的 Skill 负责“检查环境”
- 有的 Skill 负责“整理单篇论文”
- 有的 Skill 负责“全库维护”
- 有的 Skill 负责“周报 / 复盘 / 检索 / 会话沉淀”

这样做的好处是：职责清楚，不容易混用，也方便长期维护。

---

## 仓库目录简要说明

### `Attachments/`

存放 PDF、图片、补充材料等附件资源。

你通常不需要专门对它做整理操作。更多时候它是 Skill 读取证据时会参考的附件层。

### `LiteratureNotes/`

这是原始笔记层，通常由 ZotLit 从 Zotero 导入。

这里的内容一般包含：

- citekey
- zotero-key
- 标题
- 页码批注
- PDF 附件链接

这层的定位是“原始阅读痕迹”，通常不直接改，Skills 默认也不会直接改它。

### `Notes/`

这是知识库 wiki 层，主要存放整理后的长期页面，例如：

- `Notes/Papers/`：单篇论文页
- `Notes/Concepts/`：概念页
- `Notes/Methods/`：方法页
- `Notes/Comparisons/`：对比页
- `Notes/Topics/`：专题页

如果你已经读完一篇论文，想把它正式写进知识库，主要就是在这一层产出结果。

### `Management/`

这是控制与管理层，负责放：

- Skills 规范
- 调用模板
- 索引
- 日志
- SessionNotes
- 周报
- 复盘笔记
- Lint 报告
- 模板文件

可以把它理解为“这个仓库的调度中心和运行记录区”。

---

## 七个 Skills 总览表

| Skill | 名称 | 一句话作用 | 最常见使用场景 |
| --- | --- | --- | --- |
| S0 | 规范检查 / 初始化 | 检查仓库结构和基础文件是否齐全 | 初次搭建、批量导入后、大改结构后 |
| S1 | 单篇论文笔记整理 | 把一篇论文正式编译进本地 wiki | 读完一篇论文，准备正式入库时 |
| S2 | 全库 Wikis 整理 | 检查并维护整个知识库结构健康 | 周期性维护、批量整理后 |
| S3 | 周报生成 | 根据本周知识库变化生成周报 | 每周末、组会前、阶段总结时 |
| S4 | 论文复盘 | 帮你回想自己是怎么理解这篇论文的 | 面试、组会、复习、答辩前 |
| S5 | 本地知识库检索 / 重读导航 | 优先基于本地库回答问题并给重看路径 | 查概念、查方法、查实验现象时 |
| S6 | 阅读会话沉淀与论文匹配 | 把高价值讨论沉淀成 SessionNote | 阅读过程中聊出了关键理解时 |

如果你只想先记住一句话：

- 想检查系统：`S0`
- 想整理一篇论文：`S1`
- 想维护整个知识库：`S2`
- 想生成周报：`S3`
- 想快速回顾一篇论文：`S4`
- 想从本地库里找答案：`S5`
- 想把讨论内容先存下来：`S6`

---

## 每个 Skill 的详细说明

### S0：规范检查 / 初始化

**它是干什么的**

S0 可以理解为“环境检查员”。它负责检查当前仓库结构是否完整、基础管理文件是否存在、索引是否齐全，以及有没有明显的一致性问题。

**它主要会处理哪些文件 / 目录**

- `Management/index.md`
- `Management/log.md`
- `Management/PaperRegistry.md`
- `Management/SessionIndex.md`
- `Management/CurrentQuestions.md`
- `Management/BootstrapReports/`

**什么时候应该调用它**

- 第一次搭建这套仓库时
- 批量导入很多论文笔记之后
- 手动移动过文件或改过结构之后
- 想确认当前仓库能不能稳定继续跑下去时

**不适合什么时候调用**

- 不适合用来整理单篇论文内容
- 不适合拿它代替 S1 做论文入库

**该怎么理解**

如果你说“执行 S0”，意思就是让 agent 先检查这套系统是否健康，而不是去写论文 wiki。

**调用模板**

```text
执行 S0，对当前 OMyPaper vault 做一次 bootstrap 与一致性检查，补齐缺失基础文件，并输出报告。
```

```text
执行 S0，只检查 LiteratureNotes / Notes / Management 的结构一致性，不做激进修复，把冲突写进报告。
```

---

### S1：单篇论文笔记整理

**它是干什么的**

S1 是最核心的 Skills 之一。它负责把一篇论文正式编译进本地 wiki。

你可以把它理解为：

- 输入：原始论文笔记 + 相关 SessionNotes + 已有 wiki
- 输出：正式的论文页，以及必要的概念页 / 方法页 / 对比页 / 专题页

**它主要会处理哪些文件 / 目录**

- 读取 `LiteratureNotes/{citekey}.md`
- 读取 `Management/SessionNotes/{citekey}/`
- 读取 `Management/CurrentQuestions.md`
- 更新 `Notes/Papers/{citekey}.md`
- 必要时更新 `Notes/Concepts/`、`Notes/Methods/`、`Notes/Comparisons/`、`Notes/Topics/`
- 更新 `Management/PaperRegistry.md`、`Management/log.md` 等

**什么时候应该调用它**

- 一篇论文已经读完或至少读到“可以正式入库”的程度
- 已经有 ZotLit 导入的原始笔记
- 最好已经有一部分阅读讨论通过 S6 被沉淀下来

**不适合什么时候调用**

- 还没有 `LiteratureNotes` 原始笔记时
- 论文身份还不稳时
- 只是想临时问一个问题时

**该怎么理解**

如果你说“执行 S1”，意思就是：请正式把这篇论文编译进知识库，而不是只做临时总结。

**调用模板**

```text
执行 S1，把 citekey=vaswani2017attention 正式编译进本地 wiki。优先读取 pending session notes，并显式区分 paper claim / my note / LLM synthesis。
```

```text
执行 S1，目标论文标题是《Attention Is All You Need》。如果 citekey 不确定，先保守匹配；匹配不稳就不要正式写入，只说明原因。
```

---

### S2：全库 Wikis 整理

**它是干什么的**

S2 是全库维护器，主要负责整个 `Notes/` 的结构健康检查与轻量维护。

它擅长处理的问题包括：

- 孤儿页
- 重复页
- 断链
- 命名不一致
- 缺少 backlinks
- 缺少 comparison 页
- 轻量规范化

**它主要会处理哪些文件 / 目录**

- 读取整个 `Notes/`
- 读取 `Management/index.md`、`Management/PaperRegistry.md`
- 输出 `Management/LintReports/YYYY-MM-DD.md`
- 必要时做低风险修复

**什么时候应该调用它**

- 周期性维护知识库时
- 连续 ingest 了多篇论文之后
- 想检查知识库结构是否健康时

**不适合什么时候调用**

- 不适合拿来代替 S1 整理单篇论文
- 不适合让它做大规模语义重写

**该怎么理解**

如果你说“执行 S2”，意思是让 agent 扫描整个知识库结构，而不是深度重写某篇论文内容。

**调用模板**

```text
执行 S2，对整个 Notes/ 做 lint 和低风险修复，生成今天的 lint 报告，并更新 management index。
```

```text
执行 S2，只检查 Concepts 和 Methods，重点找重复页、断链、命名不一致和缺失 comparison 页；高风险项只写报告。
```

---

### S3：周报生成

**它是干什么的**

S3 负责根据本周知识库更新生成周报。

它不是简单列论文名，而是会尽量整理出：

- 本周新读论文
- 本周新增概念
- 本周重要判断更新
- 仍未解决的问题
- 知识库建设进展
- 下周建议重点

**它主要会处理哪些文件 / 目录**

- 读取 `Management/log.md`
- 读取本周更新过的 `Notes/`
- 读取本周新增的 `LiteratureNotes/`
- 读取本周 `SessionNotes/`
- 读取 `Management/CurrentQuestions.md`
- 输出到 `Management/WeeklyReports/`

**什么时候应该调用它**

- 每周末
- 组会前
- 阶段性回顾时

**不适合什么时候调用**

- 不适合在仓库几乎没有本地更新时强行生成“丰富”的周报
- 不适合把它当成流水账导出器

**该怎么理解**

如果你说“执行 S3”，意思是让 agent 基于本地文件生成一份真正有结论和未决问题的周报。

**调用模板**

```text
执行 S3，基于本周的 log、SessionNotes、Notes 更新和 CurrentQuestions 生成周报，不要写成流水账。
```

```text
执行 S3，为 2026-W17 生成周报，并明确哪些结论值得保留、哪些仍不确定、哪些论文值得二刷。
```

---

### S4：论文复盘

**它是干什么的**

S4 是帮你回想“自己是怎么理解这篇论文的”。

重点不是重新写一遍 abstract，而是：

- 你当时抓住了什么重点
- 你哪里还没吃透
- 如果要重新看，先看哪一部分

**它主要会处理哪些文件 / 目录**

- 读取 `LiteratureNotes/{citekey}.md`
- 读取 `Notes/Papers/{citekey}.md`
- 读取相关概念页 / 方法页 / comparison 页
- 读取相关 `SessionNotes`
- 输出到 `Management/ReviewNotes/`

**什么时候应该调用它**

- 面试前
- 组会前
- 复习时
- 答辩前
- 隔了一段时间想快速捡回理解时

**不适合什么时候调用**

- 不适合拿它代替正式入库整理
- 不适合把它当成“帮我重写论文摘要”的工具

**支持的常见模式**

- 一分钟回忆版
- 五分钟深入版
- 口头讲解版
- 自测提问版

**该怎么理解**

如果你说“执行 S4”，就是让 agent 帮你把“当时你自己的理解”唤醒出来。

**调用模板**

```text
执行 S4，citekey=he2016resnet，模式用一分钟回忆版，重点回忆我自己的理解和还没吃透的地方。
```

```text
执行 S4，citekey=vaswani2017attention，生成口头讲解版复盘，适合组会表达，并给出建议重看位置。
```

---

### S5：本地知识库检索 / 重读导航

**它是干什么的**

S5 优先基于本地知识库回答问题，并给出“答案 + 页面路径 + 建议重看顺序”。

它适合的问题类型包括：

- 某个概念出现在哪些论文里
- 某种方法有哪些变体
- 某个实验现象有哪些解释
- 某两个东西在本地库里是怎么被区分的

**它主要会处理哪些文件 / 目录**

- 优先读取 `Notes/`
- 读取 `Management/index.md`、`Management/PaperRegistry.md`
- 必要时回溯 `LiteratureNotes/`
- 必要时参考 `WeeklyReports/` 和 `ReviewNotes/`

**什么时候应该调用它**

- 想优先从本地知识库找答案时
- 想知道“应该先重看哪些页面”时
- 想把概念、方法、现象与已有论文关联起来时

**不适合什么时候调用**

- 不适合只要一个泛泛聊天回答
- 不适合跳过本地证据，直接让模型自由发挥

**该怎么理解**

如果你说“执行 S5，问题是……”，意思就是：先查本地，再回答我，不要直接靠常识聊天。

**调用模板**

```text
执行 S5，问题是：在我的本地知识库里，哪些论文讨论过 exposure bias？它们各自是怎么解释的？请给我页面路径和建议重看顺序。
```

```text
执行 S5，问题是：为什么有些工作在 long-context 任务上会出现 retrieval saturation？请优先基于本地知识库回答，并指出证据不足的地方。
```

---

### S6：阅读会话沉淀与论文匹配

**它是干什么的**

S6 会把阅读过程中的高价值讨论沉淀成 `SessionNotes`，供 S1 后续读取。

它的作用相当于：

- 先把这次聊出来的关键理解存下来
- 别让有价值的讨论只停留在聊天窗口里

**它主要会处理哪些文件 / 目录**

- 创建 `Management/SessionNotes/{citekey}/...`
- 或在匹配不稳时写入 `Management/SessionNotes/_unmatched/`
- 更新 `Management/SessionIndex.md`
- 更新 `Management/log.md`

**什么时候应该调用它**

- 读论文过程中聊出了关键理解
- 某个难点刚被讲清楚
- 你想先把这轮讨论存起来，之后再交给 S1 正式吸收

**不适合什么时候调用**

- 不适合拿它直接正式改 wiki
- 不适合把聊天流水账原样存进去

**该怎么理解**

S6 是 S1 的前置输入准备器。一般是先 S6，后 S1。

**调用模板**

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论沉淀成 SessionNote，只保留高价值内容，并设为 merge_status: pending。
```

```text
执行 S6，把当前讨论整理成标准化 SessionNote。先尝试匹配论文；如果匹配不稳，就保存到 _unmatched/，不要正式改 Notes/。
```

---

## 推荐工作流

### A. 日常读论文流程

1. 在 Zotero 中阅读 PDF 并做批注。
2. 用 ZotLit 把笔记导入 `LiteratureNotes/`。
3. 如果阅读过程中和 agent 聊出了关键理解，用 `S6` 先把高价值讨论沉淀下来。
4. 读完一篇论文后，用 `S1` 把它正式编译进 `Notes/` 的 wiki 层。
5. 以后要快速回顾这篇论文时，用 `S4`。
6. 如果你想问某个概念、方法、实验现象在本地库里出现在哪些地方，用 `S5`。

一句话记忆：**新论文通常是先 S6，再 S1。**

### B. 每周维护流程

1. 用 `S2` 对整个知识库做一次健康检查。
2. 看 lint 报告，确认有没有重复页、断链、孤儿页、命名不一致等问题。
3. 再用 `S3` 生成周报。

一句话记忆：**周报一般是先 S2，再 S3。**

### C. 初次搭建或大调整后的流程

1. 先用 `S0` 做 bootstrap / consistency check。
2. 确认基础目录、索引、日志、注册表都正常。
3. 再继续用 S1、S2、S6 等做内容工作。

### D. 复习 / 面试 / 组会前流程

1. 先用 `S4` 做论文复盘。
2. 如果又引出了跨论文的概念问题，再用 `S5` 做本地检索与导航。

---

## 最常用调用模板汇总

下面这些模板可以直接复制后改几个关键词使用。

### S0

```text
执行 S0，对当前 OMyPaper vault 做一次 bootstrap 与一致性检查，补齐缺失管理文件，并输出报告。
```

### S1

```text
执行 S1，把 citekey=vaswani2017attention 正式编译进本地 wiki。优先读取 pending session notes，并显式区分 paper claim / my note / LLM synthesis。
```

```text
执行 S1，目标论文标题是《Attention Is All You Need》。如果 citekey 不确定，先保守匹配；匹配不稳就不要正式写入，只说明原因。
```

### S2

```text
执行 S2，对整个 Notes/ 做 lint 和低风险修复，生成今天的 lint 报告；高风险项只写报告，不直接执行。
```

### S3

```text
执行 S3，基于本周的 log、SessionNotes、Notes 更新和 CurrentQuestions 生成周报，并明确哪些结论值得保留、哪些仍不确定。
```

### S4

```text
执行 S4，citekey=he2016resnet，模式用一分钟回忆版，重点回忆我自己的理解和还没吃透的地方。
```

```text
执行 S4，citekey=vaswani2017attention，生成口头讲解版复盘，适合组会表达，并给出建议重看位置。
```

### S5

```text
执行 S5，问题是：在我的本地知识库里，local attention 和 global attention 分别出现在哪些论文里？有什么主要差别？请给我页面路径和建议重看顺序。
```

```text
执行 S5，问题是：为什么有些工作在 long-context 任务上会出现 retrieval saturation？请优先基于本地知识库回答，如果证据不足要明确说不足。
```

### S6

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论整理成标准化 SessionNote，只保留高价值内容，并设为 merge_status: pending。
```

```text
执行 S6，把当前讨论沉淀下来。先尝试匹配论文；如果匹配不稳，就放到 Management/SessionNotes/_unmatched/，不要正式改 Notes/。
```

---

## 使用建议 / 注意事项

1. 不要直接修改 `LiteratureNotes/`。
   - 这层是原始笔记层，主要用于保留阅读痕迹。

2. `citekey` 尽量总是写明。
   - 这样匹配最稳，后续更新也最方便。

3. 匹配不稳时宁可保守。
   - 不要硬归档、硬合并、硬写正式页面。

4. 新论文一般是先 `S6`，再 `S1`。
   - 先保存高价值讨论，再正式入库。

5. 周报一般是先 `S2`，再 `S3`。
   - 先整理结构，再做总结更稳。

6. 想检索本地知识时优先用 `S5`。
   - 不要直接跳过本地库去问一个泛问题。

7. 想回顾“自己当时怎么理解这篇论文”，用 `S4`。
   - 不要让 agent 直接重写一遍 abstract 来代替复盘。

8. S1 是正式入库入口，S2 是全库维护入口。
   - 不要混着用。

9. 如果你只是想先把讨论存下来，不要直接用 S1。
   - 先用 S6，后面再交给 S1。

10. 如果你不确定该用哪个 Skill，可以先问：
   - “我现在这个场景更适合用 S1、S4 还是 S5？”

---

## 一句话结尾

如果你只记住一套最常用组合，可以先记这个：

- **读论文时：S6 -> S1**
- **周维护时：S2 -> S3**
- **要回顾时：S4**
- **要检索时：S5**
- **要检查系统时：S0**

