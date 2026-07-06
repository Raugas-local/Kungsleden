# Kungsleden Companion — pilot page (brief)

## Goal

A small, purely demonstrative piece. The goal is not to build the app, but to
show on a small data sample what this can look like and do -- before we start
on the full PWA.

## Scope (deliberately small)

- Two short, disconnected stretches of the route, shown as separate sections
  in the itinerary (not the whole route):
  - Segments #1-#3 (`STF Kvikkjokk -> STF Pårte cabin -> Stopover cabin
    Laitaure (shore) -> Aktse (shore)`)
  - Segments #7-#9, around Vakkotavare (`Stopover cabin Svijnne (shore) ->
    STF Sitojaure cabin - Vaggevárasj (shore) -> STF Saltoluokta Station
    (shore) -> STF Vakkotavare cabin (shore)`)
  - Segments #4-#6 are skipped between the two stretches; the itinerary
    shows a short italic gap note where they would go, instead of pretending
    the trail is continuous
- Show it as a simple list of legs: from-to, distance, ascent/descent
- Show point-to-point trail kilometrage per leg (`km X -> Y`, plus the delta).
  Boat crossings don't advance the trail-km marker (delta = 0 by design, per
  `route.json`'s `note_on_direction`), which is different from the leg's own
  physical `distance_km` (only recorded for some boat legs, e.g. segment #3 =
  3 km; segments #7 and #9 have no recorded crossing distance -- shown as
  such, not guessed)
- Show **one specific alert** from `alerts.json` (the Vakkotavare -> Kebnats
  bus schedule gap) as a clearly distinct warning banner -- this is the best
  demonstration that the app can pull a critical piece of information out of
  the data instead of burying it in ordinary text

## Out of scope for this pilot (for now)

- No offline map, no GPS, no GPX
- No on-device data storage (journal, checklist)
- No PWA installation
- Not the whole route, just these two samples

## Data

- A small, hand-picked slice of `route.json` (waypoints + segments #1-#3 and
  #7-#9)
- One entry from `alerts.json` (`vakkotavare_kebnats_bus_gap_sep2026`)
- Full contents of the boat/bus files relevant to the two stretches:
  `boat_aktse_laitaure.json`, `boat_sitojaure_svijnne.json`,
  `boat_kebnats_saltoluokta.json`, `bus_vakkotavare_kebnats.json`

## Technical

- A single standalone HTML file (CSS + JS inline), no build step
- Can be opened directly in a browser, later uploaded to GitHub Pages unchanged

## Language

- UI text in English
- The underlying data stays in English too, as already agreed -- the page
  just selects and displays a slice of it

## Definition of done

You see: two short itinerary sections (a gap note between them where
segments #4-#6 would go), each waypoint and leg as its own compact badge
card with point-to-point kilometrage, one clearly distinct warning banner, a
bottom tab bar (itinerary / alerts / crossings), and drill-down detail pages
showing the full source data as small tables. Nothing beyond what's
hand-picked from `route.json`, `alerts.json`, and the relevant
`boat_*.json` / `bus_*.json` files for these two stretches.

## Badge set (compact pills on every card)

Show all applicable badges, always -- no filtering, no hiding "boring" ones.
Small pills, 11px font, `border-radius: 999px`, one row that wraps.

### Waypoint (hut) badges
| Badge | Source field |
|---|---|
| `hut` / `crossing point` (solid blue pill, same treatment as the leg type badge below) | `waypoint.type` |
| `no shop` / `small shop` / `large shop` / `full shop` | `waypoint.shop.size` |
| `power` / `no power` | `waypoint.shop.electricity` |
| `alt name` (click-through to alt_names list) | `waypoint.alt_names.length > 0` |

### Leg/segment badges
| Badge | Source field |
|---|---|
| `hike` (solid green pill) / `boat` (solid blue pill) | `segment.type` |
| `X km` | `segment.distance_km` |
| `↑X m` | `segment.ascent_m` |
| `↓X m` | `segment.descent_m` |
| `lake: Name` (boat legs only) | `segment.lake` |
| `shelter` | `segment.shelters.length > 0` |
| `⚠ alert` (red/danger color) | `segment.critical_alert_id` present |

Nested objects (like `season.regular_timetable`) never render as badges or
inline key-value dumps -- those stay on the detail/sub-detail page as a
proper small table (see navigation structure above).

### Card title convention

Itinerary cards are prefixed by kind, so the two card types are readable at a
glance even without the badge colors: waypoint cards start with `Waypoint:`,
leg cards start with `Leg #<segment order>:`.

## Navigation

Bottom tab bar, 3 tabs: itinerary (default), alerts (red badge = count of
unresolved alerts), crossings (list of the boat_*.json / bus_*.json files
relevant to the two stretches in scope, as cards: `aktse_laitaure`,
`sitojaure_svijnne`, `kebnats_saltoluokta`, `vakkotavare_kebnats_bus`).
Leg detail pages still drill through to a specific alert/contact contextually
-- same destination pages, just reached a second way.

## Print / PDF export

Use the browser's native print (Ctrl+P / Save as PDF), not a JS library --
works fully offline since it's just CSS, no extra weight in the app.
Add a `@media print` stylesheet covering itinerary + alerts + crossings:
hide the tab bar and any buttons, keep text readable on paper.
Intended use: the person prints/saves this to PDF once, while they still
have signal, as a paper backup before heading into the mountains -- not
something meant to run without connectivity for the first time out there.
