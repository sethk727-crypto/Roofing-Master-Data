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
**⚡ Targets**, **🚨 Alerts**, and **🌩 Forecast**. Tapping a target card flies the map to it.

## Get the AI weekly plan
Open the live app, pick your date range, and tap **⇪** in the header. On iPhone/Android it
opens the share sheet with `stormstrike-input.json` (share it straight into the Claude app);
on desktop it downloads the file and copies it to your clipboard. That file is the exact input
the `PROMPT.md` analyst expects — paste it into a Claude session along with `PROMPT.md` and you
get back the ranked, routed Mon-Fri canvassing plan.

Because all the data is fetched live in the browser, the GitHub page is always current — you
never have to re-upload anything. To change the home base or radius, edit the `CONFIG` block at
the top of the `<script>` in `index.html` and re-commit.

## Data sources (all free, public, no key)
- **Iowa Environmental Mesonet** — NWS Local Storm Reports (hail, wind damage, gusts).
- **api.weather.gov** — live severe-weather watches & warnings.
- **NOAA SPC** — convective outlook (day 1-3 severe-storm risk areas). *Best-effort: a few
  networks block spc.noaa.gov; if so, the app still shows reports + active alerts, and the
  status pill turns amber.*

## How the "best impact points" score works
Recent reports are clustered into swaths (~9 mi) and scored 0-100 on: hail size, how fresh the
newest event is (heavily weighted — fresh beats big-but-picked-over), nearby wind/gust density,
and drive distance from Media. Top clusters are ranked in the sidebar and circled on the map.
Green = damage in the last 4 days. The same rubric lives in `PROMPT.md` so an AI model scores
it identically.

## Honest limits
Local Storm Reports are volunteer/spotter reports — **ground-truth a neighborhood before
promising anyone anything.** Nothing here predicts a claim approval; every homeowner
conversation is a free documented inspection and an adjuster-*viability* read. Door-knock and
mail freely; only call or text numbers a homeowner personally gave you.
