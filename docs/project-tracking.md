# Project Tracking

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, from user-supplied details; Coda not accessed)

How work is tracked for this project. The tracker is the source of truth for task status. This repository does not sync with it: `integrations.tasks` in `.claude/project.yaml` is `provider: coda`, `enabled: false` until a doc ID is configured.

## 1. What the tracker is for

Coda. Tickets are identified as `CODA-####`. The doc and workspace names are not yet recorded.

## 2. Structure

Unknown. Record the main pages and tables (and the tasks table name for `integrations.tasks.table`) here.

## 3. Tasks

| Field | Meaning | Values |
| --- | --- | --- |
| ID | Ticket identifier, used as the branch name and task label | `CODA-1234` → branch `coda-1234` |
| Status | Workflow state | Unknown; record the Coda status values |
| Acceptance criteria | What QA verifies | Unknown; record which Coda field holds them |

## 4. Teams and projects

Unknown.

## 5. SOPs to know before filing or triaging

- One branch per ticket. Never combine tickets on one branch.
- Agents draft status updates. They never post to Coda without explicit authorization (see `zesty-ai:task-management`).

## 6. Access

Unknown. Record who has access and how to request it; no credentials.

## How this relates to this repository

- Branch naming: `coda-<number>`, lowercase (`CODA-1234` → `coda-1234`), cut from `origin/stage` with `--no-track`. The branch name also forms the preview URL: `https://hr1cm7dt-coda-1234.webengine.zesty.io/`.
- Where acceptance criteria live: the Coda ticket; copy them into the PR description.
- Which IDs may appear where: ticket IDs in branch names, commits and PRs, never in shipped files (`webengine/**`) or customer-facing notes.

## Open Questions / Flags

- Coda doc ID and tasks table name (needed to enable `integrations.tasks`).
- Status values and which field holds acceptance criteria.

## Review status

Not yet reviewed.
