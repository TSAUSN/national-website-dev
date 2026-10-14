# Content Models

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, read-only instance lookup with the zesty-ai `zesty-api.mjs` helper, plus a field-access scan of `webengine/views/**` excluding `z/`)

## How this document was built (read this first)

- **Instance data (verified 2026-10-14)**, instance `8-b28282a4fe-dszqwn`, GET only:
  - `GET /v1/content/models` → 66 models
  - `GET /v1/content/models/{zuid}/fields` → every model's fields (datatype, required, related model)
  - `GET /v1/web/views` → which view renders which model
- **Repo usage**: "Read in repo ✓" means a view in `webengine/views/` (excluding `z/`) reads the field: `{{this.field}}` in the model's own view, `alias.field` inside `{{each <model> as alias}}`, or `<model>.filter(...).field`. A blank cell doesn't prove the field is unused: layouts in `z/`, JavaScript reading JSON endpoints, and dynamic access aren't counted.
- The repository has no schema file. **This doc reflects the instance on the date above; re-run the lookup after model changes.** The template being changed is still the final word on how a field is *used*.

## Verified mismatches: views read fields that don't exist

Each was confirmed against the field list and the loop variable's model.

| ID | View (line) | Reads | Model has | Effect |
| --- | --- | --- | --- | --- |
| CM-01 | `services:9`, `modules/services-intro:3` | `pageintro` | `page_intro` | The services page intro never renders |
| CM-02 | `modules/schema:157` | `cities.filter(...).name` | `title` (no `name`) | `addressLocality` in structured data is empty |
| CM-03 | `generate-csv:119,347,575`, `get-informational-page-links:35` | `ip.page_type` on `informational_pages` | no `page_type` field | `page_type` is always empty in CSV/JSON output (`information_page_types` exists but nothing links to it) |
| CM-04 | `datasets/mobile_editor/content_list/stories.json:77` | `story.story_image` | `image` | Mobile editor story list has no image |
| CM-05 | `datasets/mobile_editor/content_list/staff.json:62`, `staffs.json:75` | `staffUser.profile` | `portrait_image` | Mobile editor staff list has no image |

Not mismatches: `seo_meta_*`, `meta_*`, `url`, `_meta_*` are item SEO/meta properties provided by the platform, not model fields. Names such as `hero_subtitle` and `sort_order` in `modules/services` and `components/header` are JavaScript reading JSON responses.

## Repository vs instance drift

- **Models whose view isn't in the repo** (built in the Zesty admin): `stuff_the_bus`, `tags`, `informational_pages_dev`, and blocks `base_card`, `cta_button`, `dynamic_stories_carousel`, `side_by_side_hero_image`, `staff_and_leadership`, `sustainability_in_action`.
- **View names that differ from model names:** `stories_new` renders `all_stories`; `service_area` renders `service_areas` (and `service_area` isn't mapped in `zesty.config.json`).
- **Repo views with no model on the instance:** `informational_page_clone` (the `informational-re` branch's target; the instance has `informational_pages_dev` instead) and `blocks_test`.
- `services_seed_content` is a **dataset** but has a pageset view.

## Model index

| Model | Label | ZUID | Type | Fields | View on instance | View in repo |
| --- | --- | --- | --- | --- | --- | --- |
| `all_stories` | Stories | `6-8cdcc4c1bf-r8jnvc` | templateset | 0 | `stories_new` | yes |
| `angel_tree_campaign` | Angel Tree Campaign | `6-eae8e9d1ac-qvzxm4` | templateset | 7 | `angel_tree_campaign` | yes |
| `homepage` | Homepage (National) | `6-a0898ca2c1-krnm0f` | templateset | 30 | `homepage` | yes |
| `location_finder` | Location Finder | `6-8e91efbf9b-jqqxjt` | templateset | 1 | `location_finder` | yes |
| `stuff_the_bus` | Stuff The Bus | `6-92cbebe898-cjkzh4` | templateset | 8 | `stuff_the_bus` | **no** |
| `about_us` | About Us | `6-b29d85ada7-5kzkcc` | pageset | 35 | `about_us` | yes |
| `cities` | Cities | `6-dcd5c9b192-tgsnht` | pageset | 4 | `cities` | yes |
| `contact_us` | Contact Us | `6-aa8c819c86-953qkz` | pageset | 8 | `contact_us` | yes |
| `country` | Country | `6-aaed86ba86-zwtnd8` | pageset | 3 | `country` | yes |
| `divisions` | Divisions | `6-acb19a94bd-4q8ftj` | pageset | 32 | `divisions` | yes |
| `events` | Events | `6-a2dbc1a6e2-x4pzmx` | pageset | 24 | `events` | yes |
| `events_landing_pages` | Events Landing Pages | `6-c4d6a894f1-rtzv2z` | pageset | 8 | `events_landing_pages` | yes |
| `informational_pages` | Informational Pages | `6-c2b7f2a9ad-k4frxv` | pageset | 17 | `informational_pages` | yes |
| `informational_pages_dev` | Informational Pages DEV | `6-d8d999eeaf-zw7rd1` | pageset | 7 | `informational_pages_dev` | **no** |
| `leadership_landing_page` | Leadership Landing Page | `6-cee4dda7e4-0md6ts` | pageset | 11 | `leadership_landing_page` | yes |
| `locations` | Locations | `6-b4c9aba69c-h2nqvm` | pageset | 37 | `locations` | yes |
| `properties_types` | Properties Types | `6-a8f0f4dfae-4tvz6n` | pageset | 7 | `properties_types` | yes |
| `service_areas` | Service Areas | `6-cabdbfd4ab-md3053` | pageset | 16 | `service_area` | yes |
| `services` | Services | `6-fef1bbcecc-cjfmfq` | pageset | 34 | `services` | yes |
| `states` | States | `6-ce888daf92-fpzvz1` | pageset | 7 | `states` | yes |
| `stories` | Stories Entries | `6-80c7e29486-k0pq6s` | pageset | 25 | `stories` | yes |
| `stories_landing_page` | Stories Landing Page | `6-f8c59d8382-h4z4pc` | pageset | 5 | `stories_landing_page` | yes |
| `tags` | Tags | `6-aef7cebfd9-q7q7lh` | pageset | 2 | `tags` | **no** |
| `territories` | Territories | `6-deab97cfd9-wb5km4` | pageset | 48 | `territories` | yes |
| `volunteer_pages` | Volunteer Pages | `6-8489c3a590-kn21jh` | pageset | 17 | `volunteer_pages` | yes |
| `accordion_items` | Accordion Items | `6-84c1a5c19f-lrdw7k` | dataset | 3 | — | — |
| `activity_box_items` | Activity Box Items | `6-b0c0e4e8a1-t96m3g` | dataset | 3 | — | — |
| `annual_reports` | Annual Reports | `6-fe8cae9ae5-lm1tvb` | dataset | 6 | — | — |
| `clippings` | Globals | `6-de95fda1c1-dxht20` | dataset | 24 | — | — |
| `faqs` | FAQs | `6-d0e3f68ae2-j4nkxt` | dataset | 7 | — | — |
| `gdos_category_map` | GDOS Category Map | `6-92eaec90cb-gkd4c4` | dataset | 3 | — | — |
| `gdos_do_not_import` | GDOS Do Not Import | `6-c8c8e8b9dd-6943f9` | dataset | 5 | — | — |
| `information_page_types` | Information Page Types | `6-fccafeffbd-91jnq1` | dataset | 2 | — | — |
| `matrix_hero_sliders` | Hero Sliders | `6-d4c0fbbee5-8j61c9` | dataset | 14 | — | — |
| `pr_contacts` | PR Contacts | `6-ecb3f9bdd8-809n6x` | dataset | 7 | — | — |
| `program_schedule` | Program Schedule | `6-bee1aabfa8-8fqd9d` | dataset | 10 | — | — |
| `service_info_cards` | Service Info Cards | `6-a2e9bf97b3-k1xxln` | dataset | 6 | — | — |
| `service_types` | Service Types | `6-f0f2e8c49b-sdbvrb` | dataset | 8 | — | — |
| `services_seed_content` | Services seed content | `6-f4c8e6a6b7-hl08zv` | dataset | 33 | `services_seed_content` | yes |
| `staff_role_types` | Staff Role Types | `6-e0f3feb7b6-5n2v06` | dataset | 2 | — | — |
| `staffs` | Staff and Community Leaders | `6-98d1cbd695-tb73kj` | dataset | 17 | — | — |
| `stats` | Stats | `6-d2dfc089be-s4p9pv` | dataset | 15 | — | — |
| `activity_boxes` | Activity Boxes | `6-cc81d488aa-tcw2ng` | block | 5 | `-/block/activity_boxes.html` | yes |
| `annual_reports_blocks` | Annual Reports Blocks | `6-8adbf5cccf-szn1s2` | block | 7 | `-/block/annual_reports_blocks.html` | yes |
| `base_card` | Base Card | `6-a2839596cd-kb0hv7` | block | 12 | `-/block/base_card.html` | **no** |
| `contact_us_block` | Contact Us Block | `6-8cf5b093f4-lcm9bd` | block | 7 | `-/block/contact_us_block.html` | yes |
| `cta_button` | CTA Button | `6-faeec0b794-xn76sj` | block | 9 | `-/block/cta_button.html` | **no** |
| `dynamic_stories_carousel` | Dynamic Stories Carousel | `6-9cba8bfcee-ln76qm` | block | 6 | `-/block/dynamic_stories_carousel.html` | **no** |
| `fundraising_thermometer_block` | Fundraising Thermometer Block | `6-acc7d5f999-bgdbpr` | block | 10 | `-/block/fundraising_thermometer_block.html` | yes |
| `general_accordion` | General Accordion | `6-caf0f4baac-ctljlz` | block | 7 | `-/block/general_accordion.html` | yes |
| `generic_image_text_button_block` | Generic Image Text Button Block | `6-98dac4fd85-s1lt65` | block | 13 | `-/block/generic_image_text_button_block.html` | yes |
| `greatest_need` | Greatest Need | `6-c0e0848e86-z2w5q0` | block | 8 | `-/block/greatest_need.html` | yes |
| `hero_full` | Hero Full | `6-bcfbdaa6d3-vjj3qs` | block | 11 | `-/block/hero_full.html` | yes |
| `info_block` | Info Block | `6-bc81a0b3d0-3d7d2p` | block | 7 | `-/block/info_block.html` | yes |
| `mobile_desktop_location_card_block` | Mobile Desktop Location Card Block | `6-b0ccabf1c1-8lfrjc` | block | 5 | `-/block/mobile_desktop_location_card_block.html` | yes |
| `mobile_desktop_videos` | Mobile Desktop Videos | `6-a6a9f4dcc9-h391v6` | block | 11 | `-/block/mobile_desktop_videos.html` | yes |
| `our_vision` | Vision Cards | `6-eab0fc919c-2n14xc` | block | 11 | `-/block/our_vision.html` | yes |
| `profile_card_block` | Community Leaders | `6-84c6fcc1c9-c2m4qm` | block | 6 | `-/block/profile_card_block.html` | yes |
| `service_selector` | Services Selector | `6-aaeafa89dd-lnqt68` | block | 6 | `-/block/service_selector.html` | yes |
| `side_by_side_hero_image` | Side by Side Hero Image | `6-b6aff782b4-2vxzp9` | block | 12 | `-/block/side_by_side_hero_image.html` | **no** |
| `staff_and_leadership` | Staff and Leadership | `6-e0a6b6b1cb-lpt2q0` | block | 0 | `-/block/staff_and_leadership.html` | **no** |
| `stats_block` | Stats Block | `6-b0ecb584c2-njl0tx` | block | 6 | `-/block/stats_block.html` | yes |
| `stories_carousel` | Stories Carousel | `6-ba8fceeb84-gfpb7l` | block | 6 | `-/block/stories_carousel.html` | yes |
| `sustainability_in_action` | Sustainability In Action | `6-b4ed96c1b5-zzkqxn` | block | 1 | `-/block/sustainability_in_action.html` | **no** |
| `theater_space_block` | Theater Space Block | `6-fed8e8f5af-wvrv8z` | block | 9 | `-/block/theater_space_block.html` | yes |
| `way_to_give_block` | Way to Give Block | `6-e6da8f8dae-4p0hbp` | block | 5 | `-/block/way_to_give_block.html` | yes |

## Relationships and the organizational hierarchy

`territories` → `divisions` → `locations`, with `states` and `cities` as lookups:

- `divisions.territory` → `territories`; `locations.division` / `locations.territory`; `states.territory`; `cities.state` → `states`.
- `stories` links widely: `related_service` → `service_types`, `article_tags` → `tags`, `related_event` → `events`, `property` → `locations`, `city`, `state`, `divisions`, `territory`. `stories` items sit under a parent item (`7-988ac392c5-cl93cs`).
- `services.service_type` → `service_types`; `services.matrix_program_schedule` → `program_schedule`; `service_types.seed_content_service` → `services`.
- `staffs.member_locations` / `member_divisions` / `member_territories` (one-to-many); `staffs.staff_role_type` → `staff_role_types`.
- Blocks point at datasets: `general_accordion.accordion_items`, `activity_boxes.activity_box_items`, `stats_block.stats`, `profile_card_block.staff_and_community_leaders`, `way_to_give_block.service_info_cards`, `stories_carousel.stories_entries`.

## Shared field conventions

- **Owner linkage:** page models carry `location` (9 models), `division` (13) and `territory` (15) as one-to-one links; templates branch on whichever is set. `informational_pages_dev` uses **plural** one-to-many `locations` / `divisions` / `territories` instead. Don't mix the two shapes.
- **National flag:** `national` (yes/no) on 9 models; `display_on_national` on `stories` and `matrix_hero_sliders`.
- **Navigation:** `navigation_parent` + `display_on_header_navigation` (required) on `informational_pages`, `volunteer_pages`, `informational_pages_dev`.
- **CTAs:** `primary_cta_name/link` + `secondary_cta_name/link` pairs on 9 models.
- **Layout switch:** `page_layout` is required on `locations`, `stories` and `events`.
- **OG image:** `og_image` on 50 models.
- **Reference names:** `reference_name` is required on `services`, `services_seed_content`, `leadership_landing_page` and `program_schedule`.

## Site-wide singletons and globals

`clippings` (label "Globals", read as `globals.*`): `site_name`, `logo`, national address/phone/hours, default and fallback images (`hero_fallback_image` required), `feathery_form_id`, sister-site links (`salvation_army_usa_url*`, `satruck_*`, `sawso_*`, `disaster_salvation_army_*`, `kroccenter_*`), `our_mission_title/description`, `custom_analytics_scripts`.

## Field reference (all 66 models)

Grouped by type: templateset, pageset, dataset, block. "Req." = required in the CMS.

### `all_stories`: Stories (`6-8cdcc4c1bf-r8jnvc`)

Type: templateset · View: `stories_new` · Updated: 2025-07-01

_No fields._

### `angel_tree_campaign`: Angel Tree Campaign (`6-eae8e9d1ac-qvzxm4`)

Type: templateset · View: `angel_tree_campaign` · Updated: 2025-08-12

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Hero Title | text | yes |  |  |
| `subtitle` | Hero Subtitle | textarea | yes |  |  |
| `hero_image` | Hero Image | images | yes |  |  |
| `map_heading` | Map Heading | text |  |  |  |
| `map_content` | Map Content | wysiwyg_basic |  |  |  |
| `content` | Content | wysiwyg_basic |  |  |  |
| `content_2` | Bottom Content | wysiwyg_basic |  |  |  |

### `homepage`: Homepage (National) (`6-a0898ca2c1-krnm0f`)

Type: templateset · View: `homepage` · Updated: 2024-11-08

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `map_excerpt_subtext` | Map Excerpt Subtext | text |  |  |  |
| `map_excerpt_description` | Map Excerpt Description | wysiwyg_basic |  |  |  |
| `featured_article` | Featured Article | one_to_one |  | `stories` |  |
| `contact_us_title` | Contact Us Title | text |  |  |  |
| `contact_us_cta_header` | Contact Us CTA Header | text |  |  |  |
| `contact_us_image` | Contact Us Image | images |  |  |  |
| `contact_us_page_link` | Contact Us Page Link | internal_link |  |  |  |
| `primary_card` | Primary Card | internal_link |  |  |  |
| `primary_card_name` | Primary Card Name | text |  |  |  |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `secondary_card` | Secondary Card | internal_link |  |  |  |
| `secondary_card_cta_text` | Secondary Card CTA Text | text |  |  |  |
| `classy_url` | Classy Campaign URL Donate Once | text |  |  |  |
| `classy_url_mobile` | Classy Campaign URL Donate Monthly | text |  |  |  |
| `classy_campaign_id` | Classy Campaign ID | text |  |  |  |
| `classy_org_id` | Classy Org ID | text |  |  |  |
| `address` | Address | textarea |  |  |  |
| `facebook_url_link` | Facebook URL Link | link |  |  |  |
| `twitter_url_link` | Twitter URL Link | link |  |  |  |
| `linkedin_url_link` | LinkedIn URL Link | link |  |  |  |
| `youtube_url_link` | Youtube URL Link | link |  |  |  |
| `instagram_url_link` | Instagram URL Link | link |  |  |  |
| `donation_title` | Donation Modal Title | text |  |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `bbb_url_link` | BBB URL link | link |  |  |  |
| `bbb_image_url_link` | BBB Image URL link | link |  |  |  |
| `candid_url_link` | Candid URL Link | link |  |  |  |
| `candid_image_url_link` | Candid Image URL Link | link |  |  |  |
| `ecfa_url_link` | ECFA URL Link | link |  |  |  |
| `ecfa_image_url_link` | ECFA Image URL Link | link |  |  |  |

### `location_finder`: Location Finder (`6-8e91efbf9b-jqqxjt`)

Type: templateset · View: `location_finder` · Updated: 2024-12-23

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | OG Image | images |  |  |  |

### `stuff_the_bus`: Stuff The Bus (`6-92cbebe898-cjkzh4`)

Type: templateset · View: `stuff_the_bus` (instance only) · Updated: 2026-05-21

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Hero Title | text | yes |  |  |
| `subtitle` | Hero Subtitle | textarea | yes |  |  |
| `hero_image` | Hero Image | images | yes |  |  |
| `map_heading` | Map Heading | text |  |  |  |
| `map_content` | Map Content | wysiwyg_basic |  |  |  |
| `map_filter_instructions` | Map Filter Instructions | wysiwyg_basic |  |  |  |
| `content` | Content | wysiwyg_basic |  |  |  |
| `content_2` | Bottom Content | wysiwyg_basic |  |  |  |

### `about_us`: About Us (`6-b29d85ada7-5kzkcc`)

Type: pageset · View: `about_us` · Updated: 2024-12-26

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `hero_image` | Hero Image | images |  |  | ✓ |
| `subtitle` | Subtitle | textarea |  |  |  |
| `primary_cta_name` | Primary CTA Name | text |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Secondary CTA Name | text |  |  |  |
| `secondary_cta_link` | Secondary CTA Link | link |  |  |  |
| `image_button_text_full_title` | Our Mission Title | text |  |  |  |
| `image_button_text_full_description` | Our Mission Description | textarea |  |  |  |
| `image_button_text_full_image` | Our Mission Image | images |  |  |  |
| `image_button_text_full_cta_name` | Image Button Text CTA Name Left | text |  |  |  |
| `image_button_text_full_cta_link` | Image Button Text CTA Link Left | link |  |  |  |
| `local_need_icon_name_1` | Local Need Icon Name 1 | text |  |  |  |
| `local_need_title_1` | Local Need Title 1 | text |  |  |  |
| `local_need_subtitle_1` | Local Need Subtitle 1 | textarea |  |  |  |
| `local_need_link_text_1` | Local Need Link Text 1 | text |  |  |  |
| `local_need_external_link_1` | Local Need External Link 1 | link |  |  |  |
| `local_need_link_1` | Local Need Link 1 | internal_link |  |  |  |
| `local_need_icon_name_2` | Local Need Icon Name 2 | text |  |  |  |
| `local_need_title_2` | Local Need Title 2 | text |  |  |  |
| `local_need_subtitle_2` | Local Need Subtitle 2 | textarea |  |  |  |
| `local_need_link_text_2` | Local Need Link Text 2 | text |  |  |  |
| `local_need_external_link_2` | Local Need External Link 2 | link |  |  |  |
| `local_need_link_2` | Local Need Link 2 | internal_link |  |  |  |
| `local_need_icon_name_3` | Local Need Icon Name 3 | text |  |  |  |
| `local_need_title_3` | Local Need Title 3 | text |  |  |  |
| `local_need_link_text_3` | Local Need Link Text 3 | text |  |  |  |
| `local_need_subtitle_3` | Local Need Subtitle 3 | textarea |  |  |  |
| `local_need_external_link_3` | Local Need External Link 3 | link |  |  |  |
| `local_need_link_3` | Local Need Link 3 | internal_link |  |  |  |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `national` | National | yes_no |  |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `cities`: Cities (`6-dcd5c9b192-tgsnht`)

Type: pageset · View: `cities` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Name | text |  |  | ✓ |
| `state` | State | one_to_one |  | `states` |  |
| `hero_image` | Image | images |  |  |  |
| `og_image` | OG Image | images |  |  |  |

### `contact_us`: Contact Us (`6-aa8c819c86-953qkz`)

Type: pageset · View: `contact_us` · Updated: 2024-12-13

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Hero Title | text |  |  | ✓ |
| `hero_image` | Hero Image | images |  |  | ✓ |
| `contact_us_form` | Contact Us Message | wysiwyg_basic |  |  | ✓ |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `national` | National | yes_no |  |  |  |
| `og_image` | OG Image | images |  |  |  |

### `country`: Country (`6-aaed86ba86-zwtnd8`)

Type: pageset · View: `country` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  |  |
| `country_code` | Country Code | text |  |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `divisions`: Divisions (`6-acb19a94bd-4q8ftj`)

Type: pageset · View: `divisions` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  | ✓ |
| `division_code` | Division Code | text | yes |  | ✓ |
| `address` | Physical Address | textarea |  |  | ✓ |
| `zipcode` | Zipcode | text |  |  | ✓ |
| `state` | State | one_to_one |  | `states` | ✓ |
| `city` | City | one_to_one |  | `cities` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `classy_url` | Classy Campaign URL Donate Once | text |  |  |  |
| `classy_url_mobile` | Classy Campaign URL Donate Monthly | text |  |  |  |
| `gdos_id` | GDOS ID | text |  |  |  |
| `map_excerpt_subtext` | Map Excerpt Subtext | text |  |  | ✓ |
| `map_excerpt_description` | Map Excerpt Description | wysiwyg_basic |  |  | ✓ |
| `services_title` | Services Title | text |  |  |  |
| `feature_article` | Feature Article | one_to_one |  | `stories` |  |
| `contact_us_title` | Contact Us Title | text |  |  | ✓ |
| `contact_us_cta_header` | Contact Us CTA Header | text |  |  | ✓ |
| `contact_us_image` | Contact Us Image | images |  |  | ✓ |
| `contact_us_page_link` | Contact Us Page Link | internal_link |  |  |  |
| `primary_card` | Primary Card | internal_link |  |  |  |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `secondary_card` | Secondary Card | internal_link |  |  |  |
| `secondary_cta_text` | Secondary CTA Text | text |  |  |  |
| `facebook_url_link` | Facebook URL Link | link |  |  |  |
| `twitter_url_link` | Twitter URL Link | link |  |  |  |
| `linkedin_url_link` | LinkedIn URL Link | link |  |  |  |
| `youtube_url_link` | Youtube URL link | link |  |  |  |
| `instagram_url_link` | Instagram URL Link | link |  |  |  |
| `og_image` | Meta Image | images |  |  | ✓ |
| `should_redirect` | Should Redirect | yes_no |  |  |  |
| `matrix_stats` | Stats | one_to_many |  | `stats` |  |
| `symphony_site_id` | Symphony Site ID | text |  |  |  |
| `site_name` | Site Name | text |  |  |  |

### `events`: Events (`6-a2dbc1a6e2-x4pzmx`)

Type: pageset · View: `events` · Updated: 2024-12-06

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `image` | Event Image | images |  |  | ✓ |
| `title` | Event Title | text |  |  | ✓ |
| `body` | Event Description | wysiwyg_basic |  |  |  |
| `contact_person_name` | Contact Person Name | text |  |  |  |
| `contact_email` | Contact Email | text |  |  |  |
| `contact_number` | Contact Number | text |  |  |  |
| `primary_cta_name` | Primary Button Text | text |  |  |  |
| `primary_cta_link` | Primary Button Link | link |  |  |  |
| `secondary_cta_name` | Secondary Button Text | text |  |  |  |
| `secondary_cta_link` | Secondary Button Link | link |  |  |  |
| `event_date_with_time` | Event Start Date with Time | datetime |  |  | ✓ |
| `event_end_date_with_time` | Event End Date With Time | datetime |  |  | ✓ |
| `date_time_summary` | Date Time Summary | textarea |  |  |  |
| `address` | Address | textarea |  |  |  |
| `zipcode` | Zipcode | text |  |  |  |
| `national` | National | yes_no |  |  |  |
| `property` | Locations | one_to_many |  | `locations` | ✓ |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `page_layout` | Page Layout | dropdown | yes |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `city` | City | one_to_one |  | `cities` | ✓ |
| `state` | State | one_to_one |  | `states` | ✓ |
| `editor_level` | Editor Level | dropdown |  |  |  |

### `events_landing_pages`: Events Landing Pages (`6-c4d6a894f1-rtzv2z`)

Type: pageset · View: `events_landing_pages` · Updated: 2024-12-11

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `hero_image` | Image | images |  |  |  |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `area_commands` | Area Commands | one_to_many |  | `6-eea09fdb99-qjwmql` |  |
| `national` | National | yes_no |  |  |  |
| `og_image` | OG Image | images |  |  |  |

### `informational_pages`: Informational Pages (`6-c2b7f2a9ad-k4frxv`)

Type: pageset · View: `informational_pages` · Updated: 2024-10-01

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `subtitle` | Subtitle | textarea |  |  |  |
| `body` | Body | wysiwyg_basic |  |  |  |
| `hero_image` | Hero Image | images |  |  | ✓ |
| `primary_cta_name` | Primary CTA Name | text |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Secondary CTA Name | text |  |  |  |
| `secondary_cta_link` | Secondary CTA Link | link |  |  |  |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `national` | National | yes_no | yes |  |  |
| `external_link` | External Url | link |  |  | ✓ |
| `navigation_parent` | Navigation Parent | dropdown | yes |  |  |
| `display_on_header_navigation` | Display On Header Navigation | yes_no | yes |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `template_layout` | Drag and Drop | yes_no |  |  | ✓ |

### `informational_pages_dev`: Informational Pages DEV (`6-d8d999eeaf-zw7rd1`)

Type: pageset · View: `informational_pages_dev` (instance only) · Updated: 2026-08-20

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `national` | National | yes_no |  |  |  |
| `display_on_header_navigation` | Display on header navigation | yes_no |  |  |  |
| `navigation_parent` | Navigation parent | dropdown |  |  |  |
| `title` | Title | text |  |  |  |

### `leadership_landing_page`: Leadership Landing Page (`6-cee4dda7e4-0md6ts`)

Type: pageset · View: `leadership_landing_page` · Updated: 2024-12-18

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `body` | Body | wysiwyg_basic |  |  |  |
| `image` | Image | images |  |  |  |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `image_button_text_full_title` | Image Button Text Full Title | text |  |  |  |
| `image_button_text_full_image` | Image Button Text Full Image | images |  |  |  |
| `image_button_text_full_description` | Image Button Text Full Description | wysiwyg_basic |  |  |  |
| `reference_name` | Reference Name | text | yes |  |  |
| `og_image` | OG Image | images |  |  |  |

### `locations`: Locations (`6-b4c9aba69c-h2nqvm`)

Type: pageset · View: `locations` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  | ✓ |
| `site_name` | Site Name | text |  |  |  |
| `address` | Address | textarea |  |  | ✓ |
| `contact_number` | Contact Number | text |  |  | ✓ |
| `hours_of_operation` | Hours of Operation | textarea |  |  | ✓ |
| `latitude` | Latitude | number |  |  | ✓ |
| `longitude` | Longitude | number |  |  | ✓ |
| `property_type` | Property Type | one_to_one |  | `properties_types` | ✓ |
| `services_offered` | Services Offered | one_to_many |  | `service_types` | ✓ |
| `state` | State | one_to_one |  | `states` | ✓ |
| `city` | City | one_to_one |  | `cities` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `gdos_id` | GDOS ID | text |  |  |  |
| `classy_url` | Classy Campaign URL Donate Once | text |  |  |  |
| `classy_url_mobile` | Classy Campaign URL Donate Monthly | text |  |  |  |
| `zipcode` | Zipcode | text |  |  | ✓ |
| `redirect_url` | Redirect URL | link |  |  | ✓ |
| `should_redirect` | Should Redirect (Only in Development Environment) | yes_no |  |  |  |
| `primary_card` | Primary Card | internal_link |  |  |  |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `secondary_card` | Secondary Card | internal_link |  |  |  |
| `secondary_card_cta_text` | Secondary Card CTA Text | text |  |  |  |
| `featured_article` | Featured Service | one_to_one |  | `services` |  |
| `contact_us_title` | Contact Us Title | text |  |  | ✓ |
| `contact_us_cta_header` | Contact Us CTA Header | text |  |  | ✓ |
| `contact_us_image` | Contact Us Image | images |  |  | ✓ |
| `contact_us_page_link` | Contact Us Page Link | internal_link |  |  |  |
| `facebook_url_link` | Facebook URL Link | link |  |  |  |
| `twitter_url_link` | Twitter URL Link | link |  |  |  |
| `linkedin_url_link` | LinkedIn URL Link | link |  |  |  |
| `youtube_url_link` | Youtube URL Link | link |  |  |  |
| `instagram_url_link` | Instagram URL Link | link |  |  |  |
| `og_image` | OG Image | images |  |  | ✓ |
| `page_layout` | Page Layout | dropdown | yes |  | ✓ |
| `matrix_stats` | Stats | one_to_many |  | `stats` |  |
| `symphony_site_id` | Symphony Site ID | text |  |  |  |

### `properties_types`: Properties Types (`6-a8f0f4dfae-4tvz6n`)

Type: pageset · View: `properties_types` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  |  |
| `og_image` | OG Image | images |  |  |  |
| `required_service_types` | Required Service Types | one_to_many |  | `service_types` |  |
| `location_meta_title` | Location Meta Title | text |  |  |  |
| `location_meta_description` | Location Meta Description | textarea |  |  |  |
| `create_landing_pages` | Create Landing Pages | yes_no |  |  |  |
| `create_service_pages` | Create Service Pages | yes_no |  |  |  |

### `service_areas`: Service Areas (`6-cabdbfd4ab-md3053`)

Type: pageset · View: `service_area` · Updated: 2024-11-19

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  |  |
| `image` | Hero Image | images |  |  |  |
| `body` | Body | wysiwyg_basic |  |  |  |
| `gdos_id` | GDOS ID | text |  |  |  |
| `properties_types` | Property Type | one_to_one |  | `properties_types` |  |
| `state` | State | one_to_one |  | `states` |  |
| `division` | Division | one_to_one |  | `divisions` |  |
| `territory` | Territory | one_to_one |  | `territories` |  |
| `county` | County | text |  |  |  |
| `service_type` | Service Type | one_to_many |  | `service_types` |  |
| `service_image` | Service Image | images |  |  |  |
| `contact_name` | Contact Name | text |  |  |  |
| `contact_number` | Contact Number | text |  |  |  |
| `email` | Email | text |  |  |  |
| `form_embed` | Form Embed | wysiwyg_basic |  |  |  |
| `og_image` | OG Image | images |  |  |  |

### `services`: Services (`6-fef1bbcecc-cjfmfq`)

Type: pageset · View: `services` · Updated: 2024-09-27

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Hero Title | text | yes |  | ✓ |
| `hero_image` | Hero Image | images | yes |  | ✓ |
| `subtitle` | Hero Subtitle | text |  |  | ✓ |
| `service_type` | Service Type | one_to_one |  | `service_types` | ✓ |
| `service_page_icon` | Service Page Icon | text |  |  | ✓ |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `reference_name` | Reference Name | text | yes |  |  |
| `matrix_program_schedule` | Program Schedule | one_to_many |  | `program_schedule` | ✓ |
| `primary_cta_name` | Hero Primary CTA Title | text |  |  |  |
| `primary_cta_link` | Hero Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Hero Secondary CTA Title | text |  |  |  |
| `secondary_cta_link` | Hero Secondary CTA Link | link |  |  |  |
| `page_intro` | Page Intro | wysiwyg_basic |  |  |  |
| `service_body` | Corps Service Description | wysiwyg_basic |  |  | ✓ |
| `image_button_text_title_left` | Highlight Title | text |  |  |  |
| `image_button_text_description_left` | Highlight Description | wysiwyg_basic |  |  |  |
| `image_button_text_cta_name_left` | Highlight CTA Name | text |  |  |  |
| `image_button_text_cta_link_left` | Highlight CTA URL Link | link |  |  |  |
| `image_button_text_image_left` | Highlight image | images |  |  |  |
| `stat_title_1` | Stat title 1 | text |  |  |  |
| `stat_subtitle_1` | Stat Subtitle 1 | textarea |  |  |  |
| `stat_title_2` | Stat Title 2 | text |  |  |  |
| `stat_subtitle_2` | Stat Subtitle 2 | textarea |  |  |  |
| `stat_title_3` | Stat Title 3 | text |  |  |  |
| `stat_subtitle_3` | Stat Subtitle 3 | textarea |  |  |  |
| `image_button_text_full_bleed_title_left` | Donate Title | text |  |  |  |
| `image_button_text_full_bleed_description_left` | Donate Description | wysiwyg_basic |  |  |  |
| `image_button_text_full_bleed_cta_name_left` | Donate CTA Button Title | text |  |  |  |
| `image_button_text_full_bleed_cta_link_left` | Donate CTA URL Link | link |  |  |  |
| `image_button_text_full_bleed_image_left` | Donate Module Background Image | images |  |  |  |
| `display_in_services_navigation_dropdown` | Display in Services navigation dropdown | yes_no | yes |  | ✓ |
| `og_image` | OG Image | images |  |  |  |

### `states`: States (`6-ce888daf92-fpzvz1`)

Type: pageset · View: `states` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  | ✓ |
| `state_code` | State Code | text |  |  | ✓ |
| `image` | Image | images |  |  |  |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `country` | Country | one_to_one |  | `country` |  |
| `gdos_id` | GDOS ID | text |  |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `stories`: Stories Entries (`6-80c7e29486-k0pq6s`)

Type: pageset · Parent: `7-988ac392c5-cl93cs` · View: `stories` · Updated: 2025-07-01

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `image` | Image | images |  |  | ✓ |
| `page_layout` | Page Layout | dropdown | yes |  | ✓ |
| `story_date_picker` | Story date | date | yes |  | ✓ |
| `display_on_national` | Display on National | yes_no | yes |  |  |
| `author` | Author | text |  |  | ✓ |
| `body` | Body | wysiwyg_basic |  |  | ✓ |
| `donation_link` | Donation Link | text |  |  |  |
| `related_service` | Related Service | one_to_one |  | `service_types` | ✓ |
| `article_tags` | Article Tags | one_to_many |  | `tags` | ✓ |
| `hash_tags` | Hash Tags | text |  |  |  |
| `related_event` | Related Event | one_to_one |  | `events` |  |
| `press_release` | Press Release | yes_no |  |  |  |
| `state` | State | one_to_one |  | `states` | ✓ |
| `property` | Properties | one_to_many |  | `locations` | ✓ |
| `city` | City | one_to_one |  | `cities` | ✓ |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territory` | Territories | one_to_many |  | `territories` | ✓ |
| `primary_cta_name` | Primary CTA Name | text |  |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  | ✓ |
| `secondary_cta_name` | Secondary CTA Name | text |  |  | ✓ |
| `secondary_cta_link` | Secondary CTA Link | link |  |  | ✓ |
| `external_url` | External URL | text |  |  | ✓ |
| `editor_level` | Editor Level | dropdown |  |  |  |

### `stories_landing_page`: Stories Landing Page (`6-f8c59d8382-h4z4pc`)

Type: pageset · View: `stories_landing_page` · Updated: 2024-12-12

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `location` | Location | one_to_one |  | `locations` | ✓ |
| `division` | Division | one_to_one |  | `divisions` | ✓ |
| `territory` | Territory | one_to_one |  | `territories` | ✓ |
| `og_image` | Meta Image | images |  |  |  |

### `tags`: Tags (`6-aef7cebfd9-q7q7lh`)

Type: pageset · View: `tags` (instance only) · Updated: 2026-09-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  |  |
| `related_service` | Related Service | one_to_one |  | `service_types` |  |

### `territories`: Territories (`6-deab97cfd9-wb5km4`)

Type: pageset · View: `territories` · Updated: 2024-08-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  | ✓ |
| `image` | Image | images |  |  |  |
| `tealium_profile` | Tealium Profile | text |  |  |  |
| `description` | Description | wysiwyg_basic |  |  |  |
| `territory_code` | Territory Code | text |  |  | ✓ |
| `address` | Address | textarea |  |  | ✓ |
| `zipcode` | Zipcode | text |  |  | ✓ |
| `country` | Country | one_to_one |  | `country` |  |
| `state` | State | one_to_one |  | `states` | ✓ |
| `city` | City | one_to_one |  | `cities` | ✓ |
| `classy_org_id` | Classy Org ID | text |  |  | ✓ |
| `classy_url` | Classy Campaign URL Donate Once | text |  |  | ✓ |
| `classy_url_mobile` | Classy Campaign URL Donate Monthly | text |  |  |  |
| `classy_campaign_id` | Classy Campaign ID | text |  |  | ✓ |
| `gdos_id` | GDOS ID | text |  |  |  |
| `map_excerpt_subtext` | Map Excerpt Subtext | text |  |  | ✓ |
| `map_excerpt_description` | Map Excerpt Description | wysiwyg_basic |  |  | ✓ |
| `featured_article` | Featured Article | one_to_one |  | `stories` |  |
| `image_button_text_title_left` | Podcast Title | text |  |  |  |
| `image_button_text_description_left` | Podcast Description | wysiwyg_basic |  |  |  |
| `image_button_text_cta_name_left` | Podcast CTA Name | text |  |  |  |
| `image_button_text_cta_link_left` | Podcast CTA URL Link | link |  |  |  |
| `image_button_text_image_left` | Podcast Image | images |  |  |  |
| `contact_us_title` | Contact Us Title | text |  |  | ✓ |
| `contact_us_cta_header` | Contact Us CTA Header | text |  |  | ✓ |
| `contact_us_image` | Contact Us Image | images |  |  | ✓ |
| `contact_us_page_link` | Contact Us Page Link | internal_link |  |  |  |
| `primary_card` | Primary Card | internal_link |  |  |  |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `secondary_card` | Secondary Card | internal_link |  |  |  |
| `secondary_card_cta_text` | Secondary Card CTA Text | text |  |  |  |
| `facebook_url_link` | Facebook URL Link | link |  |  |  |
| `twitter_url_link` | Twitter URL Link | link |  |  |  |
| `youtube_url_link` | Youtube URL Link | link |  |  |  |
| `linkedin_url_link` | LinkedIn URL Link | link |  |  |  |
| `instagram_url_link` | Instagram URL Link | link |  |  |  |
| `og_image` | Meta Image | images |  |  | ✓ |
| `dgioh_id` | DGIOH ID | text |  |  | ✓ |
| `bbb_url_link` | BBB URL link | link |  |  |  |
| `bbb_image_url_link` | BBB Image URL link | link |  |  |  |
| `ecfa_url_link` | ECFA URL Link | link |  |  |  |
| `ecfa_image_url_link` | ECFA Image URL Link | link |  |  |  |
| `candid_url_link` | Candid URL Link | link |  |  |  |
| `candid_image_url_link` | Candid Image URL Link | link |  |  |  |
| `should_redirect` | Should Redirect | yes_no |  |  |  |
| `matrix_stats` | Stats | one_to_many |  | `stats` |  |
| `symphony_site_id` | Symphony Site ID | text |  |  |  |
| `site_name` | Site Name | text |  |  |  |

### `volunteer_pages`: Volunteer Pages (`6-8489c3a590-kn21jh`)

Type: pageset · View: `volunteer_pages` · Updated: 2026-04-20

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  |  |
| `subtitle` | Subtitle | textarea |  |  |  |
| `body` | Body | wysiwyg_basic |  |  |  |
| `hero_image` | Hero Image | images |  |  |  |
| `primary_cta_name` | Primary CTA Name | text |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Secondary CTA Name | text |  |  |  |
| `secondary_cta_link` | Secondary CTA Link | link |  |  |  |
| `location` | Location | one_to_one |  | `locations` |  |
| `division` | Division | one_to_one |  | `divisions` |  |
| `territory` | Territory | one_to_one |  | `territories` |  |
| `national` | National | yes_no | yes |  |  |
| `external_link` | External Url | link |  |  |  |
| `navigation_parent` | Navigation Parent | dropdown | yes |  |  |
| `display_on_header_navigation` | Display On Header Navigation | yes_no | yes |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `template_layout` | Template Layout | yes_no |  |  |  |

### `accordion_items`: Accordion Items (`6-84c1a5c19f-lrdw7k`)

Type: dataset · View: none · Updated: 2025-07-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text | yes |  |  |
| `subtitle` | Subtitle | text | yes |  |  |
| `description` | Description | wysiwyg_basic | yes |  |  |

### `activity_box_items`: Activity Box Items (`6-b0c0e4e8a1-t96m3g`)

Type: dataset · View: none · Updated: 2025-08-04

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text | yes |  |  |
| `url` | URL | link | yes |  |  |
| `image` | Image | images |  |  |  |

### `annual_reports`: Annual Reports (`6-fe8cae9ae5-lm1tvb`)

Type: dataset · View: none · Updated: 2025-07-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text | yes |  | ✓ |
| `description` | Description | textarea |  |  | ✓ |
| `image` | Image | images | yes |  |  |
| `video` | Video | images |  |  |  |
| `cta_button_text` | CTA Button Text | text |  |  | ✓ |
| `cta_button_link` | CTA Button Link | link |  |  | ✓ |

### `clippings`: Globals (`6-de95fda1c1-dxht20`)

Type: dataset · View: none · Updated: 2022-09-22

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `site_name` | Site Name | text | yes |  |  |
| `logo` | Site Logo | images |  |  |  |
| `services_all_image` | Services All Image | images |  |  |  |
| `national_address` | National Address | text |  |  |  |
| `national_contact_number` | National Contact Number | text |  |  |  |
| `national_general_hours` | National General Hours | wysiwyg_basic |  |  |  |
| `default_images` | Default Images | images |  |  |  |
| `default_news_images` | Default news images | images |  |  |  |
| `default_og_image` | Default OG Image | images |  |  |  |
| `hero_fallback_image` | Hero Fallback Image | images | yes |  |  |
| `feathery_form_id` | Feathery Form ID | text |  |  |  |
| `salvation_army_usa_url_name` | Salvation Army USA URL name | text | yes |  |  |
| `salvation_army_usa_url` | Salvation Army USA URL Link | link | yes |  |  |
| `satruck_url_name` | Satruck URL Name | text | yes |  |  |
| `satruck_url_link` | Satruck URL Link | link | yes |  |  |
| `sawso_url_name` | Sawso URL Name | text | yes |  |  |
| `sawso_url_link` | Sawso URL Link | link | yes |  |  |
| `disaster_salvation_army_url_name` | Disaster Salvation Army URL Name | text | yes |  |  |
| `disaster_salvation_army_url_link` | Disaster Salvation Army URL Link | link | yes |  |  |
| `kroccenter_url_name` | KrocCenter URL Name | text | yes |  |  |
| `kroccenter_url_link` | KrocCenter URL Link | link | yes |  |  |
| `our_mission_description` | Our Mission Description | textarea | yes |  |  |
| `our_mission_title` | Our Mission Title | text | yes |  |  |
| `custom_analytics_scripts` | Custom Analytics Scripts | textarea |  |  |  |

### `faqs`: FAQs (`6-d0e3f68ae2-j4nkxt`)

Type: dataset · View: none · Updated: 2024-12-13

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `question` | Question | textarea |  |  | ✓ |
| `answer` | Answer | wysiwyg_basic |  |  | ✓ |
| `topic` | Topic | dropdown |  |  |  |
| `related_service` | Related Service | one_to_one |  | `service_types` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `national` | National | yes_no |  |  |  |
| `sort_order` | Sort Order | sort |  |  |  |

### `gdos_category_map`: GDOS Category Map (`6-92eaec90cb-gkd4c4`)

Type: dataset · View: none · Updated: 2026-03-31

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `gdos_category` | GDOS Category | dropdown |  |  |  |
| `gdos_subcategory` | GDOS Subcategory | dropdown |  |  |  |
| `service_type` | Service Type | one_to_one |  | `service_types` |  |

### `gdos_do_not_import`: GDOS Do Not Import (`6-c8c8e8b9dd-6943f9`)

Type: dataset · View: none · Updated: 2026-03-31

A list of GDOS locations that should not be included during migration or syncing of location data to Zesty

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `gdos_id` | GDOS ID | text |  |  |  |
| `location_name` | Location Name | text |  |  |  |
| `territory` | Territory | one_to_one |  | `territories` |  |
| `division` | Division | one_to_one |  | `divisions` |  |
| `reason` | Reason | text |  |  |  |

### `information_page_types`: Information Page Types (`6-fccafeffbd-91jnq1`)

Type: dataset · View: none · Updated: 2024-09-30

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text |  |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |

### `matrix_hero_sliders`: Hero Sliders (`6-d4c0fbbee5-8j61c9`)

Type: dataset · View: none · Updated: 2025-03-11

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  |  |
| `content` | Content | wysiwyg_basic |  |  |  |
| `image` | Image | images |  |  |  |
| `primary_cta_name` | Primary CTA Name | text |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Secondary CTA Name | text |  |  |  |
| `secondary_cta_link` | Secondary CTA Link | link |  |  |  |
| `display_on_national` | Display on National | yes_no |  |  | ✓ |
| `sort_order` | Sort Order | sort |  |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `display_on_territories` | Display on Territories | one_to_many |  | `territories` |  |
| `display_on_divisions` | Display on Divisions | one_to_many |  | `divisions` |  |
| `display_on_locations` | Display on Locations | one_to_many |  | `locations` |  |
| `editor_level` | Editor Level | dropdown |  |  |  |

### `pr_contacts`: PR Contacts (`6-ecb3f9bdd8-809n6x`)

Type: dataset · View: none · Updated: 2025-01-29

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `name` | Name | text | yes |  |  |
| `email` | Email | text | yes |  |  |
| `cell_phone` | Cell Phone | text |  |  |  |
| `office_phone` | Office Phone | text |  |  |  |
| `states` | States | one_to_many |  | `states` |  |
| `division` | Division | one_to_one |  | `divisions` |  |
| `photo` | Photo | images |  |  |  |

### `program_schedule`: Program Schedule (`6-bee1aabfa8-8fqd9d`)

Type: dataset · View: none · Updated: 2025-01-21

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text | yes |  | ✓ |
| `program_schedule_location` | Program Location | text |  |  |  |
| `timezone` | Timezone | dropdown | yes |  |  |
| `pseudo_multiselect_days` | Schedule | text |  |  |  |
| `time` | Time & Notes | text | yes |  |  |
| `description` | Description | wysiwyg_basic |  |  |  |
| `link` | Link | text |  |  |  |
| `staff_contact` | Staff Contact | one_to_one |  | `staffs` |  |
| `reference_name` | Reference Name | text | yes |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `service_info_cards`: Service Info Cards (`6-a2e9bf97b3-k1xxln`)

Type: dataset · Parent: `services` · View: none · Updated: 2024-11-15

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `icon_name` | Icon Name | text | yes |  | ✓ |
| `title` | Title | text | yes |  | ✓ |
| `description` | Description | wysiwyg_basic |  |  | ✓ |
| `service_page` | Display On Service Page | one_to_one | yes | `services` |  |
| `sort_order` | Sort Order | sort |  |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `service_types`: Service Types (`6-f0f2e8c49b-sdbvrb`)

Type: dataset · View: none · Updated: 2024-11-08

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `image` | Image | images |  |  |  |
| `description` | Description | wysiwyg_basic |  |  |  |
| `icon_name` | Icon | text |  |  | ✓ |
| `seed_content_service` | Seed Content Service | one_to_one |  | `services` |  |
| `legacy_gdos_name` | Legacy GDOS Name | text |  |  |  |
| `sort_order` | Sort Order | sort |  |  | ✓ |
| `show_on_menus` | Show On Menus | yes_no |  |  |  |

### `services_seed_content`: Services seed content (`6-f4c8e6a6b7-hl08zv`)

Type: dataset · View: `services_seed_content` · Updated: 2026-07-22

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Hero Title | text | yes |  |  |
| `hero_image` | Hero Image | images | yes |  |  |
| `subtitle` | Hero Subtitle | text |  |  |  |
| `service_type` | Service Type | one_to_one |  | `service_types` |  |
| `service_page_icon` | Service Page Icon | text |  |  |  |
| `location` | Location | one_to_one |  | `locations` |  |
| `division` | Division | one_to_one |  | `divisions` |  |
| `territory` | Territory | one_to_one |  | `territories` |  |
| `reference_name` | Reference Name | text | yes |  |  |
| `matrix_program_schedule` | Program Schedule | one_to_many |  | `program_schedule` |  |
| `primary_cta_name` | Hero Primary CTA Title | text |  |  |  |
| `primary_cta_link` | Hero Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Hero Secondary CTA Title | text |  |  |  |
| `secondary_cta_link` | Hero Secondary CTA Link | link |  |  |  |
| `service_body` | Corps Service Description | wysiwyg_basic |  |  |  |
| `image_button_text_title_left` | Highlight Title | text |  |  |  |
| `image_button_text_description_left` | Highlight Description | wysiwyg_basic |  |  |  |
| `image_button_text_cta_name_left` | Highlight CTA Name | text |  |  |  |
| `image_button_text_cta_link_left` | Highlight CTA URL Link | link |  |  |  |
| `image_button_text_image_left` | Highlight image | images |  |  |  |
| `stat_title_1` | Stat title 1 | text |  |  |  |
| `stat_subtitle_1` | Stat Subtitle 1 | textarea |  |  |  |
| `stat_title_2` | Stat Title 2 | text |  |  |  |
| `stat_subtitle_2` | Stat Subtitle 2 | textarea |  |  |  |
| `stat_title_3` | Stat Title 3 | text |  |  |  |
| `stat_subtitle_3` | Stat Subtitle 3 | textarea |  |  |  |
| `image_button_text_full_bleed_title_left` | Donate Title | text |  |  |  |
| `image_button_text_full_bleed_description_left` | Donate Description | wysiwyg_basic |  |  |  |
| `image_button_text_full_bleed_cta_name_left` | Donate CTA Button Title | text |  |  |  |
| `image_button_text_full_bleed_cta_link_left` | Donate CTA URL Link | link |  |  |  |
| `image_button_text_full_bleed_image_left` | Donate Module Background Image | images |  |  |  |
| `display_in_services_navigation_dropdown` | Display in Services navigation dropdown | yes_no | yes |  |  |
| `og_image` | OG Image | images |  |  |  |

### `staff_role_types`: Staff Role Types (`6-e0f3feb7b6-5n2v06`)

Type: dataset · View: none · Updated: 2024-09-30

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `role_name` | Role Name | text |  |  | ✓ |
| `sort_order` | Sort Order | sort |  |  |  |

### `staffs`: Staff and Community Leaders (`6-98d1cbd695-tb73kj`)

Type: dataset · View: none · Updated: 2025-03-19

This dataset stores information about staff members and community leaders who will be displayed on the website. Admins can use this section to manage profiles, including names, job roles, titles, email addresses, and other relevant details.

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `first_name` | First Name | text | yes |  | ✓ |
| `middle_name` | Middle Name | text |  |  | ✓ |
| `last_name` | Last Name | text | yes |  | ✓ |
| `title_rank` | Title/Rank | text |  |  | ✓ |
| `job_role` | Job Role | text | yes |  | ✓ |
| `staff_role_type` | Staff Role Type | one_to_one | yes | `staff_role_types` | ✓ |
| `email` | Email | text |  |  | ✓ |
| `about` | About | wysiwyg_basic |  |  | ✓ |
| `portrait_image` | Portrait Image | images |  |  | ✓ |
| `sort_order` | Sort Order | sort |  |  | ✓ |
| `national_member` | National Member | yes_no |  |  |  |
| `display_on_website` | Display on Website | yes_no | yes |  | ✓ |
| `member_territories` | Member Territories | one_to_many |  | `territories` | ✓ |
| `member_divisions` | Member Divisions | one_to_many |  | `divisions` | ✓ |
| `member_locations` | Member locations | one_to_many |  | `locations` | ✓ |
| `read_only_on_mobile_editor` | Read Only on Mobile Editor | yes_no | yes |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `stats`: Stats (`6-d2dfc089be-s4p9pv`)

Type: dataset · View: none · Updated: 2024-09-25

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `image` | Image | images |  |  | ✓ |
| `stats_name` | Stats Name | text |  |  | ✓ |
| `stats_sub_text` | Stats Sub Text | text |  |  | ✓ |
| `description` | Description | wysiwyg_basic |  |  | ✓ |
| `right_side_header` | Right Side Header | text |  |  | ✓ |
| `right_side_content` | Right Side Content | wysiwyg_basic |  |  | ✓ |
| `learn_more_url` | Learn More URL | link |  |  |  |
| `general_statistic` | General Statistic | yes_no |  |  | ✓ |
| `national` | National | yes_no |  |  | ✓ |
| `territories` | Territories | one_to_many |  | `territories` | ✓ |
| `divisions` | Divisions | one_to_many |  | `divisions` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` | ✓ |
| `categories` | Categories | dropdown |  |  | ✓ |
| `related_service` | Related Service | one_to_one |  | `service_types` | ✓ |
| `og_image` | Meta Image | images |  |  |  |

### `activity_boxes`: Activity Boxes (`6-cc81d488aa-tcw2ng`)

Type: block · View: `-/block/activity_boxes.html` · Updated: 2025-08-04

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |
| `activity_box_items` | Activity Box Items | one_to_many | yes | `activity_box_items` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `annual_reports_blocks`: Annual Reports Blocks (`6-8adbf5cccf-szn1s2`)

Type: block · View: `-/block/annual_reports_blocks.html` · Updated: 2025-08-08

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  |  |
| `youtube_url_id` | Youtube URL ID | text | yes |  | ✓ |
| `annual_reports` | Annual Reports | one_to_many | yes | `annual_reports` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `base_card`: Base Card (`6-a2839596cd-kb0hv7`)

Type: block · View: `-/block/base_card.html` (instance only) · Updated: 2026-03-10

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `image` | Image | images | yes |  |  |
| `title` | Title | text | yes |  |  |
| `description` | Description | text | yes |  |  |
| `service_types` | Service Types | one_to_one |  | `service_types` |  |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `primary_cta_link` | Primary CTA Link | link |  |  |  |
| `secondary_cta_name` | Secondary CTA Name | text |  |  |  |
| `secondary_cta_link` | Secondary CTA Link | link |  |  |  |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `contact_us_block`: Contact Us Block (`6-8cf5b093f4-lcm9bd`)

Type: block · View: `-/block/contact_us_block.html` · Updated: 2025-07-28

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `subtitle` | Subtitle | textarea |  |  | ✓ |
| `feathery_id` | Feathery ID | text | yes |  | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `cta_button`: CTA Button (`6-faeec0b794-xn76sj`)

Type: block · View: `-/block/cta_button.html` (instance only) · Updated: 2026-06-09

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `cta_button_text` | CTA Button Text | text | yes |  |  |
| `cta_button_link` | CTA Button Link | link | yes |  |  |
| `open_in_a_new_tab` | Open in a new tab | yes_no | yes |  |  |
| `cta_type` | Button Type | dropdown | yes |  |  |
| `button_size` | Button Size | dropdown | yes |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `dynamic_stories_carousel`: Dynamic Stories Carousel (`6-9cba8bfcee-ln76qm`)

Type: block · View: `-/block/dynamic_stories_carousel.html` (instance only) · Updated: 2026-03-18

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  |  |
| `article_tags` | Filter by Article Tags | one_to_many |  | `tags` |  |
| `locations` | Locations | one_to_one |  | `locations` |  |
| `divisions` | Divisions | one_to_one |  | `divisions` |  |
| `territories` | Territories | one_to_one |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `fundraising_thermometer_block`: Fundraising Thermometer Block (`6-acc7d5f999-bgdbpr`)

Type: block · View: `-/block/fundraising_thermometer_block.html` · Updated: 2025-08-14

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `goal` | Goal | number | yes |  | ✓ |
| `raised` | Raised | number | yes |  | ✓ |
| `donate_button_text` | Donate Button Text | text |  |  | ✓ |
| `donate_button_url` | Donate Button URL | link | yes |  | ✓ |
| `end_date` | End Date | date | yes |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `general_accordion`: General Accordion (`6-caf0f4baac-ctljlz`)

Type: block · View: `-/block/general_accordion.html` · Updated: 2025-06-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `open_first_accordion_item` | Open First Accordion Item | yes_no |  |  | ✓ |
| `accordion_items` | Accordion Items | one_to_many | yes | `accordion_items` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `generic_image_text_button_block`: Generic Image Text Button Block (`6-98dac4fd85-s1lt65`)

Type: block · View: `-/block/generic_image_text_button_block.html` · Updated: 2025-06-13

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `body` | Body | wysiwyg_basic |  |  | ✓ |
| `cta_buttons_alignment` | CTA Buttons Alignment | dropdown |  |  |  |
| `cta_button_text` | CTA Button Text | text |  |  | ✓ |
| `cta_button_link` | CTA Button Link | link |  |  | ✓ |
| `cta_button_text_2` | CTA Button Text 2 | text |  |  | ✓ |
| `should_link_open_to_a_new_tab` | Should link open to a new tab | yes_no |  |  | ✓ |
| `cta_button_link_2` | CTA Button Link 2 | link |  |  | ✓ |
| `should_link_open_to_a_new_tab_2` | Should link open to a new tab 2 | yes_no |  |  | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `greatest_need`: Greatest Need (`6-c0e0848e86-z2w5q0`)

Type: block · View: `-/block/greatest_need.html` · Updated: 2025-10-16

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text | yes |  | ✓ |
| `button_text` | Button Text | text |  |  | ✓ |
| `url` | Button URL | link |  |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `open_in_a_new_tab` | Open in a new tab | yes_no |  |  | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `hero_full`: Hero Full (`6-bcfbdaa6d3-vjj3qs`)

Type: block · View: `-/block/hero_full.html` · Updated: 2025-10-15

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `image` | Image | images | yes |  | ✓ |
| `title` | Title | text | yes |  | ✓ |
| `description` | Description | textarea |  |  | ✓ |
| `primary_cta_name` | Primary CTA Name | text |  |  | ✓ |
| `primary_cta_link` | Primary CTA Link | link |  |  | ✓ |
| `secondary_cta_name` | Secondary CTA Name | text |  |  | ✓ |
| `secondary_cta_link` | Secondary CTA Link | link |  |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `info_block`: Info Block (`6-bc81a0b3d0-3d7d2p`)

Type: block · View: `-/block/info_block.html` · Updated: 2025-06-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |
| `icon_name` | Icon Name | text |  |  | ✓ |
| `title` | Title | text |  |  | ✓ |
| `description` | Description | textarea |  |  | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `mobile_desktop_location_card_block`: Mobile Desktop Location Card Block (`6-b0ccabf1c1-8lfrjc`)

Type: block · View: `-/block/mobile_desktop_location_card_block.html` · Updated: 2025-08-27

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Location | one_to_one | yes | `locations` | ✓ |
| `locations_relation` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `mobile_desktop_videos`: Mobile Desktop Videos (`6-a6a9f4dcc9-h391v6`)

Type: block · View: `-/block/mobile_desktop_videos.html` · Updated: 2025-07-22

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `media_1_title` | Media 1 Title | text |  |  | ✓ |
| `youtube_url_link_1` | Youtube URL Link 1 | text |  |  |  |
| `media_2_title` | Media 2 Title | text |  |  | ✓ |
| `youtube_url_link_2` | Youtube URL Link 2 | text |  |  |  |
| `media_3_title` | Media 3 Title | text |  |  | ✓ |
| `youtube_url_link_3` | Youtube URL Link 3 | text |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `our_vision`: Vision Cards (`6-eab0fc919c-2n14xc`)

Type: block · View: `-/block/our_vision.html` · Updated: 2025-10-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `left_column_icon` | Left Column icon | text | yes |  | ✓ |
| `left_column_title` | Left Column Title | text | yes |  | ✓ |
| `left_column_description` | Left Column Description | textarea | yes |  | ✓ |
| `right_column_icon` | Right Column icon | text | yes |  | ✓ |
| `right_column_title` | Right Column Title | text | yes |  | ✓ |
| `right_column_description` | Right Column Description | textarea | yes |  | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `profile_card_block`: Community Leaders (`6-84c6fcc1c9-c2m4qm`)

Type: block · View: `-/block/profile_card_block.html` · Updated: 2025-07-22

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `staff_and_community_leaders` | Staff and Community Leaders | one_to_many | yes | `staffs` | ✓ |
| `og_image` | Meta Image | images |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `service_selector`: Services Selector (`6-aaeafa89dd-lnqt68`)

Type: block · View: `-/block/service_selector.html` · Updated: 2025-07-22

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `services` | Services | one_to_many | yes | `services` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `side_by_side_hero_image`: Side by Side Hero Image (`6-b6aff782b4-2vxzp9`)

Type: block · View: `-/block/side_by_side_hero_image.html` (instance only) · Updated: 2026-08-17

A hero with text and CTA buttons on the left and an image on the right

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  |  |
| `sub_title` | Sub Title | textarea |  |  |  |
| `cta_button_1_text` | CTA Button 1 Text | text |  |  |  |
| `cta_button_1_url` | CTA Button 1 URL | link |  |  |  |
| `cta_button_2_text` | CTA Button 2 Text | text |  |  |  |
| `cta_button_2_url` | CTA Button 2 URL | link |  |  |  |
| `button_container_sub_text` | Button Container Sub Text | text |  |  |  |
| `hero_image` | Hero Image | images |  |  |  |
| `star_rating_number` | Star Rating Number | number |  |  |  |
| `star_rating_title` | Star Rating Title | text |  |  |  |
| `star_rating_sub_text` | Star Rating Sub Text | text |  |  |  |
| `og_image` | Meta Image | images |  |  |  |

### `staff_and_leadership`: Staff and Leadership (`6-e0a6b6b1cb-lpt2q0`)

Type: block · Parent: `staffs` · View: `-/block/staff_and_leadership.html` (instance only) · Updated: 2026-08-26

Meet Our Team

_No fields._

### `stats_block`: Stats Block (`6-b0ecb584c2-njl0tx`)

Type: block · View: `-/block/stats_block.html` · Updated: 2025-10-17

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |
| `title` | Title | text |  |  | ✓ |
| `stats` | Stats | one_to_many | yes | `stats` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

### `stories_carousel`: Stories Carousel (`6-ba8fceeb84-gfpb7l`)

Type: block · View: `-/block/stories_carousel.html` · Updated: 2025-07-15

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `title` | Title | text |  |  | ✓ |
| `stories_entries` | Stories Entries | one_to_many | yes | `stories` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `sustainability_in_action`: Sustainability In Action (`6-b4ed96c1b5-zzkqxn`)

Type: block · View: `-/block/sustainability_in_action.html` (instance only) · Updated: 2026-08-20

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |

### `theater_space_block`: Theater Space Block (`6-fed8e8f5af-wvrv8z`)

Type: block · View: `-/block/theater_space_block.html` · Updated: 2025-07-31

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `hero_sliders` | Hero Sliders | one_to_many | yes | `matrix_hero_sliders` | ✓ |
| `primary_card` | Primary Card | internal_link | yes |  | ✓ |
| `primary_card_cta_text` | Primary Card CTA Text | text |  |  |  |
| `secondary_card` | Secondary Card | internal_link | yes |  | ✓ |
| `secondary_card_cta_text` | Secondary Card CTA Text | text |  |  |  |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |
| `og_image` | Meta Image | images |  |  |  |

### `way_to_give_block`: Way to Give Block (`6-e6da8f8dae-4p0hbp`)

Type: block · View: `-/block/way_to_give_block.html` · Updated: 2025-08-08

| Field | Label | Type | Req. | Links to | Read in repo |
| --- | --- | --- | --- | --- | --- |
| `og_image` | Meta Image | images |  |  |  |
| `service_info_cards` | Service Info Cards | one_to_many | yes | `service_info_cards` | ✓ |
| `locations` | Locations | one_to_many |  | `locations` |  |
| `divisions` | Divisions | one_to_many |  | `divisions` |  |
| `territories` | Territories | one_to_many |  | `territories` |  |

## Open Questions / Flags

- CM-01 to CM-05: fix in the views (field names on the instance are authoritative), or are any of these fields meant to be added to the models?
- `informational_page_clone` (repo, `informational-re` branch) vs `informational_pages_dev` (instance): which one is the target? The repo view has no model on the instance.
- `staff_and_leadership` is a block with no fields and parent `staffs`. Is it in progress?
- `services_seed_content` is a dataset with a pageset view. Intended?
- Views for `stuff_the_bus`, `tags`, `informational_pages_dev` and 6 blocks exist only on the instance. Pull them into the repo on a feature branch, or leave them admin-managed?

## Review status

Not yet reviewed.
