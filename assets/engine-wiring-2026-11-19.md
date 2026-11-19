# Engine wiring — what was actually hooked up, and two tested drafts (2026-11-19, ~14:20 EST)

Captain asked for one master file to run every Quote Challenge game, and for
Christmas to run only off its own 25-quote file. The files were made, but the
pages were never actually switched over. Checked 2026-11-19 afternoon:

- `christmas.html` still carries all 46 quotes embedded in the page (`var QUOTES = [...]`).
  It never fetches `quotes_291-25.json`. The 25-file is a subset of those 46.
- `one-player.html` and `bonus.html` still fetch `movies_and_shows.json` (the old
  8-entry file, 0 playable). Neither fetches `quotes_291-master.json` (411 quotes).
- `two-player.html` fetches `quotes.json` — a file that is not in the draft set at
  all — while its own status line still says `movies_and_shows.json`. On the live
  site it most likely plays only its 14 embedded seed quotes.
- No game yet keeps a seen-quotes list across plays, and there is no
  "Success! +10 points" style message on a first right answer (the games show
  "Source correct." and the points on the clock).

Nothing live changed. Both drafts below are in the workspace for Captain's approval.

## Draft 1 — Christmas wired to its own file

- `workspace/goals/christmas-special-quote-challenge-page/files/christmas-github/christmas-wired-25.html` (29,482 bytes)
- `.../christmas-github/quotes_291-25.json` — identical copy of the 25-file (same md5)
- Original `christmas.html` (31,121 bytes) untouched.

What changed in the draft, and nothing else:

- The 46 embedded quotes are gone (0 `{q:"` entries left). The page fetches
  `quotes_291-25.json` beside it and maps master format to the page's format
  (`s` → movie, `character` → who said it). The JSON file itself is untouched,
  still pure master format.
- Year, category and type are not in that file, so they live in a small META
  table in the page, keyed by exact quote, taken from the old page. The 5
  speaker wordings that were fuller on the page are kept as display text
  (e.g. "Johnny, in Angels with Filthy Souls", "Clarence Odbody (his inscription)").
  If a quote is added to the file later, add one META line for it (year +
  category); without one it shows with no year and lands in Family Favorites.
- Free 10 remapped to positions in the 25-file: `[3, 4, 0, 6, 7, 16, 9, 14, 24, 8]`.
  Two old free-10 picks are not in the 25 file, so they are swapped:
  "I triple-dog dare you!" for "You'll shoot your eye out, kid." (same film),
  "Bumbles bounce!" for the Charlie Brown line (keeps a cartoon in the free 10).
- Rule buttons stay disabled until the file loads. If the file is missing, the
  page says so plainly instead of showing an empty library.

Tested 2026-11-19 with Node, running the draft's own script against the real
25-file: 25 quotes load, all with movie/speaker/difficulty/year/category;
library shows "25 of 25 quotes", 25 cards; free 10 are 10 distinct valid picks;
buttons enable; with the file missing, the page shows "Could not load
quotes_291-25.json — upload it beside christmas.html." and the buttons stay off.

His call — going from 46 library quotes to 25 drops 21 quotes from the page
(they stay safe in the old `christmas.html`). If he wants all 46, the file
should be grown to 46 instead of the page shrinking to 25. The 25-file has no
before/after lines filled in, so there is nothing extra for the page to show.

Upload if approved (rename the draft to `christmas.html` when uploading):
`christmas.html`, `quotes_291-25.json` beside it, plus the 3 banners in
`assets/` — 5 files, same pattern as before.

## Draft 2 — One-player wired to the master file

- `workspace/goals/quote-challenge-website-testing/files/one-player-wired-master.html`
- Only 2 lines changed vs `one-player.html`: fetch `quotes_291-master.json`
  instead of `movies_and_shows.json`, and the status line names the new file.
  The master file already sits in that folder (411 quotes).

Tested 2026-11-19 with Node, running the draft's own script against the real
master file: status reads "410 playable quotes · 411 from
quotes_291-master.json · 0 browser additions." — 410 playable, not 411,
because "There's no place like home." is in both the master file and the
page's 14-quote seed and the game dedupes it. If the file is missing, it falls
back to the 14 seed quotes, same as before. Two-player and bonus are NOT
changed yet — one-player first, as planned, so Captain can see it work.

## Still open — his call, not built yet

- Two-player: fetch `quotes.json` or the master file? Its status line also
  needs to match whichever file it fetches.
- Bonus: same one-line switch as one-player, once one-player is approved.
- No repeats: he wants no quote to come up twice on a player's browser. That
  needs a shared seen-list across all the games (one list, or one per game?)
  that resets once every quote has been seen. Not built — needs his call on
  shared vs per-game.
- First-right-answer message: he described "Success! +10 points" / "+1 point"
  on the first right answer. Current games say "Source correct." with the
  points on the clock. Wording is his call.
- Spell checker: the local script exists
  (`quote-challenge-website-testing/hidden_files/spellcheck-quotes.py`, no
  external dictionary), but it would not run in this session — `pyspellchecker`
  is not installed here (`ModuleNotFoundError`). The 25-file was already
  reported spell-checked; the script needs its module installed before it can
  be run again on a new file.

## Related, already handled elsewhere

- Daily 100 quotes: Day 1 done (311 → 411). Daily job `daily-quote-challenge-100`
  runs ~10:10 EST, next run Nov 20, through Nov 26.
- 8.0 page: starts over from scratch, per his decision; restart brief saved at
  `grand-event-page-8-0/files/8.0-restart-brief-2026-11-19.md`. Not touched here.
