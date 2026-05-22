# Dol-Makjar Map

Interactive map of the city of Dol-Makjar from Critical Role Campaign 4.

Search for places and districts, highlight regions, and pan/zoom around the map.

## Usage

Serve the directory with any static file server:

```
python3 -m http.server 8080
```

Open `http://localhost:8080` for the map viewer, or `/calibrate.html` for the coordinate editing tool.

## Files

- `index.html` — Map viewer (Leaflet + vanilla JS)
- `calibrate.html` — Tool for placing point/polygon coordinates
- `places.json` — All locations, coordinates, and district polygons
- `original-map.jpg` — Source map image (3600x5400)
