# Project Changelog

Status: Draft — pending review

Notable changes to templates, content models, integrations, and configuration in this repository, based on what is actually committed. Maintained by the `documentation` agent after a task completes; newest entries first. Customer-facing release notes are written separately from this log.

Entry format:

    ## YYYY-MM-DD: <short title>
    - Ticket: <ID>          Branch/PR: <branch> / <PR link>
    - Changed: <models, templates, modules, integrations, config>
    - By: <human and/or agent>        Verified by: <spec-auditor, sanity-check, qa, reality-check, human>
    - Promotion: <feature branch | development | stage | production (date)>
    - Notes: <migrations, follow-ups, known issues>

<!-- entries below, newest first -->

## 2026-10-14: Content models verified against the instance (docs)
- Ticket: none          Branch/PR: not yet committed
- Changed: `docs/content-models.md` rebuilt from read-only API lookups (66 models with ZUIDs, fields, types, required flags and links); `templates.md`, `custom-patterns.md` (D-01, D-05, new D-07) and `api-tools.md` updated with view → model links and repo/instance drift
- By: Claude (zesty-ai:project-discovery)        Verified by: human review pending
- Promotion: working tree only
- Notes: 5 confirmed field-name mismatches in views (content-models CM-01 to CM-05); 2 stale view ZUIDs (`volunteer_pages`, `services_seed_content`) would fail a deploy that touches them

## 2026-10-14: zesty-ai project onboarding (drafts)
- Ticket: none          Branch/PR: not yet committed
- Changed: added `.claude/project.yaml`, `CLAUDE.md`, `docs/*` (drafts)
- By: Claude (zesty-ai:project-discovery)        Verified by: `validate.mjs` (config PASS); human review pending
- Promotion: working tree only
- Notes: review the open questions in each doc

## 2026-09-04: Informational page clone work (feature branch)
- Ticket: unknown          Branch/PR: `informational-re` / unknown
- Changed: informational page clone model target and endpoint, relationship counts, header setup timing; syndication changes reverted (`93709c7`..`83f3b78`)
- By: Kharl        Verified by: unknown
- Promotion: feature branch only (8 commits ahead of `stage`)

## 2026-09-04: Informational clone navigation (stage, production)
- Ticket: unknown          Branch/PR: direct history on `stage` (`eff496c`..`cd6ce7c`)
- Changed: one-to-many informational clone navigation added and then reverted; relationship-count fix; clone links endpoint renamed; bot ZUID mappings (`6d9179c`, `ca4ceb2`)
- By: Kharl, github-actions[bot]        Verified by: unknown
- Promotion: `stage` and `production` (both at `cd6ce7c`)

## 2026-09-03: Sync from instance
- Ticket: unknown          Branch/PR: `6390473` "SYNC"
- Changed: 406 files under `webengine/views/z/` plus sync script updates (diff-scoped sync, publish-only production, PR report)
- Promotion: `stage`, `production`
- Notes: brought platform-owned layouts into git (`docs/custom-patterns.md` D-01)

## 2026-06-23: Repository initialised
- Changed: `eb43c2d` init (views, styles, scripts, `zesty.config.json`); `6659d01` GitHub Actions setup (the last commit on `main`, now retired)
- By: Kharl
