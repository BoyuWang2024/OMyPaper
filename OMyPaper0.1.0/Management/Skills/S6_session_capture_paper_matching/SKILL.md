# S6 - Session Capture & Paper Matching

## 1. Skill Name

- ID: `S6`
- English: `Session Capture & Paper Matching`
- Chinese: `阅读会话沉淀与论文匹配`

## 2. Purpose / Goal

S6 distills the high-value parts of a reading conversation into a standardized session note that can later be consumed by S1. It is the memory-capture layer between transient chat and durable wiki writing.

S6 must preserve only reusable insight, not raw chat transcript.

## 3. Typical Use Cases

- after a good paper discussion with the agent
- after clarifying a confusing method or experiment
- after forming a better interpretation during reading
- when the user wants to save a discussion before moving on

## 4. Allowed Input Forms

S6 accepts:

- the current conversation context
- explicit `citekey`
- explicit `zotero_key`
- explicit title
- raw session summary supplied by the user

Preferred input pattern:

```text
执行 S6，把我们刚才关于这篇论文的高价值讨论沉淀成 SessionNote；先保守匹配论文，匹配不稳就保存到 _unmatched/。
```

## 5. Must Read

S6 must read:

- the current conversation content or provided session summary
- `Management/SessionIndex.md`
- `Management/PaperRegistry.md`
- `Management/FrontmatterSpec.md`

When matching a paper, S6 should also read as needed:

- candidate literature notes in `LiteratureNotes/`
- existing paper pages in `Notes/Papers/`

## 6. Allowed Write Scope

S6 may write to:

- `Management/SessionNotes/{citekey}/`
- `Management/SessionNotes/_unmatched/`
- `Management/SessionIndex.md`
- `Management/log.md`

S6 must not write to:

- `LiteratureNotes/`
- formal wiki pages under `Notes/`

## 7. Standard Execution Flow

1. Identify whether the current discussion is about a specific paper.
2. Resolve paper identity using the fixed priority:
   - citekey
   - zotero key
   - title
   - source note / attachment path
   - current-session guess
3. Assign `match_confidence`.
4. If confidence is `low` or `unmatched`, route the session note to `_unmatched/`.
5. Extract only high-value content from the discussion:
   - user questions worth preserving
   - Codex answers worth keeping
   - updated understanding
   - still uncertain points
   - evidence anchors
   - candidate wiki updates
6. Create a standardized session note using the template and required frontmatter.
7. Set `merge_status`:
   - usually `pending` for matched notes
   - `unmatched` for unstable paper identity
   - `needs_review` when useful but risky
8. Update `Management/SessionIndex.md`.
9. Append a log entry.
10. Report the created path and whether the note is ready for S1.

## 8. Output Requirements

S6 should produce:

- one standardized session note
- one session index update
- one log entry
- a clear statement of match status and remaining uncertainty

The session note body should include:

- `My questions`
- `Codex answers worth keeping`
- `My updated understanding`
- `Still uncertain`
- `Evidence anchors`
- `Candidate wiki updates`

## 9. Hard Constraints / Prohibitions

- Do not modify `LiteratureNotes/`.
- Do not save the raw chat transcript as-is.
- Only preserve high-value material.
- Do not formally modify `Notes/` inside S6.
- If paper matching is unstable, prefer `_unmatched/` over forced assignment.

## 10. Matching and Exception Rules

- `exact`: citekey or direct source-note match
- `high`: strong metadata match with little ambiguity
- `medium`: plausible match, but one anchor is missing
- `low`: guess only; store under `_unmatched/`
- `unmatched`: no reliable paper target

Additional handling rules:

- If multiple candidate papers are plausible, use `_unmatched/`.
- If the session covers more than one paper, either create separate notes or explicitly say the note is mixed and send it to `_unmatched/`.
- If the conversation is mostly logistics with little durable insight, S6 should say no high-value session note should be created.

## 11. Relationship With Other Skills

- S6 feeds S1 directly.
- S4 can benefit from session notes created by S6.
- S3 may summarize S6 outputs in the weekly report.
- S6 does not replace S1 or S5.

## 12. Call Examples

### Example 1

```text
执行 S6，把我们刚才关于 citekey=vaswani2017attention 的讨论整理成标准化 SessionNote，只保留高价值内容，并设为 merge_status: pending。
```

### Example 2

```text
执行 S6，把当前讨论沉淀下来。先尝试匹配论文；如果匹配不稳，就放到 Management/SessionNotes/_unmatched/，不要正式改 Notes/。
```

## 13. Success Criteria

S6 is complete only if:

- a session note was created or a justified refusal was given because the discussion lacked durable value
- the note contains standardized metadata
- the paper match status is explicit
- `Management/SessionIndex.md` was updated consistently
- the user can later hand the note to S1 without additional cleanup

