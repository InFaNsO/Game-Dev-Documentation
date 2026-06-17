
### List of Mechanics to be developed

- Inventory
- Crafting (incl. the **refining** + **research** minigames — the Mani crafting loop)
- Trading
- **Fighting** (grid-tactical + dual-camera + reactive parry — **full spec in [[06 Element in Looter Shooter|06]] "Combat" sections**; locked 2026-06-02. Old "Sparks of Hope dome + XCOM cover + firearms" model is RETIRED; arena procgen was restored 2026-06-10.)
- **Mani System / economy** (no guns/armor → **Mani is the loot**: raw Mani → refine → refined fuel → cast/research — see [[06 Element in Looter Shooter|Mani]])
- **Faction System** (Grinder trust arc + reputation — see [[07 Civilization - The Grinders|The Grinders]] and [[15 Grinder Trust Arc]])

> **NOTE (2026-06-02):** Combat was fully redesigned (Phases 1–4). The authoritative spec now lives in **`06` "Combat — …" sections**. The detailed lists below are being reconciled; where they conflict with `06`, `06` wins. **Procedural Level Assembly is RESTORED in the prototype** (procedurally-generated arenas within the locked single-screen frame for run variety; full level/dungeon procgen → dream-game).

> Cross-references: [[11 Factions and Species]] · [[12 The Akashic and The Bleed]] · [[13 Campaign Structure]] · [[14 Naming Glossary]]

---

### Detailed Mechanics

- [ ] **Inventory**
    - [ ] Grid-based (WxH cell footprints, drag/drop, rotate, stack)
    - [ ] Containers within containers (cases, ammo boxes, med-bags)
    - [ ] Items
        - [ ] Rarity (Common → Legendary)
            - [ ] Passive effects per rarity tier
        - [ ] Consumables
            - [ ] Blueprints
            - [ ] **Raw Mani** (the core loot — un-elemental, refine it into fuel; also = launcher ammo fired raw)
            - [ ] **Refined Mani** (Bhu/Jal/Vayu/Agni — *made* via the refining minigame; spell/launcher fuel)
            - [ ] Healing & status cures
        - [ ] Permanent
            - [ ] **Mani launchers / focuses** (the loot weapon lane — mod slots: focus crystal / chamber / charge coil. NO firearms.)
            - [ ] ~~Armor~~ — **CUT from the prototype** (no armor; mitigation lives in active parry/dodge — revisit post-build)
    - [ ] Quickslots bound to rig pouches
    - [ ] Weight affects exploration move speed

- [ ] **Crafting**
    - [ ] Crafting station at base
    - [ ] **Refining minigame** ("impose structure on chaotic Mani") — raw Mani → **refined Mani**; gated by learned elements; score → yield / quality / efficiency. Big-ore (vein) → big refined (launcher fuel).
    - [ ] **Research minigame** — permanently **unlock/develop spells** (incl. 2-element compounds); score → unlock tier / strength / **Mani refund**. (Spell identity = which element(s) researched; power = score + input quality.)
    - [ ] Minigame success = rarity/strength bump on output (the locked crafting-minigame pillar)
    - [ ] Blueprints unlock permanent recipe access
    - [ ] Salvage station — break items → materials
    - [ ] Refinement tiers expand per Mega Structure reclaimed (Bhu prototype → Jal/Vayu/Agni main game; Akash = endgame, learned from the final boss, never mixed)

- [ ] **Trading**
    - [ ] NPCs at base
        - [ ] Buying & selling at dynamic prices
        - [ ] Reputation tiers unlock inventory
    - [ ] **Vendor unlocks via [[15 Grinder Trust Arc|trust arc]]** — no vendor until Grinders reach Allied tier
    - [ ] High vendor rep unlocks Grinder-faction-exclusive gear (drills, mining charges, raw Mani supply)

- [ ] **Fighting — grid-tactical + dual-camera + reactive parry**

  > **AUTHORITATIVE SPEC = [[06 Element in Looter Shooter|06]] "Combat — …" sections** (Phases 1–4, locked 2026-06-02). The old "SoH free-form dome + XCOM cover + overwatch + firearms + continuous-space" model below is RETIRED. Headline checklist only:

    - [ ] **Mode transition → Encounter Arena:** real-time top-down stealth explore → alert + LOS → **transition into a self-contained, authored, single-screen tile arena** (carry alert/approach advantage) → on win, return to the exact explore spot (loot resolves there).
    - [ ] **Arena:** compact **tile grid (~7×7)**, no panning, **no cover, no height/verticality.** Procedurally generated for run variety (within the locked single-screen frame).
    - [ ] **Dual camera:** tilted-3/4 tactical ↔ OTS reaction-cam on enemy attacks (tactical = OTS + 15–25°). **#1 must-gray-box.**
    - [ ] **Turn structure:** side-based phases (Player Phase → Enemy Phase).
    - [ ] **Action economy:** **AP pool**, no carryover, base regen + **parry-fed**; **movement = separate splittable budget** (launcher roots you). Lanes: **melee** (free) / **spell** (AP + refined Mani) / **launcher** (AP + big refined Mani, AoE, rare).
    - [ ] **Damage:** flat **deterministic** (no variance, no hit%). **Facing & flank arcs** (Front ×1.0 / Side ×1.25 / **Rear→crit**). **NO armor.** Crit earned (rear-flank · parry-counter · status-combo). Body zones cut.
    - [ ] **Reactive layer:** Dodge (safe/no reward) vs Parry (Block/Perfect) — both → counter + flat AP; **Perfect → crit + drops raw Mani**; ranged: Block negates, **Perfect deflects.** No stamina (prototype).
    - [ ] **Intent:** ITB telegraph — action + target + **exact** damage; round-locked (prototype).
    - [ ] **Status deck (prototype core):** Burn / Freeze / Stagger / Push (Bhu enchant + raw-Mani Effect Table); combo-crit (Freeze→Shatter).
    - [ ] **Enemy AI:** pack behavior, anti-flank, focus-fire; parryable-default + some unparryable→must-dodge. **Roster DEFERRED** — build ONE simple enemy to gray-box first.
    - [ ] *(Cut: overwatch, Team Jump, dash-through-chip, body zones, ±damage variance, the Ultimate-gauge — superseded by the AP/parry economy.)*

- [ ] **Enchanted Weapons (refined Mani combat role)**
    - [ ] **Enchanted projectiles** (Mani-charged launcher ammo) — per-shot proc chance of elemental status effect
    - [ ] **Enchanted knives / melee weapons** — per-hit proc + passive elemental aura when wielded
    - [ ] Per-element flavor:
        - [ ] **Bhu** (Earth) — Stagger / Knockback *(was "Armor break" — dropped, no armor)*
        - [ ] **Jal** (Water) — Freeze / Slow
        - [ ] **Vayu** (Air) — Push / Knockdown / Displacement
        - [ ] **Agni** (Fire) — Burn *(was "Melt armor" — dropped, no armor)*
    - [ ] Akash-Mani is NOT used for enchantment — only manifested in endgame production sequence

- [ ] **Named Husks — forced-purification boss hierarchy (Topic 8)**
    - [ ] 6 named Husks total, all mandatory, forced purification (no player kill option)
    - [ ] **The conspiracy was kept even inside the Accord** — Teaching Husks (working researchers) did NOT know; only managers + Head of Research knew
    - [ ] Org-chart hierarchy preserved in corruption (climb the Accord chain of command in reverse):
        - [ ] **Tier 1 — Teaching Husks** (mini-bosses, teach a Mani during the fight at critical state; believe the OFFICIAL LIE = an accident while refining):
            - [ ] Vats Husk 1 (entry hub) → Jal-Mani
            - [ ] Vats Husk 2 (inner sanctum) → Vayu-Mani (no reveal — repeats the lie with unease)
            - [ ] Forge Husk 1 (entry hub) → Agni-Mani
        - [ ] **Tier 2 — Manager Bosses** (knew the truth; give NO Mani; require biome Mani to defeat):
            - [ ] **Vatlord** (Vats manager) — requires Jal + Vayu — **THE MID-GAME REVEAL**: on defeat, attempted purification → confesses reality (doorway experiment) + reveals sealed chamber location → **DIES before teaching the "how"** (the one named Husk who does not survive)
            - [ ] **Sun-King** (Forge manager) — requires Agni — forced-purified → ally; **drops the Prime Focus Lens** (endgame key #2)
        - [ ] **Tier 3 — Final Boss**:
            - [ ] Head of Accord Research (Amphibian) — in the sealed chamber beneath the Vats — requires ALL 4 Manis + the Prime Focus Lens to reach/enter — delivers the **endgame reckoning** + the "how to refine" Vatlord died before giving
    - [ ] **Knowledge transmission happens DURING the teaching-Husk fight at critical state** — Mani recipe unlocked as part of the encounter
    - [ ] **Manager bosses & final boss are capability-gated** — can't progress without the relevant Mani(s)
    - [ ] After purification: Husk reverts, joins base as ally, present at Council Scene — **EXCEPT Vatlord (dies)**
    - [ ] **5 named Husks become allies** at the Council Scene (Vatlord absent — honored by an empty seat)
    - [ ] No named Husks at Lithic Mow (Grinder-only biome; player wakes knowing Bhu-Mani)
    - [ ] **Escalating truth**: the lie (Teaching Husks) → the reveal (Vatlord, dies) → the means (Sun-King's Lens) → the reckoning (Head of Research)

- [ ] **Endgame two-key gate + final-boss flow**
    - [ ] **Key 1 — all four elemental Manis** → *reach* the sealed chamber beneath the Vats (descent unseals)
    - [ ] **Key 2 — Prime Focus Lens (from Sun-King)** → *enter and power* the chamber
    - [ ] Chamber location learned from Vatlord (Act 2); both keys needed before the player can act on it
    - [ ] Return to Vats, descend, fight Head of Research (final boss; requires all 4 Manis)
    - [ ] Forced purification → reckoning + the "how" → return to base
    - [ ] Council Scene → The Choice → (good) Akash Mani production at base

- [ ] **Mani System / economy (the looter engine — no guns/armor → Mani IS the loot)**
    - [ ] **Raw Mani = the loot** — un-elemental, chaotic, persistent. Two grades: small (→ spells) + **Mani Vein / big ore** (rare, biome-elemental → launcher). Three sinks: fire raw (launcher), refine→cast, research material.
    - [ ] **Raw-Mani launcher shot** — fired AoE on impact → rolls the **Mani Effect Table** (chaotic, can self-harm). *(The old "pick up shard → 5–8s live timer → throw" hand-grenade is RETIRED.)*
    - [ ] **Mani Effect Table** — Common: Burn / Freeze / Push / Stagger · Uncommon: Honey / Vamp / Disrupt (was "Ink") / Suppressed · Rare: Phase / Summon Mani-construct / Anti-grav-displacement *(removed "Create cover" — no cover)*
    - [ ] **Refined Mani = spell/launcher fuel — you MAKE it (refining minigame), never find it.** Power scales with crystal size (small → spells, big → launcher). **Consumed on casting** (cost = AP + refined Mani).
        - [ ] **Bhu** — Lithic Mow / prototype tier (player wakes knowing it)
        - [ ] Jal / Vayu — Genesis Vats · Agni — Prism Forge (main game; learned from named Husks)
        - [ ] **Akash — never refinable / never mixed** (learned from the final boss at endgame only)
    - [ ] **Mani Veins** — big-ore deposits → refine to big refined Mani (launcher); double as arena hazard-push targets
    - [ ] **(Ultimate gauge removed — the "power move" is AP→Launcher, fed by parry + vein loot)**

- [ ] **Faction System (lore-driven enemy & vendor design)**
    - [ ] **Faction definition** — AI behavior profile, weapon palette, archetype roster, reputation track
    - [ ] **[[07 Civilization - The Grinders|The Grinders]] — sole human faction (semi-hostile → allied via trust arc)**
        - [ ] [[15 Grinder Trust Arc|Trust arc]] gates camp access, vendor, crafting, companions
        - [ ] Pack-tribe AI — coordinated callouts, regrouping, never alone
        - [ ] Mining-aesthetic weapons — **NO firearms** (Mani-tech world): drills (melee) + crude **raw-Mani launchers**
        - [ ] **Mani-ignorant** — their raw-Mani launcher shots have random effects on everyone including themselves (player can bait self-harm)
        - [ ] Archetypes: Scout (MVP) · Driller (MVP) · Chief (V1) · Shaman (V1) — see [[11 Factions and Species#Grinders (Humans — primary prototype faction)|full roster]]
        - [ ] Boss: Elder Chieftain (main game)
    - [ ] **Rival Grinder Scavengers** — splinter group rejecting the player's knowledge; reuse the human rig (no new assets). Covers the threat role wildlife would have filled (wildlife cut — Topic 9).
    - [ ] **Faction reputation** — kill/spare/trade choices affect future encounters
    - [ ] **Main game additions** (deferred): Amphibian Husks, Reptile Husks, purification mechanic — see [[11 Factions and Species]]

- [ ] **Mega Structure Interiors (Topic 8 locks)**
    - [ ] **8-beat visit shape** (shared across all structures): Approach → Entry Hub → Exploration → Puzzle Gate → Inner Sanctum → Reactivation (multi-phase) → Blueprint pickup → Outro
    - [ ] **Multi-phase reactivation sequence** — 4 phases per structure: locate → restore → defend (boss is here) → activate
    - [ ] **Refinement-knowledge gating** — boss drops blueprint object; cryo-memory flashback on pickup (post-process effect + short VO); companion reaction
    - [ ] **Light environmental puzzles** — 3-5 per structure using the structure's signature mechanic
    - [ ] **Audio logs as primary lore vehicle** — distributed across hub/exploration/inner sanctum
    - [ ] **Per-structure signature mechanics:**
        - [ ] **Lithic Mow** — vertical descent + drill reactivation (Bhu-Mani gear repairs collapsed routes)
        - [ ] **Genesis Vats** — vapor management (toxic Bleed vapor vs healing vapor zones; valve-based flow redirection)
        - [ ] **Prism Forge** — light/laser redirection (aim mirrors/prisms) + heat zones (timed exposure, Agni-Mani gear extends safe time)
    - [ ] **Authored vs procgen split**:
        - [ ] Authored: entry hub, boss arena, puzzle rooms, reactivation phase areas, inner sanctum
        - [ ] Procgen: regular raid / exploration zones (themed per structure)
    - [ ] Scope target: 5-8 hours of main-path content per structure

- [ ] **Procedural Level Assembly** — **PROTOTYPE-SCOPED (2026-06-10).** The explore world is **hand-authored**; combat uses **procedurally-generated arenas for run variety** (single-screen tile grids, Encounter Arena model). Procgen is constrained to *contents* within the locked spatial frame (playable footprint, hazard/Mani-vein placement, spawn zones) — the frame itself (single-screen, no-panning, odd-sized grid, camera rules) is non-negotiable; boss fights may stay bespoke-authored. Full level/dungeon procgen scales to the **dream-game dungeon system** later. What the prototype generates and authors:
    - [ ] Hand-authored explore spaces (Lithic Mow valley — full story density)
    - [ ] **Procedurally-generated arenas**, region-themed (Lithic Mow tileset), single-plane; generated contents = footprint (holes/obstructions), Mani veins (big ore + hazard), cave-in / raw-Mani hazard tiles, spawn/facing anchors
    - [ ] Boss fights may get bespoke arena staging
    - [ ] *(Main-game procgen — Vats/Forge dungeon themes — deferred to the dream game, not this prototype.)*
