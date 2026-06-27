# Map Generation

Two kinds of map per the trip's needs: (1) a per-day **illustrated route map** (fast,
pretty, good enough for orientation) and (2) a **real OSM/Leaflet walking map** for dense
walking clusters where accuracy matters. Both drop into the day page's map slot.

## 1. Illustrated route map (image generator)

Generate one image per day, save as `img/d{n}-map.png`, referenced from the day page:
```html
<div class="sec"><h2><span class="hn">步行地圖</span> 全日動線 <span class="zs">每站撳 Naver 連結導航</span></h2></div>
<div style="margin:6px 0 4px;overflow-x:auto"><img class="ph" src="img/d{n}-map.png" alt="D{n} 步行地圖" style="width:100%;max-width:680px;height:auto;display:block;border:1px solid var(--rule);background:var(--rule-2)"></div>
<div class="rain"><span class="k">Map</span>插畫式地圖，實際導航撳各站 Naver 連結。</div>
```

**Shared style prefix** (prepend to every day's prompt):
> Clean illustrated top-down city walking map, soft muted colours, off-white background,
> thin labelled streets and subway lines, a clear red dashed walking route connecting
> numbered pins (①②③…) in order, small recognisable landmark icons, bilingual labels
> (local script + English), legible mono labels, no people, 4:3.

**Per-day prompt** = style prefix + the ordered stops with their **real local-script names,
station exits, and sequence** so the generator places them roughly right. Pattern:
> Map of **<district> walk**, <city>. Start <lodging>. Numbered stops in order:
> ① <name_local> (<gloss>) → ② … → ⑫ …. Mark <station> exits <n>. <layout hint>.

Multi-cluster day → describe LEFT/RIGHT clusters joined by a subway-line arrow (e.g. Seoul
D3 = 성수 cluster ↔ 2호선 ↔ 잠실 cluster). Regional charter day → "regional map, spots far
apart, connect by driving route," not a walking route.

> ⚠️ Illustrated maps are **not geographically precise**. For true accuracy, screenshot the
> local map app and annotate, or use the Leaflet map below. State this limitation on the page.

See the Seoul `walking-map-prompts.md` for five complete worked examples (D0–D4).

## 2. Real OSM / Leaflet walking map (for dense clusters)

For tight walking districts, build an interactive HTML map with real street tiles and
geocoded pins. Self-contained, offline-degrading, opens in any browser.

- **Tiles:** OpenStreetMap raster via Leaflet (CDN `leaflet.css` + `leaflet.js`).
- **Pins:** numbered markers at each stop's lat/lng (geocode from the verified address).
- **Route:** draw the walking path. Quick = a polyline through the stops in order; accurate =
  fetch a real walking route from **OSRM** (`router.project-osrm.org/route/v1/foot/…`) and draw
  the returned geometry (this is what `D3a-seongsu-realmap.html` does).
- **Popups:** stop number + name (local script) + the map-app deep link.
- Keep the same palette: red route line (`#E4002B`), dark pins, off-white surround.

Naming: `D{n}-<cluster>-map.html` (illustrated-data/leaflet) and
`D{n}-<cluster>-realmap.html` (OSRM real paths), matching the Seoul examples.

## Which to use
- Orientation / pretty overview / printed feel → **illustrated**.
- "Don't get lost in this 12-stop alley crawl" → **Leaflet/OSRM real map**.
- Big, spread-out charter day → illustrated regional map + the bilingual `driver-brief.html`.
