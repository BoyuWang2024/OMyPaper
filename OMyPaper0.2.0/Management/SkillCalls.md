# Skill Call Patterns

This file collects recommended invocation patterns for the current `OMyPaper` skill system.

The system is now project-aware. Calls may target:

- one paper
- one child project
- one parent project
- one hybrid discussion spanning papers and projects

## 1. Shared Calling Pattern

Recommended form:

```text
执行 Sx，目标是...，请按照对应 Skill 的规范处理，并在最后汇报创建/更新文件、精确路径和仍待确认的问题。
```

Useful target hints:

- paper: `citekey=...`
- project: `project_id=...`
- hybrid: `citekey=...` plus `project_id=...`

## 2. Routing Guide

| If the user wants to... | Prefer |
| --- | --- |
| check vault readiness, project manifests, and tree consistency | `S0` |
| compile one paper into the global wiki and get project suggestions | `S1` |
| lint `Notes/` and `projects/` structurally | `S2` |
| generate a weekly synthesis across papers and projects | `S3` |
| review one paper, one child project, or one parent project | `S4` |
| retrieve local evidence and get a re-read path | `S5` |
| capture a durable paper/project/hybrid discussion | `S6` |

## 3. S0 Call Templates

```text
执行 S0，对整个 OMyPaper 做 bootstrap 与一致性检查，补齐缺失的基础管理文件、项目索引、项目树和项目级 session 目录；如果发现可稳定识别的原始论文，也请初始化 raw-only registry 状态，并输出保守报告。
```

```text
执行 S0，只做检查不激进修复；重点看 projects/ 是否存在、project.yml 是否合法、父子关系是否成环、以及 citekey 与全局主库是否能解析。
```

## 4. S1 Call Templates

```text
执行 S1，把 citekey=vaswani2017attention 正式整理进全局 wiki；先读取 pending paper sessions，再扫描项目树，给出父项目/子项目/暂不归入项目的建议，但不要直接改任何 project.yml。
```

```text
执行 S1，目标论文标题是 “Attention Is All You Need”。请先保守匹配；如果匹配稳定，就完成全局整理，并把项目建议写入 ProjectAssignmentQueue；如果不稳定，只报告问题。
```

## 5. S2 Call Templates

```text
执行 S2，对整个 Notes/ 和 projects/ 做 lint 与低风险修复，检查 wiki 结构、项目树、project.yml、文献地图和论文大纲的一致性，并生成今日报告。
```

```text
执行 S2，重点检查父项目 uncertainty 及其子项目：看兄弟项目是否严重重叠、子项目是否偏离父项目、文献地图与论文大纲是否脱节；高风险问题只写报告。
```

## 6. S3 Call Templates

```text
执行 S3，基于本周本地文件变化生成周报；同时总结全局论文库更新、父项目推进、子项目推进、仍不确定的问题和下周优先推进的项目层级。
```

```text
执行 S3，为 2026-W17 生成周报，并明确指出下周最该推进的父项目、子项目，以及最值得复习的论文。
```

## 7. S4 Call Templates

### Paper review

```text
执行 S4，目标是 citekey=he2016resnet，模式用一分钟回忆版，帮我恢复我当时怎么理解这篇论文，并指出建议重看位置。
```

### Child-project review

```text
执行 S4，目标是子项目 uncertainty_llpr，模式用 quick-familiarization；请告诉我它在父项目中的位置、核心论文、与兄弟项目的区别、以及最快熟悉路径。
```

### Parent-project review

```text
执行 S4，目标是父项目 uncertainty，模式用 oral-explanation；请输出一个适合组会口头讲解的版本，并总结子项目分工、共通核心文献和最大缺口。
```

## 8. S5 Call Templates

### Concept / method retrieval

```text
执行 S5，问题是：在我的本地知识库里，哪些论文讨论过 exposure bias？请给我答案、对应页面路径、为什么相关，以及建议重看顺序。
```

### Project navigation

```text
执行 S5，问题是：请导览项目 uncertainty_llpr，告诉我它在项目树中的位置、上级项目、兄弟项目、核心问题、关键论文关系、当前缺口，以及如何快速熟悉它。
```

### Sibling comparison

```text
执行 S5，问题是：请比较 uncertainty_llpr 和 uncertainty_confidence_head 这两个兄弟子项目在目标、核心论文和未解决问题上的差异，并给出重读导航。
```

## 9. S6 Call Templates

### Paper session

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的关键讨论整理成标准化 paper session note，只保留高价值内容，并设为 merge_status: pending。
```

### Project session

```text
执行 S6，把我们刚才关于子项目 uncertainty_llpr 的讨论沉淀成 project session，记录 updated_project_understanding、still_uncertain 和 candidate_project_updates。
```

### Hybrid session

```text
执行 S6，把当前关于 citekey=guo2017calibration 和父项目 uncertainty 的讨论沉淀成 hybrid session；如果项目或论文匹配不稳，就保守放到对应 _unmatched/。
```

## 10. Combined Patterns

### Reading -> capture -> global ingest -> project suggestion

```text
先执行 S6，把当前阅读讨论沉淀下来；然后执行 S1，把这篇论文整理进全局 wiki，并把项目归属建议写入 ProjectAssignmentQueue。
```

### Weekly maintenance

```text
先执行 S2，检查 Notes/ 和 projects/ 的结构健康；再执行 S3，生成本周周报。
```

### Project onboarding

```text
先执行 S4，对父项目 uncertainty 做 quick-familiarization；再执行 S5，给我这个父项目及其子项目的快速熟悉阅读路径。
```

## 11. What Not To Ask One Skill To Do

- do not ask `S0` to write paper content
- do not ask `S1` to directly finalize project membership in `project.yml` by default
- do not ask `S2` to deeply rewrite paper semantics
- do not ask `S3` to invent conclusions unsupported by local files
- do not ask `S4` to replace formal wiki construction
- do not ask `S5` to answer from generic model memory without local paths
- do not ask `S6` to dump raw transcript or directly edit wiki/project manifests
