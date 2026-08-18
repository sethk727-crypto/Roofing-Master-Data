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
This repo auto-deploys to GitHub Pages on every push to `main` (see
`.github/workflows/deploy-pages.yml`). The live app:

**https://sethk727-crypto.github.io/Roofing-Master-Data/**

Bookmark it on your phone — it's your storm radar in your pocket. The page pulls fresh data on
every visit, so you never re-upload anything.

## Get the AI weekly plan
Open the live app, pick your date range, and click **⇩ Analyst JSON** in the header. It
downloads (and copies to your clipboard) `stormstrike-input.json` — the exact input the
`PROMPT.md` analyst expects. Paste it into a Claude session along with `PROMPT.md` and you get
back the ranked, routed Mon-Fri canvassing plan.

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
