# Building Manager Shift Draw

A weighted, replayable alternative to [wheelofnames.com](https://wheelofnames.com/) for scheduling UW–Madison building manager shifts. Runs entirely in the browser — no backend, no login, just this one HTML file.

**Live page:** _add your GitHub Pages link here once it's live_

## Why this exists

The old process (a plain random wheel spin) had two problems:

1. **No memory** — whoever landed last in the spin order one week had the same odds of landing last again the next week, so the same people kept ending up with the leftover shifts.
2. **Slow** — spinning a full animated wheel once per shift, for every shift in the week, ate a lot of meeting time.

This tool fixes both: it weights the draw by where each person landed last time, and it computes the whole week's order instantly, then reveals it one shift at a time with a bit of suspense instead of a slow spinning animation.

## How the weighting works

Each person's weight for this week = the position they landed in **last week's** draw (1st drawn = weight 1, last drawn = weight N). Higher weight means a higher chance of being drawn *early* this week — so whoever got stuck with the worst leftover shift last time gets first crack at a good one this time. New people with no history default to a middle-of-the-pack weight. Weights can also be manually overridden per person (e.g. someone on reduced hours) from the Roster tab.

## Scheduling rules it encodes

1. **Weekend shifts first** — the 8 fixed shifts (Fri MU/US Close, Sat MU/US Open & Close, Sun MU/US Open) are drawn first, and are eligible for the swap rule.
2. **Shift swap** — anyone assigned one of the first 10 shifts who can't work it can hand it to a volunteer who hasn't appeared on the wheel yet. Their own name still counts for next week's weighting even though someone else works the shift.
3. **Badger Bash staffing** — needs 4 building managers; volunteers/already-signed-up people fill first, the wheel only fills what's left.
4. **Red Gym before SAC** — Red Gym shifts are drawn before SAC shifts.
5. **SAC splitting** — any SAC shift can be marked splittable, letting two people share it.

## Using it week to week

1. **Roster tab** — keep the full list of building managers up to date. Weight and last week's position show automatically.
2. **This Week's Shifts tab** — edit the shift list for the week (add/remove Red Gym or SAC shifts, toggle Badger Bash on, add its volunteers).
3. **Draw & Results tab** — click **Run the wheel** to lock in the full weighted order, then click **Draw!** to reveal shifts one at a time in the meeting. Swap or split any shift as needed.
4. **Finalize & save to history** once every shift is revealed — this is what sets next week's weights.
5. **History tab** — a record of every finalized week's draw order.

## A note on where data lives

There's no shared database — the roster, this week's shifts, and history are all stored in the browser's local storage on whichever device runs the tool. Opening the GitHub Pages link on a different computer starts with a blank slate. In practice, that means one person (or one shared computer) should be the one who runs the actual weekly draw, so the roster and history stay continuous. If you outgrow that, the tool would need a small shared backend (e.g. Firebase or Supabase) to sync across devices.

## Updating the tool

Edit `index.html` directly in GitHub (pencil icon) or upload a replacement, then commit. GitHub Pages redeploys automatically within a minute or so.
