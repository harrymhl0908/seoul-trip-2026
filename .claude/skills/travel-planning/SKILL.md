---
name: travel-planning
description: Plan a trip end-to-end and produce a beautiful, reliable, offline-ready itinerary kit — day-by-day routes, shopping/food guides, budget ledger, pre-trip booking checklist, maps, souvenir map, and a bilingual driver/host brief. Use whenever the user wants to plan, research, or build an itinerary for a trip, vacation, or holiday (e.g. "plan my Tokyo trip", "build a 5-day Seoul itinerary", "help me organize our family holiday"). Scales from a weekend to a multi-week journey. Bakes in the lessons and house style proven on the Seoul 2026 trip.
---

You're helping plan a trip. Act like a meticulous local concierge: creative and warm, but obsessive about accuracy and logistics — because a wrong opening hour or a missed last train ruins a day. Scale your effort to the trip: a weekend gets a light touch; a multi-city, multi-week journey gets the full production.

This Skill is a **workflow** plus a bundled **design system + templates + quality gate** distilled from a real, production-grade Seoul itinerary. Reuse the templates and conventions — do not reinvent the format each time. The artifacts are self-contained HTML (inline CSS, no build step) so they open on any phone, online or off.

## The non-negotiable principles (learned the hard way)

1. **Never recall a time-sensitive fact — verify it.** Opening hours, prices, last-train times, ticket prices, closures, sold-out risk, and seasonal hours change constantly. Every one must be checked against a live source and stamped with the date checked. (The Seoul plan had to fix AREX last-train 22:40-not-23:00, Seoul Sky pricing, and Garden summer hours. See `references/QUALITY-CHECKLIST.md`.)
2. **Every anchor stop needs a Plan-B.** Weather, queues, and sell-outs are certainties, not edge cases. Each timed/anchor stop carries a one-line contingency.
3. **Geocode every stop before publishing.** Confirm each place against the local map app (Naver in Korea, Google/AMap/etc. elsewhere) so addresses and map links resolve. (Seoul had to fix the ÅLAND / Musinsa addresses.)
4. **One templated format — generate variants, don't hand-maintain duplicates.** Day, shopping, and glance views are derived from one data set, not copy-pasted.
5. **Record trade-offs so they're reversible.** When you drop a stop for timing, note why, so the user can put it back.

## Bundled resources (read on demand — progressive disclosure)

- `references/DESIGN-SYSTEM.md` — the Swiss-typographic house style (fonts, palette, the `:root` CSS variables, every component class). **Read before producing any HTML.**
- `references/STOP-CARD-SCHEMA.md` — the canonical data fields for a stop, a shop, and a souvenir. Fill this per place; the HTML is just a rendering of it.
- `references/QUALITY-CHECKLIST.md` — the mandatory pre-publish verification gate. Run it at Stage 4.
- `references/MAP-GENERATION.md` — how to make per-day illustrated maps and real OSM/Leaflet walking maps.
- `templates/*.html` — copy-and-fill artifacts: `index` (hub), `day`, `shopping`, `glance`, `pre-trip`, `budget-ledger`, `souvenirs`, `driver-brief`. Each is ready-to-render with placeholder content.

## Default traveler profile (override per trip)

Unless the user says otherwise, assume the profile this kit was built for, and confirm it back in Stage 0:
- **Travelers:** a small family group; segment recommendations by persona using tags (the Seoul set was 爸爸 / 媽媽 / 仔 / 全家 — adapt to the actual group).
- **Languages:** primary **Traditional Chinese (HK)**, secondary the **destination's local language** (Korean, Japanese, etc.), tertiary English. Place names always show local script for navigation.
- **Money:** show prices in **local currency primary, HKD secondary**, with the FX rate stamped and dated (Seoul used `1 HKD ≈ 195 KRW`).
- **Signature touches:** family-segmented "⭐ 別錯過" must-not-miss callouts; per-stop weather/rain contingency; a bilingual brief for any local driver/host.

If the user is not this profile, elicit their real personas, languages, and home currency in Stage 0 and use those throughout.

---

## Stage 0 — Profile & constraints

Ask these together (not one at a time), via the question tool if available, else in chat:
- **Destination(s)** and **dates** (or season/length if flexible).
- **Travelers**: how many, ages, who they are → build **personas** and their interests.
- **Budget**: ballpark total or per-day, and home currency.
- **Pace**: packed / balanced / slow. Set a **daily energy budget** (e.g. one big anchor + 2–3 minor stops for a relaxed day; more for an intense one).
- **Hard constraints**: dietary needs, accessibility/mobility, must-dos, hard avoids. Treat these as filters on *every* later suggestion.
- **Home base / lodging** area (drives clustering and transit).

For groups, do a quick **consensus pass**: collect each person's 1–2 must-dos before building, so the plan reflects everyone.

Confirm the profile (or the defaults above) back in one short summary before moving on.

## Stage 1 — Research & discovery

- **Cluster by neighborhood/region.** Group candidate stops geographically so each day is one or two tight clusters, minimizing backtracking (Seoul = Hongdae day, Seongsu+Jamsil day, etc.).
- **Find anchors.** Per day, identify 1–2 anchors: a timed entry, reservation, sunset view, or signature experience the day is built around.
- **Research with attribution.** Pull candidates and quotes from real sources (local blogs, official tourism sites, review apps) and keep the source for each — it goes on the card and lets the user re-verify.
- **Flag everything time-sensitive** (hours, price, closure days, sell-out risk, seasonal changes) for the Stage-4 gate. Do not publish these from memory.

## Stage 2 — Itinerary build

Build each day as a **narrative arc** (morning → noon → afternoon → golden hour → evening), not a flat checklist. Apply:
- **Anchor + flex.** Pin the 1–2 anchors with realistic buffers; leave deliberate open/flex time around them.
- **Golden-hour timing.** Schedule the best scenic/photo stop around the destination's actual sunset (Seoul put Seoul Sky at 19:10 for a 19:58 sunset).
- **One serendipity window + one food centerpiece per day.** Leave one unplanned slot ("wander where it takes you"); build the day around one renowned meal or market.
- **Per-stop contingency.** Every stop gets a weather/crowd line; every anchor gets a Plan-B.
- **Realistic transit.** Note the line, exit number, walk minutes, and cost between stops.

Render days using `templates/day.html` (the stop-card pattern), with companion `shopping.html` per shopping district and a `glance.html` mobile quick-view — all from the same stop/shop data (`references/STOP-CARD-SCHEMA.md`). Follow `references/DESIGN-SYSTEM.md` exactly.

## Stage 3 — Artifacts & logistics

Produce the full kit (scale to the trip — skip what doesn't apply):
- **Master hub** (`index.html`) linking every day, shopping guide, and tool.
- **Pre-trip checklist + packing list** (`pre-trip.html`): things to **book** (with live booking links and the exact details to fill in), visa/entry status, app installs, weather-adaptive packing. Track *confirmation state*, not just intent.
- **Budget ledger + booking tracker** (`budget-ledger.html`): running multi-currency totals by category and per person, plus a row per booking with confirmation number + link.
- **Maps** (`references/MAP-GENERATION.md`): a per-day illustrated route map and, for dense walking clusters, a real OSM/Leaflet map.
- **Souvenir map** (`souvenirs.html`): curated buys cross-mapped to the day/place you'll find them.
- **Bilingual driver/host brief** (`driver-brief.html`) for any charter or local host — addresses, timing, and requests in the local language + the user's.
- **Offline pack + calendar.** Ensure every artifact works offline (inline CSS, screenshot-friendly); generate an **.ics** so anchors drop into the user's calendar. Remind the user to save booking confirmation screenshots into the pack.

## Stage 4 — Quality gate (mandatory before "done")

Run `references/QUALITY-CHECKLIST.md` over the whole kit. In short:
- Every hour/price/closure/last-train is **verified + dated**, or explicitly marked "to verify on arrival."
- Every stop **geocodes** and its map link resolves.
- Every anchor has a **Plan-B**; the budget **reconciles**; every link works.
- Run a **reader test**: hand a fresh sub-agent only the artifacts + a few traveler questions ("how do I get from stop 5 to 6?", "what if it rains on day 3?") and confirm it can answer from the kit alone. Fix any gaps it surfaces.

Do not present the kit as finished until the gate passes. Report what was verified vs. what remains "verify on arrival."

## Stage 5 — In-trip & post-trip

- **In-trip re-planning.** If a flight slips, a place closes, or weather turns, reshuffle around the anchors and surface the Plan-Bs — don't rebuild from scratch.
- **Memory capture.** Offer a light journaling prompt per day and a photo-spot log; map souvenirs actually bought.
- **Close the loop.** Write a short "what worked / what didn't" note and fold the wins into the **default profile** so the next trip starts smarter.

---

## Tools & how to actually do the work

- **Verification (Principle 1).** Verify time-sensitive facts with **WebSearch / WebFetch** against live sources, ranked: official venue/operator site or the destination's transit authority > the destination map app (Naver/Google/AMap) > reputable local blogs/review apps. If a fact can't be confirmed live, mark it **"verify on arrival"** — never present a guess as fact. Re-fetch the **FX rate** at build time and stamp it.
- **Illustrated maps.** Generate the per-day `img/d{n}-map.png` with an image-generation tool (Canva MCP, Figma, or any image generator) using the prompts in `references/MAP-GENERATION.md`. For accurate walking maps, build the Leaflet/OSRM HTML described there.
- **Calendar (.ics).** Emit one `.ics` with a `VEVENT` per **anchor**, transfer, and check-in/out (not every minor stop). Use the destination timezone, a clear `SUMMARY` (stop name + local script), `LOCATION` (address), and `DESCRIPTION` (the map link). Minimal skeleton:
  ```
  BEGIN:VCALENDAR
  VERSION:2.0
  BEGIN:VEVENT
  DTSTART;TZID=Asia/Seoul:20260625T191000
  DTEND;TZID=Asia/Seoul:20260625T203000
  SUMMARY:Seoul Sky 서울스카이
  LOCATION:송파구 올림픽로 300
  DESCRIPTION:https://map.naver.com/p/search/서울스카이
  END:VEVENT
  END:VCALENDAR
  ```
- **Labels & language.** The badge/callout words (`必到 / 可選 / ⭐別錯過 / 順遊`) are the defaults for the Traditional-Chinese profile. If the trip's primary language differs, translate these labels consistently (e.g. `MUST / OPTIONAL / ⭐ DON'T MISS / ALSO`) and keep place names in the destination's local script for navigation.
- **Reader test.** At Stage 4, spawn a fresh agent (e.g. via the Agent tool / a workflow stage) with only the artifacts + the traveler questions. If sub-agents aren't available, do the pass yourself with deliberately fresh eyes, or ask the user to paste the kit into a clean chat.
- **Worked examples.** The Seoul `d*.html`, `souvenirs.html`, `walking-map-prompts.md`, `D3a-seongsu-realmap.html`, and `RETROSPECTIVE.md` are the reference set. If you copy this Skill to `~/.claude/skills/` on another machine, keep a link back to this repo for the examples.

## Tone & deviations
- Be warm and inspiring about the *experience*, ruthless about *accuracy*. Keep the budget visible.
- If the user wants to skip stages or go freeform, let them — offer the structure, don't force it.
- If a step hits a wall (sold out, over budget, closed), propose alternatives immediately and show the trade-off; don't stall.

## Reusing this Skill on a new machine
This Skill lives in the repo at `.claude/skills/travel-planning/`. To make it available for *every* future trip, copy it to your user skills dir:
`cp -r .claude/skills/travel-planning ~/.claude/skills/`. The Seoul 2026 files in this repo remain the worked reference example; see `RETROSPECTIVE.md`.
