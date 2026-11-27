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
| BLD-6 | Socials tab on the Job Stats dashboard | Social Bot (for PJ) | Normal | Done (Nov 18, 2026) |
| BLD-7 | clip-index: tag accuracy + autolog integration | Social Bot / Builder Bot | Normal | In review (Nov 27, 2026) |

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
- Status (Nov 27, 2026): IHM (#47, #48), basketball/volleyball (#49), the unattended nightly `autolog auto` (#50) and `autolog clip-index` (#51) are merged. Nightly runs through Nov 27 have had no failures. Football and XC folders are flagged as unsupported.
- Earlier status (Nov 13, 2026): soccer and basketball initial overlays and training are complete.
- Ice hockey (`IHM`) support and sport-agnostic scene-aware clip descriptions (game action, locker room/team talk, fans, bench, scenic, details) merged in `PJ1229/um-autolog` PR #47 (`42d1268`). Uses Hockey-Vision for players, goalies, and refs, plus filename tags.
- In progress: updating PR #46 (`comments.json` disk saving via `autolog serve-demo`) onto `main`, and wiring the HockeyAI 1280px puck detector on a new PR.
- Next: once those PRs merge, run the full 78 4K hockey clips on Toshiba (`um-athletics/hockey-edit/01_raw_footage`). Toshiba is read-only. Get shooter names for cams 2/4/5/7 and the staff hockey tag list, calibrate jersey colors, and evaluate `roboflow/re-ID` (queued).
- Rules: no secrets, no model weights or media in the repo, no force-pushes.

### BLD-7: clip-index tag accuracy and autolog integration (PJ1229/clip-index)
- Requester: Social Bot (tags it reads for reels) and Builder Bot (um-autolog link). PJ decides when it's done.
- Problem: the Nov 4 index (4,909 clips) over-tagged `face` (63% of clips; 863 of them from a face in under 8% of frames), `overcast` (55%) and `handheld` (71%). AutoLog results weren't in the shared index either.
- Done when: the classifier fixes are merged and the affected folders are rescanned with `--force`, AutoLog results appear in `autolog.*` for every auto batch, and `clip-index query` can filter on them.
- Data rules: scans of the footage drive are read-only, `personal/` is skipped, the index and thumbnails stay on the Mac, and no media goes in the repo.
- Source: `PJ1229/clip-index`, checked out on PJ's Mac at `~/src/clip-index`. The older `~/Documents/clip-index-src` copy is iCloud-evicted and slow to use. The index lives at `~/Documents/clip-index/`.
- Status (Nov 27, 2026): PR #2 (face, lighting and motion fixes; 24-clip read-only A/B: face 15 → 8, overcast 10 → 8, handheld 24 → 23) and PR #3 (`query --sport/--scene/--text` on `autolog.*`) are open for review. um-autolog PR #51 (`autolog clip-index`) is merged, and 40 clips (37 IHM, 3 SOW) now carry `autolog.*` in the live index. Backup: `index.jsonl.bak-20261127`. Incremental size/mtime rescans already work. Query takes about 0.2 s on 4,909 clips.
- Next: after #2 merges, run a `--force` rescan of the athletics and insta360 folders (read-only, off-hours), then consider an `indoor` lighting label for arena footage.

### BLD-6: Socials tab on the Job Stats dashboard (Social Bot, for PJ)
- Requester: Social Bot, on PJ's ask (Nov 18, 2026). PJ decides when it's done.
- Problem: PJ's goal is for total cross-platform views to grow every month, but there was no place to see it.
- Done when: the private Job Stats dashboard has a Socials section, separate from Pipeline, showing monthly views per platform, the cross-platform total, month-over-month change and followers. Blank values show as a dash and are left out of totals and MoM math. New months in the source CSV appear without manual steps from PJ.
- Data rules: aggregate counts only, no credentials. The source stays in the private content-tracker repo and the dashboard stays behind its existing access control (unchanged).
- Repo: PJ1229/career-finance-tracker (private), PR #6.
- Status: built and deployed Nov 18, 2026. The page reads the CSV when the dashboard is refreshed, so new months need a refresh and redeploy after the 1st of each month. Details are in that repo's CLOUDFLARE.md.
