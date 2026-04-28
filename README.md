# Our Plants

A one-page web app for keeping track of every plant on my property — but it kind of grew into a whole yard-management dashboard.

## What's in it

### The basics

Every tree, shrub, and container plant I own lives in here with its Latin name, where it's planted, when I bought it, what it cost, photos, tags — the whole deal. Around 50 plants right now, sortable, filterable, with a search box. Living plants, departed plants (RIP column), and a wishlist of stuff I'm thinking about getting are all tracked separately.

### A real-time gardening log

Tap a plant, pick an action (watered, fertilized, pruned, sprayed, frost damage, trunk measurement, whatever), add a note, done. The entries flow into a per-plant history, the front-page brief, and the stats. Frost events get tied to the damage they caused so I can look back and see "oh yeah, the Asayake got hit on April 10th."

### Weather is baked in

Pulls the NWS forecast for my coordinates (Lilly, PA), falls back to Open-Meteo if NWS is down, and uses it to generate a daily brief — like "lows hitting 28° on Thursday, here are the tender plants you should cover" or "no rain in 5 days, check these beds." Frost alerts auto-suggest covering tender or container plants by name.

### Photos

Each plant has its own photo timeline. They're hosted on Cloudflare R2 (basically free for personal use) and there's a rotating gallery on the front page that cycles through everything. Frost-damage shots get auto-flagged so they don't show up as a plant's "representative" photo on the print version or in the gallery.

### A clickable yard map

There's an aerial photo of the property and I can drop a colored pin for each plant exactly where it lives. Click a pin → the plant info pops open. Edit mode lets me drag pins around or place new ones. Pins are color-coded: green for trees, amber for shrubs, purple for vines.

### Trunk growth tracking

Log a trunk diameter at any date and the plant's info screen shows every reading oldest-to-newest with the delta between each measurement and total growth across all readings. So I can actually watch the Japanese maples thicken up year over year.

### Print-ready inventory

Hit print and it generates a clean PDF — summary stats up top, full plant table, a photo reference card for every plant. Looks like something you'd hand a landscaper.

### Sync everywhere

Data lives in a private GitHub repo as a single JSON file. Open the page on my phone or laptop and it pulls the latest. There's also a Google Sheet I can use as the source of truth — pull from the sheet and it merges in.

### The polish

- Two themes (a deep teal dark mode by default, and a cream light mode)
- Fahrenheit / Celsius toggle
- Hardiness zone, last/first frost dates configurable
- Mobile-first layout
- Installable as a PWA so it lives on the home screen like a native app

## How it's built

Single `index.html` file, no build step. Plain JavaScript, plain CSS. Hosted on GitHub Pages.

- **Data**: a single `data.json` in a private companion repo ([satownsend/our-plants](https://github.com/satownsend/our-plants)), read/written via the GitHub Contents API using a fine-grained PAT stored in `localStorage`.
- **Photos**: Cloudflare R2 bucket served from a public `pub-…r2.dev` URL.
- **Weather**: NWS (`api.weather.gov`) primary, Open-Meteo fallback.
- **Yard map**: stored alongside the data in the private repo, fetched as a blob URL using the same token.
- **Source-of-truth ingest**: optional Google Sheet (CSV export) sync for bulk plant edits.

It started as a way to remember "wait, what's the cultivar of that maple again?" and turned into the thing I check first thing in the morning before deciding whether to go water something.
