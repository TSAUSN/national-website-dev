# QA Test Plan

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, derived from templates and endpoints in code; no environment was accessed)

The business-critical checks for this project. Agents report results against these IDs (`PASS` / `FAIL` / `BLOCKED` / `NOT RUN`, as defined in the zesty-ai `standards/qa-and-release.md`).

## How to use this plan

- **Single change:** run §1 (cross-cutting) plus every item the change touches, plus the developer's QA targets. Use `docs/templates.md` to find every page that includes a changed module, component or block.
- **Stage re-verify or broad pass:** run the whole plan, except items marked *Excluded*.
- **Production:** only the read-only checks in §7, run by a human-initiated smoke test.
- If the plan is wrong against what's live (a page moved, a field renamed), report a documentation issue, not a defect.

## Environments

| Environment | Kind | URL | Access |
| --- | --- | --- | --- |
| Branch preview | preview | `https://hr1cm7dt-{branch-name}.webengine.zesty.io/` (e.g. `https://hr1cm7dt-coda-1234.webengine.zesty.io/`) | Password-gated: append `?zpw=<preview password>` (ask the team; never write it here). **Serves the instance's dev `site.css`, not the branch's SCSS** (observed 2026-10-14, CODA-2576), so stylesheet-only changes can only be verified on stage |
| Stage (instance dev version) | staging | `https://hr1cm7dt-dev.webengine.zesty.io/` | Password-gated, same `?zpw=` parameter; never write the password here |
| Production (instance live version) | production | `https://thesalvationarmyinternaldev.zesty.dev/` | Look-only |
| Other live domain | unknown | `https://thesalvationarmy-h4zw8zcg.zesty.dev` (hard-coded in `loader:78`, `custom_head:132`; gets the Tealium `qa` profile) | Not a test target until its role is confirmed |

## 1. Cross-cutting checks (every page)

| ID | Check | How | Breakpoints |
| --- | --- | --- | --- |
| X-01 | No console errors on load (ignoring third-party noise noted in T-*) | Browser console | all |
| X-02 | Header (`components/header`), mobile nav, and footer render and links resolve | Click through | mobile, tablet, desktop |
| X-03 | Images have meaningful `alt`; heading order is valid | Snapshot / accessibility tree | desktop |
| X-04 | Skeleton loaders clear: `.main-content` becomes visible, no permanent placeholders | Load page, wait for network idle | mobile, desktop |
| X-05 | `<title>`, meta description and OG tags are filled (`custom_head`) | View source / DOM | desktop |
| X-06 | Header modals (search, find help, donate) open and close | Click | mobile, desktop |

## 2. Page-level test cases

### Core pages

| ID | Page (path) | Template | Steps | Expected | Environments |
| --- | --- | --- | --- | --- | --- |
| PG-01 | `/` | `homepage` | Load | Hero, map, services, stats and news render | preview, stage |
| PG-02 | A location page | `locations` | Load one standard and one `page_layout = simplified` location | Standard shows all sections; simplified hides the gated sections; a location with `redirect_url` redirects | preview, stage |
| PG-03 | A service page | `services` | Load a service whose `page_intro` is filled | Hero, page intro, program schedule, stats, news cards, donate render. *Known defect: the intro is missing (content-models CM-01)* | preview, stage |
| PG-04 | A story | `stories` | Load | Body, author/date, share buttons, recent stories render | preview, stage |
| PG-05 | A contact page | `contact_us` | Load; submit the form only on preview with test data | Form, map and officers render | preview |
| PG-06 | Location finder | `location_finder` | Search by ZIP and by "use my location" | Results list and map pins match | preview, stage |
| PG-07 | An informational page (`template_layout = 1` and an autolayout one) | `informational_pages` | Load both | Generic module vs layout content renders | preview, stage |
| PG-08 | A 404 path | `404-page` | Load `/does-not-exist` | 404 page with services and FAQs | preview, stage |
| PG-09 | City / state pages | `cities`, `states` | Load | Hero and ZIP search / state list render | preview, stage |
| PG-10 | Angel Tree campaign | `angel_tree_campaign` | Load | Map and campaign modules render (Quickbase-backed; see T-03) | preview, stage |

### Excluded (do not test yet)

- Test views: `gb_test`, `api_test`, `blocks_test`, `test.html` (not production pages).

## 3. Module-level test cases

| ID | Module | Where it appears | Steps | Expected |
| --- | --- | --- | --- | --- |
| M-01 | `modules/hero` | homepage, locations | Load | Slider renders and calls `hideSlider()`/`hideMain()` (loaders clear) |
| M-02 | `modules/services` | homepage, locations, services, 404 | Load | Service cards from `service-types.json` render |
| M-03 | `modules/news` / `news-cards` | homepage, locations, services | Load; use the news filter | Cards from `stories.json`; filter changes results |
| M-04 | `modules/map*` | homepage, locations, contact | Load | Map tiles and pins render |
| M-05 | `modules/programs-schedule*` | services, locations | Load | Schedule shows, or an empty state shows without errors |

## 4. Component / block library test cases

| ID | Component | Variants | Expected |
| --- | --- | --- | --- |
| B-01 | `-/block/hero_full.html` | layout variants | Renders on layout pages (it's on ~1,083 layouts: any change is high-impact) |
| B-02 | `-/block/general_accordion.html` | — | Expands and collapses |
| B-03 | `-/block/info_block.html`, `fundraising_thermometer_block.html` | — | Render with and without optional fields |

## 5. Endpoint / data checks

| ID | Endpoint | Check | Expected |
| --- | --- | --- | --- |
| E-01 | `/service-types.json` | Valid JSON, required keys present | Parses; non-empty |
| E-02 | `/services.json`, `/stories.json`, `/locations.json` | Valid JSON, empty fields still valid | Parses |
| E-03 | `/location-finder.json` | Valid JSON for a ZIP query | Parses; results have lat/long |
| E-04 | `/get-informational-page.json?zuid=<id>` | Location, division and territory owners each return content/address/city/state | Valid JSON in all three branches |
| E-05 | `/model-info.json` | Valid JSON | Parses |

### Third-party dependency checks

| ID | Dependency | Check |
| --- | --- | --- |
| T-01 | Tealium `utag.js` | Loads with the right env profile: `dev` on stage, `prod` on production (`loader:75-85` picks `qa` only for `thesalvationarmy-h4zw8zcg.zesty.dev`) |
| T-02 | Microsoft Clarity | Loads only when `instance.env == 'dev'` |
| T-03 | Quickbase (`api.quickbase.com`) | Angel Tree data loads (security flag D-06: the token is in client code) |
| T-04 | Classy donations | Donate links/forms resolve |

## 6. Open items to resolve before this plan is final

- How access to the branch preview and stage is obtained (if any is needed).
- Role of `thesalvationarmy-h4zw8zcg.zesty.dev`. It's hard-coded in `loader` and `custom_head` but isn't the production URL.
- Specific test items (ZUIDs or paths) for PG-02, PG-03, PG-04 and PG-07.
- Known field-name defects to confirm on stage, then turn into regression checks once fixed: CM-01 (services intro), CM-02 (schema `addressLocality`), CM-03 (`page_type` empty in CSV/JSON), CM-04/05 (mobile editor images). See `docs/content-models.md`.
- Pages that exist only on the instance (`stuff_the_bus`, `tags`, `informational_pages_dev`): add test cases if they're live.
- Whether the contact form may be submitted on stage.

## 7. Running against stage vs. production

- Stage shows the instance **dev** version; production shows **live**. Before a production PR merges, they're expected to differ for exactly the files listed in that PR's "Production publish preview" comment.
- Production smoke subset (read-only, human-initiated): X-01, X-02, X-04, PG-01, PG-02, PG-08, E-01, E-02, T-01.

## Review status

Not yet reviewed.
