# S4 - Paper Recall & Review Assistant

## 1. Skill Name

- ID: `S4`
- English: `Paper Recall & Review Assistant`
- Chinese: `论文复盘`

## 2. Purpose / Goal

S4 helps the user quickly reconstruct **their own understanding** of a target object.

The upgraded target types are:

- one paper
- one child project
- one parent project

The emphasis is not on rephrasing an abstract. It is on recovering how the user understood the object, what they focused on, what still felt unclear, and what to revisit next.

## 3. Typical Use Cases

- before a meeting or interview
- before returning to a paper after a long gap
- before onboarding into a child or parent project
- before comparing one project branch with another
- when the user wants a self-test or quick recall drill

## 4. Allowed Input Forms

S4 accepts:

- `citekey` plus a mode
- paper title plus a mode
- `project_id` plus a mode
- explicit path to the paper wiki, literature note, or project directory

Supported modes:

- `one-minute`
- `five-minute`
- `oral-explanation`
- `self-test`
- `quick-familiarization`

Preferred input pattern:

```text
执行 S4，目标是 your_target，生成 five-minute 或 quick-familiarization 版本，重点回忆我自己的理解、当前缺口和建议重看路径。
```

## 5. Must Read

For paper review, S4 must read:

- `LiteratureNotes/{citekey}.md`
- `Notes/Papers/{citekey}.md`
- related concept pages
- related method pages
- relevant comparison pages
- relevant paper sessions under `Management/SessionNotes/{citekey}/`

For project review, S4 must read:

- `projects/{project_id}/project.yml`
- `projects/{project_id}/文献地图.md`
- `projects/{project_id}/论文大纲.md`
- relevant project sessions under `Management/ProjectSessionNotes/{project_id}/`
- `Management/ProjectTree.md`
- linked paper pages in `Notes/Papers/`

When reviewing a parent project, S4 should also read:

- child project manifests
- child literature maps or outlines when needed

## 6. Allowed Write Scope

S4 may write to:

- `Management/ReviewNotes/{citekey}-review-YYYYMMDD.md`
- `Management/ReviewNotes/project-{project_id}-review-YYYYMMDD.md`
- `Management/log.md`

S4 should not modify the formal wiki or project manifests by default.

## 7. Standard Execution Flow

1. Resolve the target identity conservatively.
2. Identify whether the target is:
   - paper
   - child project
   - parent project
3. Read the highest-value durable summary first:
   - paper wiki for papers
   - project manifest plus literature map / outline for projects
4. Read the raw / session evidence that recovers the user's own understanding:
   - raw literature note and paper sessions for papers
   - project sessions and linked paper pages for projects
5. Read closely related concept, method, comparison, or sibling-project pages when they sharpen recall.
6. Generate the chosen review mode:
   - one-minute: compressed memory refresh
   - five-minute: richer structure and caveats
   - oral explanation: suitable for speaking in a meeting or interview
   - self-test: question-driven recall prompts
   - quick-familiarization: fastest path to become usable on the object again
7. For paper review, explicitly note:
   - what the user seems to have understood well
   - what still appears shaky
   - where to re-read
8. For child-project review, explicitly include:
   - its position under the parent project
   - how it differs from sibling projects
   - shared papers inherited from the parent project
   - its own core papers
   - its current largest unresolved problem
9. For parent-project review, explicitly include:
   - project goal
   - tree position
   - child projects and what each child tries to solve
   - common core papers
   - child-specific papers
   - the largest gap across the whole direction
10. Always include a `quick familiarization path` when the target is a project:
    - which parent-project papers to review first
    - which child-project papers to review next
    - what sections to focus on
    - which papers can wait
11. Save the review note.
12. Append a log entry.

## 8. Output Requirements

The review output should include:

- a recall summary
- the user's likely understanding
- still-not-internalized parts
- suggested re-read locations
- self-test questions where relevant

For projects it should also include:

- project-tree position
- parent / child / sibling context where relevant
- common versus project-specific papers
- the current biggest gap
- a quick familiarization path

## 9. Hard Constraints / Prohibitions

- The core job is recall and orientation, not paper or project rewriting.
- Prioritize `My note` and session-derived understanding over abstract paraphrase.
- Do not package new speculation as if it were the user's prior understanding.
- Do not ignore uncertainty.
- Do not modify `LiteratureNotes/`.
- Do not edit `project.yml` during default S4.

## 10. Matching and Exception Rules

- If the paper wiki does not exist yet, S4 may still generate a weaker review from raw notes and session notes, but it should say that the durable wiki is missing.
- If project files are thin, S4 should be honest that recall quality will be limited.
- If the match is unstable, stop and ask for a more precise paper or project identifier rather than producing a wrong review note.
- If a parent project has ambiguous child boundaries, surface that as part of the review rather than silently flattening the tree.

## 11. Relationship With Other Skills

- S4 depends on outputs from S1 and S6 whenever possible.
- S5 may be used after S4 when the user wants deeper topic or project navigation.
- S3 may reference review notes as part of weekly progress.

## 12. Call Examples

### Example 1

```text
执行 S4，citekey=he2016resnet，模式用 one-minute，帮我快速想起我到底怎么理解这篇论文。
```

### Example 2

```text
执行 S4，目标是父项目 uncertainty，模式用 oral-explanation，并指出建议重看路径、子项目分工和当前整个方向最大的缺口。
```

## 13. Success Criteria

S4 is complete only if:

- the target paper or project was matched correctly
- the review note file exists
- the note reflects the user's understanding rather than only source prose
- uncertain or weakly remembered parts are explicit
- suggested re-read locations or quick familiarization paths are given
