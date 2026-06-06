# Testbed → Dream-Game Build Plan

> How we build up to the four-pillar dream game: a **shared foundation** every game needs, then **small per-pillar testbed games** (each a complete, *shipped* product), integrated in stages up to the full fusion.
> System numbers reference [[06 List Of Game Mechanics]]. High-level ladder summary in `CLAUDE.md`.
> **Principles:** ship everything (no throwaway prototypes) · tech carries forward additively (no rewrites) · each game proves the riskiest unproven piece with the least scope.

---

## Three kinds of systems
1. **Pillar systems** → become their own small game(s): Combat · Looter · City · Automation.
2. **Cross-cutting threads** → NOT standalone games; they **grow as versioned threads woven through other games**, leveling up whenever a host game needs more: **Procgen** (v1→v4, below), plus the **Tier-0 foundation** (save / tech / art all grow this way).
3. **Integration games** → where pillars merge: the Looter Shooter · the Dungeon/Roguelike loop · the Dream Game.

---

## Tier 0 — Shared Foundation *(a cross-cutting thread)*
*Built once (in the first game), carried forward into EVERY game. Additive, no rewrites — it grows as games demand more.*
- **13 Foundational Tech** — game state machine · service locator + typed event bus · ScriptableObject data layer · camera · input · UI/UX framework
- **12 Save system** — the save/load skeleton (meta-progression *content* scales per game)
- **14 Art & Audio** — one-rig discipline · Mani VFX / particle systems · shaders · audio pipeline

## Procgen Thread — System 7 *(a cross-cutting thread, grown across host games)*
*Not its own game. Scope it for the dream game (procedural dungeons + island/town generation), then build it up in versions, each developed inside whichever game first needs that level:*
- **v1 — Procedural placement** (scatter loot / enemies / resources within an authored space) → host: an early **combat / looter** game
- **v2 — Simple rooms** (generate a single room layout) → host: a **combat / dungeon** game
- **v3 — Complete dungeons** (stitch rooms + encounter/loot directors into full dungeons) → host: the **Dungeon/Roguelike integration**
- **v4 — Entire towns / islands** (procedural settlements + the Triangle's islands) → host: the **City-builder** and/or the **Dream Game**
> Host-game links are tentative — the rule is "build the next procgen version inside the game that first needs it."

---

## Pillar decompositions → the small games

### A. Combat *(System 1)*
1. **Parry Combat** — proves the **reactive half**: souls-like parry / dodge / counter + the dual-camera reaction-cam. The #1 identity mechanic, in isolation.
   - *uses:* 1 (reactive layer, camera) + 1 simple enemy + Tier 0
2. **Tactical Combat** — proves the **tactical half**: grid format, movement, facing / flank, AP economy, turn structure, enemy AI + intent telegraphs.
   - *uses:* 1 (tactical, action economy, enemy AI/intent) + Tier 0

### B. Looter *(System 4 + refine/research from 2 & 3 + 11 vendor)*
3. **Looting & Crafting** — proves loot + inventory + the refining minigame + the research minigame + vendor/economy.
   - *uses:* 4 + 2/3 (refining + research minigames) + 11 + Tier 0

### ▶ INTEGRATION — The Looter Shooter *(combine 1 + 2 + 3)*
4. **The Looter Shooter** — fuse parry + tactical combat + the looter, then add: the full Mani spell system (3 lanes, **2-element compounds — LS scope-cap**), Encounter Arena, extraction loop (10), story / trust arc (9), authored world, enemy roster. *First full combat + loot + story product.*
   - This is where combat's two halves first integrate — and **that's exactly what the Looter Shooter IS** (the merge). **No separate "combined combat" game** — it would be redundant.

### C. City-Building *(System 5)*
5. **City-Builder** — proves building + adjacency + population/needs + production chains + multi-town logistics (Anno-lite, Mani world).
   - *(Defenses / tower-defense = the bridge to the dungeon loop; folds in at Integration B.)*

### D. Automation *(System 3 + Mani 2)*
6. **Spell-Shop** — proves the gem-factory (factory-inside-gem, resource-nodes = gem-size) + particle-spell construction + production chains + research, with **full-depth mixing (UNCAPPED — not the LS's 2-element cap)**. The magic-spell shop.
   - *uses:* 3 + 2 + 11 (economy) + Tier 0

### E. Procgen *(System 7)* — **NOT a standalone game.** It's the cross-cutting **Procgen Thread** (see top: v1 placement → v2 rooms → v3 dungeons → v4 towns/islands), grown inside whichever game first needs each version.

### ▶ INTEGRATION — The Dungeon / Roguelike Loop *(Systems 6 + 8 + Procgen v3 + city-defense)*
7. Combine **combat + procgen (v3 dungeons) + city-defense + the dungeon-break clock + clear-vs-defend + re-animation.** The dream game's core threat loop.
   - This is where the **deferred dream-game internal decisions** (the dungeon-break clock, extract-before-fall, clear-vs-defend balance, run granularity) finally get *designed against a playable build* — exactly where they belong.

### ▶ FINAL — The Dream Game *(combine everything)*
8. **The Dream Game** — combat + looter + city + automation + procgen (v4) + dungeon/roguelike + the dual-city structure + the immortal protagonist + story (9) + extraction (10). The full four-pillar fusion.

---

## Woven through (NOT their own testbeds)
- **9 Storytelling** — proven in the Looter Shooter (trust arc) + the dream game (Triangle mystery).
- **10 Extraction** — proven in the Looter Shooter + the dream game.
- **11 Economy / trading** — proven in the looter game + the spell-shop + the integrations.

---

## Build order (the ladder)
**1** Parry Combat → **2** Tactical Combat → **3** Looting & Crafting → **▶ 4 The Looter Shooter** → **5** City-Builder → **6** Spell-Shop → **▶ 7 Dungeon/Roguelike integration** → **▶ 8 The Dream Game**
*(Procgen is a thread woven through these — v1/v2 in the early combat/looter games, v3 at the Dungeon integration, v4 at City/Dream Game.)*

> **OPEN (refine when relevant):** exact ship-vs-internal per game · exact ordering of 5/6 (city / automation are independent and can reorder) · the deep-scope of each remaining pillar (City, Automation) the way Combat was scoped.
> **DECIDED:** no "combined combat" game (the LS *is* that merge) · procgen is a cross-cutting thread, not a standalone game.
