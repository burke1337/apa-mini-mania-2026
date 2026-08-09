# MiniMania 2026

A searchable, filterable version of the MiniMania tournament schedule for the 2026 APA World Pool Championships at the Westgate Las Vegas, August 4–14.

The official guide is a 24-page flipbook. All 940 events are there, but finding the ones you can actually enter means reading the whole thing line by line. This is the same data as a single offline web app: filter by day, format, skill level, entry fee, and field size, and star the ones you're planning to play.

**Unofficial.** Not affiliated with or endorsed by the American Poolplayers Association. Always confirm at the registration desk before you commit — times and field sizes change on site.

## Features

- **Search** by event number, format, skill level bracket, start time, or fee
- **Filters** for day, format (8-Ball, 9-Ball, and both Scotch Doubles variants), entry fee, field size, and time of day
- **Skill level highlighting** — set your 8-Ball and 9-Ball levels once and your number lights up inside every bracket, so you can scan a day without reading it
- **"Only what I can enter"** hides everything you're not eligible for
- **Scotch doubles math** — combined-limit events show the highest-rated partner you can bring
- **Starred shortlist**, saved between sessions
- **Fully offline** once installed. Opens on today's date during the tournament.

## Install on iOS

1. Enable GitHub Pages for this repo (Settings → Pages → deploy from `main`, folder `/`)
2. Open the resulting URL in Safari on your phone, on any connection, and let it load
3. Share → Add to Home Screen

The service worker caches everything on that first load, so it works with no signal afterward — which matters at the Westgate, where the wifi struggles under 16,000 people. Android is the same flow via Chrome's install prompt.

You can also just open `index.html` directly from the Files app. Everything works except the home screen icon, and starred events may not persist under `file://`.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The entire app — markup, styles, schedule data, and logic in one file |
| `sw.js` | Service worker, cache-first (the schedule never changes) |
| `manifest.webmanifest` | Standalone display mode and icons |
| `icon-*.png`, `apple-touch-icon.png` | Home screen icons |

No build step, no dependencies, no network calls at runtime.

## The data

All 940 events are embedded in `index.html` as a compact text block near the top of the `<script>`, parsed at load:

```
#Tuesday, August 4|2026-08-04
@5:00 PM
1,16,9,345,20
```

Day header, then a start time, then rows of `event, entrants, format, skill, fee`. Format codes are `8`, `9`, `S8`/`S9` for Scotch Doubles, and `L8`/`L9` for the League Operator and Player events. Skill is a digit string (`345` = the 3-4-5 bracket), `A` for Any SL, or `Lnn` for a combined Scotch Doubles limit.

Transcribed by hand from the published event guide. Event numbers run 1–940 with no gaps. If you spot a discrepancy against the official program, open an issue.

### Eligibility logic

Standard events match your skill level against the bracket. Scotch doubles are scored on a *combined* limit, so eligibility depends on your partner: you qualify if your level plus at least an SL 1 partner stays under the cap, and the app shows you the highest partner you can bring.

## License

MIT for the code. The schedule data is the APA's.
