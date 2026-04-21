# S5 - Local Retrieval & Re-read Navigator

## 1. Skill Name

- ID: `S5`
- English: `Local Retrieval & Re-read Navigator`
- Chinese: `本地知识库检索 / 重读导航`

## 2. Purpose / Goal

S5 answers a user question by searching the local vault first and returning not just an answer, but also a **navigation plan** for where to re-read. It is the default skill for concept, method, comparison, and phenomenon queries that should be grounded in the local knowledge base.

S5 is primarily read-only.

## 3. Typical Use Cases

- "Where in my vault have I seen this method before?"
- "Which papers in my notes talk about this failure mode?"
- "What is the difference between two concepts according to my local notes?"
- "What should I re-read to answer this question well?"

## 4. Allowed Input Forms

S5 accepts:

- a natural-language question
- one or more target keywords
- a concept or method name
- a comparison request
- an experiment phenomenon or failure mode

Preferred input pattern:

```text
执行 S5，回答“问题内容”，优先基于本地知识库，给我答案、相关页面路径和建议重看顺序。
```

## 5. Must Read

S5 must read:

- `Notes/`
- `Management/index.md`
- `Management/PaperRegistry.md`

When needed, S5 may also read:

- relevant raw notes in `LiteratureNotes/`
- recent weekly reports
- recent review notes

## 6. Allowed Write Scope

S5 should normally be read-only.

It should not write to `Notes/`, `Management/`, or `LiteratureNotes/` unless the user explicitly asks for a separate write action outside the core S5 task.

## 7. Standard Execution Flow

1. Parse the question and identify:
   - target concept or method
   - desired granularity
   - likely evidence types
2. Search local wiki pages first:
   - paper pages
   - concept pages
   - method pages
   - comparison pages
   - topic pages
3. Use `Management/index.md` and `Management/PaperRegistry.md` to resolve stable paper identities and candidate paths.
4. If wiki evidence is insufficient, backtrack into `LiteratureNotes/` conservatively.
5. Rank the most relevant pages and note why each one matters.
6. Produce a response that includes:
   - the question
   - most relevant page paths
   - why each page is relevant
   - corresponding papers
   - suggested re-read order
   - conflicts or unresolved questions
7. If the local record is too thin, state that clearly and do not fill the gap with unsupported model knowledge.

## 8. Output Requirements

S5's response should aim to include:

1. the question itself
2. the top relevant local pages with exact paths
3. why each page is relevant
4. the linked papers
5. recommended re-read order
6. conflicts or unresolved gaps
7. a short answer grounded in local evidence

Whenever possible, the response should say **which section to look at**, not only which file.

## 9. Hard Constraints / Prohibitions

- Always prioritize the local vault over model memory.
- Do not give a generic answer without file paths.
- Do not hide evidence gaps.
- Do not claim the vault says something when the vault does not actually support it.
- Do not modify `LiteratureNotes/`.

## 10. Matching and Exception Rules

- If multiple pages are relevant, return a ranked list instead of pretending one page is enough.
- If the best evidence only exists in raw notes, say that the wiki is not yet mature on this topic.
- If the question is broader than the current vault supports, answer partially and mark missing evidence explicitly.

## 11. Relationship With Other Skills

- S5 depends heavily on outputs from S1, S2, S3, and S4.
- S5 does not replace S1; if a paper is missing from the wiki, S5 may recommend running S1.
- S5 may recommend S4 when the user needs a recall-oriented output rather than a cross-vault retrieval answer.

## 12. Call Examples

### Example 1

```text
执行 S5，回答“在我的本地知识库里，哪些论文讨论过 exposure bias，它们各自是怎么解释的？”并给我建议重看顺序。
```

### Example 2

```text
执行 S5，问题是“local attention 和 global attention 在我这个库里各自出现在哪些页面，有什么主要差别？”如果证据不足，请明确说不足。
```

## 13. Success Criteria

S5 is complete only if:

- the answer is grounded in local files
- exact relevant paths are provided
- the user gets a re-read plan, not just a summary
- uncertainty or evidence gaps are explicit

