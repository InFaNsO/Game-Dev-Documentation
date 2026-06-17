# Dream Game — Full System Inventory

> Complete system tree of the four-pillar dream game, ordered most-central → down. Used to carve out the **granular testbed games** (each small game proves one system or a small combination — see the build ladder in `CLAUDE.md`). Living list — refine as we go.

## 1. Combat *(action core — dungeon-crawler pillar; fully designed, see [[Mechanics/Looter Shooter/06 Element in Looter Shooter|LS 06]])*
- Format
  - Tactical (single-screen tile grid, side-based phases)
  - Reactive (souls-like parry / dodge)
- Camera
  - Dual-camera (tilted-3/4 tactical ↔ OTS reaction-cam)
  - Blend / transition system
- Action economy
  - AP pool (parry-fed, no carryover)
  - Separate splittable movement budget
- Weapons (3 lanes)
  - Melee (free, adjacent)
  - Spells (refined Mani, mid)
  - Launcher / ultimate (big Mani, AoE)
- Damage model
  - Deterministic flat damage
  - Facing & flank arcs (Front / Side / Rear→crit)
  - Earned crit (flank / parry / status-combo)
- Reactive layer
  - Parry (Block / Perfect) + counter
  - Dodge
  - Ranged deflect
- Status effects
  - Burn / Freeze / Stagger / Push
  - Combos (Freeze→Shatter)
- Enemy
  - AI (goal-based, pack, anti-flank, focus-fire)
  - Intent telegraphs
  - Weapons
    - Ranged
    - Melee
  - Roster (per-biome)
- Positioning / hazards
  - Hazard-push
  - Collision damage
  - No cover / no armor / no height
- Encounter Arena
  - Transition in / out
  - Alert / approach carry
  - Loot resolution on return

## 2. Mani System *(the substance — connective tissue: combat + automation + loot)*
- Raw Mani (loot)
  - Small (common → spells)
  - Veins / big ore (rare, biome-elemental → launcher)
- Refining minigame (raw → refined, gem-size scaling)
- Refined Mani (spell / launcher fuel, consumed on cast)
- Elements
  - Bhu / Jal / Vayu / Agni
  - Akash (endgame)
- Raw Effect Table (chaos)

## 3. Automation / Spell-Engineering *(automation pillar — the dream-game signature)*
> **SCOPE NOTE:** spell *mixing* is **expanded to the MAX here** — chaining, deep multi-element combination, the full Infinite-Craft-style depth. The **2-element-compound cap is a Looter-Shooter scope-limit ONLY**, not the dream game's ceiling. This pillar is where the real combinatorial depth lives.
> **DESIGN PASS IN PROGRESS (2026-06-08) — see `Mechanics/Automation/00`–`07`.** Framing = a **spell-gear workshop** (Accord frontier-boom era); **two surfaces** (A spell-design / B production); **Conjuration Engineering**; **14-element matrix** (capped at 3-Mani here; full depth = dream game); **competitor-AI v1** (market-level start of the cross-portfolio AI thread). The earlier "cozy-management" candidate is superseded → **tone = competitive market.**
- **Surface A — Conjuration Engineering** (spell *design* — deep, signature)
  - Gem = 3D voxel-grid factory floor (node count = gem size · purity = power)
  - **Process-built 3D constructs** (Form + Substance + Behavior); form from *shapers* (process-ops), never hand-placed
  - Particle **sim = source of truth** → **VFX Graph** = visual skin
  - **Test Room** (fire-and-watch instant feedback)
- **Surface B — Workshop production** (the "regular automation")
  - Belt/machine line makes the **parts + gear** (**throughput = market output**)
  - Curated part catalog; coupled to A by shared parts (one economy, not two games)
  - *(human-vs-machine = open; rec machine + thin artisan-flavor)*
- Gear & housing (gauntlet/wand/sword/armor; **gem-slots = element-combination tier**)
- Elements / spell-mixing
  - 4 base + 6 pairs + 4 triples = **14** (capped at 3 here; **full depth = dream game**, NOT 2-element-capped — that cap is LS-only)
  - all-4 = **Akash** (forbidden / teased "new Mani")
- Research / **tech tree** (new operations = the Modulus depth-fix; spell library)
- **Market & competitor** (= AI-thread v1; market = shared substrate · competitor = market-level agent, 2–4 rival personalities, behind-the-curtain — see `Automation/07`)

## 4. Loot System *(looter pillar)*
- Item definitions + rarity (Common → Legendary)
- Inventory
  - Grid-based
  - Weight
  - Containers
  - Stacking
- Loot generation / drops (enemies, dungeons, veins)
- Equipment / gear
  - Mani launchers
  - Focuses
  - Mod slots
- Salvage

## 5. City-Building *(city pillar — Anno-style; DESIGN IN PROGRESS 2026-06-07 → see `Mechanics/City Builder/` 03/04/05)*
> **Locked direction:** Anno biome-as-map (Bhu/Jal/Agni/Vayu + Akash space-time tech capstone) · 4 species (incl. new **Avian** — canon ripple) · 3-tier population w/ escalating Mani-element complexity · Mani-as-infrastructure economy (utilities + Mani-tech goods, multi-element goods force trade) · refining-as-building · **NO TD/combat** (reserved for the dream game; tension = pure economic/logistics; environmental-weather threat deferred to post-prototype) · **grid substrate + free decoration** (cross-portfolio) · procgen islands · NO AI colonies.
- Building placement + adjacency bonuses
- Population / colonists
  - Needs
  - Demands
  - Happiness
- Production chains / resource economy
- Multi-town logistics
  - Main City ↔ Satellite
  - Resource transfer routes
- City tech / era progression
- Defenses (tower-defense vs dungeon-breaks)
- Two-city structure
  - Persistent Main City
  - Ephemeral Satellite City

## 6. Dungeon System *(the threat loop — Solo-Leveling portals)*
- Portal / gate spawning
- Dungeon-break clock (the keystone tension — juggling multiple)
- Clear-vs-defend resolution
- Dungeon ranks / difficulty
- Boss encounters
- Rewards
  - Loot
  - Era-advancing otherworldly tech

## 7. Procedural Generation *(powers dungeons; islands for the City-Builder)*
> **2026-06-07:** runs on the **grid substrate** (cross-portfolio lock → cheap discrete-tile procgen). The **City-Builder needs procgen islands** earlier than the old "v4 = dream game" assumption. **⚑ A dedicated cross-game procgen-planning discussion** will assign each version (placement→rooms→dungeons→islands/towns) to its host game and set dev-order.
- Dungeon procgen
  - Room prefab library
  - Layout assembler (grammar / WFC)
  - Encounter director (enemy placement)
  - Loot director
  - Navmesh / runtime bake
- Island / world procgen (maybe — the Triangle's islands)
- Seeding + variety control

## 8. Roguelike Structure
- Run structure (colonize → delve → fall → re-animate)
- Re-animation / death loop
- Persistent vs run split
  - Persist: Main City + tech + knowledge
  - Reset: Satellite city
- Meta-progression
- Difficulty escalation across runs

## 9. Storytelling / Narrative *(roguelike-storytelling pillar)*
- Story delivery (through dungeons + the Triangle mystery)
- Character system (the immortal protagonist, NPCs)
- Branching / emergent narrative (RNG + player-choice influenced)
- Lore delivery
  - Audio logs
  - Environmental
  - Dialogue
- Consequence / legacy system

## 10. Extraction / Raid Loop
- Raid structure (deploy → delve → extract)
- Extraction tension (ship loot home before the island falls)
- Death-drop / insurance / loss-on-fail

## 11. Economy / Trading
- Vendor / shop (buy / sell Mani + goods)
- Pricing / supply-demand
- Faction reputation

## 12. Meta-Progression & Save
- Save / load system
- Skill tree / permanent unlocks
- Blueprint / recipe unlocks
- Era / tech progression tracking

## 13. Foundational Tech *(engine architecture — see [[Games/1a Last Rite - Code Architecture|Code Architecture]], the concrete Tier-0 foundation)*
- Game state machine
- Service locator + typed event bus
- ScriptableObject data layer
- Camera systems
- Input system
- UI / UX framework

## 14. Art & Audio Systems
- One-rig art discipline (shared humanoid rig; **now 4 species** incl. the new **Avian** humanoid bird-folk — canon ripple from the City-Builder)
- Mani VFX / particle systems (= also the automation output)
- Shaders (corruption / elemental)
- Audio (SFX, music, audio logs)
