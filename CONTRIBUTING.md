# Contributing to LA Boba Map 🧋

Thanks for helping grow the Pearl Index! Whether you're adding a shop, fixing hours, or correcting a parking note, this guide covers everything.

## The one rule that matters

**Don't invent menus, prices, safety ratings, or anything you can't source.**

The whole point of this map is that it's honest about what it knows. So:

- ✅ Menu highlights = real crowd favorites pulled from reviews
- ✅ Seating / vibe / parking = summarized from real reviews or first-hand visits
- ❌ No made-up prices, no fabricated full menus, no invented "safety scores"
- ❌ Don't guess coordinates or hours — look them up

If a detail changes too often to pin down (live menus, delivery prices), that's what the platform links are for. Leave them to the buttons.

## Ways to contribute

- **Add a shop** that's missing
- **Fix data** — wrong hours, closed locations, a moved address, an outdated rating
- **Improve a note** — a better parking tip, a seating correction, a more accurate vibe
- **Expand a region** — deeper OC, the eastern SGV, Long Beach, etc.

Open an issue first if it's a big change (a whole new region); small fixes can go straight to a PR.

## How to add a shop

All shops live in one array called `SHOPS` inside `la-boba-map.html`. Each entry is one object. Copy this template and fill it in:

```js
{
  n:"Shop Name",                 // display name
  h:"Region / Neighborhood",     // e.g. "SGV / Arcadia" — must match an existing region group
  lat:34.1267663,                // from Google Maps (right-click → coordinates)
  lng:-118.0544228,
  r:4.8,                         // Google rating
  rc:318,                        // Google review count
  p:2,                           // price level 1–3 (use a number, never null)
  ph:"+1 213-555-0100",          // phone, or "" if none
  a:"1130 S Baldwin Ave, Arcadia",
  hrs:[                          // EXACTLY 7 entries, Monday → Sunday
    "11:00 AM – 10:00 PM",       // use "Closed" for closed days
    "11:00 AM – 10:00 PM",
    "11:00 AM – 10:00 PM",
    "11:00 AM – 10:00 PM",
    "11:00 AM – 11:00 PM",
    "11:00 AM – 11:00 PM",
    "11:00 AM – 10:00 PM"
  ],
  drinks:["Jasmine Milk Tea","Taro","Brown Sugar Boba"],  // 3–4 crowd favorites
  seat:"One sentence on seating & atmosphere, review-sourced.",
  park:"One sentence on parking. Be specific (lot / street / validated structure).",
  vibe:["Short tag","Another tag","Open late"]            // 2–4 short tags
}
```

### Field rules

- **`h` (neighborhood)** must match a region already in the `order` array inside `buildHoods()`. Reuse an existing one (`"SGV / Arcadia"`, `"OC / Irvine"`, …) when you can. Adding a brand-new region? Add it to that `order` array too, in a sensible position, and add a label shortcut in the `c.textContent` chain if the prefix is new.
- **`hrs`** must have exactly **7** strings, **Monday first, Sunday last**. The open-now parser reads `"H:MM AM/PM – H:MM AM/PM"`. A bare noon open (`"12:00 – 9:00 PM"`) is fine — the parser treats it as noon. Use `"Closed"` for days the shop is closed. For split hours (e.g. an afternoon break), just use the full open→close span; the parser doesn't model mid-day gaps.
- **`p`** is always a **number** (`1`, `2`, or `3`). If Google shows no price level, estimate from reviews — don't use `null`.
- **`lat`/`lng`** are numbers, not strings.
- Keep `drinks` to ~4 items and `vibe` to ~4 short tags so cards stay tidy.

### Where to put it

Anywhere in the `SHOPS` array is fine — sort and filtering handle ordering at runtime. Grouping new shops near others in the same region just makes the diff easier to read.

## Before you open a PR

Run a quick sanity check that the array still parses and your entry is well-formed:

```bash
node -e "
const f=require('fs').readFileSync('la-boba-map.html','utf8');
const m=f.match(/const SHOPS\s*=\s*\[([\s\S]*?)\n\];/);
eval('var SHOPS=['+m[1]+'\n]');
console.log('Total shops:', SHOPS.length);
const bad = SHOPS.filter(s =>
  !s.n || typeof s.lat!=='number' || typeof s.lng!=='number' ||
  !Array.isArray(s.hrs) || s.hrs.length!==7 ||
  !s.drinks || !s.drinks.length || !s.seat || !s.park ||
  !s.vibe || !s.vibe.length || typeof s.p!=='number'
);
console.log('Malformed entries:', bad.length, bad.map(b=>b.n));
const names = SHOPS.map(s=>s.n);
console.log('Duplicates:', names.filter((n,i)=>names.indexOf(n)!==i));
"
```

You want: **Malformed entries: 0** and **Duplicates: []**.

Then open the file in a browser and confirm:

- Your shop's pin shows on the map
- The card expands and all four sections look right
- "Open now" reflects reality at the current time

## PR checklist

- [ ] Shop object follows the template (7 hours, numeric `p`/`lat`/`lng`)
- [ ] `h` matches an existing region (or the new region is wired into `buildHoods()`)
- [ ] No invented menus, prices, or safety claims
- [ ] Sanity script prints 0 malformed, 0 duplicates
- [ ] Verified visually in a browser

## Style notes

- Cards stay scannable: one sentence each for `seat` and `park`, short tags for `vibe`.
- Match the existing tone — plain, specific, useful. "Tight rear lot (~6 cars); street parking off Wells St is the smarter bet" beats "parking available."
- Keep the honesty bar high. When in doubt, link out instead of asserting.

Questions? Open an issue. Happy mapping. 🧋
