# Clickable Marker Overlays — Design Notes

GitHub issue: #1

## Goal

Make the numbered markers on the map clickable, with a highlighted state that looks native to the map image (as if the marker itself changed colors — orange background, white text).

## Approach: Dual Image Overlay with CSS clip-path

The idea is to layer two copies of the map image:

1. **Bottom layer**: `original-map.jpg` (the normal map, markers as-is)
2. **Top layer**: `original-map-selected.jpg` (identical map, but all 65 markers recolored to the selected style — orange bg, white text)

The top layer has `clip-path: circle(0% at 0% 0%)` by default (fully hidden). When a marker is hovered or selected, we set:

```css
clip-path: circle(1.25% at {x*100}% {y*100}%)
```

This reveals a small circular window in the top layer at the marker's position, showing the orange version of that marker while the rest of the map stays normal. The effect is that the marker appears to change color in-place.

## Why This Works

- Both `L.imageOverlay`s share the same Leaflet bounds and CSS transforms, so they stay pixel-perfect aligned through all pan/zoom.
- `clip-path` percentages are relative to the element's own coordinate space, which maps directly to the 0-1 fractional coordinates in `places.json`.
- Changing `clip-path` is GPU-accelerated — no layout or paint cost.
- The result looks truly native to the map image, not like a web overlay.

## Spike Results (validated 2026-05-22)

- Created a test with a black overlay image and `clip-path: circle(1.25% at ...)`.
- Confirmed: circle stays locked to the correct map position across pan and zoom.
- Confirmed: hover interaction works via transparent `L.circle` hit targets (radius ~57 map units).
- Confirmed: `testOverlay.getElement()` is null immediately after `addTo()` — must use the `load` event to access the DOM element.

## What's Needed to Finish

### Image editing (the hard part)
- Create `original-map-selected.jpg` by editing all 65 numbered markers to have orange backgrounds and white text.
- Each marker is a small circle with a number. The editing needs to look native to the map's art style.
- The file `public/original-map-selected.jpg` is already a copy of the original, ready to be edited.

### Code changes (straightforward)
- Add the selected overlay in `buildOverlays()`, hidden by default.
- Make all `L.circle` hit targets interactive (transparent fill, `interactive: true`, radius ~57).
- Wire `mouseover`/`mouseout` on each circle to toggle `clip-path` on the overlay.
- Wire `click` on each circle to call `selectItem()`.
- Integrate with existing `highlightPlace()` / `clearHighlights()` / `selectItem()` flow.
- The clip-path radius of `1.25%` looked good in testing.

## Alternative Approaches Considered

1. **CSS-styled L.divIcon markers** — easier to implement but looks like a web overlay, not native to the map. Alignment with painted markers is imperfect.
2. **Individual marker sprite images** — 65 pairs of images, lots of cropping work.
3. **Canvas-based drawing** — programmatically draw orange circles, but hard to match the map's art style.
