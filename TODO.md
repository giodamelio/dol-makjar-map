# Dol-Makjar Map TODO

## Features

- [ ] **Custom marker overlays per location** — Replace each numbered circle on the map with a custom graphic (normal + highlighted state with inverted colors: orange background / white text). One per location.
- [x] **HTML popups on click** — Click a marker to show a positioned HTML popup with title, description, and links to wiki pages. Real DOM, not canvas.
- [x] **Clickable image legend** — Overlay invisible hit regions on the two legend panels (left and right side of the map) so clicking a legend entry highlights and pans to that location.
- [x] **URL hash state** — Store selected place/district in `window.location.hash` so links can point directly to a location (e.g. `#place/15` or `#district/rookery`).
- [ ] **Measure tool** — Distance measurement tool based on the scale bar in the upper-right of the map image. Click two points to see the in-world distance between them.
- [ ] **Draw Barsizi polygon** — Use calibrate.html to draw the polygon for the Barsizi district (places 31-32, southern area near Lake Nahami).
- [ ] Mini map
- [ ] Small Info/About with link to my site and the source code
- [ ] Integrate the calibrate page with the current map page
- [ ] Search: Allow searching by district to show all items in district, should be lowest priority only after only after the search is narrowed down to the district, so the top item in the dropdown is the district itself, and the rest of the items are the places in that district. So we can get a quick glance at what a neighborhood has in it
- [ ] Allow searching for the lakes, rivers, fields that have labels
- [ ] Setup SEO stuff
- [ ] Setup cache busting

## Data

- [ ] **Wiki links** — Add `url` field to each place in places.json linking to the Critical Role wiki page.
- [ ] **Place descriptions** — Add `description` field to places for popup content.
- [x] **Legend hit regions** — Add coordinate data for clickable legend areas.
- [ ] **Scale calibration** — Determine the pixel-to-distance ratio from the map's scale bar for the measure tool.
