# API Tools

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, read from `scripts/sync-to-zesty.js` and the zesty-ai plugin; GET endpoints verified with the plugin helper against `8-b28282a4fe-dszqwn` on 2026-10-14)

The API clients and MCP tools that agents can use on this project, with what each one has been verified to do. Credentials are referenced by environment variable **name** only.

## What this is

Agents can always read live data through the zesty-ai plugin's GET-only helper (`scripts/zesty-api.mjs`; see the `zesty-ai:zesty-api` skill). This project also has its own Instances API client, the deploy script, which **writes** and is human/CI-only except for its report mode.

- `scripts/sync-to-zesty.js`: Node 20 script, no dependencies, uses `fetch`. Read/write. Safe for agents only with `--report` or `--dry-run`.
- zesty-ai plugin Playwright MCP (`plugin:zesty-ai:playwright`): browser for QA. On Windows it needs `ZESTY_AI_BROWSER_PATH` set to a Chrome executable.
- Postman CLI is installed on the developer machine for manual API work. There are no collections in this repo.

## Architecture (as implemented)

- The sync script runs in GitHub Actions (`.github/workflows/zesty-deploy.yml`, `zesty-pr-preview.yml`) on `ubuntu-latest`. It reads `ZESTY_INSTANCE_ZUID` and `ZESTY_TOKEN` from the environment. **The committed workflows still set `ZESTY_DEVELOPER_TOKEN` and need updating to set `ZESTY_TOKEN`** (human-only).
- Base URL: `https://<ZESTY_INSTANCE_ZUID>.api.zesty.io/v1`, `Authorization: Bearer <token>` (`scripts/sync-to-zesty.js:107-122`).

## Auth

| Variable              | Used for                                                                   | Where it is set                      |
| --------------------- | -------------------------------------------------------------------------- | ------------------------------------ |
| `ZESTY_INSTANCE_ZUID` | Instance (`8-b28282a4fe-dszqwn`) for the sync script and plugin helper     | `.env` (gitignored), CI secret       |
| `ZESTY_TOKEN`         | Auth for the sync script and the zesty-ai plugin read-only helper          | `.env` (gitignored), CI secret       |

### Running the helper

```bash
# From the repo root. Git Bash rewrites "/content/..." into a Windows path, so disable that:
MSYS_NO_PATHCONV=1 node "<plugin>/scripts/zesty-api.mjs" /content/models --pick ZUID,name,type
MSYS_NO_PATHCONV=1 node "<plugin>/scripts/zesty-api.mjs" /verify --api auth      # token check
```

Output over 100 KB is cut off unless you pass `--max-bytes`. The `/web/views` list needs about 3 MB.

## Tool inventory

### Instances API: content models (read-only, plugin helper)

| Tool | Endpoint | Inputs | Output (verified shape) | Verified on | Status |
| --- | --- | --- | --- | --- | --- |
| Verify token | `GET /verify` (`--api auth`) | — | `{ message: "Session valid", status: "OK", data: <user id> }`; an expired token returns 401 "Invalid or expired developer token" | 2026-10-14 | Approved for agents |
| List models | `GET /content/models` | — | `{ data: [{ ZUID, masterZUID, parentZUID, name, label, description, type, metaTitle, metaDescription, metaKeywords, sort, listed, createdAt, updatedAt, … }] }`; 66 models | 2026-10-14 | Approved for agents |
| Model fields | `GET /content/models/{zuid}/fields` | model ZUID | `{ data: [{ ZUID, contentModelZUID, name, label, description, datatype, sort, required, relationship, options, settings, relatedModelZUID, relatedFieldZUID, … }] }` | 2026-10-14 (all 66) | Approved for agents |
| List views | `GET /web/views` | — | `{ data: [{ ZUID, fileName, type, contentModelZUID, … }] }` (only these fields were requested; `code`/`version` unverified); 9,604 records for 4,802 unique ZUIDs (several versions per view). Use `--pick ZUID,fileName,type,contentModelZUID` | 2026-10-14 | Approved for agents |

### Instances API: web resources (used by `scripts/sync-to-zesty.js`)

| Call             | Endpoint                                                     | Inputs                      | Output (as the code expects it)                       | Verified on                | Status                 |
| ---------------- | ------------------------------------------------------------ | --------------------------- | ----------------------------------------------------- | -------------------------- | ---------------------- |
| List resources   | `GET /web/{views\|stylesheets\|scripts}?status={dev\|live}`  | status                      | `{ data: [{ ZUID, code, version }] }`                 | Not verified               | Draft — pending review |
| Get one resource | `GET /web/{endpoint}/{zuid}?status={dev\|live}`              | zuid, status                | `{ data: { ZUID, code, version } }` (array tolerated) | Not verified               | Draft — pending review |
| Save to dev      | `PUT /web/{endpoint}/{zuid}`                                 | `{ code }`                  | 2xx                                                   | Used by CI on `stage`      | **Write: CI only**     |
| Create           | `POST /web/{endpoint}`                                       | `{ code, fileName, type? }` | `{ data: { ZUID } }`                                  | Used by CI on `stage`      | **Write: CI only**     |
| Publish version  | `POST /web/{endpoint}/{zuid}/versions/{n}?purge_cache=true`  | version number              | 2xx                                                   | Used by CI on `production` | **Write: CI only**     |
| Create + publish | `PUT /web/{endpoint}/{zuid}?action=publish&purge_cache=true` | `{ code }`                  | 2xx                                                   | Only via `MANUAL_PUBLISH`  | **Write: human only**  |

### Third-party APIs called from the browser

| API                                                         | Where                                                | Notes                                                           |
| ----------------------------------------------------------- | ---------------------------------------------------- | --------------------------------------------------------------- |
| Quickbase `POST https://api.quickbase.com/v1/records/query` | `modules/map-angel-tree:344`, `angel-tree-script:37` | Token hardcoded in client code (`docs/custom-patterns.md` D-06) |
| Mobile editor app `salvationarmy.mobile.zesty.io`           | `loader:274`                                         | Links only                                                      |

## Needs live verification

- `GET /web/{endpoint}/{zuid}?status=dev|live` (single resource) and the `?status=` filter on the list call, which the sync script relies on. Not called yet.

## Open Questions / Flags

- The workflows must pass `ZESTY_TOKEN` to the sync script (they currently pass `ZESTY_DEVELOPER_TOKEN`), or the deploy fails.
- Should agents use a separate read-only token rather than the developer token that can also write?

## Review status

Not yet reviewed.
