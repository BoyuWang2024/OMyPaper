# S5 - Local Retrieval & Re-read Navigator

## 1. Skill Name

- ID: `S5`
- English: `Local Retrieval & Re-read Navigator`
- Chinese: `本地知识库检索 / 重读导航`

## 2. Purpose / Goal

S5 answers a user question by searching the local vault first and returning not just an answer, but also a **navigation plan** for where to re-read.

It is the default skill for concept, method, comparison, phenomenon, and project-tree queries that should be grounded in the local knowledge base.

S5 is primarily read-only.

## 3. Typical Use Cases

- "Where in my vault have I seen this method before?"
- "Which papers in my notes talk about this failure mode?"
- "What is the difference between two concepts according to my local notes?"
- "What is this child project trying to solve?"
- "How should I quickly get up to speed on this parent project?"

## 4. Allowed Input Forms

S5 accepts:

- a natural-language question
- one or more target keywords
- a concept or method name
- a comparison request
- an experiment phenomenon or failure mode
- a project ID or parent-child navigation request

Preferred input pattern:

```text
执行 S5，回答“问题内容”，优先基于本地知识库，给我答案、相关页面路径、为什么相关，以及建议重看顺序。
```

## 5. Must Read

S5 must read:

- `Notes/`
- `projects/`
- `Management/index.md`
- `Management/PaperRegistry.md`
- `Management/ProjectIndex.md`
- `Management/ProjectTree.md`

When needed, S5 may also read:

- relevant raw notes in `LiteratureNotes/`
- recent weekly reports
- recent review notes
- project sessions

## 6. Allowed Write Scope

S5 should normally be read-only.

It should not write to `Notes/`, `projects/`, `Management/`, or `LiteratureNotes/` unless the user explicitly asks for a separate write action outside the core S5 task.

## 7. Standard Execution Flow

1. Parse the question and identify:
   - target concept, method, experiment, or project
   - desired granularity
   - likely evidence types
2. Search local wiki pages first:
   - paper pages
   - concept pages
   - method pages
   - comparison pages
   - topic pages
3. Search the project layer when relevant:
   - `Management/ProjectTree.md`
   - `Management/ProjectIndex.md`
   - `projects/{project_id}/project.yml`
   - `文献地图.md`
   - `论文大纲.md`
4. Use `Management/index.md` and `Management/PaperRegistry.md` to resolve stable paper identities and candidate paths.
5. If wiki or project evidence is insufficient, backtrack into `LiteratureNotes/` conservatively.
6. Rank the most relevant pages and note why each one matters.
7. Produce a response that includes:
   - the question
   - most relevant page paths
   - why each page is relevant
   - corresponding papers or project nodes
   - suggested re-read order
   - conflicts or unresolved questions
8. For project questions, try to include:
   - its position in the tree
   - parent project
   - child projects
   - core problem
   - key paper logic
   - current gap
   - fastest familiarization path
9. If the local record is too thin, state that clearly and do not fill the gap with unsupported model knowledge.

## 8. Output Requirements

S5's response should aim to include:

1. the question itself
2. the top relevant local pages with exact paths
3. why each page is relevant
4. the linked papers or projects
5. recommended re-read order
6. conflicts or unresolved gaps
7. a short answer grounded in local evidence

Whenever possible, the response should say **which section to look at**, not only which file.

For project questions, the answer should additionally aim to include:

- project-tree position
- upper-level project
- lower-level projects
- shared vs. specific papers
- current project gap
- quick familiarization path

## 9. Hard Constraints / Prohibitions

- Always prioritize the local vault over model memory.
- Do not give a generic answer without file paths.
- Do not hide evidence gaps.
- Do not claim the vault says something when the vault does not actually support it.
- Do not modify `LiteratureNotes/`.
- Do not invent project structure that is not supported by local files.

## 10. Matching and Exception Rules

- If multiple pages are relevant, return a ranked list instead of pretending one page is enough.
- If the best evidence only exists in raw notes, say that the wiki or project layer is not yet mature on this topic.
- If the question is broader than the current vault supports, answer partially and mark missing evidence explicitly.
- If the project tree is incomplete, describe the visible structure and note what is missing.

## 11. Relationship With Other Skills

- S5 depends heavily on outputs from S1, S2, S3, S4, and S6.
- S5 does not replace S1; if a paper is missing from the wiki, S5 may recommend running S1.
- S5 may recommend S4 when the user needs a recall-oriented or onboarding-oriented output rather than a cross-vault retrieval answer.

## 12. Call Examples

### Example 1

```text
执行 S5，回答“在我的本地知识库里，哪些论文讨论过 exposure bias，它们各自是怎么解释的？”并给我建议重看顺序。
```

### Example 2

```text
执行 S5，问题是“请导览项目 uncertainty_llpr：它在项目树中的位置是什么、和兄弟项目有什么区别、我该先重读哪些论文？”如果证据不足，请明确说明不足。
```

## 13. Success Criteria

S5 is complete only if:

- the answer is grounded in local files
- exact relevant paths are provided
- the user gets a re-read or familiarization plan, not just a summary
- uncertainty or evidence gaps are explicit
