# Quality Gates

A gate passes only on evidence. Each gate lists **who can judge it** and **how**, ordered
by preference: automated code-based checks first (fast, repeatable), then agent checks with
recorded artifacts (screenshots, logs), then human checks for what only a person or a real
device can judge.

**Evidence rule:** every "pass" links to something in the repo — a test name and the
`scripts/check.sh` run that included it, a log file, a screenshot path, or a completed row in
`docs/HUMAN_VERIFICATION.md`. A claim without a link is recorded as "unverified".

**On failure:** identify the actual defect (not the symptom), fix it, re-run the failed check
and the full regression suite, and record the defect and fix in `PROGRESS.md`.

---

## Gate report format

At M8 (and whenever asked), produce `docs/GATE_REPORT.md` with one table:

| Gate | Status (PASS / FAIL / PARTIAL / NEEDS_HUMAN) | Evidence | Open defects |
|---|---|---|---|

Then list every known defect with severity (Critical / Major / Minor). **Critical** =
lost or duplicated purchases, corrupted or lost progression, an unreachable mode, an
unplayable match, a persistent crash, or any way to buy Rift Crystals or paid random items
while restricted.

---

## GATE-COMBAT
- **Pass when:** a complete 12-wave match runs from deployment through boss victory, and
  separately through defeat, with attacks, upgrades, every targeting mode, rewards and
  cleanup (no leftover enemies, effects, connections or instances after the match).
- **Automated (agent):** headless simulation tests of full matches with seeded scripted
  placements; unit tests for TEC-06 combat items; an instance/connection leak check after
  match end.
- **Human:** play one full match on desktop and one on mobile; record result in
  HUMAN_VERIFICATION.md.

## GATE-FUSION
- **Pass when:** every listed fusion activates only from its real requirements (owner,
  range, energy, cooldown), applies its defined mechanical effect, plays its unique VFX/SFX
  sequence, and respects cooldown and the Full/Short/Off setting.
- **Automated (agent):** per-fusion tests: negative cases (out of range, partner missing,
  energy low, on cooldown, different owners) and positive case asserting the effect on
  simulated enemies; config check that each fusion references a unique VFX id, SFX id and
  camera preset.
- **Human:** watch each fusion once; confirm it is distinct and readable.

## GATE-PROGRESSION
- **Pass when:** unlocks, evolution requirements, duplicate conversion, rewards, pity and
  levels survive rejoin and match server records.
- **Automated (agent):** save → load round-trip tests; migration tests from every older
  schema version; simulated DataStore failures (throttle, error, timeout) prove no data
  loss and no stale overwrite; duplicate reward request and reconnect tests prove exactly-
  once grants.
- **Human:** in a published test place, earn rewards, leave, rejoin on another device, and
  compare.

## GATE-SUMMON (part of progression)
- **Automated:** ≥100 000 seeded rolls per banner; each rarity's observed rate within
  ±0.3 percentage points of the configured rate (excluding pity-forced pulls, tested
  separately); pity guarantees Mythic+ on pull 100 and resets correctly; displayed odds are
  generated from the same config the server rolls from.

## GATE-MONETIZATION
- **Pass when:** every product grants exactly its described entitlement; duplicate receipts
  don't duplicate grants; canceled/failed purchases grant nothing; receipt recovery after a
  server crash works; PolicyService restrictions are enforced; no path converts Robux into
  Rift Crystals or summons.
- **Automated (agent):** ProcessReceipt tests with mocked MarketplaceService/DataStore:
  first receipt, duplicate receipt, save failure → `NotProcessedYet`, retry → single grant,
  unknown product → no grant; gamepass entitlement tests; restricted-player tests for the
  Capsule (hidden in UI data and rejected by server); static audit listing every product and
  what it grants, checked against SPEC MON tables.
- **Human:** real test purchases in a private test place (Studio test purchases and a
  live private server), including a canceled prompt; confirm product configuration in
  Creator Dashboard; confirm current Roblox policy for paid random items and subscriptions
  on the date of release.

## GATE-PRESENTATION
- **Pass when:** Lobby, Battle, Summon, Inventory, Shop and Unit Progression share
  typography, spacing, color semantics, surfaces, iconography and interaction behavior,
  and each was inspected at 1920×1080 and 390×844.
- **Automated (agent):** lint rule or test that fails on raw `Color3`/font literals outside
  the Theme module; layout tests asserting essential HUD frames stay inside the viewport
  and touch targets are ≥44 px at 390×844 (where the test runner supports GUI layout).
- **Agent with artifacts:** if a renderer is available (Studio via MCP or Open Cloud
  screenshots), capture every screen at both sizes into `docs/screens/` and self-review
  against DESIGN_SYSTEM.md. Otherwise write precise human steps.
- **Human:** visual inspection at both sizes.

## GATE-USABILITY
- **Pass when:** every essential feature (tutorial, team select, all modes, placement,
  upgrade, sell, targeting, fusion, summon, level, evolve, artifacts, shop, settings,
  party join) can be completed without developer commands on desktop and on touch.
- **Human:** checklist run on desktop and a phone (or Studio device emulator as a
  minimum, noted as such).

## GATE-PERFORMANCE
- **Pass when:** the PRF-04 stress scenario meets PRF-01 FPS targets and PRF-03 stability.
- **Agent:** builds the scenario and overlay; runs any server-side benchmarks possible
  (simulation step time for 80 enemies + 18 units must stay well under one frame budget);
  documents results.
- **Human:** runs the scenario on the target desktop and mobile devices for 10 minutes
  and records FPS min/avg/p5, memory start/end, and errors.

## GATE-CONTENT
- **Pass when:** everything in CNT-05 exists and functions as specified.
- **Automated:** config validation test that enumerates units, maps, bosses, fusions,
  evolutions, mutations and artifacts against the SPEC counts and checks every reference
  (asset ids, partner ids, wave enemy ids) resolves.

## GATE-VISUAL-POLISH (human only)
- Three independent reviewers score: scene composition, character consistency, interface
  readability, animation clarity, VFX clarity, environmental quality — each 1–5.
- **Pass when:** each category averages ≥4.0 and no reviewer scores any category below 3.
- The agent prepares `docs/REVIEW_KIT.md`: what to play, what to look at, the scoring
  sheet, and the reference board. The agent never fills in scores.

## GATE-FINAL-ACCEPTANCE
- All gates above PASS (or the owner has explicitly accepted a NEEDS_HUMAN item as done).
- Zero known Critical defects.
