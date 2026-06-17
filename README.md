# 🧋 LA Boba Map — The Pearl Index

An interactive map of **87 real boba shops across 27 LA-area neighborhoods** — filterable by area, rating, and open-now, with seating, vibe, parking & safety notes plus live Yelp / UberEats / Fantuan / Google links for each shop.

Built as a single self-contained HTML file. No build step, no backend, no API keys.

![status](https://img.shields.io/badge/shops-87-7B5EA7) ![status](https://img.shields.io/badge/neighborhoods-27-8AAE4B) ![status](https://img.shields.io/badge/build-none%20required-E8924A)

---

## What it does

- **Map first.** Every shop is a rating-colored pin on a Leaflet map that auto-fits to show the whole region on load. Click a pin to jump to its card; click a card to fly to its pin.
- **Filter the way you actually decide.** Search by shop *or* drink ("ube", "jasmine", "matcha"), filter by neighborhood, flip on **Open now**, set a minimum rating, and sort by top-rated / most-reviewed / A–Z.
- **Cards built around the real questions.** Each shop expands to four sections: **Menu highlights**, **Seating & vibe**, **🅿️ Parking & safety**, and **Go / order** (Google, Yelp, UberEats, Fantuan, phone).

## Coverage

San Gabriel Valley (Arcadia, San Gabriel, Monterey Park, Alhambra, Montebello, Rowland & Hacienda Heights) · West LA (Sawtelle, Westwood, Century City, Mar Vista) · Culver City · Sherman Oaks · SF Valley (Glendale, Northridge) · South Bay (Torrance, Gardena) · Lynwood · Downtown / Chinatown / Little Tokyo · Echo Park · Koreatown · Pasadena · Orange County (Irvine, Garden Grove, Costa Mesa).

Chains include Gong Cha, 7 Leaves, Sharetea, Chatime, heytea, Ume Tea, Tastea, Boba Guys, TP Tea, plus the Westfield Century City lineup (CHAGEE, Wushiland, Cha Cha Matcha, Redstraw).

## Run it

It's one file. Either:

```bash
# just open it
open la-boba-map.html        # macOS
xdg-open la-boba-map.html    # Linux

# or serve it (recommended, so map tiles load cleanly)
python3 -m http.server 8000
# then visit http://localhost:8000/la-boba-map.html
```

No install, no dependencies to fetch — Leaflet and the fonts load from CDNs at runtime.

## How the data works (and what's honest about it)

This map deliberately separates **facts** from **estimates** from **things that change too fast to freeze**:

| Field | Source | Notes |
|---|---|---|
| Name, address, coordinates | Google Places | Live at time of capture |
| Rating, review count, price level, hours, phone | Google Places | Live at time of capture |
| Menu highlights | Summarized from real Google reviews | Crowd favorites, *not* a full menu |
| Seating & vibe | Summarized from real Google reviews | Labelled as review-derived |
| Parking & safety | Summarized from real Google reviews + general safety guidance | Not a safety rating — verify in person |
| Live menu & prices | **Not stored** | The Yelp / UberEats / Fantuan / Google buttons open each shop's *live* page, because menus and delivery prices change daily |

Nothing in the cards is invented. Where a number would go stale (menus, delivery prices, availability), the map links out instead of guessing.

## Tech

- **Leaflet 1.9.4** for the map, **CartoDB Positron** tiles
- **Vanilla JS** — all 87 shops live in one `SHOPS` array; the UI renders from it
- **No framework, no bundler, no localStorage** — state is in memory
- Self-contained HTML/CSS/JS in `la-boba-map.html`

## Contributing

Adding a shop or fixing details is welcome and easy — see **[CONTRIBUTING.md](CONTRIBUTING.md)** for the data format and the one rule that matters (don't invent menus or prices).

## Credits

Made with 🧋 by [**wuisabel**](https://github.com/wuisabel-gif).

## License

MIT
