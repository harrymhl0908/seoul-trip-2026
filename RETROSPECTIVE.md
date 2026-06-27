# Seoul 2026 — Retrospective & Reference Case Study

This document formalizes the work done on the **Seoul family trip (22–27 Jun 2026)** and
turns its lessons into the rules now encoded in the reusable `travel-planning` skill
(`.claude/skills/travel-planning/`). It is both a record of what was built and an honest
evaluation of what went wrong — so the next trip starts from the standard, not from scratch.

---

## 1. What was produced

A production-grade, phone-first itinerary system for a 6-day / 5-night Hong Kong family trip:

- **5 day itineraries** (D0–D4), each with a weather banner, narrative masthead, route
  summary, illustrated walking map, and numbered **stop cards** (time, 必到/可選 badge,
  bilingual name, USP, price KRW≈HKD, walk time, persona tag, Naver link, rain/Plan-B line,
  photo, sourced quote).
- **Per-day shopping guides** (`d{n}-shopping.html`) with numbered shop cards, brand lists,
  and a family-segmented **"⭐ 別錯過"** must-not-miss callout.
- **Mobile quick-glance** views (`glance-d{n}.html`) and an all-day digest (`daily-itinerary.html`).
- **Pre-trip card** (`pre-trip.html`): the 4 must-book items with live links + exact
  fill-in details, entry/visa status, app installs, and a weather-adaptive packing list that
  persists ticks to `localStorage`.
- **Souvenir map** (`souvenirs.html`): 20 curated buys cross-mapped to the day/place they're
  found, sourced from 11+ media outlets.
- **Bilingual driver brief** (`D2-driver-brief.html`) for the Gapyeong charter day — Korean +
  Chinese, with local-script addresses and Naver navigation.
- **Maps**: illustrated per-day maps (`walking-map-prompts.md`) and real OSM/OSRM Leaflet
  maps for dense clusters (`D3a-seongsu-realmap.html`).
- A consistent **Swiss-typographic design system** (white/ink/Swiss-red, Inter + Space Mono +
  Noto Sans TC) applied across every file, all self-contained HTML that works offline.

## 2. The process that produced it

1. Research each day's route — cluster attractions/food/shops by neighborhood to minimize backtracking.
2. Build the HTML from the design system (inline CSS, no build step).
3. Generate an illustrated map per day; add a real OSM map for dense walking clusters.
4. Curate 3–5 photos per stop (own + sourced, credited), optimized to 720px/~72%.
5. Extract and **verify** opening hours, prices, addresses, station exits, Naver URLs.
6. Add the per-district shopping companion with brands + persona targeting.
7. Commit with a "problem → solution → assets refreshed" message.
8. Iterate: fix hours, reorder stops, add weather contingencies, re-optimize images.

## 3. Evaluation — errors & rework, and the measure each one became

The git history and content show real corrections. Each is now a rule in the skill so it
can't recur. (Cross-reference: `references/QUALITY-CHECKLIST.md`.)

| # | What went wrong | Root cause | Measure now in the workflow |
|---|---|---|---|
| 1 | **AREX last train** stated as 23:00; actually **22:40** | Time-sensitive fact recalled, not verified | **Never state hours/prices/last-train from memory** — verify against a live source + stamp the date. (Checklist §A) |
| 2 | **Seoul Sky price** wobbled (31k vs 33k) | Ticket price assumed | Prices carry an "as of <date>, confirm at checkout" stamp. (§A) |
| 3 | **Garden summer hours** wrong (used winter light-fest hours) | Seasonal hours not checked for the travel month | Verify seasonal hours for the actual dates. (§A) |
| 4 | **Addresses fixed** (ÅLAND Myeongdong; Musinsa old vs new store) | Place not geocoded before publishing | Geocode every stop against the map app; confirm the deep link resolves. (§B) |
| 5 | **Sell-out risk** (Dubai cookie, London Bagel) found late | Queue/stock reality not modeled | Every hyped item gets a stock/queue note + mitigation (go early / take a number / book). (§A) |
| 6 | **South Gate Market dropped** for London Bagel timing | Trade-off made silently | Record every dropped/changed stop and why, so it's reversible. (§E) |
| 7 | **Glance files + inline-vs-separate shopping** format drifted | Duplicated artifacts hand-maintained | One data set → generate variants; don't hand-maintain duplicates. (§E) |
| 8 | **Hotel pickup/luggage & app installs** left as untracked templates | Checklist tracked intent, not state | Pre-trip checklist tracks **confirmation state** (booked? called? installed?). (§F) |

### Process gaps also addressed
- **No verification gate** existed — facts shipped as soon as written. → Stage 4 mandatory
  quality gate + a fresh-sub-agent **reader test**.
- **No budget ledger / booking tracker** — prices lived scattered in day files. → dedicated
  `budget-ledger.html` with multi-currency roll-up + confirmation numbers.
- **No offline/calendar packaging** named explicitly. → Stage 3 offline pack + `.ics`.
- **No memorable-experience layer** was deliberate. → Stage 2 now bakes in golden-hour
  timing, a serendipity window, and a food centerpiece per day; Stage 5 adds memory capture.

## 4. What worked well (keep doing)

- The **stop-card + ⭐別錯過 persona segmentation** — one itinerary that serves a whole
  family without forcing one taste on everyone.
- **Per-stop weather/Plan-B** lines — the monsoon-onset trip never had a dead day.
- **Bilingual driver brief** — removed all friction with a non-English-speaking charter driver.
- **Souvenir map cross-referenced to the route** — buying happened in passing, no extra trips.
- **Swiss design discipline** — every page legible on a phone in sunlight; print-clean.

## 5. From this trip → the reusable skill

Everything above is now generalized in `.claude/skills/travel-planning/`:
the **workflow** (`SKILL.md`), the **design system** and **schema** (`references/`), the
**quality gate** (`references/QUALITY-CHECKLIST.md`), and **copy-and-fill templates**
(`templates/`). The Seoul files in this repo remain the worked example to learn the format from.
