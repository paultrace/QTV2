TRADER DRILL — install guide
============================

A self-contained, offline-first mental drill app for quant trading prep.
Three modes, persistent score history, interactive tutorial.

FILES
-----
  index.html              the app
  sw.js                   service worker (offline cache)
  manifest.webmanifest    PWA metadata
  icon-192.png            app icon
  icon-512.png            app icon (large)
  README.txt              this file

MODES
-----
PARITY
  Solve a leg of put-call parity. Two models:
  - Real (default): straddle + r/c, matches the parity-zetamac trainer
    C + P = straddle,  C - P = S - K + r/c
    Solve for C, P, S, straddle, or r/c.
  - Simple: zero-rate C - P = S - K, with optional discounting toggle.

MARKET MAKING
  You're shown an instrument's fair value (abstract or via parity); quote
  a two-sided market (bid then ask). Each quote scores 0-100: half on
  width (tighter wins, up to the target), half on centre (mid vs fair).
  Quotes through fair are pick-offs and score zero. Toggleable per-round
  or session timer, tight/medium spread target, pick-off detection on/off.

POSITION P&L
  Given a single-leg position (long/short × call/put), strike, premium,
  and terminal stock S_T, compute total P&L at expiry. Quantity 1, 2-10,
  or 10-100 contracts; optional contract multiplier (default 100).

SCORE HISTORY
  Every completed session is saved to localStorage with a trend chart
  per mode. Export-to-JSON button in Settings for off-device backup.
  Cap of ~800 sessions before oldest start to trim.

INSTALL
-------
The service worker needs http(s) — it will NOT run from a file:// URL.
Two paths:

A) GITHUB PAGES (recommended — proper PWA, ~5 min)
   1. New GitHub repo, e.g. "trader-drill".
   2. Upload all five files to the repo root.
   3. Repo Settings > Pages > Source: "Deploy from a branch",
      branch "main", folder "/ (root)". Save.
   4. Wait ~1 min, open the URL it gives you (https://<you>.github.io/trader-drill/)
      in Safari on your phone.
   5. Share > Add to Home Screen. Done.
   Service worker registers, app is fully cached, survives iOS clearing
   web data more robustly than an unhosted PWA.

B) QUICK — no hosting (service worker stays dormant)
   AirDrop or email index.html to your phone, open in Safari, Share >
   Add to Home Screen. Still 100% offline (all logic is inline). You
   just don't get service-worker cache hardening.

UPDATING
--------
When you replace index.html on GitHub Pages, bump CACHE_VERSION in
sw.js (currently "trader-drill-v4") so already-installed devices
fetch the new version on next open.

DATA
----
Settings and score history live in browser localStorage, per-device
and per-browser. They survive normal use but iOS can evict storage
for web apps not opened in a while — export your history periodically
from Settings > Data > Export if you care about long-term records.
