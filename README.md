# StormStrike — Live Roof Lead Radar

A single-file web app that shows, live in your browser, every hail and wind-damage report within
120 miles of Delaware County, PA, ranks the best impact points to canvass this week, overlays
the National Weather Service's forecast risk for **upcoming** storms, and lists active severe
alerts. No server, no database, no API key — it fetches public NWS data client-side every time
it loads and auto-refreshes every 15 minutes.

## What's in here
- **`index.html`** — the entire app. Open it and it works.
- **`PROMPT.md`** — the ROOF-IQ Storm Targeting Analyst master prompt (feed it the live data to
  get a ranked, routed weekly plan from Gemini/Claude).
- **`leads-template.csv`** — starter file for importing your address lists into the Leads tab.
- **`README.md`** — this file.

## Run it right now (no setup)
Double-click `index.html`. It opens in your browser and starts pulling live data.

## It's already online
This repo auto-publishes to GitHub Pages on every push to `main` (see
`.github/workflows/deploy-pages.yml`, which mirrors `main` to the `gh-pages` branch). The live app:

**https://sethk727-crypto.github.io/Roofing-Master-Data/**

The page pulls fresh data on every visit, so you never re-upload anything.

## Deploy it anywhere else in one click
The repo is a zero-build static site, pre-configured for the big hosts:

- **Vercel** — [deploy now](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fsethk727-crypto%2FRoofing-Master-Data)
  (or vercel.com → **Add New → Project** → import `Roofing-Master-Data` → **Deploy**; `vercel.json`
  is already in the repo, no build settings needed — leave everything default). Every push to
  `main` auto-redeploys.
- **Netlify** — [deploy now](https://app.netlify.com/start/deploy?repository=https%3A%2F%2Fgithub.com%2Fsethk727-crypto%2FRoofing-Master-Data)
  (`netlify.toml` included, publish directory is the repo root).

## Put it on your iPhone like an app
1. Open the live URL above in **Safari**.
2. Tap the **Share** button → **Add to Home Screen** → **Add**.
3. It installs as a full-screen app with the StormStrike bolt icon — your storm radar in your
   pocket. Works the same on Android (Chrome → ⋮ → Add to Home screen).

On a phone the map is full-screen with a swipeable bottom sheet: drag the grab bar up/down
(or tap it) to switch between map view, half view, and full list. Tabs switch between
**⚡ Targets**, **🕘 History**, **📍 Leads**, **🚨 Alerts**, and **🌩 Forecast**. Tapping a
target card flies the map to it.

## 3-month storm history
The app always pulls the full **last 3 months** of NWS reports in one shot — the range dropdown
just filters that instantly (no reload). The **🕘 History** tab lists every storm day in the
window with hail sizes, wind counts, and the towns hit; tap a day to light its reports up on
the map. Everything in it is still inside the typical 1-year insurance-claim window. The last
good dataset is also saved on your device, so the app opens with data even on a bad connection.

**If Targets is empty, that's real weather, not a bug** — it means no reportable hail/wind
damage inside 120 mi for the selected window. The empty screen now says so and offers a
one-tap "Show last 3 months."

## Leads: your address list on the map
Tap **📍 Leads → ⇧ Import CSV** and pick a spreadsheet of addresses (export one from wherever
your lists live). Columns it understands: `name`, `address`, `city`, `state`, `zip`, `notes` —
or `lat`/`lon` if you already have coordinates. Rows without coordinates are located
automatically (OpenStreetMap, ~1/sec). A starter file is in the repo: **`leads-template.csv`**.

Or skip the file entirely: tap **⚡ Airtable** and paste a personal access token
(airtable.com/create/tokens, `data.records:read` scope), your base ID, and a table name — the
browser pulls the records straight from your base (token is saved only on your device, sent
only to api.airtable.com). It understands the Roofing Lead Engine columns (Property Address,
Town, ZIP, County/State, Owner Name, Campaign, Status, Year Built) and re-syncing never
overwrites statuses you've set. Tap ⚡ again anytime to pull new records.

Each lead becomes a colored pin + card. Tap the status chip (or the pin's popup) to cycle
**New → Visited → Follow-up → Signed → Dead** — colors update on the map and everything is
saved on your device. Every lead and every target zone has a **🧭 Navigate** button that opens
turn-by-turn directions. Top target zones also show a street-level **"Start near"** address —
a concrete corner to park the truck and start knocking.

## Live location in the field
Tap the **🎯** button on the map to turn on your live GPS position — a blue dot with an
accuracy ring that follows you as you walk a neighborhood, so you can see exactly which storm
pins and lead pins are around you. Tap again to turn it off. (Your location never leaves the
phone; the browser will ask permission the first time.)

## Command-center look
The app defaults to a dark, high-contrast command-center theme (dark map tiles included).
The **☀/🌙** button in the header flips light/dark; your choice is remembered on the device.

## Exports — the ⇪ button
The **⇪** button opens a small export menu with three one-tap actions:

1. **🤖 Analyst JSON** — the exact input `PROMPT.md` expects. On iPhone/Android it opens the
   share sheet (send it straight into the Claude app); on desktop it downloads + copies to
   clipboard. Paste it into a Claude session with `PROMPT.md` to get the ranked, routed
   Mon-Fri canvassing plan.
2. **📊 Zones CSV** — the ranked target zones (rank, name, lat/lon, start address, score, hail,
   recency, drive miles, wind counts) formatted for territory apps like Spotio: import it as a
   pin list.
3. **🔗 Send to n8n** — POSTs the ranked zones as JSON to an n8n webhook URL you enter once
   (saved on the device) for automated CRM ingestion. In your n8n Webhook node, allow this
   site's origin under CORS "Allowed Origins" (or `*`).

Because all the data is fetched live in the browser, the GitHub page is always current — you
never have to re-upload anything. To change the home base or radius, edit the `CONFIG` block at
the top of the `<script>` in `index.html` and re-commit.

## Data sources (all free, public, no key)
- **Iowa Environmental Mesonet** — NWS Local Storm Reports (hail, wind damage, gusts). The app
  asks for a 120-mi radius around Media, PA **and** re-checks every report's distance itself,
  so nothing outside the service area sneaks in.
- **api.weather.gov** — live severe-weather watches & warnings (PA, NJ, MD, DE, NY, CT, VA —
  every state the 120-mi circle touches).
- **OpenStreetMap Nominatim** — turns lead addresses into pins and zone centers into
  street-level starting addresses.
- **NOAA SPC** — convective outlook (day 1-3 severe-storm risk areas). *Best-effort: a few
  networks block spc.noaa.gov; if so, the app still shows reports + active alerts, and the
  status pill turns amber.*

## How the "best impact points" score works
Recent reports are clustered into swaths (~9 mi) and scored 0-100 on: hail size (with a bonus
for hail on multiple days), how fresh the newest event is (heavily weighted — fresh beats
big-but-picked-over), nearby wind/gust density, and drive distance from Media. Wind-only
swaths are capped at 60 so they never outrank a real hail core. Top clusters are ranked in the
sidebar and circled on the map. Green = damage in the last 4 days. The rubric matches
`PROMPT.md` exactly (housing scores as "unknown" here since the app has no parcel data), so an
AI model scores it the same way.

## Honest limits
Local Storm Reports are volunteer/spotter reports — **ground-truth a neighborhood before
promising anyone anything.** Nothing here predicts a claim approval; every homeowner
conversation is a free documented inspection and an adjuster-*viability* read. Door-knock and
mail freely; only call or text numbers a homeowner personally gave you.
