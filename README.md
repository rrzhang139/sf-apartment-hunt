# SF Apartment Hunt

A single-file web app for comparing San Francisco 2BR apartments along the Caltrain commute. Map, per-building externalities, unit availability, which-way-to-face guidance, and a route-to-Caltrain planner (walk / bus / light rail with time, fare, frequency).

**Live:** https://apt-site-opal.vercel.app

## Run it

No build step. It's one HTML file with everything inline.

```
open index.html          # macOS
# or serve it:
python3 -m http.server 8000   # then visit http://localhost:8000
```

External dependencies load from CDNs at runtime: Leaflet (map), CARTO tiles, and the public OSRM router (route geometry). An internet connection is required for the map and routes.

## How to contribute

All data lives in the `DATA` array inside `index.html` (search for `const DATA = [`). To add a building, copy an existing entry and edit the fields. Then open the file to check it renders.

### Building schema

| Field | What it is |
|---|---|
| `id` | unique short slug |
| `name`, `hood`, `address` | display strings. `hood` must match a chip in `HOODS` |
| `lat`, `lng` | map coordinates |
| `year`, `modern` | build year / "is it modern" note |
| `avail` | `"yes"` (green), `"warn"` (amber), or `"no"` (red) — drives the badge + map dot |
| `rentBand`, `each`, `parking` | rent summary strings |
| `caltrain` | one-line station + walk note |
| `verdict` | the one-line take |
| `units[]` | `{u, bb, sf, rent, av, link}` rows |
| `contact` | `{phone, email, site, apply}` |
| `externalities[]` | `{f, s, n}` — factor, severity (`high`/`med`/`low`), day-to-day note |
| `facing` | `{best, sides:[{dir, what, s}]}` — which way each side looks out and how loud |
| `commute` | `{station, best, caltrainLeg, doorToOffice, options:[{mode, time, cost, freq, route, notes}], rushTip}` |
| `sound` | construction / soundproofing paragraph |
| `amenities` | `{coffee[], grocery[], transit[], gym}` |
| `reviews[]` | `{src, rating, quote, url}` |
| `complaints[]` | short strings |

Notes:
- `commute.station` must be a key in the `STATIONS` lookup (`"22nd St Caltrain"` or `"4th & King Caltrain"`) for the route to draw.
- `commute.options[].mode` emoji picks the route color: 🚶 walk (green), 🚲 bike (amber), 🚊 light rail (purple), 🚌 bus (blue, dashed).
- Keep rents as snapshots and verify live — big buildings price per-unit and post inventory ~30–60 days out.

### Submitting

Fork, branch, edit `DATA`, open a PR. Keep the tone of existing entries: honest about downsides, severity-tagged externalities, real links.

## License

MIT — see [LICENSE](LICENSE).
