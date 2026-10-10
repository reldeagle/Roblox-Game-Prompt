# Prompt 2 — Milestone session (use for M1 through M8)

Start a **fresh session** for each milestone (or when a session's context gets long). The
builder re-orients from the repository, which is more reliable than a long compacted
conversation. Replace `{{MILESTONE}}` with e.g. `M1 Combat slice`.

---

<task>
Continue building RIFTBOUND: MULTIVERSE DEFENSE. This session's goal is {{MILESTONE}} from
docs/SPEC.md ROADMAP, through its exit criteria.
</task>

<orientation>
1. Read CLAUDE.md, PROGRESS.md, docs/requirements.json, and `git log --oneline -20`.
2. Read the SPEC sections and QUALITY_GATES entries that this milestone touches.
3. Run `scripts/check.sh`. If anything fails, fix that first — do not build on a red base.
4. Write a short plan into PROGRESS.md: the requirement IDs in scope, the order you'll
   build them, and how each will be verified. Then start.
</orientation>

<how_to_work>
- Build in small vertical steps: implement → write/extend tests → run check.sh → commit
  with the requirement IDs → update requirements.json. Repeat.
- Test logic headlessly wherever possible. For anything visual or device-dependent, either
  capture evidence with the tools you have or add precise steps to
  docs/HUMAN_VERIFICATION.md.
- Follow docs/DESIGN_SYSTEM.md for every UI element and effect; if you need a new
  component, add it to the design system first, then use it.
- Keep docs/ASSETS_NEEDED.md current whenever you reference a model, animation or sound.
- If a SPEC detail is ambiguous, decide, record it in DECISIONS.md, and continue.
</how_to_work>

<self_review>
Before declaring the milestone done, review your own work as a demanding QA lead and art
director would:
- Go through every requirement ID in scope and check it against the code and tests, not
  against your memory of writing them.
- Look for: untested remote handlers, client-trusted values, missing rejection reasons,
  double-grant paths, leaked connections/instances after a match, raw colors or fonts
  outside Theme, controls missing a state, touch targets under 44 px, attacks without a hit
  effect or sound cue.
- Fix what you find, re-run check.sh, and only then update statuses.
</self_review>

<report>
When the exit criteria are met (or blocked only by things that need me), end with:
1. Milestone status per exit criterion: verified (with evidence path), implemented but
   unverified (why), needs human (where the steps are).
2. Final `scripts/check.sh` output summary (test counts, pass/fail).
3. Defects found and fixed this session.
4. Decisions worth my review.
5. What you need from me, if anything, and what the next milestone starts with.
</report>

---

## Continuing an interrupted milestone

If a session ended before the milestone was finished, start the next fresh session with:

```text
Continue {{MILESTONE}} of RIFTBOUND: MULTIVERSE DEFENSE. Read CLAUDE.md, PROGRESS.md,
docs/requirements.json and the last 20 commits, run scripts/check.sh, fix any failure, then
carry on from the plan in PROGRESS.md until the milestone's exit criteria are met. Finish
with the milestone report described in prompts/02_MILESTONE.md.
```

## If the builder stops early

If a turn ends with work still open and no blocker named, reply with:

```text
PROGRESS.md still has open items for this milestone: {{list them}}. Continue with them.
If one is blocked, say exactly what is blocking it.
```
