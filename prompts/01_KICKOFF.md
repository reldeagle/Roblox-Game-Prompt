# Prompt 1 — Kickoff (first session only)

Paste everything between the lines as your first message to the builder, after placing
`CLAUDE.md` at the repo root and `SPEC.md` + `QUALITY_GATES.md` in `docs/`. Fill in the
`{{…}}` fields first; delete any line that doesn't apply.

---

<context>
We are starting a new Roblox game: RIFTBOUND: MULTIVERSE DEFENSE. Your standing
instructions are in CLAUDE.md. The full game specification is in docs/SPEC.md and the
acceptance criteria are in docs/QUALITY_GATES.md. Read all three completely before doing
anything else — they are the source of truth and this message only tells you how to start.

My environment:
- Operating system the builder runs on: {{e.g. Windows 11 with Roblox Studio installed / Linux container without Studio}}
- Roblox Studio access for you: {{e.g. "Roblox Studio MCP server is connected" / "none — I will run Studio checks myself"}}
- Open Cloud API key for Luau execution / place publishing: {{available as env var ROBLOX_API_KEY / not available}}
- Web access to check current Roblox documentation: {{yes / no}}
- Art and audio: {{e.g. "no artist yet — build procedural/placeholder assets and keep ASSETS_NEEDED.md current" / "artist delivers models in assets/incoming/"}}
</context>

<task>
This session is Milestone 0 (Foundation) from SPEC ROADMAP. Its job is to make every later
session fast and safe: a working toolchain, an architecture that matches CLAUDE.md, data
schemas for all content, the design system, the test harness, and the tracking files.
Do not build gameplay features in this session beyond what is needed to prove the harness
works (one trivial module with tests is enough).

Deliverables:
1. Rojo project, Wally dependencies, StyLua, Selene and Lune configured; strict Luau.
2. `scripts/check.sh` (and a `.ps1` twin if I'm on Windows) that formats-checks, lints and
   runs all tests, and exits non-zero on any failure.
3. Folder architecture separating client presentation, server combat, data, summon and
   inventory, monetization, matchmaking, UI and shared config, with a short
   `docs/ARCHITECTURE.md` explaining module boundaries and remote contracts.
4. Typed config schemas and a validation test for units, enemies, waves, maps, bosses,
   fusions, artifacts, banners, mutations and products, populated with the SPEC launch
   data (stats may be first-pass values marked as tunable).
5. `src/shared/UI/Theme.lua` with the ART-01/ART-02 tokens plus typography, spacing, radii,
   stroke and button-state tokens, and `docs/DESIGN_SYSTEM.md` describing them with usage
   rules and a component list (button, card, panel, unit card, rarity frame, tooltip,
   modal, toast, progress bar, currency chip).
6. Tracking files from CLAUDE.md: PROGRESS.md, docs/requirements.json (every SPEC ID),
   docs/DECISIONS.md, docs/ASSETS_NEEDED.md, docs/HUMAN_VERIFICATION.md.
7. A short `docs/VERIFICATION_PLAN.md` stating, for this environment, which QUALITY_GATES
   checks you can run yourself, which need Studio or a device, and how each will be
   evidenced.
</task>

<approach>
Before writing files, think through the architecture end to end — especially how the
server combat simulation will represent 80+ enemies cheaply, how remotes are validated,
how the data layer makes rewards and receipts idempotent, and how pure-logic modules stay
testable outside Studio. Record the key choices in DECISIONS.md with reasons. Choices made
now are expensive to change later, so take the time here; later sessions should be able to
move quickly on top of them.
</approach>

<done_when>
- `scripts/check.sh` passes from a clean clone (paste the final output in your report).
- The config validation test passes against the full SPEC launch data.
- requirements.json contains every ID in SPEC.md (write a test that checks this).
- Everything is committed.
</done_when>

<report>
End the session with a short report: what was built; the check output; the decisions most
worth my review; anything you need from me (credentials, tools, assets) to proceed with M1;
and what M1 will start with. Keep it plain and scannable.
</report>

---
