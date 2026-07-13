# FosterCars — project notes

Road-trip car identification game. Snap a passing car, kids get a countdown to guess make/model/year, Claude identifies it via API, points awarded by tapping player buttons.

## Live app (primary version)

- **URL:** https://rwl2411.github.io/car-id-game/ (add to iPhone home screen)
- **Repo:** https://github.com/rwl2411/car-id-game (GitHub Pages, deploy from `main` branch root)
- **File:** single-file `index.html` — all HTML/CSS/JS inline, no build step
- To update: edit `index.html`, commit to `main`; Pages redeploys in ~1–2 min (CDN may cache another minute)

## How it works

- Live camera preview via `getUserMedia` (rear camera); fallback to `<input capture>` if denied
- SNAP → frame frozen on screen → 7s countdown (beeps, last 3 accelerated) runs **in parallel** with the API call
- Calls Anthropic Messages API directly from the browser (`anthropic-dangerous-direct-browser-access` header), model `claude-sonnet-5` (Haiku selectable in ⚙︎ settings)
- Prompt returns strict JSON: make, model, years, trim_or_notes, confidence, zero_to_sixty, horsepower, top_speed, fun_fact, no_car
- Reveal shows name/years/trim + 3 spec tiles (0–60, power, top speed) + fun fact
- Scoring: Make +1, Model +2, Year +2 buttons per player; scores in localStorage
- **Trip log (📋):** every identified car recorded in localStorage; tally view (count × make model, years), Export → downloads `fostercars-trip-log.txt`, Clear button
- All state (API key, model choice, countdown secs, players, scores, log) in localStorage — per device, nothing synced
- API key entered on first launch per device; never committed to the repo (public repo — key scrapers)

## Versions

- v1: camera + countdown + reveal + scoring
- v2 (current): renamed FosterCars, trip log with export, 0–60/hp/top-speed specs in reveal

## Raspberry Pi version (built first, kept as alternative)

- In `car-id-game.zip` / `car-id-game/` folder: Flask app (`app.py`) + touchscreen web UI (`static/index.html`)
- picamera2 3-frame burst, sharpest-frame selection (gradient variance), Chromium kiosk on Pi touchscreen
- Mock mode for desk testing: `MOCK_CAMERA=1 python3 app.py` (canned DeLorean answer without API key)
- Setup details in its README.md

## Ideas not yet built

- Sync trip log/scores across devices (would need a backend or gist)
- Rarity bonus points (uncommon cars worth more)
- Photo gallery of the trip's best spots
