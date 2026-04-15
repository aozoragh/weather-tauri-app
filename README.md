# Weather Desktop App

![App screenshot](static/screenshots/result.png)

A real-time weather desktop application built with **Tauri**, **SvelteKit**, **TypeScript**, and **Tailwind CSS v4**. Search any city in the world and get live conditions, a 5-day forecast, and a dynamic background that changes with the weather.

## Features

- **Live weather data** — current temperature, feels like, humidity, wind speed, visibility, and coordinates
- **5-day forecast** — daily high/low temperatures with weather icons
- **Dynamic backgrounds** — full-screen background image that crossfades to match the current weather condition
- **Geolocation** — one-click weather for your current location
- **Unit toggle** — switch between °C and °F, persisted across sessions
- **Secure API** — API key never exposed to the client; all OWM requests proxied through SvelteKit server routes
- **Custom loading** — animated loader component while fetching
- **Error handling** — auto-dismiss error banner for API and network failures

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | [SvelteKit 2](https://kit.svelte.dev) + Svelte 5 (Runes) |
| Frontend | TypeScript |
| Styling | Tailwind CSS v4 |
| Desktop Framework | [Tauri](https://tauri.app) |
| Backend | Rust |
| Weather API | [OpenWeatherMap](https://openweathermap.org/api) |

## Project Structure

```
src/
├── lib/
│   ├── components/
│   │   ├── SearchBar.svelte       # City search input + geolocation button
│   │   ├── UnitToggle.svelte      # °C / °F toggle
│   │   ├── WeatherCard.svelte     # Main weather display card
│   │   ├── WeatherIcon.svelte     # Animated condition icon
│   │   ├── FeatureGrid.svelte     # Feels like, humidity, wind, visibility, coords
│   │   ├── ForecastStrip.svelte   # 5-day forecast row
│   │   ├── LoadingOverlay.svelte  # Custom animated loader
│   │   └── ErrorBanner.svelte     # Auto-dismiss error toast
│   ├── stores/
│   │   └── settings.svelte.ts     # Unit preference (localStorage)
│   ├── types/
│   │   └── weather.ts             # TypeScript interfaces
│   └── utils/
│       ├── weatherMapping.ts      # OWM ID → condition, icon, background
│       └── formatters.ts          # Temperature, wind, visibility, date
├── routes/
│   ├── api/
│   │   ├── weather/+server.ts     # Server-side OWM current weather proxy
│   │   └── forecast/+server.ts   # Server-side OWM 5-day forecast proxy
│   ├── +layout.svelte
│   └── +page.svelte
src-tauri/
├── src/
│   ├── main.rs                    # Tauri application entry point
│   └── lib.rs                     # Tauri library code
├── Cargo.toml                     # Rust dependencies
├── tauri.conf.json                # Tauri configuration
└── build.rs                       # Build script
static/
├── icons/                         # SVG weather condition icons (7 files)
├── backgrounds/                   # Dynamic background images (7 .webp files)
└── fonts/
```

## Getting Started

### Prerequisites

- Node.js 18+
- Rust (latest stable)
- An [OpenWeatherMap API key](https://openweathermap.org/api) (free tier works)

### Installation

```bash
git clone <repository-url>
cd weather-desktop-app
npm install
```

### Environment Variables

Create a `.env` file in the project root:

```env
OPENWEATHER_API_KEY=your_api_key_here
```

### Background Images

Place 7 `.webp` background images in `static/backgrounds/`, named exactly:

```
clear.webp       clouds.webp      rain.webp
drizzle.webp     storm.webp       snow.webp
atmosphere.webp
```

### Development

```bash
npm run tauri dev
```

This will start the Tauri development server, opening the desktop app in development mode.

### Build & Type Check

```bash
npm run check   # TypeScript + Svelte type checking
npm run tauri build   # Production build for your platform
```

## Deployment

After building, distribute the generated binaries from `src-tauri/target/release/` (or `debug/` for development builds) for your target platforms (Windows, macOS, Linux).

## API Routes

All weather data is fetched server-side to keep the API key secure.

| Route | Parameters | Description |
|---|---|---|
| `GET /api/weather` | `q` or `lat`+`lon` | Current weather for a city or coordinate |
| `GET /api/forecast` | `q` or `lat`+`lon` | 5-day forecast, aggregated to daily summaries |

Data is always fetched in metric (°C) and converted client-side based on the user's unit preference.

## License

MIT
