# BM Drafts

A weighted, live-synced alternative to [wheelofnames.com](https://wheelofnames.com/) for scheduling Wisconsin Union building manager shifts. Runs as a single HTML file backed by a small Supabase database, so it works the same way on every device instead of being stuck in one browser's local storage.

**Live page:** _https://1immy.github.io/union-BM-scheduler/_

## Why this exists

The old process (a plain random wheel spin) had two problems:

1. **No memory** — whoever landed last in the spin order one week had roughly the same odds of landing last again the next week, so the same people kept ending up with the leftover shifts.
2. **Slow** — spinning a full animated wheel once per shift, for every shift in the week, ate a lot of meeting time.

This tool fixes both: it weights the draw using a running fairness score that carries over week to week, computes the whole week's order instantly, then reveals it one pick at a time with a confetti moment instead of a slow spinning animation.

## How the weighting works

Each person has a **credit** that persists across weeks instead of resetting. After every finalized draft, whoever landed later than the middle of the field gains credit; whoever landed earlier loses some. That credit carries forward, so repeated bad luck keeps compounding until it's actually corrected — not just reset the following week. Weight for the next draft = a neutral baseline + that credit. New people with no history start at the baseline. Weight can also be manually overridden per person (e.g. someone on reduced hours) from the Roster tab, and "Reset weighting" zeroes everyone's credit for a fresh season without deleting the saved history log.

## Scheduling rules it encodes

1. **Weekend shifts first** — the 8 fixed slots (Fri MU/US Close, Sat MU/US Open & Close, Sun MU/US Open) are drafted first and are eligible for the swap rule. Each is tagged with a day (Fri/Sat/Sun) so the live status board can group them.
2. **Shift swap** — anyone drafted into one of the first 10 picks who can't work it can hand it to a volunteer who hasn't been revealed on the wheel yet. Their own name still counts for next week's weighting even though someone else covers it. (A "Swap Insurance" Brownie Shop perk can unlock this outside the normal first-10 window.)
3. **Special events** — Badger Bash, Idea Fest Support, or anything similar; you can set up more than one per week. Volunteers/already-signed-up people fill first, the draft only fills what's left.
4. **Red Gym before SAC** — both are recurring fixed slot lists (edit them once, they persist week to week) drafted in that order, after special events.
5. **Splitting a shift between two people** is handled on the actual scheduling page now, not in this tool — this tool only decides pick order.

## Using it week to week

1. **Roster tab** — keep the list of building managers current. Weight, credit, and last week's position show automatically. Adding, removing, importing, and resetting weighting all require a signed-in lead.
2. **This Week's Draft tab** — pick the weekend date (or click "Use upcoming weekend" to auto-fill the coming Fri–Sun), set up this week's special event(s), and adjust the Weekend/Red Gym/SAC slot lists if they've changed.
3. **Draw & Reveal tab** — click **Run the draft** to lock in the full weighted order, then click **Draw!** to reveal picks one at a time with confetti. A calendar-style status board shows which slots are still open, grouped by day and building. There's a **Full screen** toggle here for projecting during the actual meeting (Esc exits it).
4. **Finalize & save to history** once everyone's drafted — this is what updates everyone's weighting.
5. **History tab** — every finalized week's draw order, visible to everyone; click "More info" on a week for the full detail (weights used, setup, individual picks).

## Brownie Points & Shop

Leads can award or dock points (with a reason attached) for things like picking up a shift, a shoutout from another department, or an unexcused absence. Points are redeemable in the Brownie Shop for a handful of perks — some apply automatically (a credit boost, sitting out the next draft, swap insurance), others are just logged for a lead to honor in person (like an actual brownie). The **Points tab is only visible when signed in** — balances and point history stay out of public view on purpose, to keep it from turning into hallway drama. The Shop's catalog is publicly browsable, but redeeming, adding, or removing items requires a lead.

## Signing in as a lead

There's one shared lead account rather than individual logins per person — a deliberate choice to keep this simple, since it's a small trusted group running the actual meeting. It's a local account system built specifically for this app (not tied to email or any third-party login), and the only way in is signing in with that username and password — there's no self-serve signup. The first time it's used, the app forces a "set your own username and password" step before anything else works, so the shared temporary credentials get replaced immediately. Ask whoever's currently holding the login if you need it.

## Public vs. lead view

Anyone with the link can see the roster, weights, this week's setup, the live draft as it's revealed, and past weeks' history — good for transparency. Actually changing anything (running a draft, editing the roster or shift setup, swapping a pick, adjusting or redeeming points, editing the shop) requires being signed in as a lead. That's enforced by the database itself, not just hidden in the page, so it holds even if someone pokes around in the browser's dev tools.

## A note on where data lives

Unlike earlier versions of this tool, there's now a real shared database (Supabase) behind it — the roster, shift setup, live draft state, history, and points all sync across every device in close to real time. The browser's local storage is only used as an offline fallback cache, not the source of truth. The API key embedded in the page is meant to be public (it's the same kind of key Supabase recommends for client-side apps); what actually protects the data is server-side access control, not keeping that key secret.

## Updating the tool

Edit `index.html` directly in GitHub (pencil icon) or upload a replacement, then commit. GitHub Pages redeploys automatically within a minute or so. Database schema changes (new tables, columns, or rules) happen on the Supabase project directly and aren't part of this repo.
