# S4 - Paper Recall & Review Assistant

## 1. Skill Name

- ID: `S4`
- English: `Paper Recall & Review Assistant`
- Chinese: `论文复盘`

## 2. Purpose / Goal

S4 helps the user quickly reconstruct **their own understanding** of a paper. The emphasis is not on rephrasing the abstract, but on recovering how the user read it, what they focused on, and what still felt unclear.

## 3. Typical Use Cases

- before a meeting or interview
- before returning to a paper after a long gap
- before comparing one paper with another
- when the user wants a self-test or quick recall drill

## 4. Allowed Input Forms

S4 accepts:

- `citekey` plus a mode
- paper title plus a mode
- explicit path to the paper wiki or literature note

Supported modes:

- `一分钟回忆版`
- `五分钟深入版`
- `口头讲解版`
- `自测提问版`

Preferred input pattern:

```text
执行 S4，citekey=yourcitekey，生成五分钟深入版复盘，重点回忆我自己的理解和还没吃透的地方。
```

## 5. Must Read

S4 must read:

- `LiteratureNotes/{citekey}.md`
- `Notes/Papers/{citekey}.md`
- related concept pages
- related method pages
- relevant comparison pages
- relevant session notes under `Management/SessionNotes/{citekey}/`

## 6. Allowed Write Scope

S4 may write to:

- `Management/ReviewNotes/{citekey}-review-YYYYMMDD.md`
- `Management/log.md`

S4 should not modify the formal wiki by default.

## 7. Standard Execution Flow

1. Resolve the paper identity conservatively.
2. Read the paper wiki first to understand the current durable summary.
3. Read the raw literature note to recover the user's original reading trace.
4. Read relevant session notes to recover later clarification or reinterpretation.
5. Read closely related concept/method/comparison pages when they sharpen recall.
6. Generate the chosen review mode:
   - one-minute: compressed memory refresh
   - five-minute: richer structure and caveats
   - oral explanation: suitable for speaking in a meeting or interview
   - self-test: question-driven recall prompts
7. Explicitly note:
   - what the user seems to have understood well
   - what still appears shaky
   - where to re-read
8. Save the review note.
9. Append a log entry.

## 8. Output Requirements

The review output should include:

- a recall summary
- the user's likely understanding
- still-not-internalized parts
- suggested re-read locations
- self-test questions where relevant

## 9. Hard Constraints / Prohibitions

- The core job is recall, not paper rewriting.
- Prioritize `My note` and session-derived understanding over abstract paraphrase.
- Do not package new speculation as if it were the user's prior understanding.
- Do not ignore uncertainty.
- Do not modify `LiteratureNotes/`.

## 10. Matching and Exception Rules

- If the paper wiki does not exist yet, S4 may still generate a weaker review from raw notes and session notes, but it should say that the durable wiki is missing.
- If the literature note is thin and there are no sessions, S4 should be honest that recall quality will be limited.
- If the match is unstable, stop and ask for a more precise paper identifier rather than producing a wrong review note.

## 11. Relationship With Other Skills

- S4 depends on outputs from S1 and S6 whenever possible.
- S5 may be used after S4 when the user wants deeper topic navigation.
- S3 may reference review notes as part of weekly progress.

## 12. Call Examples

### Example 1

```text
执行 S4，citekey=he2016resnet，模式用一分钟回忆版，帮我快速想起我到底怎么理解这篇论文。
```

### Example 2

```text
执行 S4，目标论文是 "Attention Is All You Need"，模式用口头讲解版，并指出建议重看位置和我还没真正吃透的部分。
```

## 13. Success Criteria

S4 is complete only if:

- the target paper was matched correctly
- the review note file exists
- the note reflects the user's understanding rather than only paper prose
- uncertain or weakly remembered parts are explicit
- suggested re-read locations are given

