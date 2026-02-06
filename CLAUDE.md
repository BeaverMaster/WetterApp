# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Private iOS Home-Screen Weather PWA (single HTML file). Displays current temperature, 7-day forecast with rain probability, and auto-detects location via GPS. Offline fallback via LocalStorage.

## Hard Constraints (from Master-Promt)

- **Vanilla JavaScript only** — no frameworks, no TypeScript, no external JS libraries
- **Tailwind CSS via CDN** — the only allowed external dependency
- **No build step** — files are served directly
- **Single HTML file** (`index.html`) contains all markup, styles, and logic
- **No animations, transitions, or effects**
- **No cards, shadows, or borders** — separation only via spacing/subtle dividers
- **All code comments must be in German** — explain WHAT and WHY, no redundant comments
- **Variables must use `const`/`let`** — never `var`

## Architecture

`index.html` is the entire app — head (meta + Tailwind CDN + custom CSS), body (three view states: loading/error/content), and a single `<script>` block with all logic.

**Key flow:** `init()` → `navigator.geolocation` → parallel `fetchWeather()` + `fetchLocationName()` → `renderWeather()` → `saveToCache()`

**APIs used:**
- Open-Meteo (`api.open-meteo.com/v1/forecast`) — weather data, no API key
- Nominatim (`nominatim.openstreetmap.org/reverse`) — reverse geocoding for city name

**Offline:** On network/GPS failure, `handleOffline()` loads last cached data from LocalStorage and displays a "Gespeicherte Daten" hint.

`AppIcon.html` is a standalone utility that draws a 1024×1024 app icon on canvas and offers PNG download. Referenced in `index.html` via `<link rel="apple-touch-icon">`.

## Design Rules

- Blue vertical gradient background (`#1e3a8a` → `#2563eb` → `#3b82f6`)
- White text, secondary text with reduced opacity
- System font (`-apple-system`)
- Single-column dashboard, touch-optimized, no hover UI
- Full viewport height, no page scrolling (content area scrolls internally)
- iOS safe-area insets via `env(safe-area-inset-*)`

## No Build/Test/Lint

There are no build tools, test frameworks, or linters configured. The project is intentionally zero-tooling — open `index.html` in a browser to run.
