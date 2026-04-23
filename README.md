# Virtual Flights Map

Real-time virtual flight tracker aggregating VATSIM, IVAO, POSCON, and SayIntentions.AI/Skynet networks on a single interactive map. A self-contained HTML/CSS/JS application (no build tools, no framework) deployed on Cloudflare Pages.

Live at [maps.pushstartsims.com](https://maps.pushstartsims.com)

## Features

### Multi-Network Tracking

- Live aircraft tracking across VATSIM, IVAO, POSCON, and SayIntentions.AI/Skynet with unified data normalization
- Network badge indicators on non-VATSIM aircraft (IVAO amber, POSCON indigo, Skynet emerald green)
- Batch data endpoint for parallel multi-network fetching (VATSIM/IVAO/POSCON)
- Direct API integration with SayIntentions.AI tracker for Skynet multiplayer pilot positions
- CORS proxy worker on Cloudflare for APIs lacking browser CORS headers, with automatic fallback
- Network health monitoring with color-coded status (green for connected, red for failed) and dismissable alert banner when any network is unreachable

### Aircraft Display

- Seven aircraft categories with distinct color coding: Heavy, Airliner, Biz Jet, Turboprop, GA, Helicopter, Military
- SVG aircraft icons that rotate with heading and scale with altitude/state
- Emergency squawk detection (7500/7600/7700) with red ring highlight
- Parked/ground aircraft rendered in gray; selected aircraft highlighted in yellow
- Fade effect on non-selected aircraft when one is selected
- Aircraft type filter dropdown to isolate a single category

### ATC Coverage

- ATC coverage zones rendered as polygons (FSS/DEL/GND/TWR/APP/CTR)
- FIR/UIR boundary layer from VATSpy boundary data
- ATIS text display in the ATC info drawer tab

### ATIS Voice (Text-to-Speech)

- SpeechSynthesis API integration for reading ATIS text aloud
- Regional voice selection based on ATC callsign/location
- Standalone toggle button and per-station voice button inside the ATC drawer
- Visual speaking indicator with orange glow

### Flight Routes and Navaids

- Great-circle flight route rendering with arc segments for long-haul routes (>1200km)
- Prefiled flight plan display at departure airports
- Embedded AIRAC navaid database: VOR, NDB, VORDME worldwide
- Hundreds of embedded fixes/waypoints (NAT, European, US domestic, Middle East, Asian)
- Airways overlay: NAT tracks, European upper airways, US jet routes, Q routes, international routes
- Dynamic navaid lookup via OpenNav API with FlightPlanDatabase fallback
- CSV waypoint import from GitHub (Global Aviation Waypoints)

### Weather

- RainViewer precipitation radar tiles on a 5-minute refresh cycle
- METAR airport markers color-coded by flight category: VFR (green), MVFR (blue), IFR (red), LIFR (magenta)
- SIGMET polygons from NOAA Aviation Weather with alert pulse animation

### Search and Filtering

- Live search by callsign, ICAO code, departure, or arrival (limited to 8 results)
- Aircraft type filter dropdown
- Network filter indicators

### UI and Map

- Leaflet.js (v1.9.4) interactive map with CartoDB basemap tiles (dark and light variants)
- Nine layer toggle buttons: ATC, Routes, Prefiles, Speed Vectors, FIR Boundaries, SIGMETs, Navaids, Airways, Weather
- Flight info drawer (bottom panel) with tabbed interface for pilot and ATC details
- Side panel with scrollable pilot and controller lists
- Header with live stats (pilot count, ATC count, live indicator)
- Footer with last-update timestamp, per-network health indicators, data source links, and cursor coordinates
- Legend panel showing aircraft category key with color-coded icons
- Welcome/onboarding overlay on first visit
- Loading screen with glow animation and fade-out on first data load
- Speed vector lines showing 4-minute projected positions

### Theme System

- Day/night auto-detection based on time of day (6:30 AM to 6:30 PM)
- Manual theme toggle with localStorage persistence
- Flash-prevention via inline script applied before CSS paints
- CSS custom properties throughout for consistent theming

### Performance and Reliability

- Resilient fetch engine (pfetch) with automatic retry, circuit breaker pattern, and request deduplication
- Adaptive polling: 30s active, 60s idle (>2 min no interaction), paused when tab hidden
- Smooth 60fps position interpolation between 30-second data polls for in-flight aircraft
- Debounced localStorage writes for navaid cache
- Responsive layout with mobile breakpoints at 799px, 640px, 479px, 380px

### Airport Data

- OurAirports CSV import (~70,000+ airports worldwide)
- VATSIM airport API for search
- Airport activity layer on the map

## Data Sources

- [VATSIM Data API](https://data.vatsim.net/v3/vatsim-data.json) (primary, direct fetch)
- [IVAO Whazzup v2](https://api.ivao.aero/v2/tracker/whazzup) (via batch proxy)
- [POSCON Online](https://hqapi.poscon.net/online.json) (via batch proxy)
- [SayIntentions.AI Tracker](https://lambda.sayintentions.ai/tracker/map) (direct fetch, Skynet multiplayer pilots)
- [VATSpy Boundaries](https://github.com/vatsimnetwork/vatspy-data-project) (FIR/UIR GeoJSON)
- [NOAA Aviation Weather](https://aviationweather.gov) (METAR, SIGMET)
- [RainViewer](https://www.rainviewer.com) (precipitation radar tiles)
- [OpenNav](https://opennav.com) / [FlightPlanDatabase](https://flightplandatabase.com) (navaid lookup)
- [OurAirports](https://ourairports.com) (global airport database)
- [CartoDB Basemaps](https://carto.com) (dark and light map tiles)

## Tech Stack

- HTML5 / CSS3 / Vanilla JavaScript (single-file, ~4930 lines)
- Leaflet.js for mapping
- Google Fonts: Inter (UI), SF Mono (monospace/data)
- Cloudflare Pages deployment with Cloudflare Workers CORS proxy
- Wix iframe compatibility for embedded deployment

## Companion Tool

**PSS Tools Updater** (`pss_tools_updater.py`): Adaptive CDN and API endpoint checker using a 3-tier escalating fetch strategy (stealth HTTP, cloudscraper, Playwright). Validates CDN library versions, API endpoint health, and feature presence in the HTML.

## License

Proprietary - Pushstart LLC

## Author

Pawan K Jajoo, Pushstart LLC
