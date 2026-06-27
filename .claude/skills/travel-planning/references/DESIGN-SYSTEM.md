# Design System — Travel Itinerary House Style

A Swiss-typographic, print-inspired system. White paper, near-black ink, a single red
accent, hairline rules instead of boxes, big sans-serif numerals, mono for metadata.
It reads like a well-set magazine and stays legible on a phone in sunlight. Every artifact
is **one self-contained HTML file with inline CSS** — no external stylesheet, no JS build,
works offline. Fonts load from Google Fonts but the page degrades gracefully without them.

## Tokens (`:root`) — paste verbatim into every file

```css
:root{
  --paper:#fff;--ink:#111;--ink-2:#444;--ink-3:#7a7a7a;--red:#E4002B;
  --rule:#dcdcdc;--rule-2:#eaeaea;
  --sans:"Inter","Noto Sans TC","PingFang TC",system-ui,sans-serif;
  --mono:"Space Mono","IBM Plex Mono",ui-monospace,Menlo,monospace;
}
```

Font load (in `<head>`):
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800;900&family=Space+Mono:wght@400;700&family=Noto+Sans+TC:wght@400;700;900&display=swap" rel="stylesheet">
```

## Rules of the system

- **Color:** ink for text, `--red` (#E4002B) for *one* accent per context (kickers, "必到/必", anchors, callout borders, link arrows). Never use red as a fill behind body text except the dark weather/zone bars.
- **Type scale:** `body` 15px / line-height 1.5. Day numeral 72–86px/900. Section `h2` 14–15px/900 uppercase with a 2px ink underline. Stop name 17–19px/800. Metadata always in `--mono`, ~9–11px, uppercase, letter-spaced.
- **Structure, not boxes:** separate items with `1px solid var(--rule)` bottom borders, not cards/shadows. The only filled bars are the black weather banner (`.wx`) and black zone headers (`.zonehd`).
- **Numerals lead.** Each stop/shop/item starts with a large 900-weight index numeral in its own grid column.
- **Bilingual:** primary language in the name; local script in `--mono` `--ink-3` as a subtitle (`.ko` / `.t`) so it's tappable into the map app.
- **Layout:** `.wrap{max-width:720px;margin:0 auto;padding:0 18px}` (760px for checklist/budget pages). Mobile-first; one column.
- **Photos:** `.ph` — `width:100%;max-width:260px;aspect-ratio:16/10;object-fit:cover;filter:grayscale(.08)`, `loading="lazy"`. Keep images ≤720px wide, ~72% JPEG, in `img/`.
- **Footer:** every page ends with a mono `.foot` — date verified, FX rate, "maps via <local app>", and a credit line.

## Component classes (canonical CSS)

### Weather banner (top of a day) — `.wx`
```css
.wx{background:var(--ink);color:#fff;padding:12px 18px;display:flex;flex-wrap:wrap;gap:4px 16px;align-items:baseline}
.wx .d{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.14em;text-transform:uppercase}
.wx .t{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.04em;opacity:.85}
.wx .rn{color:#fff;background:var(--red);font-family:var(--mono);font-size:10px;font-weight:700;letter-spacing:.06em;padding:1px 7px;text-transform:uppercase}
```

### Masthead (day number + title)
```css
.kicker{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.2em;text-transform:uppercase;color:var(--red);margin-top:18px}
.masthead{display:grid;grid-template-columns:auto 1fr;column-gap:18px;align-items:start;border-top:2px solid var(--ink);padding-top:10px;margin-top:6px}
.daynum{font-weight:900;font-size:72px;line-height:.78;letter-spacing:-.045em}
.daynum small{display:block;font-family:var(--mono);font-size:12px;letter-spacing:.1em;color:var(--ink-3);font-weight:700;margin-top:6px}
.title{font-weight:900;font-size:clamp(22px,4.8vw,32px);line-height:1.04;letter-spacing:-.022em}
.sub{font-size:13.5px;line-height:1.5;color:var(--ink-2);margin-top:9px;max-width:50ch}
@media(max-width:520px){.daynum{font-size:54px}}
```

### Callout (route summary / tips / ⭐別錯過)
```css
.callout{display:grid;grid-template-columns:auto 1fr;gap:12px;align-items:baseline;padding:11px 0 11px 14px;margin:16px 0;border-left:3px solid var(--red)}
.callout .ct{font-family:var(--mono);font-size:10px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--red);white-space:nowrap}
.callout .cv{font-size:13.5px;line-height:1.55;font-weight:600}
.callout.ink{border-left-color:var(--ink)}.callout.ink .ct{color:var(--ink)}
```
Use `.callout` (red) for highlights/weather, `.callout.ink` for money/fallback notes.

### Section header — `.sec h2`
```css
.sec h2{font-weight:900;font-size:14px;letter-spacing:-.005em;text-transform:uppercase;padding-bottom:7px;border-bottom:2px solid var(--ink);margin-top:26px;display:flex;align-items:baseline;gap:8px;flex-wrap:wrap}
.sec h2 .hn{font-family:var(--mono);font-size:11px;color:var(--red);font-weight:700}
.sec h2 .zs{font-family:var(--mono);font-size:10px;color:var(--ink-3);font-weight:400;letter-spacing:.04em}
```

### Stop card — `.stop` (the core itinerary unit)
```css
.stop{display:grid;grid-template-columns:40px 78px 1fr;column-gap:14px;align-items:start;padding:15px 0 13px;border-bottom:1px solid var(--rule)}
.stop:last-child{border-bottom:none}
.idx{font-weight:900;font-size:32px;line-height:.82;letter-spacing:-.04em}
.stop.opt .idx{color:var(--red);opacity:.5}
.when{font-family:var(--mono);font-size:14px;font-weight:700;line-height:1.1}
.when .must{display:inline-block;font-size:8.5px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:#fff;background:var(--ink);padding:1px 5px;margin-top:6px}
.when .can{display:inline-block;font-size:8.5px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:var(--red);border:1px solid var(--red);padding:1px 5px;margin-top:6px}
.nm{font-weight:800;font-size:17px;line-height:1.12;letter-spacing:-.01em}
.nm .ko{font-family:var(--mono);font-weight:400;font-size:12px;color:var(--ink-3);letter-spacing:.02em}
.usp{font-size:12.5px;line-height:1.5;color:var(--ink-2);margin-top:5px;max-width:48ch}
.row{margin-top:7px;font-size:11.5px;line-height:1.5;display:flex;flex-wrap:wrap;gap:3px 14px}
.row .k{font-family:var(--mono);font-size:9px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:var(--ink-3)}
.nav{margin-top:7px;font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.03em}
.nav a{border-bottom:1px solid var(--ink);text-decoration:none;margin-right:14px}
.nav a::after{content:" →";color:var(--red)}
.rain{margin-top:6px;font-size:11.5px;line-height:1.45;color:var(--ink-2);padding-left:11px;border-left:2px solid var(--rule)}
.rain .k{font-family:var(--mono);font-size:9px;font-weight:700;letter-spacing:.06em;text-transform:uppercase;color:var(--red);margin-right:5px}
.ph{display:block;width:100%;max-width:260px;aspect-ratio:16/10;object-fit:cover;margin-top:10px;background:var(--rule-2);filter:grayscale(.08)}
.cmt{margin-top:8px;padding-left:11px;border-left:2px solid var(--red);font-size:12px;line-height:1.5;color:var(--ink)}
.cmt .src{display:block;margin-top:2px;font-family:var(--mono);font-size:9.5px;color:var(--ink-3);letter-spacing:.02em}
```
`.stop.opt` = optional stop (red faded numeral + `可選` badge). `.must` = required, `.can` = optional. `.rain` carries the Plan-B/contingency. `.cmt` is a sourced quote.

### Shopping card — `.shop` + `.zonehd`
```css
.zonehd{font-family:var(--mono);font-size:11px;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:#fff;background:var(--ink);padding:7px 12px;margin-top:30px}
.shop{display:grid;grid-template-columns:46px 1fr;column-gap:14px;align-items:start;padding:18px 0 17px;border-bottom:1px solid var(--rule)}
.snum{font-weight:900;font-size:30px;line-height:.82;letter-spacing:-.04em}
.stitle{font-weight:900;font-size:18px;line-height:1.16;letter-spacing:-.015em}
.stitle .t{display:block;font-family:var(--mono);font-size:10.5px;font-weight:700;letter-spacing:.04em;color:var(--red);margin-top:5px}
.sd{font-size:12.5px;line-height:1.6;color:var(--ink-2);margin-top:9px}
.brands{font-size:11.5px;line-height:1.7;margin-top:10px;padding:10px 12px;background:var(--rule-2)}
.brands .bh{font-family:var(--mono);font-size:9px;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:var(--ink-3);margin-right:5px}
.fam{font-size:12px;line-height:1.55;margin-top:10px;padding:7px 0 7px 11px;border-left:3px solid var(--red);color:var(--ink);font-weight:600}
.meta2{margin-top:11px;font-family:var(--mono);font-size:10.5px;line-height:1.7;color:var(--ink-3);display:flex;flex-wrap:wrap;gap:1px 16px}
.meta2 .k{color:var(--red);font-weight:700;margin-right:4px}
```
`.fam` carries the persona-targeted "⭐ 別錯過" note. `.meta2` carries address / transit exit / hours.

### Footer — `.foot`
```css
.foot{margin-top:30px;padding-top:12px;border-top:2px solid var(--ink);font-family:var(--mono);font-size:10px;line-height:1.7;color:var(--ink-3);letter-spacing:.02em}
.foot b{color:var(--ink)}
```

### Checklist / packing extras (pre-trip page)
`.facts` (4-col trip facts strip), `.item` (numbered booking item), `.book` (mono bordered booking link, `:hover` turns red), `.fill` (red-left-border "fill in when booking" block), `.done li` (DONE-stamped completed list), `.pack label` (checkbox packing rows persisted to `localStorage`). Full CSS lives in `templates/pre-trip.html`.

### Other pages with their own component CSS (in the template files)
- **Glance** (`templates/glance.html`): a denser variant — `.tl`/`.t-row` compact timeline, `.t-when`/`.t-nm` (with `.anchor` badge), `.hi` highlight blocks. Same tokens, tighter scale.
- **Budget ledger** (`templates/budget-ledger.html`): `.total` big-number strip, `table`/`th`/`td.cat`/`td.num`/`tfoot` ledger, `.brow`/`.st` (`.todo`/`.booked`) booking-tracker rows. Adds one token `--ok:#0a7d34` for the "booked" state.
- **Driver/host brief** (`templates/driver-brief.html`): a deliberately *different*, high-contrast card style (rounded cards, blue nav buttons) optimized for reading in a moving car — it does not use the Swiss paper style. Keep it as-is.

When you need these, open the template; don't re-derive the CSS.

## How to use
Open the relevant `templates/*.html`, keep the `:root` + component CSS, and replace the
placeholder content. Don't restyle per trip — consistency across trips is the point. New
component? Add it to this file so the next trip inherits it.
