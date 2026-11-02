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
| BLD-2 | Job-application tracker reconciler | Career Bot | High | Queued |
| BLD-3 | Subscription / recurring-charge watcher | Finance Bot | Low | Queued |
| BLD-4 | Course deadlines export | School Bot | Low | Queued |

### BLD-1: Demo recorder (Yazaki Bot)
- Spec and done-when: to come from Yazaki Bot.
- Rules: synthetic data only, no customer data or export-controlled (ITAR) material, PRs only (never merges) on Yazaki Bot's repos, avoid its scheduled weekday dev run, and stay out of its private repo and private files.

### BLD-2: Tracker reconciler (Career Bot)
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

### um-autolog clip timeline (tracked as BLD-1 in PJ1229/um-autolog, PR #40)
- Current direction (Nov 2, 2026): very early stage; soccer only, since most clips are soccer.
- Near-term priority: detection showreel with a person box around every detected player, the jersey number above each box, and a ball marker. No proposed names, no crowd detection.
- Deferred: other sports and crowd/fan detection.
