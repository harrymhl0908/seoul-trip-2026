# CLAUDE.md — repo guide

## What this repo is
A **travel-planning kit** plus its worked example. Two layers:
- **Reusable workflow:** the Claude Code Skill at `.claude/skills/travel-planning/`.
- **Worked example:** the Seoul 2026 family trip (`index.html`, `d0`–`d4` + `*-shopping` +
  `glance-*`, `souvenirs.html`, `pre-trip.html`, `daily-itinerary.html`, maps, `img/`).

## When planning or editing a trip
1. **Use the skill.** Read `.claude/skills/travel-planning/SKILL.md` and follow its stages.
   It auto-triggers on trip-planning requests; you can also invoke `/travel-planning`.
2. **Follow the house style.** All itinerary HTML uses the design system in
   `references/DESIGN-SYSTEM.md` (white / ink / Swiss-red `#E4002B`; Inter + Space Mono +
   Noto Sans TC; hairline rules, big numerals, mono metadata). Don't restyle per trip.
3. **Data first.** Capture places via `references/STOP-CARD-SCHEMA.md`, then render through
   `templates/`. One data set feeds the day, glance, map, and calendar — don't duplicate by hand.
4. **Run the quality gate.** Before calling anything done, run
   `references/QUALITY-CHECKLIST.md` + the fresh-agent reader test.

## Hard rules (from real mistakes — see RETROSPECTIVE.md)
- **Never state hours / prices / last-train / closures from memory.** Verify against a live
  source and stamp the date in the footer. (Seoul shipped a wrong AREX last-train time.)
- **Every anchor stop needs a Plan-B** (the `.rain` line).
- **Geocode every stop**; confirm each map deep link resolves (use local script).
- **Record trade-offs** when dropping a stop, so it's reversible.

## Conventions
- **對話語言：與用戶溝通一律用繁體中文（香港．粵語書面語），語氣參照本 repo 既有規劃文件（例：「邊度買」「唔好錯過」「落單」）。技術名詞／程式碼可保留英文。Communicate with the user in Traditional Chinese (Hong Kong Cantonese), matching the tone of the existing planning files.**
- Artifacts are **self-contained HTML** (inline CSS, no build, works offline).
- Prices: **local currency primary, home currency secondary**, FX rate stamped + dated.
- Place names show **local script** for map navigation; maps use the destination's app
  (Naver in Korea — Google Maps is unreliable there).
- Images: ≤720px wide, ~72% JPEG, in `img/`.
- Footer on every page: date verified · FX · map app · credit.

## Files not to "tidy"
The duplicate-looking `glance-*.html` and `*-shopping.html` are intentional generated
variants of the same trip data — regenerate from data, don't hand-merge. `index.html` and
`seoul-trip-artifact-B.html` are large (embedded artifact); don't read them whole.
