# Travel Planning Kit — with Seoul 2026 as the worked example

This repo is two things:

1. A **reusable travel-planning workflow** — a Claude Code Skill that plans any future trip
   end-to-end and produces a beautiful, reliable, offline-ready itinerary kit.
2. The **Seoul 2026 family trip** itself (the `d*.html`, `*-shopping.html`, `glance-*.html`,
   `souvenirs.html`, `pre-trip.html`, maps, and `img/`) — the production-grade trip that the
   workflow was distilled from, kept as the worked reference example.

## Start here

| You want to… | Open |
|---|---|
| Plan a **new trip** | Invoke the skill — say *"plan my <city> trip"* or run `/travel-planning`. |
| Understand the **workflow** | [`.claude/skills/travel-planning/SKILL.md`](.claude/skills/travel-planning/SKILL.md) |
| See **what was learned** from Seoul | [`RETROSPECTIVE.md`](RETROSPECTIVE.md) |
| Reuse the **look & components** | [`.claude/skills/travel-planning/references/DESIGN-SYSTEM.md`](.claude/skills/travel-planning/references/DESIGN-SYSTEM.md) |
| Browse the **Seoul example** | `index.html` → links every day, shopping guide, and tool. |

## What the workflow produces

A self-contained kit (plain HTML, inline CSS — opens on any phone, online or off):
day-by-day itineraries · per-district shopping + food guides · mobile quick-glance views ·
a master hub · a pre-trip booking checklist + packing list · a multi-currency **budget ledger
+ booking tracker** · per-day maps (illustrated + real OSM) · a souvenir map cross-mapped to
the route · a **bilingual driver/host brief** · an offline pack + `.ics` calendar.

## The five principles it enforces

1. **Never recall a time-sensitive fact — verify it** (hours, prices, last trains, closures).
2. **Every anchor stop has a Plan-B** (weather, queues, sell-outs are certainties).
3. **Geocode every stop** before publishing.
4. **One format — generate variants, don't hand-maintain duplicates.**
5. **Record trade-offs** so dropped stops are reversible.

A mandatory **quality gate** (`references/QUALITY-CHECKLIST.md`) + a fresh-agent reader test
run before any kit is called finished. These come straight from real Seoul errors — see the
retrospective.

## Skill structure

```
.claude/skills/travel-planning/
├── SKILL.md                 # the multi-stage workflow (Stage 0 profile → Stage 5 post-trip)
├── references/
│   ├── DESIGN-SYSTEM.md     # fonts, palette, every component's CSS
│   ├── STOP-CARD-SCHEMA.md  # data fields for stops / shops / souvenirs / bookings
│   ├── QUALITY-CHECKLIST.md # the pre-publish verification gate
│   └── MAP-GENERATION.md    # illustrated + real-OSM map recipes
└── templates/               # copy-and-fill HTML: index, day, shopping, glance,
                             # pre-trip, budget-ledger, souvenirs, driver-brief
```

## Use it on every trip (install globally)

The skill lives in this repo so it's versioned with the Seoul example. To make it available
in **any** project/session:

```bash
cp -r .claude/skills/travel-planning ~/.claude/skills/
```

Then start a session anywhere and say *"plan my trip to …"* — the skill triggers on its
description. Or fork this repo per trip and replace the Seoul content.

## Default profile (override per trip)

Tuned for the travelers it was built for: a small family group; primary **Traditional Chinese
(HK)** + the **destination's local language**; prices in **local currency + HKD**;
family-segmented "⭐ 別錯過" picks. The workflow asks for and adapts to the real travelers,
languages, and currency at the start of every trip.

---
*Built with the `travel-planning` skill. Seoul 2026 content is the reference example.*
