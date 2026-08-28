# 168 — week time audit

A single-page web app for logging where all 168 hours of your week go, on your
phone. No accounts, no server, no build step. Everything is saved in your
browser's localStorage on that device. The grid taps and drags in 15-minute
blocks; typed entries (see below) are exact to the minute.

![7 x 24 grid](icons/icon-512.png)

## Use it

Four tabs:

- **Day** — data entry, one day at a time, zoomed in for easy tapping. **Tap** a
  cell to log 15 minutes of the selected category. **Press and hold ~0.2s, then
  drag** to paint a longer block. (The hold is what stops the page from scrolling
  while you drag.) Tap a filled cell again to clear it, or pick **Erase** and drag.
  **‹ ›** next to the date move a day at a time; tap the date to jump back to today.
  For exact-minute entries, use **+ → Type it in** (see below) — any new entry,
  from the grid or typed, overwrites whatever was logged there before.
- **Week** — the whole week at a glance, view only. Use **Day** to make changes.
- **Timer** — a big tile per activity. Tap one to start timing it; tap a different
  tile to switch (the previous activity is logged automatically); use **Stop & Log**
  or **Discard** on the running card to end it.
- **Totals** — hours and % of 168 per category, an average per day, and a stacked
  bar for each day of the week.

**↺** undoes the last stroke. **‹ ›** in the header move between weeks — each week
is stored separately. **⋯** has copy-last-week, category editing, backup
export/import, and clear-week.

The **+ Add Activity** sheet has two ways to set the time: **Pick** (quick
quarter-hour dropdowns) or **Type it in** (type an exact time like `9:07a`,
`2:30pm`, or `14:52` — down to the minute). It remembers whichever you used
last. The start time defaults to the minute the day's last logged activity
ended, so back-to-back entries continue where you left off.

## Put it on GitHub Pages

1. Create a new repo (e.g. `168`), and upload every file in this folder, keeping the
   `icons/` folder intact.
2. Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   branch `main`, folder `/ (root)`. Save.
3. Wait ~1 minute. Your app is at `https://<your-username>.github.io/168/`.

All paths are relative, so it works from a repo subfolder without any config.

## Add it to your iPhone home screen

Open the Pages URL in **Safari** (not Chrome — only Safari can install web apps on iOS)
→ Share → **Add to Home Screen**. It then launches full-screen with no browser chrome,
and works offline via the service worker.

## Notes on your data

- Data lives in localStorage under keys starting with `tt:` — one key per week
  (`tt:w:YYYY-MM-DD`, keyed to that week's Monday) plus `tt:cats` for your categories.
- It is per-device and per-browser. It does not sync between your phone and laptop.
- iOS can evict localStorage for sites you haven't opened in ~7 days. Installing to the
  home screen makes this much less likely, but use **⋯ → Export backup** now and then if
  the history matters to you. **Import backup** restores it.

## Changing things

Everything is in `index.html`. A few things you may want to edit:

- Default categories and colors: the `DEFAULTS` array near the top of the script.
- Color choices offered when adding a category: `SWATCH`.
- Block size: `--cell` in the CSS (`15px` per 15 minutes) for the Week overview, and
  `--cellz` (`34px`) for the zoomed-in Day view. Raising either makes cells easier
  to hit but pushes more of the day below the fold. If you change `--cellz`, also
  update the matching `CELLZ` constant near the top of the script (used for
  scroll-position math).
- Hold delay before painting starts: `170` (ms) in the `touchstart` handler.

If you edit any file after deploying, bump `CACHE = "168-v1"` in `sw.js` to `"168-v2"`,
otherwise phones that already installed it will keep serving the old cached copy.
