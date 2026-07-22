# ThirdEye prototype — deploy & hosting

A single self-contained page (`index.html`) plus a Netlify config (`netlify.toml`)
that proxies the CORS-sensitive feeds. No build step, no API keys, no backend.

## What's in this folder
- `index.html` — the whole app (CesiumJS from CDN + three live public feeds).
- `netlify.toml` — same-origin `/feeds/*` proxies so the hosted site has no CORS errors.
- `DEPLOY.md` — this file.

## Fastest way to put it online (2 minutes)
1. Go to **https://app.netlify.com/drop**
2. Drag this whole **`ThirdEye`** folder onto the page.
3. Netlify gives you a live URL like `https://random-name-123.netlify.app` — share it.

That's it. Because `netlify.toml` is included, the feed proxies work automatically,
so the aircraft/satellite/quake layers populate for you and your friends.

## Continuous deploy (optional, nicer long-term)
1. `git init && git add . && git commit -m "ThirdEye prototype"`
2. Push to a GitHub repo.
3. In Netlify: **Add new site → Import an existing project → pick the repo.**
   Leave build command empty, publish directory `.`. Every push redeploys.

## Custom domain
- In Netlify: **Site → Domain management → Add a domain.**
- Buy through Netlify, or buy anywhere and point its DNS at Netlify (they show the exact records).
- HTTPS is automatic (free Let's Encrypt cert).

## Why the proxy matters
Opened as a local file, the page calls the feed APIs directly; some browsers block
that (CORS), so the aircraft layer may show "unavailable". Hosted on Netlify, the
page calls same-origin `/feeds/...` paths and Netlify fetches the upstream
server-side — no CORS, and it's the same pattern the full build will use.

If a hosted feed still shows unavailable, the upstream may be refusing Netlify's
egress; the AIR layer's **SIM** control loads clearly-labelled sample data so the
symbology still demos. Satellites (CelesTrak) and earthquakes (USGS) are reliable.

## What this prototype is (and isn't)
This is the Phase-0/Phase-1 "get the globe right" milestone from the brief: a calm,
tactical globe with three genuinely-live, key-free layers and the honesty scaffolding
(evidence class, freshness, propagated-vs-observed labelling). It deliberately does
**not** yet include the Node/TypeScript backend, history/playback, geofencing,
fusion, or the Claude Analyst — those need the always-on backend and land in later
phases. Netlify hosts this frontend perfectly; the later live-streaming backend will
want a small always-on host (e.g. a cheap VPS) alongside it.
