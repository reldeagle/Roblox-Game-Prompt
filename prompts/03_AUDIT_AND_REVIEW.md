# Prompt 3 — Audits and reviews

Use these in a **fresh session** so the reviewer isn't anchored on what the builder
intended. You can also run them as a separate subagent or with a different person driving.

---

## 3A — Gate audit (after M8, or any time you want an honest status)

<task>
You are auditing RIFTBOUND: MULTIVERSE DEFENSE as an independent QA lead. You did not
build it; assume nothing works until you have seen evidence. Read CLAUDE.md, docs/SPEC.md,
docs/QUALITY_GATES.md, docs/requirements.json and docs/HUMAN_VERIFICATION.md.
</task>

<instructions>
1. Run `scripts/check.sh` and keep the output.
2. For every gate in QUALITY_GATES.md, check each pass condition against the code, tests
   and recorded evidence. A requirements.json "verified" status without evidence you can
   find counts as unverified.
3. Actively hunt for Critical defects: ways to gain currency or units from the client,
   double reward or double purchase grants, lost data on failed saves, Robux paths to Rift
   Crystals or summons, paid random items visible to restricted players, modes unreachable
   without dev commands, matches that can soft-lock (enemy stuck, wave never ends, boss
   immune forever).
4. For each defect: severity, reproduction steps or failing test, file:line, and the
   suggested fix. Write a failing test for each one you can.
5. Write docs/GATE_REPORT.md in the format QUALITY_GATES.md specifies.
</instructions>

<rules>
Do not fix anything in this session unless I ask — the goal is an accurate picture. Do not
soften findings. If you could not check something, say so and why.
</rules>

---

## 3B — Visual self-review against the reference board

Attach screenshots of the current build (or have the builder capture them) and your
visual reference board images.

<task>
Compare the attached screenshots of RIFTBOUND: MULTIVERSE DEFENSE against the attached
reference board and docs/DESIGN_SYSTEM.md. Screens: {{list which screenshot is which}}.
</task>

<instructions>
For each screen, score 1–5 and justify briefly: scene composition, character consistency,
interface readability, animation/VFX clarity (from stills: is the attack type identifiable?),
environmental quality, and design-system consistency (palette semantics, typography,
spacing, surfaces, iconography, button states).
Then list the concrete differences from the reference board, ordered by visual impact,
each with the specific change that would close the gap (e.g. "deploy-bar cards use 8 px
radius; design system says 4 px cut corners; unit portraits lack the rarity frame glow").
Point out anything that looks generic or templated, and anything that reads like an
existing franchise's character or logo.
</instructions>

<rules>
Score what is on screen, not what was intended. A 4 means "would not look out of place in
a top-50 Roblox game"; a 5 means "would stand out among them".
</rules>

Then start a builder session with: "Apply the visual review in docs/reviews/{{file}}.md,
highest-impact items first; recapture the screens and re-score afterwards."

---

## 3C — Balance review (after M5)

<task>
Using the config data and the headless match simulator, analyze RIFTBOUND's balance.
</task>

<instructions>
Run seeded simulations of each map on Normal and Hard with: the starter team; a typical
day-3 free-to-play team; a team using each fusion pair; and a best-in-slot team. Report
win rates, match lengths (target 8–12 min), Flux curves, which units are never worth
placing, which dominate every composition, and whether any fusion is mandatory or useless.
Propose config-only changes with expected effects, then re-run to confirm. Remember the
design goal: creative team-building and fusion discovery should beat simply placing the
highest-rarity units.
</instructions>
