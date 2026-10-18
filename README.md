# RimRunnerHawaii

Unofficial Hawaii HS boys basketball ratings. Not affiliated with the HHSAA, its leagues or MaxPreps.
Status: **HELD (waiting on hosting)**. Preview: https://joshua-kpg.github.io/RimRunnerHawaii/ (GitHub Pages). No Firebase project yet
(the Google accounts are at their project quota) and Cloudflare Pages is at its project limit, so accounts, picks and follows save only in the
visitor's browser until the site's own Firebase project exists.

## Files
- `index.html` — the whole app (data embedded). `logos/` — small WebP school logos (from MaxPreps school profiles), served with the site.
- `data/results.json` — 2026-27 finals ledger written by the nightly updater.
- `firebase/` — Firestore rules and auth config for the site's future Firebase project (accounts, picks, follows).

## Teams
66 teams: the HHSAA (Hawaii High School Athletic Association) 2026-27 boys basketball Division I and Division II eligible teams
(HHSAA 2026-27 league representation sheet, Winter tab) that have a MaxPreps boys varsity team: Division I 30, Division II 36.
Conferences are the real leagues split by division: OIA Division I East (6), OIA Division I West (6), OIA Division II (10),
ILH Division I (7), ILH Division II (6), BIIF Division I (6), BIIF Division II (9), MIL Division I (5), MIL Division II (7), KIF (4, all Division II).
Not on the site: Anuenue (1 game in 2025-26, none on the 2026-27 schedule), Christian Academy (no current MaxPreps varsity schedule),
Connections, Makua Lani and Maui Christian (no MaxPreps basketball team). No Other (non-HHSAA) programs met the 8-game rule.

## Season rule
HHSAA 2026-27 boys basketball season: weeks 19-33, starting Mon Nov 9, 2026. Games involving an HHSAA team before Nov 9 are left out
(schedule build and nightly updater); games between two non-members are kept. Everything from Nov 9 on counts, including mainland and
holiday tournaments. MIL Division II plays Friday/Saturday back-to-back league series; both games count (the shared tournament-repeat
filter is replaced by a variant that only collapses 3+-game clusters or two tournament games).
HHSAA state tournament: Division I Feb 15, 17-19, 2027; Division II Feb 17-19, 2027.

## Nightly scores
`.github/workflows/nightly-scores.yml` runs `scripts/update_scores.py` at 10:30 and 18:00 UTC (12:30 am / 8:00 am HST) and
on manual dispatch. Before `SEASON_START` in index.html (the first game on the MaxPreps schedules, Sat Nov 14, 2026) the script makes no requests.
From then on it reads boys basketball finals for the last 3 days from the MaxPreps Hawaii scores-by-date page (team-page fallback), matches them
to the schedule by MaxPreps team id + date, records them in `data/results.json` and bakes scores and W-L records into `index.html`. The page
recomputes Elo from those finals. It commits only when data changed; GitHub Pages updates from the commit.
