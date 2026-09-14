# Travel Atlas

A personal map of everywhere I've been — 450 pins across 15 countries, 34 US states, and DC.
Cities, monuments, parks, and the places I've lived, filterable and zoomable.

**Live site:** `https://<your-username>.github.io/travel-atlas/`

---

## What's in here

| File | What it does |
|---|---|
| `index.html` | The whole dashboard — map, filters, list, pin editor. Self-contained except for the data. |
| `travel-data.js` | Every pin. This is the only file you need to touch to add places. |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is. Leave it alone. |

The map geometry (coastlines, borders, lakes, rivers, country and state labels) is baked into
`index.html`, so the map draws even with no network. When the site is online it also loads Esri
terrain tiles on top, which adds elevation shading, greenery, and roads. The **Terrain** button
switches between the two.

---

## Publishing it

1. Create a new repository on GitHub named `travel-atlas`. Public, no README (this one is it).
2. Upload `index.html`, `travel-data.js`, `README.md`, and `.nojekyll` to the root of the repo.
   Drag-and-drop into the web uploader works fine — no git required.
3. Go to **Settings → Pages**.
4. Under **Source**, pick **Deploy from a branch**. Branch: `main`, folder: `/ (root)`. Save.
5. Wait a minute or two, then open `https://<your-username>.github.io/travel-atlas/`.

To update later, upload a new `travel-data.js` over the old one. The site refreshes within a minute.

If you'd rather keep it private, GitHub Pages on a private repo requires a paid plan. The
alternative is to keep the repo private and just open `index.html` locally from your computer.

---

## Adding pins — the easy way

1. Open the site and tap **Add pin**.
2. Fill in the name and country.
3. Tap **Place on map**, then tap the spot. Coordinates fill in automatically.
4. For a monument, set **Kind** to "Monument or sight" and put its city in **Part of which city?**
   so it nests under that city in the list.
5. Tap **Add pin**.
6. Scroll to the bottom, hit **Download updated file**, and replace `travel-data.js` in the repo
   with the file you just downloaded.

Pins you add live in the browser until you export. Closing the tab without downloading loses them.

---

## Adding pins — by hand

Open `travel-data.js` and add an entry to the `places` array. A minimal one:

```js
{
 "n": "Space Needle",
 "c": "United States",
 "r": "North America",
 "t": "sight",
 "lat": 47.6205,
 "lng": -122.3493,
 "city": "Seattle, WA",
 "sights": []
}
```

### Fields

| Field | Required | What it is |
|---|---|---|
| `n` | yes | Display name. For US cities the convention here is `"Boston, MA"`. |
| `c` | yes | Country. Must match other pins exactly or it creates a second country heading. |
| `r` | yes | Region. One of: `North America`, `Europe`, `Asia`, `Oceania`, `Caribbean`. Drives the filter chips. |
| `t` | yes | `city`, `sight`, or `nature`. Sets the marker colour and glyph — red dot, gold diamond, green triangle. |
| `lat` / `lng` | yes | Decimal degrees. Negative for west and south. |
| `city` | no | For monuments: which city pin it nests under. Must match that pin's `n` exactly. |
| `sights` | yes | Free-text notes. `[]` is fine. |
| `visits` | no | Number of trips. Shows as a `×35` badge and makes the dot bigger. |
| `stay` | no | Free text about time spent, e.g. `"8 months total"`. |
| `lived` | no | Free text. Marks it as somewhere you lived — gold label, dark ring, biggest dot. |
| `tag` | no | Overrides the badge text, e.g. `"300+ Maine trips"`. |

### Country-level stats

The `countryStats` block at the top of the file adds trip counts to country headings:

```js
"India": { "visits": 5, "time": "2 months total" }
```

Accepts `visits`, `time`, and `note`.

### Finding coordinates

Easiest is the **Place on map** button. Otherwise: right-click a spot in Google Maps and the
lat/lng appears at the top of the menu — click to copy.

---

## Gotchas

- **Names must match exactly.** A monument with `"city": "Boston"` won't attach to a pin named
  `"Boston, MA"`. Same for country names.
- **Editing by hand means valid JSON.** Double quotes, commas between entries, no trailing comma
  after the last one. If the map comes up empty, that's usually why — the page shows an import
  banner when the data file fails to load.
- **Zoom is capped** on the built-in vector map because the coastlines are simplified. Switch on
  Terrain for street-level detail.
- **Monuments don't hold monuments.** A sight pin shows its parent city rather than a nested list.

---

## Credits

Country and state boundaries from [Natural Earth](https://www.naturalearthdata.com/) via
[world-atlas](https://github.com/topojson/world-atlas) and [us-atlas](https://github.com/topojson/us-atlas).
Lakes and rivers from [natural-earth-vector](https://github.com/nvkelso/natural-earth-vector).
Map rendering by [Leaflet](https://leafletjs.com/). Terrain tiles by Esri.
