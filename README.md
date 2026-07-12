# Game Night Tracker

A board game night stat tracker for a weekly crew running since March 2015. Contains 677 imported games (2015–2026) plus tools to log new ones.

## Contents

| File | What it is |
|---|---|
| `game-night-tracker.jsx` | React app: scorepad, per-game stats, editable history, game entry form, CSV export. All 677 historical games are embedded as seed data. |
| `game_night_tracker.xlsx` | The parallel data file / backup of record. `Game Log` sheet holds one row per game; `Leaderboard` and `Games` sheets recalculate automatically (formulas cover rows up to 3000). |

## The app

Built as a Claude artifact (single-file React, no build step). Tabs:

- **Scorepad** — leaderboard with year filters. Wins render as tally marks color-coded by game (top 10 most-won games get fixed colors; the rest are gray). All-time view uses stacked bars.
- **Games** — every title with play count, last played, and current leader.
- **History** — full log, newest first; every entry (including imported ones) can be edited or deleted. Edits to imported rows are stored as overrides.
- **Add** — tap-to-select players/winners, co-op Win/Loss handling for Pandemic, new game/player support. Player chips are ordered by attendance frequency.

### Persistence

The app uses the Claude artifact `window.storage` API under the key `gamenight-additions-v1`:

```json
{ "additions": [ ...entries added in-app ], "overrides": { "seed-N": { ...edited fields } } }
```

**Note:** `window.storage` only exists inside Claude artifacts. To run this app anywhere else (Vercel, local dev, etc.), replace the storage calls with `localStorage` or a backend — they're isolated in the load effect and the `persist()` function.

### Syncing app → spreadsheet

The app can't write to the xlsx. Use **History → Export → "New & edited (CSV)"**, copy the text, and paste rows into the bottom of the `Game Log` sheet. Column order matches. Edited imported rows can't be auto-replaced — find and fix those rows manually.

## Data provenance

Imported from the original `2015_gaming_stats.xlsx` (a Google Sheets export) in July 2026, with cleanup:

- Game/player name normalization ("Munchkin " → "Munchkin", "*ennis" → "Dennis", "PAul" → "Paul", casing fixes)
- Winners assumed present when a row had no attendance recorded (8 co-op games still have no attendance data)
- One 2015 entry read "Ling of" — assumed to be Kings of Tokyo, flagged in its notes
- Pandemic results recorded as "Win"/"Loss" (table vs. board) rather than a player name

## Players

Lucanor, Dennis, Ian, Mark, Alistair, Martin, Gavin, Rod, Miguel, Andy, Paul, Andre, Johnny
