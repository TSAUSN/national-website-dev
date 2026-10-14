# Templates & Views

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, automated scan of `{{ include }}`, `block()` and `fetch()` usage on `production` @ `cd6ce7c`; view → model links verified with a read-only `GET /v1/web/views` on 2026-10-14)

What each view renders, which content model it reads, and **where it is included**, so new work reuses existing pieces instead of duplicating them.

## How views are stored in this repository

- `webengine/views/<path>`: the path (minus `webengine/views/`) is the Zesty view name. `zesty.config.json` maps each one to a ZUID and type (`templateset`, `pageset`, `snippet`, `block`, `ajax-json`, `ajax-html`, `404`). Never hand-edit the mapping.
- Extension-less files are Parsley snippets/pages; `*.json` / `*.html` are endpoint views; `-/block/*.html` is the block library.
- `z/` (1,096 files) is **platform-owned** Zesty Layout-editor output. It is excluded from this doc; never edit it.
- Counts outside `z/`: 221 views (5 templatesets, 18 pagesets, 105 snippets, 21 blocks, 70 JSON endpoints, 1 404, 1 unmapped).

## Global layout & head

| View | Purpose | Included by | Notes |
| --- | --- | --- | --- |
| `loader` | Site-wide wrapper: header, skeleton loaders, `{{ current_view }}`, footer, Tealium `utag.js`, Clarity (dev only) | Platform (global layout) | Body stays hidden until `hideMain()` runs (`docs/custom-patterns.md` P-04) |
| `custom_head` | `<title>`, meta/OG tags, includes `modules/client-global-navigation`, `modules/indexdb`, `modules/schema` | Platform (head) | Reads `seo_meta_title`, `_meta_description` |
| `404-page` | Not-found page: header, `hero-full-404`, `services`, `faqs`, footer | Platform (404) | Includes header/footer itself |

## Page-type templates

`pageset` = rendered for each item of its model; `templateset` = a single parent page. The view name usually matches the model name; exceptions are noted. Model ZUIDs and fields: `docs/content-models.md`.

| View | Type | Renders | Includes | Notes |
| --- | --- | --- | --- | --- |
| `homepage` | templateset | Home page | `modules/hero`, `map`, `services`, `stats`, `news` | |
| `locations` | pageset | A local Salvation Army location (corps, center, store) | `hero`, `map-with-info`, `services`, `stats`, `news-cards`, `community-leaders`, `programs-schedule-landing`, `service-wy-landing` | Sections vary with `page_layout` (`simplified` hides some); honours `redirect_url` |
| `services` | pageset | A service/program page | `hero-full-donate`, `service-wy`, `services`, `programs-schedule`, `stats-modules`, `news-cards`, `donate`, `become-a-member`, `benefits-perks` | Uses `service_type`, `parent_zuid`. Reads `pageintro`, but the field is `page_intro` (content-models CM-01) |
| `stories` | pageset | News story / article | `recent-stories`, `/components/socials-share` | 477 lines of its own markup |
| `contact_us` | pageset | Contact page per location/division/territory | `contact-form`, `contact-info`, `contact-us`, `contact-us-officers`, `faqs`, `map-contact`, `map-with-info-contact` | |
| `about_us` | pageset | About page | `hero-full`, `image-button-text-full`, `leadership-list`, `local-needs`, `stats` | |
| `leadership_landing_page` | pageset | Leadership listing | `hero-full-leadership`, `leadership-list` | |
| `events_landing_pages` | pageset | Events listing | `hero-full-events`, `upcoming-events` | |
| `stories_landing_page` | pageset | News archive | `news-archive` | |
| `stories_new` | templateset | All-stories archive | `all-stories-archive` | Renders model `all_stories` (name differs) |
| `informational_pages` | pageset | Informational page | `modules/generic` when `template_layout == 1`, else `{{this.autolayout}}` | Current work on `informational-re` |
| `cities` | pageset | City page | `hero-full-cities`, `zip-search` | |
| `states` | pageset | State page | `hero-full-state`, `state-list` | |
| `angel_tree_campaign` | templateset | Angel Tree campaign | `hero-full-angel-tree`, `angel-tree-wy`, `angel-tree-wy-2`, `map-angel-tree` | See security flag D-06 |
| `location_finder` | templateset | Location finder | `location-finder` | |
| `country`, `divisions`, `territories`, `properties_types` | pageset | Layout-driven pages | `{{this.autolayout}}` only | Content comes from Zesty layouts in `z/pvl/` |
| `volunteer_pages`, `services_seed_content` | pageset | Layout-driven pages | `{{this.autolayout}}` only | **Repo ZUID is stale**: the instance has these views under different ZUIDs (`docs/custom-patterns.md` D-07). `services_seed_content` is a dataset model |
| `informational_page_clone` | pageset | Layout-driven page | `{{this.autolayout}}` only | **Not on the instance; no model by this name.** The instance has `informational_pages_dev` |
| `service_area` | pageset (unmapped) | Service-area page (model `service_areas`) | `hero-full-service-area`, `service-area-body-details`, `services-service-area` | Exists on the instance; missing from `zesty.config.json` (D-05) |
| `blocks_test` | templateset | Test page | `{{this.autolayout}}` | Not on the instance (D-02) |

### Views that exist only on the instance

Built or edited in the Zesty admin and never pulled into git. Agents can't see or review these from the repo.

| View | Type | Model |
| --- | --- | --- |
| `stuff_the_bus` | templateset | `stuff_the_bus` (also `modules/hero-full-stuff-the-bus`, `map-stuff-the-bus`, `stuff-the-bus-wy`, `stuff-the-bus-wy-2`) |
| `tags` | pageset | `tags` (also `components/tags*`, `modules/tags-archive`, `modules/tags/*`, `tags-pages/`) |
| `informational_pages_dev` | pageset | `informational_pages_dev` |
| `-/block/base_card.html`, `cta_button.html`, `dynamic_stories_carousel.html`, `side_by_side_hero_image.html`, `staff_and_leadership.html`, `sustainability_in_action.html` | block | same-named block models |
| `components/header-dynamic`, `components/nav-mobile-dynamic`, `modules/external-links`, `modules/services-national-wysiwyg*` | snippet | — |
| `paginated-divisions.json`, `paginated-locations.json`, `generate-locations.json`, `get-volunteer-page`, `contact-us-cookie-fallbacks.json`, `analytics.html`, `global-analytics.html`, `api/*`, 4 `datasets/mobile_editor/*` | ajax-json | — |
| `menu-config/*` (3,144) | ajax-json | Platform menus (treat as platform-owned) |

Copies on the instance with "copy" in the name (`get-informational-page copy.json`, `get-informational-page-links copy`) and `_zesty/rollbacks/*` are not listed.

## Modules

"No include site found" means no `{{ include }}` anywhere in the repository, including `z/` layouts; there are no field-driven dynamic includes. Confirm before deleting: the page may be set up in the Zesty admin.

Zesty layouts in `z/` also include `modules/services`, `service-wy`, `benefits-perks`, `donate`, `hero-full-donate`, `news-cards`, `programs-schedule` and `stats-modules`.

| Module | Type | Included by |
| --- | --- | --- |
| `modules/advisory-board` | snippet | `modules/leadership` |
| `modules/all-stories-archive` | snippet | `stories_new` |
| `modules/angel-tree-wy` | snippet | `angel_tree_campaign` |
| `modules/angel-tree-wy-2` | snippet | `angel_tree_campaign` |
| `modules/become-a-member` | snippet | `modules/sponsorship`, `services` |
| `modules/benefits-perks` | snippet | `modules/employment`, `services` |
| `modules/client-global-navigation` | snippet | `custom_head` |
| `modules/community-leaders` | snippet | `locations`, `modules/leadership` |
| `modules/contact-form` | snippet | `contact_us` |
| `modules/contact-info` | snippet | `contact_us` |
| `modules/contact-us` | snippet | `contact_us` |
| `modules/contact-us-officers` | snippet | `contact_us` |
| `modules/corporate-partners` | snippet | `modules/sponsorship` |
| `modules/donate` | snippet | `services` |
| `modules/employment` | snippet | **no include site found** |
| `modules/faqs` | snippet | `404-page`, `contact_us` |
| `modules/featured-stories` | snippet | **no include site found** |
| `modules/generic` | snippet | `informational_pages` |
| `modules/generic-info` | snippet | `modules/generic` |
| `modules/global-navigation` | snippet | **no include site found** |
| `modules/hero` | snippet | `homepage`, `locations` |
| `modules/hero-full` | snippet | `about_us`, `modules/employment`, `modules/generic`, `modules/leadership`, `modules/sponsorship`, `modules/volunteer` |
| `modules/hero-full-404` | snippet | `404-page` |
| `modules/hero-full-angel-tree` | snippet | `angel_tree_campaign` |
| `modules/hero-full-cities` | snippet | `cities` |
| `modules/hero-full-donate` | snippet | `services` |
| `modules/hero-full-events` | snippet | `events_landing_pages` |
| `modules/hero-full-leadership` | snippet | `leadership_landing_page` |
| `modules/hero-full-service-area` | snippet | `service_area` |
| `modules/hero-full-state` | snippet | `states` |
| `modules/image-button-text` | snippet | `modules/employment`, `modules/leadership`, `modules/volunteer` |
| `modules/image-button-text-full` | snippet | `about_us` |
| `modules/image-button-text-reverse` | snippet | `modules/volunteer` |
| `modules/indexdb` | snippet | `custom_head` |
| `modules/info-cards-stats` | snippet | `modules/volunteer` |
| `modules/job-positions` | snippet | `modules/employment`, `modules/volunteer` |
| `modules/leadership` | snippet | **no include site found** |
| `modules/leadership-list` | snippet | `about_us`, `leadership_landing_page` |
| `modules/leadership-parent` | snippet | **no include site found** |
| `modules/local-needs` | snippet | `about_us` |
| `modules/location-finder` | snippet | `location_finder` |
| `modules/location-finder-script` | snippet | `modules/location-finder` |
| `modules/map` | snippet | `homepage`, `modules/volunteer` |
| `modules/map-angel-tree` | snippet | `angel_tree_campaign` |
| `modules/map-contact` | snippet | `contact_us` |
| `modules/map-with-info` | snippet | `locations` |
| `modules/map-with-info-contact` | snippet | `contact_us` |
| `modules/news` | snippet | `homepage` |
| `modules/news-archive` | snippet | `stories_landing_page` |
| `modules/news-cards` | snippet | `locations`, `modules/volunteer`, `services` |
| `modules/partnership-opportunities` | snippet | `modules/sponsorship` |
| `modules/programs` | snippet | **no include site found** |
| `modules/programs-schedule` | snippet | `services` |
| `modules/programs-schedule-landing` | snippet | `locations` |
| `modules/recent-stories` | snippet | `stories` |
| `modules/schema` | snippet | `custom_head` |
| `modules/service-area-body-details` | snippet | `service_area` |
| `modules/service-area-form` | snippet | **no include site found** |
| `modules/service-program-schedule` | snippet | **no include site found** |
| `modules/service-wy` | snippet | `services` |
| `modules/service-wy-landing` | snippet | `locations` |
| `modules/services` | snippet | `404-page`, `homepage`, `locations`, `services` |
| `modules/services-intro` | snippet | **no include site found** |
| `modules/services-service-area` | snippet | `service_area` |
| `modules/sponsorship` | snippet | **no include site found** |
| `modules/state-list` | snippet | `states` |
| `modules/stats` | snippet | `about_us`, `homepage`, `locations` |
| `modules/stats-modules` | snippet | `services` |
| `modules/stats-with-stat-group` | snippet | **no include site found** |
| `modules/testimonials` | snippet | `modules/employment` |
| `modules/text-block` | snippet | `modules/employment` |
| `modules/upcoming-events` | snippet | `events_landing_pages` |
| `modules/video-module` | snippet | `modules/employment` |
| `modules/volunteer` | snippet | **no include site found** |
| `modules/what-we-do` | snippet | `modules/sponsorship` |
| `modules/zip-search` | snippet | `cities` |

## Components / block library

| Component | Type | Callers |
| --- | --- | --- |
| `components/header` | snippet | `loader`, `404-page`; includes `nav-mobile`, `header-search-modal`, `header-find-help-modal`, `header-donate-drawer-modal` |
| `components/footer` | snippet | `loader`, `404-page`; includes `news-letter-modal` |
| `components/header-donate-modal` | snippet | `loader` |
| `components/current-location-card` | snippet | `modules/location-finder` |
| `components/map-contact-information` | snippet | `modules/map-with-info` |
| `components/map-contact-information-pointer` | snippet | `modules/map-with-info-contact` |
| `components/news-archive-search` | snippet | `modules/all-stories-archive`, `modules/news-archive`; includes `news-archive-list` |
| `components/news-filter` | snippet | `modules/news` |
| `components/service-area-body`, `service-area-contact-detail` | snippet | `modules/service-area-body-details` |
| `components/socials-share` | snippet | `stories` (included as `/components/socials-share`) |
| `components/stat-group` | snippet | `modules/stats-with-stat-group` (itself unused) |
| `components/biograph-card`, `card`, `location-finder-results`, `service-card` | snippet | **no include site found** |

Block library (`-/block/*.html`, type `block`), rendered from Zesty layouts with `block('/-/block/<name>.html?variant=…')`. Call counts are from `z/pvl/`:

| Block | Layout calls |
| --- | --- |
| `hero_full.html` | 1,083 |
| `general_accordion.html` | 99 |
| `info_block.html` | 48 |
| `generic_image_text_button_block.html` | 18 |
| `fundraising_thermometer_block.html` | 15 |
| `way_to_give_block.html`, `stories_carousel.html`, `service_selector.html`, `mobile_desktop_videos.html` | 9 each |
| `theater_space_block.html`, `stats_block.html`, `profile_card_block.html`, `our_vision.html`, `greatest_need.html`, `contact_us_block.html` | 6 each |
| `text_left_with_cta_image_right.html`, `mobile_desktop_location_card_block.html`, `info_block_4_column.html`, `find_a_location___cta_box___ja.html`, `annual_reports_blocks.html` | 3 each |

A change to a block affects every layout page that uses it. `hero_full.html` is on nearly every layout page.

## Data / JSON endpoint views

70 endpoint views. The ones the site's own front end fetches:

| View | URL path | Consumers (fetch sites) |
| --- | --- | --- |
| `service-types.json` | `/service-types.json` | 9 calls in views/scripts |
| `model-info.json` | `/model-info.json` | 6 calls |
| `services.json` | `/services.json` | 4 calls |
| `stories.json` | `/stories.json` | 3 calls |
| `location-finder.json` | `/location-finder.json` | 3 calls (location finder) |
| `locations.json`, `services-offered` | `/locations.json`, `/services-offered.json` | 1 call each |

Other groups: `get-*` (informational pages, services by division/location/national/territory), `find-event.json`, `find-story.json`, `datasets/mobile_editor/**` (mobile editor app: content lists, locations, navigation, queries), `api/territory-stories.json`, CSV tools (`generate-csv`, `csvgenerator.html`, `about-us-json-to-csv.json`). There is also a GraphQL-style `-/gql/locations.json` fetch.

## Include graph notes

- 16 views have no include site: 12 modules and 4 components (tables above), plus the top-level snippets `angel-tree-script` and `contact-form`. Some are parent "page" modules (`employment`, `sponsorship`, `volunteer`, `leadership`) that include others but are never included themselves.
- Test views (`gb_test`, `api_test`, `blocks_test`, `test.html`) and debug output are in production code (`docs/custom-patterns.md` D-02, D-04).
- 13 `hero-full*` variants exist. Check them before adding another.

## Open Questions / Flags

- Are the "no include site found" modules used through the Zesty admin, or are they dead code?
- Which views does the platform use as the global layout and head? Assumed `loader` and `custom_head` from their content.
- Repo blocks `info_block_4_column.html`, `text_left_with_cta_image_right.html` and `find_a_location___cta_box___ja.html`, plus `modules/services-intro` and `test.html`, aren't on the instance (stale ZUIDs). The repo's (stale) copies of the `z/pvl/` layouts still call those three blocks, 3 times each. Were they deleted on purpose?
- Should the instance-only views above be pulled into the repo (feature branch + PR), or stay admin-managed and be listed as platform-owned?

## Review status

Not yet reviewed.
