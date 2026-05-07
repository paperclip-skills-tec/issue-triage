---
name: issue-triage
description: Triage Paperclip backlog issues for the Deltek Developer Dashboard — assess priority, assign labels, identify duplicates, and recommend next actions.
---

# Issue Triage

Triage Paperclip backlog issues to maintain a healthy, prioritised backlog.

## Purpose

Systematically review backlog issues to ensure they are properly prioritised, labelled, and actionable. Reduces noise and keeps the team focused on high-impact work.

## Inputs

- **Scope** — `all` (full backlog sweep) or a specific Paperclip issue ID (e.g., `TEC-xxx`)
- **Filters** (optional) — status, project, or priority filters to narrow the triage set

## Workflow

1. **Fetch backlog** — Query Paperclip for issues with `status=backlog` or `status=todo` in the target project.
2. **Assess each issue:**
   - **Clarity** — Is the title and description clear enough to act on? Flag vague issues for refinement.
   - **Priority** — Recommend `critical`, `high`, `medium`, or `low` based on impact and urgency.
   - **Duplicates** — Search for similar issues by title/description. Flag potential duplicates with links (e.g., "Possible duplicate of [TEC-42](/TEC/issues/TEC-42)").
   - **Dependencies** — Identify issues that block or are blocked by others.
   - **Assignability** — Can this be picked up now, or does it need scoping first?
3. **Recommend actions** — For each issue, suggest one of:
   - `ready` — Well-scoped, can be assigned immediately.
   - `needs-scoping` — Requires requirements breakdown (invoke `/requirements-scoping`).
   - `needs-info` — Missing context; comment asking the creator for clarification.
   - `duplicate` — Close with link to the original issue.
   - `defer` — Low priority; move to backlog with a note.
4. **Update issues** — Apply recommended priority and post triage comments on each issue.
5. **Summarise** — Produce a triage report listing actions taken and issues requiring human decision.

## Outputs

- Triage comments posted on each reviewed issue
- Priority updates applied where appropriate
- Summary report with:
  - Issues triaged (count and links)
  - Actions taken per issue
  - Issues requiring human decision

## Example Invocation

```
/issue-triage all --project deltek-developer-dashboard
/issue-triage TEC-85
```
