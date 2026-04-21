# Skill Call Guide

## Purpose

This file gives direct, reusable call patterns for the seven skills in this vault. The goal is to make future invocation consistent, short, and unambiguous.

## Minimal Routing Guide

| If you want to... | Use |
| --- | --- |
| check whether the vault is ready to run and initialize missing files | S0 |
| turn one paper into a formal wiki page | S1 |
| inspect the whole wiki for structural issues | S2 |
| generate a weekly synthesis of reading and vault updates | S3 |
| quickly recall how you understood a paper | S4 |
| answer a concept/method question from local knowledge first | S5 |
| preserve a useful discussion for later paper ingest | S6 |

## Shared Calling Pattern

When possible, calls should explicitly provide:

- the target skill ID
- the citekey if known
- the desired output mode if relevant
- the scope if it is not the whole vault
- any special caution, such as "do not create new concept pages unless necessary"

## S0 Call Templates

### Use Cases

- first-time setup
- after moving files manually
- after adding many imported notes

### Example Calls

```text
执行 S0，对当前 OMyPaper vault 做一次 bootstrap 与一致性检查，补齐缺失管理文件，并输出报告。
```

```text
执行 S0，只检查 LiteratureNotes / Notes / Management 的结构一致性，不做激进修复，把冲突写进报告。
```

## S1 Call Templates

### Example Calls

```text
执行 S1，把 citekey=vaswani2017attention 正式编译进本地 wiki。优先读取 pending session notes，显式区分 paper claim / my note / LLM synthesis。
```

```text
执行 S1，目标论文标题是 "Attention Is All You Need"。如果 citekey 不确定，先保守匹配；匹配不稳就停止正式写入并说明原因。
```

### Good Extra Constraints

- `只在必要时新增 Concepts/Methods 页`
- `不要把 session 里的猜测写成论文事实`
- `把待核实内容单独标注`

## S2 Call Templates

### Example Calls

```text
执行 S2，对整个 Notes/ 做 lint 和低风险修复，生成今天的 lint 报告，并更新 management index。
```

```text
执行 S2，只检查 Concepts 和 Methods，优先找重复页、孤儿页、断链和缺失 comparison 页；高风险项只写报告，不直接执行。
```

## S3 Call Templates

### Example Calls

```text
执行 S3，基于本周的 log、SessionNotes、Notes 更新和 CurrentQuestions 生成周报。
```

```text
执行 S3，为 2026 年第 17 周生成周报。不要写成流水账，要明确哪些结论值得保留、哪些仍不确定、哪些论文值得二刷。
```

## S4 Call Templates

### Example Calls

```text
执行 S4，citekey=he2016resnet，模式用一分钟回忆版，重点回忆我自己的理解和还没吃透的地方。
```

```text
执行 S4，针对 citekey=vaswani2017attention 生成口头讲解版复盘，适合组会表达，并给出建议重看位置。
```

### Supported Modes

- `一分钟回忆版`
- `五分钟深入版`
- `口头讲解版`
- `自测提问版`

## S5 Call Templates

### Example Calls

```text
执行 S5，回答“local attention 和 global attention 在这个库里分别出现在哪些论文里，有什么主要差别？”优先使用本地知识库，并给我重看顺序。
```

```text
执行 S5，问题是“为什么有些工作在 long-context 任务上会出现 retrieval saturation？”请给我最相关页面、对应论文和建议重看位置；如果本地证据不够，要明确说明。
```

### Expected Output Shape

S5 的标准回答应尽量包含：

1. 问题本身
2. 最相关页面路径
3. 每个页面为什么相关
4. 对应论文
5. 建议重看顺序
6. 可能存在的冲突或未解决问题

## S6 Call Templates

### Example Calls

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论沉淀成 SessionNote，只保留高价值内容，并设为 merge_status: pending。
```

```text
执行 S6，把当前讨论整理成标准化 SessionNote。先尝试匹配论文；如果匹配不稳，保存到 _unmatched/，不要正式改 Notes/。
```

### Good Extra Constraints

- `保留 evidence anchors`
- `只沉淀可复用判断，不保存流水账`
- `把仍不确定的内容单列`

## Recommended Combined Calls

### Reading Session -> Durable Wiki

```text
先执行 S6，保存当前高价值讨论；然后执行 S1，把这篇论文正式编译进 wiki。
```

### Weekly Maintenance

```text
先执行 S2，做结构 lint 和低风险修复；再执行 S3，输出本周周报。
```

### Recall Before Meeting

```text
先执行 S4，生成口头讲解版复盘；如果我继续追问某个概念，再执行 S5 做本地导航和重看建议。
```

## What Not To Ask A Skill To Do

- 不要要求 S0 整理单篇论文内容。
- 不要要求 S1 顺手做全库大扫除。
- 不要要求 S2 深度重写某篇论文页。
- 不要要求 S3 编造本地不存在的结论。
- 不要要求 S4 代替正式 wiki 写作。
- 不要要求 S5 偷偷用常识补本地证据缺口。
- 不要要求 S6 直接修改 `Notes/` 正式页面。

