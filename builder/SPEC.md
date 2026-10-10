# RIFTBOUND: MULTIVERSE DEFENSE — Game Specification

Version 1.0 · Source of truth for the builder. Every requirement has an ID; cite it in
commits, tests and `docs/requirements.json`. Numbers marked *(tune)* are design targets,
not tested values — implement them as config data so they can be balanced without code
changes.

Contents: VIS · CORE · CMB · MUT · BOS · FUS · PRG · SUM · CNT · ART · UI · SOC · MON ·
TEC · PRF · ROADMAP

---

## VIS — Vision and priorities

The Rift Collapse has fused many fictional realities. Players are **Rift Commanders** who
collect heroes, villains and monsters from those realities, deploy them as towers, evolve
them, and combine them through the **Multiverse Fusion System** to defend their universe.

- **VIS-01 Unique selling point.** Players don't just collect strong characters; they
  discover unexpected combinations, build creative teams and trigger spectacular
  cooperative abilities. When a trade-off appears, protect this first.
- **VIS-02 Feel.** Exciting, expensive-looking, polished and rewarding from the first
  minute, with enough strategic depth and variety for long-term play.
- **VIS-03 Scope rule.** Fewer, finished, excellent systems beat many unfinished ones.
- **VIS-04 Audience.** Roblox players aged ~13–24 who like anime, superheroes, sci-fi,
  collectibles, tower defense and RPG progression; readable for younger players and casual
  friend groups.
- **VIS-05 First ten minutes.** A new player has: played a complete or nearly complete
  battle, deployed and upgraded several characters, seen at least one fusion, received
  meaningful rewards, and can name their next progression goal. Target timeline:

  | Time | Beat |
  |---|---|
  | 0:00–0:30 | Join → short Rift Collapse intro (skippable) → tutorial match starts with 3 starter units |
  | ~3:00 | Tutorial wave 3 cleared → **first free summon** at the Starter Portal (guaranteed **Hiro Kazeyama**) |
  | ~4:00 | Tutorial wave 5 → guided first fusion **F06 Thousand Leaf Tempest** (Hiro + Mira) |
  | ~5:00 | Tutorial match won → **first permanent upgrade** (unit level-up with before/after stats) |
  | 5:00–10:00 | Lobby reveal, first real match on Map 1; HUD shows next goal (e.g. "Collect Vex Arden to unlock fusion F02") |

## CORE — Game loop and modes

- **CORE-01** Loop: Lobby → pick team (≤6 units) → match (8–12 min) → rewards → progress
  (summon / level / evolve / artifacts / cosmetics) → next match.
- **CORE-02** Modes at launch: **Tutorial** (6 waves, solo), **Story** (3 maps × Normal and
  Hard, 12 waves + boss, 1–4 players), **Daily Challenge** (a story map with a rotating
  fixed modifier set and bonus rewards). Events (CORE-04) reuse these modes.
- **CORE-03** Every mode is reachable from the lobby's mode-selection interface without
  developer commands, on desktop and touch.
- **CORE-04** Seasonal events are data-driven bundles (map, units, fusions, boss mechanics,
  cosmetic theme) with real start and end dates shown in the UI.

## CMB — Combat

- **CMB-01** A match has 12 waves; wave 12 is the boss wave. Total length 8–12 minutes for a
  competent team *(tune)*.
- **CMB-02** Enemies follow authored routes (path waypoints with optional branches) from a
  clearly marked spawn to a clearly marked exit/objective. They never stop, leave the path,
  vanish without a death effect, or register a leak incorrectly.
- **CMB-03** The objective (the Rift Anchor) has health; each leaked enemy removes health
  according to its type. Anchor at 0 = defeat. Boss defeated and all enemies cleared =
  victory.
- **CMB-04** Battle currency **Flux** is earned per kill and per wave cleared, and is shared
  per player (each player has their own Flux) in multiplayer.
- **CMB-05 Placement.** The server validates: the unit is owned and in the player's
  equipped team; the position is inside map bounds, on a placeable surface, not on the path
  or within the path buffer, not overlapping another unit's footprint; per-unit and
  per-player placement limits; and sufficient Flux. Placement preview shows valid (cyan)
  and invalid (red) state with a reason before the player commits.
- **CMB-06 Upgrades.** Each unit has upgrade levels 0–4 in a match *(tune)*. Each upgrade
  changes stats and visibly changes the attack (larger/brighter projectile, extra hit,
  new effect at level 4). The unit panel shows current → next stats before purchase.
- **CMB-07 Selling** refunds a configured percentage of total Flux spent *(tune: 60 %)*.
- **CMB-08 Targeting modes** (selectable per unit): **First, Last, Strongest, Weakest,
  Closest**, plus **Fastest** for units that have it. Ties break deterministically
  (furthest along path, then lowest spawn index).
- **CMB-09 Attack types** — every one must exist in the launch roster: single target, area
  of effect, continuous beam, damage over time, crowd control (slow / stun / knockback /
  pull), buff (ally aura), economy (Flux generation).
- **CMB-10 Enemy types** (minimum): Grunt; Runner (fast, low HP); Brute (armored, takes
  reduced damage from weak hits); Warded (regenerating energy shield that must be broken
  first); Stealth (untargetable unless revealed by a detector unit or effect); Splitter
  (splits on death); Mender (heals nearby enemies); Flyer (only units flagged
  `hitsAir` can target it).
- **CMB-11** Every unit attack has anticipation, impact and recovery phases; at least 90 %
  of attacks show a matching hit effect and play a synchronized sound cue.
- **CMB-12** Status effects (slow, stun, burn, mark, shield break) have consistent icons on
  enemies and diminishing returns on hard CC for bosses.

## MUT — Rift Mutations and Overload Waves

- **MUT-01 Rift Mutation** triggers after waves 5 and 10. A 5-second warning banner names
  the mutation and its effect; it lasts for the next 2 waves *(tune)*.
- **MUT-02** Launch mutation pool (at least 6): **Shielded Surge** (all enemies gain a
  Ward), **Fracture Route** (an alternate path opens for some spawns), **Rift Storm**
  (periodic hazard zones that disable units inside for 3 s, telegraphed 1.5 s earlier),
  **Haste Field** (+25 % enemy speed), **Bounty Rift** (+50 % Flux from kills), **Crystal
  Echo** (enemies have a chance to drop bonus end-of-match rewards). Selection per match is
  seeded and shown in advance in the wave indicator.
- **MUT-03 Overload Wave.** Before each non-boss wave the player may vote/opt in to
  Overload: the next wave gains a displayed modifier (e.g. +40 % enemy HP, +1 elite) and a
  displayed bonus (e.g. +30 % wave Flux and a bonus reward chest at match end). In
  multiplayer it requires a majority vote. It never risks paid items, Robux, or anything
  the player already owns.

## BOS — Bosses

- **BOS-01** Each map ends with a boss with an on-screen health bar showing name, phase
  markers and current phase.
- **BOS-02** Each boss has 3 phases triggered at HP thresholds *(tune: 70 %, 35 %)*, each
  with a telegraphed signature mechanic that rewards a strategic change, not just more DPS.
  Phase transitions have a short animation; the boss is not damageable for ≤2 s during it.
- **BOS-03** Launch bosses:
  - **Overseer Kronn** (Map 1): P1 deploys shield drones that make it immune until the
    drones (Flyer-flagged) are destroyed; P2 projects jammer zones that disable units in a
    telegraphed circle (reposition or sell/replace); P3 enrages (+speed) and spawns Runners.
  - **Vaelith, the Starbound Hydra** (Map 2): three heads with separate HP bars; P2 heals
    from nearby minion deaths (prioritize minions or use AoE); P3 the last head flies (needs
    `hitsAir` units).
  - **The Fractured King** (Map 3): P1 swaps which path branch is active every 20 s; P2
    steals one active buff per pulse from units in range (spread buffers out); P3 splits into
    two halves on different routes.

## FUS — Multiverse Fusion System (the headline feature)

- **FUS-01 Activation rules.** A fusion is available when (a) both partner units are
  deployed by the same player, (b) they are within the fusion range *(tune: 14 studs)*,
  (c) both have full **Fusion Energy** (fills from dealing damage/support actions, shown as
  a violet ring on the unit), and (d) the fusion is off cooldown. A violet tether visibly
  links eligible partners; a fusion button appears in the unit panel and as a compact
  prompt above the deploy bar.
- **FUS-02** Activating a fusion consumes both units' energy, plays a 2–4 s cinematic,
  applies its mechanical effect, and starts the fusion cooldown *(tune: 90 s)*.
- **FUS-03 Mechanical effect.** Every fusion changes the battle in a way that is not just a
  damage multiplier (path control, CC, debuff, economy, buff, defense), and its effect is
  described in one line in the Fusion Codex.
- **FUS-04 Presentation.** Each fusion has unique VFX, a unique sound sequence, and a
  unique camera framing. The cinematic never hides the HUD's wave/Anchor info, never pauses
  the server simulation, and can be set to **Full / Short (1 s) / Off** for repeats
  (first-ever activation always plays full unless Reduced Motion is on).
- **FUS-05 Launch fusions (10 public):**

  | ID | Name | Partners | Effect |
  |---|---|---|---|
  | F01 | Starfall Cannon | Kai Emberfist + Seraph Null | Colossal beam down the path: heavy piercing damage, knocks enemies back 10 studs |
  | F02 | Arcane Singularity Gate | Vex Arden + Master Orrin Vale | Tech-portal traps all enemies in a radius for 3 s, then implodes for AoE damage |
  | F03 | Nightfall Protocol | Vanta Corvus + Sable Wren | Blackout: reveals Stealth and marks enemies in range (+25 % damage taken) for 8 s |
  | F04 | Twin Eclipse Duel | Lyra Solenne + Lord Kaelvex | Pulls enemies to a point, then a crossing blade strike; strips Wards |
  | F05 | Thermal Shock | Doc Ignis + Frost Warden Ysolde | Freezes enemies in area 2 s, then shatters them for burst damage that ignores armor |
  | F06 | Thousand Leaf Tempest | Hiro Kazeyama + Mira Tsukino | A cyclone travels backward along the path, pushing enemies back and slowing them |
  | F07 | Rally Line | Commander Bastion + Rook Vance | Units in radius +30 % attack speed for 10 s; Anchor gains a 1-leak shield |
  | F08 | Overclock Foundry | Juno Ferris + Vex Arden | Next wave's Flux income ×2; upgrades for units in radius 25 % cheaper for 15 s |
  | F09 | Infinite Domain | Sora Akatsuki + Master Orrin Vale | Dome for 6 s: enemies inside slowed 80 %, units inside always critically strike |
  | F10 | Orbital Barrage | Pixel Kade + Lyra Solenne | Calls 8 orbital strikes on the 8 strongest enemies on the map |

- **FUS-06 Secret fusions (3)** — hidden in the Codex until discovered:

  | ID | Name | Partners | Effect |
  |---|---|---|---|
  | S01 | Limitbreak Convergence | Kai Emberfist + Sora Akatsuki | Screen-spanning dual blast; all non-boss enemies on screen take massive damage; boss loses 8 % HP |
  | S02 | Dark Accord | Lord Kaelvex + Vanta Corvus | All enemies' speed −50 % and armor removed for 10 s |
  | S03 | Rift Rewrite | The Unbound + any Mythic unit | Rewinds every enemy 25 % of the way back along its path |

- **FUS-07 Fusion Codex (discovery UI).** Lists all fusions: discovered ones show partners,
  effect, and a replay of the cinematic; undiscovered public ones show partner silhouettes
  and a hint; secret ones show only "???" and a cryptic hint after the player owns one
  partner. Discoveries persist on the profile.
- **FUS-08 First discovery** of a secret or Mythic-tier fusion triggers a special reveal
  (title card, Codex unlock animation) and offers a shareable moment (FUS-09, SOC-05).
- **FUS-09 Highlight framing.** After a fusion, an optional 3-second "highlight" replay can
  be captured/shared from the results screen; it is never forced during play.

## PRG — Progression

- **PRG-01 Unit levels** 1–50 *(tune)* using XP from matches and XP items; level-up shows
  before/after stats.
- **PRG-02 Evolution.** Every Epic-or-higher unit, and at least 3 lower-rarity units, has an
  evolved form requiring level 25+, its own unit shards, and Rift Cores. Evolution changes
  appearance (model/colors/aura), attack visuals, and gameplay (new or modified ability) —
  at least two of these noticeable in a match. The evolve screen shows before/after stats
  and plays a short evolution cinematic (first time per unit).
- **PRG-03 Mastery.** Using a unit earns mastery points; mastery ranks unlock a unit title,
  a card border, and one choice of **side-grade** per rank (e.g. +range/−speed), never pure
  stat inflation.
- **PRG-04 Artifacts.** One artifact slot per unit. Artifacts are trade-offs, not strictly
  better multipliers. Launch at least 8, e.g. *Glass Lens* (+25 % range, −15 % attack
  speed), *Overcharged Core* (+30 % damage, fusion energy fills 30 % slower), *Tactician's
  Badge* (+15 % Flux from this unit's kills, −10 % damage), *Anchor Ward* (unit cannot be
  disabled by boss mechanics, −20 % damage).
- **PRG-05 Always a next goal.** The lobby and results screen surface one medium-term
  objective (evolution in reach, missing fusion partner, pity progress, Rift Pass tier).
- **PRG-06 No hostile retention.** No mandatory grinding loops, no login streaks that reset
  punitively, no energy/stamina gating of play, no systems requiring spending to remain
  competitive.
- **PRG-07 Currencies.** *Flux* (in match only); *Credits* (earned, spent on levels and
  upgrades); *Rift Crystals* (earned only, spent on summons); *unit shards* (per unit);
  *Rift Cores* (evolution material); *Rift Pass XP*. **There is no Robux-purchasable
  currency.** Rift Crystals can never be obtained with Robux in any form.

## SUM — Summoning, duplicates, pity

- **SUM-01** Summons cost Rift Crystals (1× or 10×) at themed portals in the Summon
  Chamber. Rift Crystals come from matches, first-clears, achievements, Codex discoveries
  and the free Rift Pass track.
- **SUM-02 Standard banner odds** (must total exactly 100 %): Common 45 %, Rare 30 %, Epic
  17 %, Legendary 6 %, Mythic 1.8 %, Secret 0.2 % *(tune)*. Odds per rarity and per unit
  are displayed in the summon UI.
- **SUM-03 Pity.** A per-banner counter increments each summon and resets when a Mythic or
  Secret is obtained. Summon #100 without one is guaranteed Mythic. The counter is shown
  ("Mythic guaranteed in N").
- **SUM-04** Displayed odds are the true final-outcome odds, including the effect of pity
  (show base odds plus the pity rule in plain language).
- **SUM-05 Duplicates** convert to that unit's shards (Common 5, Rare 10, Epic 20,
  Legendary 40, Mythic 80, Secret 150 *(tune)*), used for evolution and the Shard Exchange.
- **SUM-06 Deterministic path.** Every unit is obtainable without randomness: story
  first-clear rewards and the **Shard Exchange** (universal shards → specific unit). The
  Secret unit is also earnable by completing all 10 public fusions in the Codex.
- **SUM-07 Reveal.** Each rarity has its own color (ART-02) and progressively more elaborate
  reveal (Common quick flash → Secret full-screen glitch-rift sequence). 10× summons can
  skip to results; Mythic/Secret reveals always show once.
- **SUM-08** The summon roll happens on the server using a server-side RNG; the client only
  animates the result it receives. Summon results and pity are saved in the same profile
  write as the Crystal deduction.

## CNT — Launch content

- **CNT-01 Universes** (original): **Kaizen Reach** (shonen anime), **Aegis Metro**
  (superhero city), **Noctis Bay** (dark gothic vigilantes), **Veyl Expanse** (space
  fantasy), **Arcanum Verge** (mystic arts), **The Rift** (rift-born entities).
- **CNT-02 Roster (18 units).** Archetype inspirations are references only; designs, names
  and lore are original.

  | ID | Name | Universe | Inspiration archetype | Rarity | Role / attack type |
  |---|---|---|---|---|---|
  | U01 | Rook Vance | Aegis Metro | street-level vigilante cadet | Common | Single target, fast |
  | U02 | Pixel Kade | Veyl Expanse | blaster scout trooper | Common | Single target, long range, `hitsAir` |
  | U03 | Mira Tsukino | Kaizen Reach | ninja student | Common | Multi-hit shuriken, Fastest targeting |
  | U04 | Juno Ferris | Aegis Metro | salvage engineer | Rare | Economy (Flux per wave) |
  | U05 | Sable Wren | Noctis Bay | shadow detective | Rare | Smoke stun CC, Stealth detector |
  | U06 | Hiro Kazeyama | Kaizen Reach | wind swordsman | Rare | Cone AoE |
  | U07 | Vanta Corvus | Noctis Bay | gadget dark knight | Epic | Single target + mark debuff |
  | U08 | Lyra Solenne | Veyl Expanse | plasma-blade monk | Epic | Melee AoE, deflects projectiles |
  | U09 | Doc Ignis | Aegis Metro | pyro chemist | Epic | Damage over time (burn) |
  | U10 | Frost Warden Ysolde | Arcanum Verge | ice mage | Epic | Slow CC |
  | U11 | Vex Arden | Aegis Metro | armored genius inventor | Legendary | Continuous beam, `hitsAir` |
  | U12 | Seraph Null | The Rift | cosmic guardian | Legendary | Ally buff aura + star projectiles |
  | U13 | Master Orrin Vale | Arcanum Verge | dimensional sorcerer | Legendary | Portal CC (pull/teleport back) |
  | U14 | Kai Emberfist | Kaizen Reach | martial-arts energy fighter | Legendary | Charged AoE blast |
  | U15 | Commander Bastion | Aegis Metro | shield-throwing soldier | Legendary | Ricochet chain hits |
  | U16 | Sora Akatsuki | Kaizen Reach | blindfolded limitless sorcerer | Mythic | Space-distortion AoE + slow |
  | U17 | Lord Kaelvex | Veyl Expanse | dark armored space warlord | Mythic | Telekinetic choke CC + heavy single target |
  | U18 | The Unbound | The Rift | rift-born reality anomaly | Secret | Fusion catalyst; attacks warp enemies backward |

  Starters: U01, U02, U03. Tutorial free summon: U06.
- **CNT-03 Maps (3):** **Aegis Ruins** (shattered futuristic metropolis, single path with
  one branch), **Celestine Sanctum** (floating cosmic temple, path across floating
  platforms, air lane for Flyers), **Rift Nexus** (unstable dimensional battlefield, two
  switching branches). Each has a clearly visible path, readable spawn and exit, placement
  areas that don't fight the scenery, and background depth (skyline, nebulae, rift tears).
- **CNT-04** Each map has 12 authored waves on Normal and a Hard variant.
- **CNT-05** At least 8 artifacts, 18 units, 3 bosses, 10 public + 3 secret fusions,
  evolutions per PRG-02, 6 mutations.

## ART — Visual identity: "Premium Anime Sci-Fi Multiverse"

- **ART-01 Palette (semantic, fixed):**

  | Token | Hex | Meaning |
  |---|---|---|
  | `bg.deep` | #0A0F1F | Deep navy background |
  | `bg.surface` | #141925 | Charcoal panels |
  | `bg.raised` | #1C2333 | Raised panels/cards |
  | `accent.interactive` | #22D3EE | Electric cyan: interactive, sci-fi, valid placement |
  | `accent.rift` | #8B5CF6 | Luminous violet: dimensional energy, fusion |
  | `accent.premium` | #F5B83D | Warm gold: premium, legendary rewards |
  | `accent.danger` | #EF4444 | Red: enemy mechanics, danger, invalid — sparingly |
  | `text.primary` | #EEF2FF | Main text |
  | `text.secondary` | #94A3B8 | Secondary text |

- **ART-02 Rarity colors:** Common #9CA3AF (steel), Rare #3B82F6 (blue), Epic #A855F7
  (purple), Legendary #F5B83D (gold), Mythic #FF3EA5→#22D3EE animated gradient, Secret
  black core with chromatic-glitch white edge. Used identically on cards, reveals,
  nameplates and drop effects.
- **ART-03 UI surfaces:** dark glass panels (`bg.surface` at ~90 % opacity), 1–2 px
  luminous strokes in the semantic accent, angular cut corners (not fully rounded pills),
  one display font for headings and one highly legible font for body/numbers (choose from
  Roblox's built-in families and record the choice in DESIGN_SYSTEM.md).
- **ART-04 Characters:** Roblox-scale stylized proportions, strong readable silhouettes at
  battle zoom, layered clothing, signature weapon/prop, clean materials, consistent
  lighting. All units must look like one cohesive collectible roster (same proportions,
  outline/shading approach, material vocabulary).
- **ART-05 Maps:** gameplay readability first, then atmosphere. Path material contrasts
  clearly with placeable ground; spawn and exit have distinct landmarks.
- **ART-06 Camera:** three-quarter isometric, 35–50° downward pitch, smooth zoom and pan
  (mouse/keyboard and touch pinch/drag), clamped to map bounds.
- **ART-07 VFX language:** cyan = player tech, violet = rift/fusion, gold = rewards, red =
  enemy threats. Each attack type has a distinct shape language (beam = continuous line, AoE
  = ground ring, DoT = lingering particles, CC = icon + tint). Reduced Effects mode cuts
  particle counts ≥60 % while keeping every hit readable.

## UI — Interface and accessibility

- **UI-01 Battle HUD:** compact wave indicator and enemy progress at top center; Anchor HP
  and Flux in fixed positions; mutation/overload indicator next to the wave indicator; boss
  bar appears only during the boss; six-slot deploy bar along the bottom (unit portrait,
  cost, placement count); contextual unit panel when a placed unit is selected (stats,
  upgrade with next-level preview, targeting mode, sell, fusion). No ads, giant promo
  buttons or permanent popups in battle.
- **UI-02 Screens that must share one design system:** Lobby HUD, Battle HUD, Summon,
  Inventory/Unit management, Shop, Unit progression (level/evolve/artifact), Fusion Codex,
  Settings, Results.
- **UI-03** All interactive controls have hover, pressed, disabled and selected states;
  local feedback ≤100 ms; pending state for server-confirmed actions.
- **UI-04 Desktop 1920×1080:** all essential battle info visible without covering the path
  or main placement zones; body text ≥16 px equivalent.
- **UI-05 Mobile 390×844:** complete game loop playable by touch; touch targets ≥44×44
  logical px; no clipped essential UI; safe-area aware.
- **UI-06 Settings:** Reduced Effects, Reduced Motion (disables camera shake, shortens
  cinematics), fusion cinematic Full/Short/Off, music/SFX volume, damage numbers on/off.

## SOC — Social and shareability

- **SOC-01 Lobby hub:** central rotating Rift Portal, Summon Chamber, Unit Management area,
  clearly signposted Mode Select (portal gates), event board showing upcoming events.
- **SOC-02** Party system: join friends, queue together for co-op (1–4 players).
- **SOC-03 Showcase:** each player arranges up to 5 units on a display pedestal in the
  lobby; other players can inspect it (unit, rarity, evolution, equipped cosmetic).
- **SOC-04** Players' equipped cosmetics (nameplate, aura, title) are visible in the lobby.
- **SOC-05 Shareable moments:** fusion discoveries, first evolutions, rare summons and boss
  victories produce a framed moment with an optional screenshot/capture prompt (use
  Roblox's capture APIs where available).

## MON — Monetization

Principles: free to play; no paid exclusive power; transparent value; every product
delivers exactly what it says; no pressure tactics (no fake timers, no "only N left" unless
true, no purchase prompts during combat, no popups on join).

- **MON-01 Storefront** with clearly separated tabs: Passes (permanent), Cosmetics
  (bundles/skins), Rift Pass, Rift Club, and (if eligible and enabled) Capsules.
- **MON-02 Gamepasses** *(test prices)*:
  | ID | Name | Price | Entitlement |
  |---|---|---|---|
  | GP-01 | Extra Team Presets | 299 R$ | +6 saved team preset slots |
  | GP-02 | Premium Cosmetic Effects | 399 R$ | Exclusive attack-trail and summon-portal cosmetic variants |
  | GP-03 | Auto-Retry & Batch Upgrade | 499 R$ | Auto-requeue after a match; level multiple units in one action from the menu |
  | GP-04 | Rift Commander VIP | 799 R$ | VIP nameplate, lobby aura, profile frame, 3 emotes, VIP chat tag; no combat stats |
- **MON-03 Cosmetic bundles** (developer products or limited items) 99–699 R$ by content:
  skins change costume, aura colors, finishing animation, summon appearance, attack
  cosmetics — never hitboxes, range indicators, or readability of attacks. The top tier
  ("Celestial") has high-quality alternate attack visuals and lobby presentation.
- **MON-04 Limited collections** show real start/end dates and honest availability.
- **MON-05 Rift Pass** (season): 30 levels, free and premium tracks, every reward fully
  inspectable before purchase; premium 499 R$ *(test)*. Progress from normal play (a
  typical player completes it in a season with ~1 hour/day or less *(tune)*). Rewards are
  cosmetics, banners, emotes, skins and deterministic items — no Rift Crystals on the
  premium track.
- **MON-06 Rift Club subscription** (Roblox experience subscription; price chosen from the
  current platform tiers): rotating monthly exclusive cosmetic, +3 preset slots, premium
  profile effect, monthly fixed cosmetic bundle. Benefits are granted while active and the
  UI states what lapses on cancellation.
- **MON-07 Developer products** may include fixed-content cosmetic packs and clearly
  labeled time-savers (Credits or XP item bundles with exact quantities). Never Rift
  Crystals, never random contents (except MON-08).
- **MON-08 Mystery Rift Capsule** (cosmetic only) — behind a server feature flag, **off by
  default** until GATE-MON's compliance checks pass and the owner enables it. Outcome tiers:
  Standard 60 %, Enhanced 25 %, Elite 12 %, Celestial 3 % *(tune)*; per-item probabilities
  and a preview of every possible item shown before purchase; duplicates compensated with
  a stated fixed value; every item also purchasable directly at a fixed price. Players for
  whom `ArePaidRandomItemsRestricted` is true never see or can buy it (server-enforced).
  No trading of capsule items. Follow current Roblox paid-random-item policy.
- **MON-09** Every purchase path: server grants via `ProcessReceipt`/gamepass checks only;
  duplicate receipts don't duplicate grants; canceled/failed purchases grant nothing;
  pending UI state until confirmed.

## TEC — Technical requirements

- **TEC-01** Clean modular Luau with separation: client presentation, server combat,
  player data, summon/inventory, monetization, matchmaking, UI (see CLAUDE.md).
- **TEC-02** All economy, combat, ownership, rewards, purchases and progression are
  server-authoritative; every remote validates arguments and rate-limits.
- **TEC-03** Session-locked persistent data with schema versioning; data survives rejoin,
  server shutdown, and simulated save failures (retry with backoff; never overwrite newer
  data with older).
- **TEC-04** Match rewards granted exactly once per player per match, including across
  reconnects, duplicate requests, and server interruption.
- **TEC-05** Multiplayer via reserved servers for matches (TeleportService), with party
  data carried in teleport data and validated server-side.
- **TEC-06 Automated regression suite** covering at minimum: placement (valid + every
  rejection reason), selling, upgrading, every targeting mode, damage application incl.
  armor/wards, wave transitions, mutation timing, overload, boss phase changes, match
  outcomes (win/lose/leak counting), reward distribution and idempotency, summon
  probabilities (statistical test over ≥100 000 seeded rolls within tolerance), pity
  progression and reset, duplicate conversion, save/load and migration, receipt handling
  (grant, duplicate, failure → NotProcessedYet), PolicyService gating, fusion activation
  requirements and cooldown.
- **TEC-07** Debug/dev tooling (stress-test spawner, wave skip, give-unit) exists only for
  allow-listed developer UserIds, is server-checked, and is excluded from player-facing UI.

## PRF — Performance

- **PRF-01** 60 FPS on a representative mid-range desktop; stable ≥30 FPS on selected
  mid-range mobile devices, under a late-wave stress scene: ≥80 visible enemies, 18 deployed
  units, overlapping attack effects.
- **PRF-02** p95 input-to-local-feedback latency <150 ms (excluding network-confirmed
  results).
- **PRF-03** 4-player sustained matches: no recurring script errors, memory that
  stabilizes, no server crashes.
- **PRF-04** A built-in developer stress scenario and performance overlay (FPS, frame
  time, memory, enemy/unit/effect counts) that logs results to the output, so humans can
  measure on real devices.

## ROADMAP — Build order and milestone exit criteria

Build a vertical slice first, then widen. Do not start a milestone until the previous
one's exit criteria are verified or recorded as `needs_human`.

| Milestone | Scope | Exit criteria |
|---|---|---|
| **M0 Foundation** | Rojo/Wally/StyLua/Selene/Lune toolchain; `scripts/check.sh` (format, lint, tests); folder architecture; config schemas for units/enemies/waves/fusions/banners/products; `Theme` tokens + `docs/DESIGN_SYSTEM.md`; tracking files; `requirements.json` listing every SPEC ID | `scripts/check.sh` passes from a clean clone; every SPEC ID present in requirements.json |
| **M1 Combat slice** | Aegis Ruins (graybox OK), path + spawner, placement/sell/upgrade, units U01–U06, 5 enemy types, all targeting modes, 12 waves, Overseer Kronn with 3 phases, win/lose, Battle HUD, server-side rewards | Automated tests for CMB/BOS-03(Kronn) pass; a scripted full 12-wave headless simulation completes with victory and with defeat; HUD screenshots at both resolutions if a renderer is available, otherwise human steps recorded |
| **M2 Fusion** | Fusion energy, range, tether, activation, cooldown, cinematic framework with Full/Short/Off; add units U11, U13, U15 so that F06, F02, F07, F08 can be built and polished first; remaining fusions land in M5 as their units arrive; Fusion Codex | Each implemented fusion has a test proving it activates only from its real requirements, applies its effect, and observes cooldown |
| **M3 Progression & data** | ProfileStore data layer, Credits/Crystals/shards, summon + pity + duplicates, levels, evolution (≥3 units first), artifacts, mastery, tutorial/FTUE (VIS-05), results screen | SUM/PRG/TEC-03/TEC-04 tests pass incl. simulated save failure and duplicate reward request; FTUE timeline walkthrough documented |
| **M4 Presentation** | Lobby hub (SOC-01), Summon Chamber with rarity reveals, all screens on the design system, VFX/SFX pass for every attack type, settings & accessibility, mobile layouts | Screen inventory in DESIGN_SYSTEM.md with screenshots or human-check steps for each of the UI-02 screens at both resolutions; self-review against ART/UI IDs recorded |
| **M5 Content** | Maps 2–3, bosses 2–3, units U07–U18, all evolutions, all fusions, 6 mutations, Overload, Hard mode, Daily Challenge, 8 artifacts | CNT-05 checklist all `implemented` with tests; full 12-wave headless sims on all maps |
| **M6 Multiplayer & social** | Parties, reserved-server matches for 1–4 players, shared waves, per-player Flux, Overload voting, showcase + inspect | Multiplayer logic tests; 4-player session test steps in HUMAN_VERIFICATION.md |
| **M7 Monetization** | Store UI, GP-01..04, cosmetic bundles, Rift Pass, Rift Club, receipt pipeline, PolicyService gating, Capsule behind flag | MON tests incl. duplicate receipt, failed save → NotProcessedYet, restricted player cannot see/buy Capsule; product setup steps documented |
| **M8 Hardening & acceptance** | Stress scenario, perf fixes, failure-injection tests, full regression, audit against QUALITY_GATES.md | Gate report produced per QUALITY_GATES.md; zero known critical defects |
