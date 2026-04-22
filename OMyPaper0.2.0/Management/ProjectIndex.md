---
type: project-index
last_updated: 2026-04-22
source_root: projects/
---

# Project Index

This file tracks project-level objects in the hierarchical project tree.

Project membership is managed only through each project's `project.yml`.
Do not write project assignment back into `LiteratureNotes/` or `Notes/Papers/`.

| project_id | title | project_type | parent_project | scope | objective | keywords | project_path | paper_counts | status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

## Notes

- `project_type` should be one of `parent`, `child`, or `standalone`.
- `parent_project` should be empty for `parent` and `standalone` projects.
- `paper_counts` is a derived summary from `project.yml`, not the source of truth.
- The source of truth for hierarchy is `projects/{project_id}/project.yml`.
