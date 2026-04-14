# Virtual Flights Map

Live flight tracking map for VATSIM, IVAO, and POSCON virtual aviation networks. Real-time display of pilots, controllers, aircraft routes, weather, and navigational data across all major flight simulation platforms.

**Author:** Pawan K Jajoo, Pushstart Simulators  
**License:** MIT  
**Repository:** Virtual-Flights-Map

## Features

### Live Traffic Networks
- **Multi-network support:** VATSIM, IVAO, and POSCON pilots and ATC stations on single map
- **Real-time data:** 30-second polling for VATSIM primary feed, 90-second secondary feed (IVAO + POSCON)
- **Adaptive polling:** Flight phase detection adjusts refresh rates (ground: 120s, cruise: 30s)
- **Smooth animation:** Aircraft interpolated at 60fps between API polls using heading and groundspeed

### Map Layers (Toggleable)
- **ATC Coverage:** Active air traffic control zones and facility data
- **Flight Routes:** Parsed waypoint routes with great-circle arcs, color-coded (flown vs. remaining)
- **Aircraft Markers:** Ground and prefiled aircraft with on-ground indicators
- **Speed Vectors:** 4-minute projected position lines based on heading and groundspeed
- **FIR/UIR Boundaries:** Flight Information Region and Upper Information Region airspace (VATSpy data)
- **SIGMETs:** Significant meteorological advisories (aviation alerts)
- **Navaids:** VOR, NDB, and fix markers with AIRAC navigation data
- **Airway Centerlines:** High/low altitude routes between navaids
- **Live Weather:** RainViewer radar overlay with METAR flight categories

### Aircraft & ATC Data
- **Pilot information:** Callsign, aircraft type, route, altitude, groundspeed, flight plan
- **Controllers:** ATIS, Approach, Tower, Ground, Delivery frequencies and facility details
- **Aircraft classification:** Heavy, Medium, Light Jet, Regional, General Aviation, Military, Helicopter
- **Flight plan details:** Departure, arrival, route string, aircraft equipment, filed altitude/speed
- **Logon times:** User connection timestamp across networks
- **Network identification:** Explicit pilot/controller network tagging (VATSIM/IVAO/POSCON badges)

### Search & Filtering
- **Callsign search:** Find pilots and controllers by callsign with autocomplete
- **Airport search:** Locate airports by ICAO code
- **Aircraft type filter:** Show/hide by aircraft category (heavy, regional, GA, military, etc.)
- **Real-time results:** Search index updated on every data poll

### Route Parsing & Navigation
- **VOR/Fix resolution:** Multi-strategy waypoint lookup (static navaids â AIRAC CSV â GitHub JSON â OpenNav API)
- **Ambiguity resolution:** Distance-weighted disambiguation for identically-named navaids across regions
- **Great-circle routing:** Long segments (>400km) rendered with geodesic accuracy
- **Route amendment detection:** Auto-reparse on flight plan change
- **Fallback routing:** Dep â Current Position â Arrival when waypoint parsing fails
- **Navaid caching:** localStorage persistence across sessions

### Weather Integration
- **RainViewer radar:** Live precipitation and storm animation
- **METAR parsing:** Aviation color categories (VFR green, MVFR blue, IFCR red, LIFR magenta)
- **Weather timers:** 5-minute update cycle
- **Airport weather display:** METAR data for selected airports

### ATIS Voice
- **Text-to-speech:** Synthesized ATIS playback from selected ATC stations
- **Real-time ATIS:** Live ATIS remarks from network data
- **Voice control:** Browser Web Speech API integration with fallback

### User Interface
- **Day/Night themes:** Auto-detection based on local time with manual override
- **Dark cockpit aesthetic:** Professional aviation-style interface
- **Responsive design:** Mobile-friendly layout with adaptive responsiveness
- **Live statistics:** Header display of pilot count, ATC count, prefile count, live status

### Developer Features
- **Resilient fetch engine:** Automatic retry, circuit breaker per domain, request deduplication
- **Adaptive API strategy:** Scraper-style escalation (HTTP â cloudscraper â Playwright)
- **CORS proxy support:** Fallback for APIs lacking CORS headers (OpenNav, Aviation Weather)
- **Network diagnostics:** Console logging for data fetch, parsing, route resolution
- **Batch API endpoint:** Aggregated VATSIM + IVAO + POSCON in single 12-second cached call

## How It Works

### Data Sources
1. **VATSIM:** `/api/v3/vatsim/pilots`, `/api/v3/vatsim/controllers`, `/status`
2. **IVAO:** Whazzup v2 endpoint (`/clients.php`)
3. **POSCON:** Online players JSON endpoint

### Normalization
All pilot/controller objects normalized to common shape regardless of network source for seamless integration.

### Route Parsing Pipeline
1. Extract route string from flight plan
2. Tokenize into VOR/NDB/fix identifiers
3. Resolve each token through priority chain:
   - Static navdata cache (localStorage)
   - AIRAC CSV (GitHub)
   - Navaid JSON (external source)
   - Airport coordinates
   - OpenNav API (live lookup)
4. Disambiguate duplicates by distance-to-previous-point
5. Filter unrealistic detours (>1.5x direct distance)
6. Render as polyline with departure/arrival markers

### Speed Vector Calculation
- Bearing: true heading from flight plan or calculated from last two positions
- Distance: groundspeed x 4 minutes / 3600
- Projection: haversine formula forward calculation

### Theme System
- CSS variables for light/dark modes
- Auto-switch 6:30 AM - 6:30 PM local time
- localStorage persistence of user preference
- Real-time map tile swapping (OpenStreetMap day/night)

## Map Controls

### Top-Right Zoom
- **+** Zoom in
- **â** Zoom out
- **Reset** Return to fit-to-data bounds

### Layer Toggle Panel
- **ATC** Air traffic control coverage zones
- **RTE** Flight routes and waypoints
- **GRND** On-ground / prefiled aircraft
- **VEC** Speed vectors (4-minute projection)
- **FIR** Flight information region boundaries
- **SIG** SIGMET weather alerts
- **NAV** Navaid markers (VOR/NDB/Fix)
- **AWY** Airway centerlines
- **WX** Live weather radar (RainViewer + METAR)

### Bottom-Left Controls
- **ATIS** Play synthesized ATIS from selected controller
- **KEY** Toggle aircraft type legend

### Top-Center Display
- Virtual Pilots In-Flight (across all networks)
- Active ATC count
- VATSIM pre-flight / filed count
- Live indicator

## Installation

Single HTML file deployment. Serve over HTTPS with:
- Leaflet 1.9.4+ (CDN loaded)
- Modern browser (ES6+ support, Web Speech API)
- No backend required (all APIs called from client)

## Responsive Design

- Desktop: Full feature set with all layers and controls
- Tablet: Optimized layout, collapsible panels
- Mobile (<480px): Condensed stats, search-first UI

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Modern mobile browsers

## Notes

- Aircraft interpolated between 30s polls for smooth animation
- Circuit breaker auto-silences APIs after 3 consecutive failures (60s reset)
- CORS proxy required for: opennav.com, aviationweather.gov
- METAR data cached across poll cycles to reduce API load
- FIR/SIGMET layers load on-demand (single fetch per session)

## Keyboard Shortcuts

- **D** Toggle day/night theme
- **K** Toggle aircraft legend
- **S** Focus search box

## Performance

- Canvas-based rendering via Leaflet
- Layer clustering for 500+ aircraft
- Deduped API requests (inflight map)
- LocalStorage caching for navaids and preferences
