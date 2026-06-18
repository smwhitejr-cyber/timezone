# Time Zone Sync

A tiny, dependency-free web app for coordinating times across multiple time
zones. Edit the date or time in **any** zone and every other zone updates
instantly, with correct Daylight Saving Time handling.

It started life as a fixed five-zone tool (Sydney, Brisbane, India, US Eastern,
US Central) and is now fully customizable.

## Features

- **Bidirectional sync** — change any field and the rest follow.
- **Correct DST** — uses the browser's `Intl` time-zone database, so zones like
  Sydney switch between AEDT/AEST automatically while Brisbane stays on AEST.
- **Live analog clocks** — an animated clock per zone with a digital readout,
  date, and current zone abbreviation (EDT, IST, AEST, …).
- **Customizable zones** — add any IANA time zone, remove zones, and reorder
  them with the ▲ ▼ buttons. Your list is saved in the browser.
- **Light / dark theme** — toggle in the top-right; your choice is remembered
  and it respects your OS preference on first visit.
- **Reset to now** — the ⟳ button, or press <kbd>R</kbd>.

## Run it

No build step, no install. Just open `index.html` in any modern browser.

To serve it locally instead:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## How it works

Everything lives in the single `index.html` file:

- A `zones` array is the single source of truth; each entry holds its IANA
  time zone, label, accent color, and flag. It's persisted to `localStorage`
  (key `tzsync.zones.v1`) and falls back to the default five if nothing is
  stored or the data is invalid.
- Wall-clock times are converted to absolute instants via a small offset
  routine (`zonedWallTimeToInstant`) that resolves DST ambiguity, then every
  zone is rendered from that single instant.
