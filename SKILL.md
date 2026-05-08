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
   - **Blocked-state linkage** — If an issue is in `blocked` status, verify it has machine-actionable blocker linkage (see policy below).
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

## Blocked-State Policy

Before setting an issue to `blocked`, or when triaging an issue already in `blocked` status, machine-actionable linkage is **required**. Free-text "blocked by X" comments without structured linkage break automated unblock wakes and prevent dependent work from resuming.

### Required linkage (one of)

1. **Issue dependency** — set `blockedByIssueIds` (array of Paperclip issue IDs) on the issue:
   ```json
   PATCH /api/issues/{issueId}
   { "blockedByIssueIds": ["<blocking-issue-id>"] }
   ```
   When all blocking issues reach `done`, Paperclip automatically fires `issue_blockers_resolved` and wakes the assignee.

2. **External blocker metadata** — post a structured comment with all four fields:
   - **Blocker**: what is blocking (system, team, third-party dependency)
   - **Owner**: the named person or team responsible for resolution
   - **Expected resolution**: absolute date (e.g., `2026-05-15`)
   - **Unblock condition**: the specific state or action that will clear the block

### Triage actions for non-compliant blocked issues

- If blocked with only free-text: flag as `needs-info` and post a comment requesting the assignee supply `blockedByIssueIds` or add the structured external-blocker metadata.
- If blocked with no comment at all: escalate in the triage summary under "Issues requiring human decision".

### Reference

See [TEC-1087](/TEC/issues/TEC-1087) for the platform rule that this skill enforces.

## Example Invocation

```
/issue-triage all --project deltek-developer-dashboard
/issue-triage TEC-85
```

---

*TEC Custom Skill — maintained by the Deltek Technical Services Engineering team.*