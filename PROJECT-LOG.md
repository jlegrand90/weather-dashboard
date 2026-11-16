# Weather Dashboard — Project Log

**Last updated**: November 15, 2026
**Status**: Working on mobile (web app) and desktop

---

## Locations

| Item | Path |
|------|------|
| GitHub repo (live) | `/Users/Josh/Documents/OS/weather-dashboard/index.html` |
| GitHub remote | `git@github.com:jlegrand90/weather-dashboard.git` |
| GitHub Pages URL | `https://jlegrand90.github.io/weather-dashboard/` |
| Project log | `/Users/Josh/Documents/OS/weather-dashboard/PROJECT-LOG.md` |

**Note**: The old `Weather-Radar/` duplicate folder was deleted on 2026-11-15. `weather-dashboard/` is the only copy.

---

## What It Does

- Live storm radar (Windy.com embedded)
- Current conditions (temp, feels like, humidity, wind, pressure, UV, visibility, dew point)
- Hourly forecast (24 hours, wind, humidity, UV)
- 7-day forecast (highs, lows, rain chance, sunrise/sunset)
- Sun & moon data (phase, sunrise, sunset)
- Outdoor conditions summary (flood risk, driving, flights, air quality, temp stress, wind impact)
- NOAA weather alerts (US only)
- City search with geocoding
- Auto-geolocation on load
- All temperatures in Fahrenheit

## Design

- Dark warm theme: background #1c1610, cards #2d2418, borders #3d3428
- Accents: honey oak #b8956a, sage #7a8a6a, alert red #c75b39, info blue #6b9bb8
- Mobile responsive: breakpoints at 768px and 480px

---

## Known Quirks & Fixes

### Mobile Safari iframe rendering
**Problem**: Windy radar iframe does not render on initial page load in iOS Safari or web app mode.
**Solution**: Create iframe dynamically via JavaScript, then auto-trigger by simulating a click on the Precipitation button. Retries at 500ms, 1500ms, 3000ms, and on first touchstart.

### Web app mode (Add to Home Screen)
**Problem**: iOS web apps block iframes from loading without a user gesture.
**Solution**: Synthetic `MouseEvent('click')` on the radar button tricks the OS into treating it as user-initiated. Works in both Safari and web app mode.

### API hanging on "loading..."
**Problem**: Fetch to Open-Meteo could hang indefinitely on slow connections.
**Solution**: 15-second timeout via `Promise.race`. All sections show "Failed to load. Tap Refresh." on timeout or error.

### `sandbox` attribute on iframe
**Problem**: Adding `sandbox="allow-scripts allow-same-origin"` broke Windy entirely on mobile.
**Solution**: Do not use sandbox attribute on the Windy iframe.

### HTML entity encoding
**Problem**: Using `&amp;` in JavaScript template literals corrupts the URL.
**Solution**: `&amp;` only in HTML attributes. JavaScript strings use plain `&`.

---

## APIs Used

| Service | Endpoint | Purpose | Key? |
|---------|----------|---------|------|
| Open-Meteo | `api.open-meteo.com/v1/forecast` | Weather data | No |
| Open-Meteo Geocoding | `geocoding-api.open-meteo.com/v1/search` | City → lat/lon | No |
| Open-Meteo Air Quality | `air-quality-api.open-meteo.com/v1/air-quality` | AQI, PM2.5 | No |
| NOAA | `api.weather.gov/alerts/active` | US weather alerts | No |
| Windy | `embed.windy.com/embed.html` | Radar iframe | No |

---

## Workflow

1. Edit `/Users/Josh/Documents/OS/weather-dashboard/index.html`
2. Test locally by opening in browser
3. `git add index.html && git commit -m "description" && git push origin main`
4. On phone: delete old homescreen shortcut, re-add from Safari after hard refresh

GitHub Pages cache can take 1-5 minutes to update.

---

## History

- **2026-10-29**: Initial build. Local file, storm radar + 7-day forecast.
- **2026-10-29 (later)**: Renamed to "Josh's Outdoor Dashboard". Added alerts, sun/moon, AQI, conditions summary.
- **2026-10-29 (later)**: Pushed to GitHub Pages at jlegrand90/weather-dashboard.
- **2026-11-15**: Fixed mobile radar not loading. Added responsive CSS for iPhone 12. Added 15s API timeouts. Created this log.
