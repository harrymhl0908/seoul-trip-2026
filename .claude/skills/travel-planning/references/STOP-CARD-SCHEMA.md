# Data Schema — Stops, Shops, Souvenirs

The itinerary is **data first, HTML second**. Capture each place with these fields, then
render it through the matching template component. The same stop data feeds the day view,
the glance view, the map, and the calendar — fill it once.

Mark every field tagged 🔎 as **time-sensitive**: it must be verified against a live source
and dated before publishing (see `QUALITY-CHECKLIST.md`). Never publish a 🔎 field from memory.

---

## Stop (a place on a day's route) → renders as `.stop`

| Field | Required | Notes |
|---|---|---|
| `index` | ✓ | Order on the day (1, 2, 3…). |
| `time` | ✓ | `HH:MM` or label (`午餐 12:30`). Mono. |
| `status` | ✓ | `must` (必到) or `can` (可選/optional). Drives badge + faded numeral. |
| `name` | ✓ | Primary-language name. |
| `name_local` | ✓ | Local script (한글/日本語/…) — tappable into map app. |
| `usp` | ✓ | 1–2 sentence pitch: why this place, what's the one thing. |
| `price` 🔎 | ✓ | Local currency primary + home-currency secondary (e.g. `₩4,000–7,000 · HKD 187`). |
| `walk` | ✓ | Transit from previous stop: walk minutes or line+exit. |
| `persona_tag` | ✓ | Who it's for (爸爸/媽媽/仔/全家 or the trip's personas). |
| `map_url` | ✓ | Local map-app deep link (Naver: `https://map.naver.com/p/search/<name_local>`). |
| `contingency` | ✓ | The Plan-B / weather line (`.rain`). What to do if rain / closed / queue. |
| `hours` 🔎 | ✓ | Opening hours + closed days. Often lives on the card or shop meta. |
| `photo` | – | `img/…` local path, ≤720px. |
| `quote` + `source` | – | A short sourced review snippet (`.cmt` + `.src`). Keep the source. |
| `anchor` | – | Boolean: is this the day's anchor (timed/sunset/reservation)? Anchors *must* have a strong `contingency`. |
| `tradeoff` | – | If this replaced/dropped another option, note why (reversibility). |

**Golden-hour stops:** set `time` from the destination's actual sunset for that date 🔎.

## Shop (a retail stop in a shopping district) → renders as `.shop`

| Field | Required | Notes |
|---|---|---|
| `num` | ✓ | Order in the district (01, 02…). |
| `title` + `tagline` | ✓ | Shop name + one-line `.t` pitch (what's special / new / for whom). |
| `desc` | ✓ | ~80–130 words: what to buy, why it's worth it, quirks. |
| `brands` | – | Grouped brand/category list (`.brands`, with `.bh` group headers). |
| `fam` | ✓ | The "⭐ 別錯過" persona-targeted note: who should not miss this and the rough price. Use `👀 順遊` for nice-to-have. |
| `address` 🔎 | ✓ | Street address (local script). |
| `transit` | ✓ | Station + exit number. |
| `hours` 🔎 | ✓ | Hours + closed day. Many shops open late (11:00) — note it; it affects sequencing. |

## Souvenir (a buy) → renders in `souvenirs.html`

| Field | Required | Notes |
|---|---|---|
| `name` + `name_local` | ✓ | Product/brand. |
| `pitch` | ✓ | Why it's a good buy / who it's for. |
| `price` 🔎 | ✓ | Local + home currency. |
| `where` | ✓ | **Cross-map to the day/place** it's found on the route (the souvenir guide is not a separate wishlist — it points back into the itinerary). |
| `tags` | – | e.g. `hot 2026`, `NEW`, `multi-source` consensus. |
| `sources` | – | The media/blogs it was curated from (attribution). |
| `photo` | – | `img/souvenirs/…`. |

## Day (assembles stops) — header fields

`day_no`, `date` 🔎(weekday), `district(s)`, `title`, `sub` (the day's narrative in 1–2 sentences),
`weather` 🔎 (condition + a `.rn` route summary), `route_summary` (`.callout`), and the ordered `stops[]`.
Each day should contain: 1–2 **anchors**, one **food centerpiece**, and one **flex/serendipity** window.

## Booking (for pre-trip + budget tracker)

`item`, `who_books`, `price` 🔎, `details_to_fill` (exactly what to type at checkout),
`booking_url` + `alt_url`, `status` (todo / booked), `confirmation_no`, `date_needed`.
Time-critical logistics (airport transfer, last train 🔎, timed tickets 🔎) live here first.
