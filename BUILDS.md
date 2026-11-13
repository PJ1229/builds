# BUILDS.md: Builder Bot tracker

Builder Bot builds and maintains small tools for PJ's assistant bots. Each request gets a BLD-# ID here.
Builds start **Wed 11/4/2026**, and triage happens in Mon/Wed/Fri sessions.

Each entry has: requester, who decides it's done, the problem, testable done-when, data rules, repo, deadline.
Flow: branch → PR → tests. Low-risk PRs may merge on green; risky ones wait for PJ.
Rules for every build: no secrets, credentials, personal financial data, grades, customer data or media in any repo. No force-pushes.

## Queue

| ID | Tool | Requester | Priority | Status |
|----|------|-----------|----------|--------|
| BLD-1 | Demo recorder | Yazaki Bot | Top | Queued |
| BLD-2 | Job-application tracker reconciler | Career Bot | Low | Queued |
| BLD-3 | Subscription / recurring-charge watcher | Finance Bot | Low | Queued |
| BLD-4 | Course deadlines export | School Bot | Low | Queued |

### BLD-1: Demo recorder (Yazaki Bot)
- Spec and done-when: to come from Yazaki Bot.
- Rules: synthetic data only, no customer data or export-controlled (ITAR) material, PRs only (never merges) on Yazaki Bot's repos, avoid its scheduled weekday dev run, and stay out of its private repo and private files.

### BLD-2: Tracker reconciler (Career Bot)
- Priority: Low while PJ's recruiting is paused (as of Nov 12, 2026).
- Compares the application tracker with the bot's application database, flags duplicates, missing stages and per-company application-cap breaches, then regenerates the stats page.
- Done when all sources agree on one count with no manual fixes.
- Lower-priority add-on: a local checkout so the stats page can be rebuilt on PJ's machine after each batch.

### BLD-3: Recurring-charge watcher (Finance Bot)
- Flags new recurring charges, price increases and duplicates from a transaction export, and writes a summary file.
- Rules: no balances, account numbers or raw transactions in the repo.

### BLD-4: Course deadlines export (School Bot)
- Exports course deadlines and extra-credit items to a machine-readable file other bots can read.
- Done when it matches the course site after School Bot's daily check. Rules: logins stay with School Bot; no grades in the repo.

## Owned projects

| Project | Repo | Notes |
|---------|------|-------|
| um-autolog | PJ1229/um-autolog | Athletics footage logging assistant (local only). Athletics deadlines come first. |
| clip-index | [PJ1229/clip-index](https://github.com/PJ1229/clip-index) | Local, read-only video clip classifier and searchable index. |

When requests conflict, Grok Bot sets the order and athletics deadlines come first.

### BLD-5: um-autolog clip timeline (PJ1229/um-autolog)
- Status (Nov 13, 2026): soccer and basketball initial overlays and training are complete.
- Ice hockey (`IHM`) support and sport-agnostic scene-aware clip descriptions (game action, locker room/team talk, fans, bench, scenic, details) merged in `PJ1229/um-autolog` PR #47 (`42d1268`). Uses Hockey-Vision for players, goalies, and refs, plus filename tags.
- In progress: updating PR #46 (`comments.json` disk saving via `autolog serve-demo`) onto `main`, and wiring the HockeyAI 1280px puck detector on a new PR.
- Next: once those PRs merge, run the full 78 4K hockey clips on Toshiba (`um-athletics/hockey-edit/01_raw_footage`). Toshiba is read-only. Get shooter names for cams 2/4/5/7 and the staff hockey tag list, calibrate jersey colors, and evaluate `roboflow/re-ID` (queued).
- Rules: no secrets, no model weights or media in the repo, no force-pushes.
