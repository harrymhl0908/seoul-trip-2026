# Quality Gate — Pre-Publish Verification Checklist

Run this over the whole kit at **Stage 4**, before telling the user it's done. It exists
because real itineraries fail on small, checkable facts. Every item below traces to an
actual error caught (and fixed) on the Seoul 2026 plan — see `RETROSPECTIVE.md`.

The rule behind all of it: **AI must not state a time-sensitive travel fact from memory.**
Verify against a live source, or mark it "verify on arrival." Never present a guess as fact.

## A. Time-sensitive facts (the #1 failure mode)

- [ ] **Opening hours & closed days** for every stop/shop are verified against a live source (official site / map app) and **dated**. Seasonal/summer hours checked for the actual travel month. *(Seoul: Garden summer hours 19:00 vs. winter light-fest were wrong.)*
- [ ] **Prices / ticket prices** carry a "as of <date>, confirm at checkout" stamp. *(Seoul: Seoul Sky 31k vs 33k confusion.)*
- [ ] **Last train / last bus / transfer cut-offs** verified to the minute, with the source. *(Seoul: AREX last direct train is 22:40, not 23:00 — a 20-min error that would have stranded the group on a red-eye night.)*
- [ ] **Closures / renovations / one-off events** on the travel dates checked.
- [ ] **Sell-out / queue risk** noted for hyped food/tickets, with a mitigation (go early / get a number / book ahead). *(Seoul: Dubai cookie, London Bagel.)*
- [ ] Every verified fact is **dated** in the page footer (`核實 <date>`), with the source named.

## B. Geocoding & navigation

- [ ] Every stop's **address resolves** in the destination's primary map app (Naver in Korea; Google/AMap/etc. elsewhere). *(Seoul: ÅLAND Myeongdong and the Musinsa old-vs-new-store address both needed fixing.)*
- [ ] Every `map_url` deep link **opens the right place** (use local script in the query).
- [ ] **Station exits and walk times** between consecutive stops are realistic, not guessed.

## C. Resilience (every anchor survives a bad day)

- [ ] Every **anchor** stop has a concrete **Plan-B** (rain/closed/queue) in its `.rain` line.
- [ ] Each day has at least one **all-indoor fallback** for a rain day.
- [ ] Weather outlook for the dates is reflected (e.g. monsoon onset → indoor-weighted days).

## D. Budget & bookings

- [ ] Budget **reconciles**: per-stop prices roll up to the day and trip totals; FX rate stamped + dated.
- [ ] Every **booking** in the tracker has a status; "to book" items have a working link and the exact details to fill in.
- [ ] **Entry/visa** status confirmed for the travelers' passports for the travel dates.

## E. Consistency & format

- [ ] **One format**: day/shopping/glance views derive from the same data — no contradictions between them. Don't hand-maintain duplicate "glance" files that drift; regenerate them. *(Seoul: glance files + inline-vs-separate shopping format drifted.)*
- [ ] Every **dropped/changed** stop has its trade-off recorded so it's reversible. *(Seoul: South Gate Market dropped for London Bagel timing — flagged, not silently deleted.)*
- [ ] Design system applied uniformly (`DESIGN-SYSTEM.md`); footers present with date + FX + map-app + credit.
- [ ] All **links resolve** (no 404s); images load and are ≤720px/optimized.

## F. Logistics state, not intent

- [ ] Pre-trip checklist tracks **confirmation state** (booked? called? installed?), not just "to do." *(Seoul: hotel pickup/luggage and app installs were left as untracked templates.)*
- [ ] Apps the destination requires are listed (e.g. Korea needs Naver Map + Papago + Kakao T — Google Maps is unreliable there).

## G. Reader test (catch blind spots)

- [ ] Hand a **fresh sub-agent** only the artifacts (no chat context) + 4–6 traveler questions:
  *"How do I get from stop 5 to 6?" · "What's the rain plan for day 3?" · "What time must we leave for the airport?" · "Where do I buy <souvenir>?" · "How much is day 2 total?"*
  It must answer from the kit alone. Any miss → fix the gap, re-run.

## Output of the gate
Report two lists to the user: **Verified** (fact → source → date) and **Verify on arrival**
(facts that couldn't be confirmed live). Do not silently present unverified facts as confirmed.
