# RIFTBOUND: MULTIVERSE DEFENSE — Builder Prompt Pack

Your original game idea, reorganized into a set of prompts and documents that an AI coding agent
(Claude Opus 5.5 in Claude Code) can build from over many sessions.

## Why this isn't one big prompt

Your idea describes a full commercial game. No model builds that in one turn, and a single
3,000-word prompt mixes three different kinds of content that work better apart:

| Content | Where it goes | Why |
|---|---|---|
| Rules that apply every session (authority, honesty, code quality, art direction, IP) | `builder/CLAUDE.md` | Claude Code loads it automatically each session, so the rules never get lost when the context is compacted |
| What to build | `builder/SPEC.md` | A structured spec with requirement IDs (`CMB-05`, `FUS-03`…) makes progress traceable and testable |
| How "done" is judged | `builder/QUALITY_GATES.md` | Each gate says who checks it (tests, the agent, or a human) and what counts as evidence |
| What to do now | `prompts/` | Short session prompts: kickoff, one per milestone, and audits |

## What changed from your original text, and why

These follow Anthropic's prompting guidance for Opus 5.5 and its eval guidance:

1. **Turned descriptions into decisions.** Where the original said "e.g." or "such as", the
   spec makes a choice: an 18-unit original roster, 10 named fusions plus 3 secret ones
   (partners and effects included), 3 maps, 3 bosses with phase mechanics, and 6 mutations.
   An agent given vague targets either invents them inconsistently or asks. Change any of
   them, but change them in SPEC.md so everything stays consistent.
2. **Gave the rules their reasons.** Claude generalizes better from "validate on the server
   *because* clients can be exploited" than from bare commands.
3. **Separated what an agent can verify from what it can't.** The agent can unit-test
   combat, summon odds and receipt handling. It can't hold a phone to measure FPS or be
   three independent art reviewers. The prompts make it run what it can, write
   step-by-step human checks for the rest (`docs/HUMAN_VERIFICATION.md`), and never
   report a result it didn't measure. Your original "don't claim a test passed" rule is
   now enforced through evidence links.
4. **Made the gates measurable.** "Summon odds work" becomes "≥100,000 seeded rolls,
   each rarity within ±0.3 percentage points". "Receipts are safe" becomes specific
   cases: duplicate receipt, failed save → `NotProcessedYet`, and a canceled prompt.
5. **Vertical slice first** (`SPEC.md` → ROADMAP, M0–M8): one map, six units, a full
   12-wave match and a boss, then fusion, progression, presentation, content,
   multiplayer, monetization and hardening. This follows your "fewer, excellent systems"
   rule.
6. **State tracking across sessions.** `PROGRESS.md`, `requirements.json` and git history
   let each new session pick up where the last one stopped. Anthropic recommends this over
   relying on one very long conversation.
7. **A compliance issue the original didn't address.** If Rift Crystals could ever be
   bought with Robux, even indirectly, every character summon would count as a paid random
   item under Roblox rules. The spec forbids any Robux path to Crystals. It also puts the
   Mystery Capsule behind a flag that stays off until its PolicyService checks pass.
8. **Named the UI looks to avoid.** Opus 5.5 handles a specific "don't do X, Y, Z" list
   better than "make it not look generic". The spec also fixes the palette as hex
   tokens with set meanings.
9. **Turn-ending instructions** for unattended runs, adapted from the Opus 5.5 guide, so
   the builder doesn't stop mid-milestone just to post a summary.
10. **IP safety built in.** Famous characters appear only as archetypes in the roster table,
    and the builder is told never to use their names, catchphrases, costumes or logos.

## How to use it

1. Create an empty game repository. Copy `builder/CLAUDE.md` to its root and
   `builder/SPEC.md` + `builder/QUALITY_GATES.md` into `docs/`.
2. **Recommended setup:** run Claude Code on a Windows or Mac machine that has Roblox Studio
   and the Roblox Studio MCP server connected. Then the agent can open your place, run tests
   in Studio and take screenshots. A Linux cloud container works too, but there the agent
   can only test pure logic (through Lune). Everything visual becomes a human check.
3. **Effort:** use `high` for M0 (architecture decisions are costly to change later) and for
   the audits. `medium` is Opus 5.5's default and fine for most milestone sessions; move up
   to `high` if quality slips. Set max output tokens high (up to 128k) for long turns.
4. Fill in the `{{…}}` fields in `prompts/01_KICKOFF.md` and send it as the first message.
5. For each milestone, start a **fresh session** with `prompts/02_MILESTONE.md`. Read the
   milestone report before moving on. Check that what it calls "verified" has evidence.
6. After M4 and after M8, run `prompts/03_AUDIT_AND_REVIEW.md` in a separate fresh session:
   3A for the gate audit and 3B for the visual review against your reference board. Run 3C
   for balance after M5.
7. Do the human checks listed in `docs/HUMAN_VERIFICATION.md`: devices, real purchases and
   the three-reviewer polish score.

## Things only you can provide

- **A visual reference board.** Your idea mentions one. Collect 15–30 images covering
  characters, UI, VFX and maps and put them in `docs/reference/`. Review 3B compares the
  game against them.
- **Art assets.** Code can produce procedural and placeholder art, and good VFX and UI. A
  cohesive, premium roster of 3D characters with animations needs a 3D artist or animator.
  The builder keeps `docs/ASSETS_NEEDED.md` as a brief for that person.
- **Roblox Creator Dashboard setup:** the experience, gamepasses, developer products,
  subscription and prices. The builder documents the exact steps.
- **Policy confirmation.** Check Roblox's current rules on paid random items and
  subscriptions on the day you launch them.

## Files

```
builder/CLAUDE.md           standing instructions (→ game repo root)
builder/SPEC.md             game specification with requirement IDs (→ docs/)
builder/QUALITY_GATES.md    acceptance gates and evidence rules (→ docs/)
prompts/01_KICKOFF.md       first session: Milestone 0 foundation
prompts/02_MILESTONE.md     each later milestone, plus resume/nudge snippets
prompts/03_AUDIT_AND_REVIEW.md  independent gate audit, visual review, balance review
```
