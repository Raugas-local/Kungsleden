Kungsleden Companion — pilot page (brief)

Goal

A small, purely demonstrative piece. The goal is not to build the app, but to
show on a small data sample what this can look like and do -- before we start
on the full PWA.

Scope (deliberately small)


Use just a short stretch of the route around Vakkotavare (segments #7-#9
from route.json: Sitojaure -> Saltoluokta -> Vakkotavare), not the whole
route
Show it as a simple list of legs: from-to, distance, ascent/descent
Show one specific alert from alerts.json (the Vakkotavare -> Kebnats
bus schedule gap) as a clearly distinct warning banner -- this is the best
demonstration that the app can pull a critical piece of information out of
the data instead of burying it in ordinary text


Out of scope for this pilot (for now)


No offline map, no GPS, no GPX
No on-device data storage (journal, checklist)
No PWA installation
Not the whole route, just this one sample


Data


A small, hand-picked slice of route.json (waypoints + segments #7-#9)
One entry from alerts.json (vakkotavare_kebnats_bus_gap_sep2026)


Technical


A single standalone HTML file (CSS + JS inline), no build step
Can be opened directly in a browser, later uploaded to GitHub Pages unchanged


Language


UI text in English
The underlying data stays in English too, as already agreed -- the page
just selects and displays a slice of it


Definition of done

You see: a short list of legs + one clearly distinct warning banner. Nothing more.