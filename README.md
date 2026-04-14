# Virtual Flights Map

Real-time virtual flight tracker aggregating VATSIM, IVAO, and POSCON networks on a single interactive map.

## Features

- Live aircraft tracking with altitude-based coloring and speed vectors
- ATC coverage zones (FSS/DEL/GND/TWR/APP/CTR)
- Weather layer with METAR markers and RainViewer radar overlay
- Great-circle flight route rendering
- Prefiled flight plan display at departure airports
- Navaid markers (VOR/NDB/Fix) and airway visualization
- ATIS text-to-speech synethesis
- Search/filter by callsign or airport
- Side panel with pilots and controllers list
- Adaptive polling (30s active/60s idle/0s hidden)
- Smooth 30fps interpolation between data polls
- Day/night theme toggle with localStorage persistence
- Resiliant fetch with retry and circuit breaker

## Tech Stack

- HTML5/CSS3/Vanilla JS
- Leaflet.js for mapping
- RainViewer API for weather
- VATSIM/IVAO/POSCON data APIs
- Cloudflare Pages deployment

## License

Proprietary - Pushstart LLC

## Author

Pawan K Jajoo, Pushstart LLC
