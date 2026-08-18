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

## Put it online so it "updates live" for you (free, ~3 minutes)
GitHub Pages hosts it for free and the page pulls fresh data on every visit.

1. Create a free account at github.com if you don't have one.
2. Click **New repository** → name it `stormstrike` → **Public** → **Create**.
3. Click **uploading an existing file** → drag in `index.html` (and the other two files) → **Commit**.
4. Go to the repo's **Settings → Pages** → under "Branch" pick **main** / **/(root)** → **Save**.
5. Wait ~1 minute. Your live app is at:  `https://<your-username>.github.io/stormstrike/`
   Bookmark it on your phone — it's your storm radar in your pocket.

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
