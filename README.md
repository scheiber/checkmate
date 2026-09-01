# Checkmate

A study time tracker that works like a chess clock. Two sides, one running at a
time: **Focused** and **Break**. Tap a side to hand the time over, the way you
slap a chess clock to pass the turn.

The whole app is a single file, [index.html](index.html) — no build step, no
dependencies to install. Open it in a browser and it runs.

## How it works

- Press **Start studying** (or tap either panel) and the session opens on
  Focused. Sessions always start focused; break time can never be the first
  thing logged.
- While a session is running, tapping either panel flips the clock to the other
  mode. The active side glows and its pip pulses.
- Every switch is logged to the **This session** ticker with a timestamp.
- Press **End** to save the session. You get a confirmation with the total time,
  then it drops into **Past sessions**.
- Be honest with yourself — only count time when you are truly focused, and
  switch to break when you step away or lose focus.

## Storage

Everything is stored locally on the device with IndexedDB (database
`studyClockDB`). Nothing leaves the machine unless you turn on sync.

### Past sessions

Each saved session shows its title, date, total time, and a focused/break ratio
bar. Expand a card to see the exact split, or delete it.

### Backup

From **Settings** you can:

- **Export JSON** — full backup of every session.
- **Export CSV** — one row per session (title, timestamps, focused/break/total
  in both `hh:mm:ss` and seconds, switch count, sync status).
- **Import JSON** — merge sessions from a backup file.
- **Clear all data** — wipe every session and setting on the device.

### Optional cloud sync

Set a **Sync endpoint** in Settings and enable **Sync sessions on end**.
Finished sessions are then `POST`ed as JSON to that URL. The endpoint is assumed
to be an API Gateway endpoint backed by DynamoDB, but anything that accepts a
JSON POST works. Each session card is tagged `local only`, `synced`, or
`sync failed`.

## Tech notes

- Single static HTML file — HTML, CSS, and vanilla JS inline.
- External resources: W3.CSS and Google Fonts (Spectral, JetBrains Mono).
- No framework, no bundler, no server required.
- Warns before you close the tab while a session is still running.
