---
type: project-assignment-queue
last_updated: 2026-04-22
default_status: pending_user_confirmation
---

# Project Assignment Queue

This queue stores project-placement suggestions produced by `S1`.

Default rule:

- `S1` may suggest project placement.
- `S1` must not write directly into any `project.yml` unless the user explicitly requests a follow-up action.
- Confirmed placement should later be written into the target `project.yml`, not into `LiteratureNotes/` or `Notes/Papers/`.

| queue_id | citekey | paper_wiki | suggested_outcome | suggested_path | candidate_parent | candidate_child | suggested_bucket | relevance | theme | role | priority | outline_sections | status | source | notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Suggested Outcome Legend

- `parent_child`: recommend a parent project plus a child project
- `parent_only`: recommend a parent project but no child project yet
- `none`: recommend keeping the paper outside the project tree for now
- `needs_review`: project fit is too weak or too ambiguous for a safe suggestion

## Status Legend

- `pending_user_confirmation`: waiting for the user to confirm the suggested placement
- `confirmed_parent_child`: confirmed for both parent and child project
- `confirmed_parent_only`: confirmed for parent project only
- `confirmed_none`: explicitly keep this paper outside the project tree for now
- `rejected`: suggestion rejected and should not be auto-reapplied without new evidence
- `needs_review`: useful suggestion exists, but reasoning or project identity is still too weak
