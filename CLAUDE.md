# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this is

A **static, hand-authored HTML travel itinerary** for a family trip to Seoul,
**22–27 June 2026** (the rainy/梅雨 season). It is a set of self-contained web
pages — one per day plus several cross-cutting reference pages — written in
**Traditional Chinese / Cantonese (`zh-Hant`)** with Korean (`한글`) place names
and some English labels. The audience is the travelling family, not developers.

There is **no build system, no bundler, no framework, no package manager, and no
JavaScript app**. Every page is plain HTML with inline `<style>`. To preview,
just open a file in a browser or serve the directory statically
(e.g. `python3 -m http.server`). There is nothing to compile, lint, or test.

## The trip (content map)

Five touring days, each with its own area and page set:

| Day | Area (zh / 한글) | Theme |
|-----|------------------|-------|
| D0  | 명동 Myeongdong | Arrival-night food + night-market loop |
| D1  | 弘大 + 延南洞 Hongdae + Yeonnam-dong | Cafés, design shops, 京義線숲길 forest park |
| D2  | 加平 Gapyeong | Full-day **private-car charter** (남이섬 / 쁘띠프랑스 / 晨靜수목원 / 레일바이크) — spots far apart, not a walking day |
| D3  | 성수 + 잠실 Seongsu + Jamsil | Red-brick Seongsu street + Lotte/Jamsil cluster |
| D4  | 安國 → 明洞 → 東大門 | Three districts linked by subway |

Base/hotel area: 충무로 (Chungmuro).

## File taxonomy

Naming is consistent — match it when adding files.

**Master artifact**
- `index.html` — the all-in-one master artifact (~9 MB; **68 base64-embedded
  images**, fully offline-portable). `seoul-trip-artifact-B.html` is currently a
  **byte-identical copy** of `index.html`; keep them in sync if you edit either,
  or treat `index.html` as canonical.

**Per-day pages** (reference external images in `img/`, not embedded)
- `d0.html` … `d4.html` — full day pages (行程 itinerary + 購物 shopping + map slot).
- `d0-shopping.html`, `d1-shopping.html`, `d3-shopping.html`, `d4-shopping.html`
  — shopping-focused variants.
- `glance-d0.html` … `glance-d4.html` — "at a glance" photo + 影評 (review) versions.

**Cross-cutting reference pages**
- `daily-itinerary.html` — combined daily quick-reference (sticky day-nav, accordion).
- `souvenirs.html` — 20 souvenir picks with photos (`img/souvenirs/sv_*.jpg`).
- `pre-trip.html` — pre-departure order card + 梅雨 (rainy-season) packing list.
- `D2-driver-brief.html` — bilingual (中/한) brief for the Gapyeong charter driver.

**Standalone schematic maps** (inline SVG, not geographic)
- `D0-map.html`, `D3a-seongsu-map.html`, `D3a-seongsu-realmap.html` — hand-built
  SVG walking/route diagrams. Header note: positions are *approximate / 示意*,
  not GPS-scaled (`1 walking minute ≈ 75 m`).

**Authoring notes**
- `walking-map-prompts.md` — ChatGPT image-gen prompts (one per day) for producing
  illustrated walking maps, to be saved as `img/d{n}-map.png`.

**Assets** (`img/`)
- Day photos: `img/d{n}_<slug>.jpg` (e.g. `d1_yeonnam.jpg`), ~60 files.
- Souvenir photos: `img/souvenirs/sv_<slug>.jpg`, ~20 files.
- Descriptively-named PNG maps: `img/D0-myeongdong.png`, `img/D2-gapyeong.png`, etc.

### ⚠️ Known map-slot gap
Day pages reference their map as `img/d{n}-map.png` (e.g. `d2.html` → `img/d2-map.png`),
but those exact files **do not exist yet**. The available maps are the
descriptively-named PNGs (`img/D2-gapyeong.png` …) and the standalone
`*-map.html` SVG pages. So the in-page map slots currently render broken/empty.
To fill one: generate via `walking-map-prompts.md`, save as `img/d{n}-map.png`,
or update the `<img src>` to point at the existing descriptive PNG.

## Design System B (the visual language — do not break it)

Every page implements **"Design System B" (`seoul-ds-b.css`)** — a Swiss /
Müller-Brockmann International Typographic style. The canonical spec lives as a
comment block at the top of `index.html`'s `<style>`. The system is **redefined
inline in each file** (there is no shared external stylesheet), so changes must be
applied per-file.

**Tokens** (CSS custom properties, identical across files):
```
--paper:#FFFFFF;  --ink:#111111;  --ink-2:#444;  --ink-3:#7a7a7a;
--red:#E4002B;    /* the ONE Swiss-red accent — never introduce a second accent colour */
--rule:#dcdcdc;   --rule-2:#eaeaea;
--sans:"Inter","Noto Sans TC","PingFang TC",system-ui,sans-serif;
--mono:"Space Mono","IBM Plex Mono",ui-monospace,Menlo,monospace;
```
Fonts load via Google Fonts `<link>` (Inter / Space Mono / Noto Sans TC).

**Idioms**: huge weight-900 day numbers; flush-left modular grid; 8px baseline;
mono kicker/label text (uppercase, letter-spaced, red); **hairline rules instead
of filled boxes**; mono `~N 分 · NNNm` walk-time labels on maps.

**Forbidden** (per the spec comment — do not add these):
> 米色/紙感 (beige/paper texture), 藍紫漸變 (blue-purple gradients), 柔陰影
> (soft shadows), 圓角卡 (rounded-corner cards), **emoji**, Fraunces 斜體 (italics).

When creating or editing a page, **copy the `:root` + base styles from an existing
sibling page** (e.g. `d1.html`) rather than inventing new CSS, so the system stays
consistent.

## Conventions

- **Language**: content is Cantonese/Traditional Chinese. Place/shop names keep the
  original `한글` (Korean) alongside Chinese; subway exits noted as `N번출구`.
- **Self-containment**: each page carries its own CSS inline. No shared JS/CSS files.
- **Mobile-first**: pages use `viewport-fit=cover`, `max-width` ~720–880px wraps,
  and `@media(max-width:…)` breakpoints. Test at phone width.
- **Maps are schematic**, not to scale — always keep the "示意 / APPROXIMATE" disclaimer.

## Git workflow

- Active development branch for this work: **`claude/claude-md-docs-bh54rt`**.
  Develop here; do not push to `main` without explicit permission.
- **Commit message style** (match the existing history): prefix with the day code
  and a terse, specific summary, often in Chinese, e.g.
  `D2: add in-attraction highlights (Nami/Petite France/Garden) + fix Garden hours`.
  Describe the *content* change, not just "update file".
- Push with `git push -u origin <branch>`; after pushing, open a **draft PR** if
  none exists.

## Working tips for assistants

- Edits are content-driven (times, prices, routes, photos, opening hours). Verify
  real-world facts (opening hours, booking links, seasonal notes) before changing
  them — the history shows corrections like winter-vs-summer hours.
- When you change a day's plan, check whether the same info is duplicated in
  `daily-itinerary.html`, `glance-d{n}.html`, the `*-shopping.html` variant, and the
  master `index.html` — keep them consistent.
- Don't introduce a toolchain (no npm, no framework). Keep everything plain HTML/CSS.
