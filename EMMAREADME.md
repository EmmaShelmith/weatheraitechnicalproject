# Field Console — a WeatherAI API client

A single-page, client-side console for the [WeatherAI API](https://weather-ai.co/docs). It calls `api.weather-ai.co` directly from the browser to show live weather, AI agronomic insights, usage quota, and tree-canopy analysis — no backend, no build step.

## Features

| Module | Endpoint | What it shows |
|---|---|---|
| Current conditions | `GET /v1/weather` | Temperature dial, condition, AI summary, multi-day forecast strip |
| Location tools | `GET /v1/ip-lookup`, browser geolocation | Auto-fill coordinates from IP or GPS |
| AI insights | `GET /v1/insights` | Risk flags and recommendations (Pro/Scale only — shows a locked state on 403) |
| Usage | `GET /v1/usage` | Requests and AI-request quota gauges, plan badge, reset date |
| Tree canopy scanner | `POST /v1/trees/analyze` | Drag-and-drop image upload → tree count, density/acre, canopy %, confidence, health breakdown, species guess, Gemini observations & recommendations, original + overlay images |

Every module also has a **raw JSON** panel, since the exact response shape for some endpoints wasn't fully documented — the UI tries several likely field names and falls back to showing you the real payload rather than guessing silently.

## Setup

There's nothing to install or build.

1. Download `weatherai-field-console.html`.
2. Open it in any modern browser (double-click, or `open weatherai-field-console.html`).
3. Paste an API key (format `wai_...`) into the **API Key** field in the sidebar.
   - Generate one from your WeatherAI **Dashboard → API Keys**. The plaintext key is only shown once at creation — copy it then.
4. Set a location (use a preset town, type lat/lon, or click **Use my location** / **IP lookup**).
5. Click **▶ Run console**.

To try the tree scanner, drop a farm/drone/satellite image (JPEG, PNG, or WEBP, max 20 MB) into the scanner panel and click **▶ Analyze image**.

## Notes on behavior

- **The API key never leaves your browser tab.** It's held in memory only, sent as a `Bearer` header on each request, and is not written to disk, localStorage, or any third-party server.
- **Plan-gated endpoints** (`/v1/insights`, `/v1/ip-lookup`, 14+ day forecasts) will return `403` on a Free plan. The console shows this as a clear "gated to Pro/Scale" message instead of a raw error.
- **Rate limits**: on `429`, the console reads the `X-RateLimit-Reset` header and tells you exactly when quota resets.
- **CORS**: this app calls `api.weather-ai.co` straight from the browser. If the API doesn't allow browser-origin requests, you'll see a clear network error explaining that a small server-side proxy would be needed — the requests themselves are correctly formed either way.

## Project structure

```
weatherai-field-console.html   # everything — markup, styles, and JS in one file
README.md                     # this file
```

## Tech

Plain HTML/CSS/JS. No frameworks, no dependencies, no build tooling — open the file and it runs.
