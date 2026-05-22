# CLAUDE.md

## Project Overview

Interactive map viewer for the city of Dol-Makjar from Critical Role Campaign 4. Displays a 3600x5400 JPG map image with pan/zoom, place search, district polygon overlays, and URL hash state for deep linking.

## Tech Stack

- **Leaflet.js** via unpkg CDN with `L.CRS.Simple` for non-geographic image mapping
- Vanilla JS/CSS, no build step, fully self-contained HTML files
- Static file serving only (e.g. `python3 -m http.server 8080`)

## Files

- `index.html` — Map viewer (Leaflet + inline CSS/JS). All application code is in this single file.
- `calibrate.html` — Coordinate editing tool for placing point/polygon coordinates. Uses vanilla JS (not Leaflet) with CSS transforms for pan/zoom. Has Point mode and Polygon mode with Draw/Edit/Done/Clear buttons.
- `places.json` — All locations (65 places), coordinates (0-1 fractions), district names (16 districts), and district polygon boundaries.
- `original-map.jpg` — Source map image (3600x5400 JPEG)
- `TODO.md` — Feature backlog

## Coordinate System

Places in `places.json` use 0-1 fractional coordinates relative to the image dimensions. Conversion to Leaflet coordinates:

```js
function toLatLng(x, y) {
  return [IMG_H * (1 - y), IMG_W * x];
}
```

Leaflet's Y axis is inverted relative to image pixel coordinates.

## Data Format (places.json)

```json
{
  "districts": [
    {
      "name": "District Name",
      "polygon": [[x, y], ...],
      "places": [
        { "id": 1, "name": "Place Name", "x": 0.23, "y": 0.08 }
      ]
    }
  ]
}
```

- 12 of 16 districts have polygon arrays. The Bridges, The Walls, and Others do not.
- Barsizi district (places 31-32) is separate from Marud (places 33-38) and has no polygon yet.

## URL Hash Format

- Places: `#place/{id}/{slug}` (e.g. `#place/15/the-pentevral`)
- Districts: `#district/{slug}` (e.g. `#district/the-rookery`)

## Key Implementation Details

- Place markers use `L.circle` (map-coordinate radius that scales with zoom), not `L.divIcon`
- Labels use `L.popup` with `autoPan: false` to prevent unwanted map panning on hover
- Fly animations defer popup display to `map.once('moveend')` to avoid glitching
- District popups anchor at the top edge of the polygon (minY) with horizontal center
- Search supports both name substring matching and exact ID matching
- Auto-zoom on selection: `getTargetZoom() = Math.log2(smallerScreenDim / 800)`, only zooms in

## Version Control

This project uses **jj (Jujutsu)**, not git. Use `jj` commands for commits and history.

## Development

```
python3 -m http.server 8080
```

- `/` — Map viewer
- `/calibrate.html` — Coordinate editing tool
