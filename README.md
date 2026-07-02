# ROXBACK — personal Hyrox comeback coach

A single-file PWA that plans, logs, and adapts Hyrox training around a recovering
patellar tendon. No backend, no accounts — all data lives in the browser's
storage on the device.

## Run it

```bash
python3 -m http.server 8471
# open http://localhost:8471
```

## Install on iPhone

The app needs to be served over HTTPS for full PWA behavior (offline + home-screen
install). Easiest free option: push this folder to a GitHub repo and enable
GitHub Pages, then on the iPhone open the page in Safari → Share → **Add to Home
Screen**.

## Files

- `index.html` — the entire app (UI, plan engine, charts, storage)
- `manifest.webmanifest`, `sw.js`, `icon-*.png` — PWA shell (installability + offline)
- `shot.html` — dev-only: renders the app at 390px width for headless screenshots
- `test.html` — dev-only: click-through smoke test; run headless and grep console for `[TEST]`

## Dev notes

- `#demo` in the URL seeds throwaway sample data (never written to storage);
  `#tab=race` etc. opens a specific tab. Combine: `index.html#demo&tab=progress`.
- Bump `CACHE` in `sw.js` when shipping changes (pages are network-first, so
  this only matters for the icon/manifest assets).
- Data model + adaptation rules are documented inline in `index.html`; key knobs:
  10%/week volume ramp, deload every 4th week, pain gating (amber −25%, red −50%),
  phase gate to Build = one 8 km run with knee ≤2/10 after.
- Zone colors and status colors were validated for colorblind separation and
  contrast against the dark surface (`#171A1F`).
