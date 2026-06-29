# Contour — Trail Segment Analyzer

Contour finds the trail segments you've ridden more than once across your GPX
tracks and ranks every attempt fastest-first — your own private,
Strava-style leaderboard, with no account and no server. Everything runs in the
browser from a single `index.html`.

**Live app:** https://jwbono.github.io/trail-segments/

---

## What it does

- **Automatic segment detection.** Drop in two or more rides and Contour finds
  the stretches of trail they share — no manual segment drawing.
- **Personal progress.** Each segment shows your PR, your latest run vs your
  best, where you stand among all your attempts, and a time-trend sparkline —
  with a 🎉 PR alert when a newly imported ride beats your best.
- **Leaderboards.** Every matched effort is ranked fastest-first and dated, with
  deltas to the best time and per-effort speed.
- **Ride stats.** Distance, elapsed/moving time, vertical gain/loss, average and
  max speed, and an elevation profile for each ride.
- **Sub-segments.** Split any segment at points you click on the map; each part
  gets its own ranking, plus a "latest run, part by part vs your best" breakdown.
- **Organize.** Rides carry editable tags and dates; filter the Rides tab by
  rider, tag, or name search. Segments group by area (Tahoe, Bay Area, …).
- **Clean imports.** Duplicate GPX uploads are detected and skipped, so
  re-uploading the same ride from your phone won't clutter your data.
- **Metric or imperial.** One toggle converts every distance, speed, and
  elevation. Your choice is remembered.
- **Optional sharing.** Push your rides and detected segments to this GitHub repo
  so friends can pull them in and compare.

Your data stays in your browser (`localStorage`) unless you explicitly sync it to
the repo.

---

## Basic usage flow

1. **Add rides.** Drag `.gpx` files onto the **Rides** panel, or tap it to pick
   files (multiple is fine). No files handy? Click **Demo rides** to explore with
   sample data.
2. **Segments detect themselves.** Any stretch ridden on at least *Min rides*
   rides is matched automatically as you add rides, and listed in the
   **Segments** panel (grouped by area — e.g. Tahoe vs Bay Area — when your
   rides span more than one region). Beat your best on a segment and you'll get
   a 🎉 PR alert. Adjust the detection controls (below) and click **Detect
   segments** to re-run by hand.
3. **Track your progress.** Click a segment to open your **PR**, your **latest
   run vs your best**, a **time trend**, and every attempt ranked — plus the
   elevation profile and start/finish pins on the map. Click a ride's name to
   see full ride stats. Re-running detection preserves any segments you've
   renamed or split.
4. **Split (optional).** Inside a segment, choose **Split into sub-segments** and
   click the highlighted trail to drop cuts. Click a white dot to remove a cut.
   Each part is ranked separately. Rename segments or parts by typing in their
   title fields.
5. **Switch units.** Toggle **Metric / Imperial** in the top bar at any time.
6. **Share (optional).** See [Syncing](#syncing-with-the-repo-optional) below.

### Detection controls

| Control | Meaning | Default |
| --- | --- | --- |
| **Match radius** | How close two tracks must be to count as the same trail. Raise it if detection misses obvious overlaps; lower it if unrelated trails get merged. | 25 m |
| **Min length** | Shortest stretch that counts as a segment. | 250 m |
| **Min rides** | How many rides must share a stretch for it to become a segment. | 2 |

Tip: if detection comes back empty, **raise Match radius** or **lower Min
length**. Detection always works in meters internally; the labels just follow
your unit choice.

---

## Syncing with the repo (optional)

Sharing is opt-in and uses this GitHub repository as the backing store.

1. Create a **fine-grained GitHub token** scoped to this repo with
   **Contents: read/write**.
2. In the app, open **GitHub token (needed to save)** and paste it. The token is
   stored only in your browser and is never written to the repo.
3. Pick your **rider name** (or add a new one).
4. **Save rides** writes your rides to `data/rides/<name>.json` and updates
   `data/manifest.json`. **Save segments + cuts** writes the detected segments to
   `data/segments.json`.
5. Anyone opening the app can press **Reload all** to pull every rider's shared
   rides and segments.

> Note: rides synced this way are public — anyone who can see the repo can see
> them.

---

## Running locally

The app is a single static file with no build step. Any static server works:

```bash
# from the repo root
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly via `file://` mostly works, but serving over HTTP
is recommended so the relative `data/` sync requests behave the same as on
GitHub Pages.

External dependencies are loaded from CDNs at runtime: **Leaflet** (map) and
map tiles (Esri / OpenTopoMap / OpenStreetMap). An internet connection is needed
for the map to render.

---

## Data & privacy

- Rides and detected segments are cached in your browser's `localStorage`
  (`contour:*` keys). **Clear all** removes them.
- Nothing leaves your browser unless you use the sync buttons with a token.
- GPX parsing, segment detection, and all stats run entirely client-side.

---

## Project layout

```
index.html              The entire app (markup, styles, logic)
data/manifest.json      List of riders who have shared rides
data/rides/<name>.json  Per-rider shared rides (created on sync)
data/segments.json      Shared detected segments (created on sync)
```
