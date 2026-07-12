# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

ROXBACK — a personal training app for one user (Jochem) rebuilding toward his first
full Hyrox after patellar tendonitis. That injury context drives the design: the
adaptive plan engine treats knee safety as a hard constraint (daily pain check-ins
gate weekly volume; knee-heavy stations — wall balls, lunges, burpee broad jumps,
sled push — progress deliberately slowly). When adding or changing training logic,
"could this ramp load too fast for a healing tendon" outranks "is this optimal
training".

## Hard constraints

- Single-file vanilla JS PWA: all UI, logic, and styles live in `index.html`.
  No frameworks, no build step, no backend, no auth, no new dependencies.
- All user data is in `localStorage` under key `roxback-v1`. This holds the user's
  real training history — never change the stored schema without migrating existing
  data in `load()`.
- Dark theme only, race-yellow (#FFD22E) accent. Zone colors Z1–Z5
  (#7671CE #3697CE #2BA874 #C67C15 #E8503A) and status colors were validated for
  colorblind separation and 3:1 contrast on #171A1F — don't swap them casually.
- Metric units, pace as min/km, HR zones as % of user max HR.

## Key logic (all in index.html)

Adaptation knobs: ~8%/week volume ramp, deload every 4th week, pain gating
(amber = −25% volume + quality→steady swap, red = −50% + running off-legs),
phase gate Rebuild→Build = one 8 km run with knee ≤2/10 after. Race split model:
runs 47% / stations 47% / Roxzone 6% of target time. Weekly sessions have no
fixed order: any slot can be logged in any order, backdated (slot week follows
the workout date), or marked skipped (`state.skips[week] = [slot,...]`).

## Test & deploy

- No test framework. Smoke test (drives real click flows in an iframe):
  `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu --enable-logging=stderr --v=0 --virtual-time-budget=8000 --dump-dom http://localhost:8471/test.html 2>&1 >/dev/null | grep TEST`
  (serve first: `python3 -m http.server 8471`)
- Visual check at true iPhone width: screenshot `shot.html#demo&tab=<tab>` headless
  at window-size 500×2050 (`shot.html` iframes the app at 390px; `#demo` seeds
  throwaway data, never written to storage).
- Deploy = push to `main`; GitHub Pages serves https://mechoj08.github.io/roxback/
  in ~1 min. Bump `CACHE` in `sw.js` when icons/manifest change (pages themselves
  are network-first).
