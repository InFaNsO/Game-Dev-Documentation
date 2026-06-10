# Testbed → Dream-Game Build Plan

> How we build up to the four-pillar dream game: a **shared foundation** every game needs, then **small per-pillar testbed games** (each a complete, *shipped* product), integrated in stages up to the full fusion.
> System numbers reference [[06 List Of Game Mechanics]]. High-level ladder summary in `CLAUDE.md`.
> **Principles:** ship everything (no throwaway prototypes) · tech carries forward additively (no rewrites) · each game proves the riskiest unproven piece with the least scope.

---

## Three kinds of systems
1. **Pillar systems** → become their own small game(s): Combat · Looter · City · Automation.
2. **Cross-cutting threads** → NOT standalone games; they **grow as versioned threads woven through other games**, leveling up whenever a host game needs more: **Procgen** (v1→v4, below), plus the **Tier-0 foundation** (save / tech / art all grow this way).
3. **Integration games** → where pillars merge: the Looter Shooter · the Dream Game.

---

## Tier 0 — Shared Foundation *(a cross-cutting thread)*
*Built once (in the first game), carried forward into EVERY game. Additive, no rewrites — it grows as games demand more.*
- **13 Foundational Tech** — game state machine · service locator + typed event bus · ScriptableObject data layer · camera · input · UI/UX framework
- **12 Save system** — the save/load skeleton (meta-progression *content* scales per game)
- **14 Art & Audio** — one-rig discipline (**now 4 species** incl. the new **Avian** — canon ripple from the City-Builder) · Mani VFX / particle systems · shaders · audio pipeline
- **GRID substrate (CROSS-PORTFOLIO lock, 2026-06-07)** — ONE grid-based placement / pathfinding / tile system is the **universal substrate across ALL games** (city · combat · automation · dungeon · procgen); **decorations are free-build** (the cozy layer). Researched decision (free-build breaks logistics/automation; Townscaper proves cozy ≠ gridless). Combat is already grid → consistent.

## Procgen Thread — System 7 *(a cross-cutting thread, grown across host games)*
*Not its own game. Scope it for the dream game (procedural dungeons + island/town generation), then build it up in versions, each developed inside whichever game first needs that level:*
- **v1 — Procedural placement** (scatter loot / enemies / resources within an authored space) → host: an early **combat / looter** game
- **v2 — Simple rooms** (generate a single room layout) → host: a **combat / dungeon** game
- **v3 — Complete dungeons** (stitch rooms + encounter/loot directors into full dungeons) → host: the **Dream Game** (no separate dungeon-integration game — see below)
- **v4 — Entire towns / islands** (procedural settlements + the Triangle's islands) → host: the **City-builder** and/or the **Dream Game**
> Host-game links are tentative — the rule is "build the next procgen version inside the game that first needs it."
> **⚑ FLAGGED (2026-06-07):** a dedicated **cross-game procgen-planning discussion** will assign each version to its host game and **set dev-order across all games.** Note: the **City-Builder uses procgen islands** (a v4-flavored need surfacing earlier than the table assumes) — exactly the kind of host-reassignment that discussion will settle. Also note the **grid substrate** (Tier 0) makes every procgen version cheaper (discrete tiles).

---

## Pillar decompositions → the small games

### A. Combat *(System 1)*
1. **Parry Combat** — proves the **reactive half**: souls-like parry / dodge / counter + a **single smart action-cam** reaction-cam (push-to-OTS on telegraph). The #1 identity mechanic, in isolation. *(The two-mode dual-camera rig is NOT here — it needs a grid to switch away from, so it moves to Tactical Combat. Design-lock 2026-06-10, see [[Games/1 Parry Combat - Last Rite]].)*
   - *uses:* 1 (reactive layer, smart action-cam) + 1 simple enemy + Tier 0
2. **Tactical Combat** — proves the **tactical half**: grid format, movement, facing / flank, AP economy, turn structure, enemy AI + intent telegraphs, **+ the two-mode dual-camera rig (tactical ↔ OTS — moved here from Parry Combat, since the switch only earns its keep with a grid).**
   - *uses:* 1 (tactical, action economy, enemy AI/intent, dual-camera) + Tier 0

### B. Looter *(System 4 + refine/research from 2 & 3 + 11 vendor)*
3. **Looting & Crafting** — proves loot + inventory + the refining minigame + the research minigame + vendor/economy.
   - *uses:* 4 + 2/3 (refining + research minigames) + 11 + Tier 0

### ▶ INTEGRATION — The Looter Shooter *(combine 1 + 2 + 3)*
4. **The Looter Shooter** — fuse parry + tactical combat + the looter, then add: the full Mani spell system (3 lanes, **2-element compounds — LS scope-cap**), Encounter Arena, extraction loop (10), story / trust arc (9), authored world, enemy roster. *First full combat + loot + story product.*
   - This is where combat's two halves first integrate — and **that's exactly what the Looter Shooter IS** (the merge). **No separate "combined combat" game** — it would be redundant.

### C. City-Building *(System 5)* — **SPLIT INTO 3 GAMES (decided 2026-06-07; design in `Mechanics/City Builder/` 00–09)**
The city pillar follows the **combat pattern** (Parry + Tactical → Looter Shooter): **two focused proving-games + a fusion.** Each is a complete, polished, shipped product (no early-access "demo-then-finish-in-public"). **Build order: Logistics → Cozy Town → Fusion** (revised 2026-06-07 — ordered by scope + risk). **DEV SLOTS 7 → 8 → 9** (the whole city pillar now runs *after* automation — see the dev-order note in the ladder).
- **① Mani Logistics Game (the reusable CORE)** — the **Anno route-network with ABSTRACT DEMAND** (ship to satisfy demand nodes; throughput; scale the Vayu-freight web), **stripped of the cozy-town micro but KEEPING demand** so it has purpose (not a spreadsheet). Constraints: built as the **reusable logistics core** the fusion sits on; uses the **SAME mechanic the fusion needs** (Anno routes serving demand), not a divergent abstract puzzle (else it won't transfer). *Built first: it's the **smaller build** (systems-heavy, art-lean), the **design-riskiest** system (de-risk first), and defers the art-heavy game. ⚠ Gray-box the fun cheaply before full production. Reused twice (fusion + dream game's Main↔Satellite).* (Touchstone: Transport Tycoon / OpenTTD × Mani.)
- **② Cozy Mani-Town Builder** — single town, **needs + desirability + planning**, Mani-as-infrastructure, household granularity. *Perfects the cozy half.* ⚠ Must be "the cozy gem," not "the fusion minus features" (it overlaps ③). (Touchstone: Town-to-City × Mani.)
- **③ The City Builder (fusion)** — **the full Phases 0–6 design**: cozy towns + multi-biome network + era/tech staircase + Akash Synthesis. Reuses ① (logistics core) + ② (cozy town) additively.
- **Tower-defense** stays at the Dungeon/Roguelike integration (reverted 2026-06-07 — clashed with the cozy identity; only *actual* TD forward-maps to the dream game's TD). All three city games ship **no combat/TD**; tension = pure economic/logistics. *(Environmental Mani-weather = deferred, post-prototype.)*
- **Other locks:** Anno **biome-as-map** (Bhu/Jal/Agni/Vayu + **Akash space-time capstone**) · **4 species** incl. new **Avian** (canon ripple) · 3-tier population w/ 1→2→3 Mani-element escalation → all-4 Akash endgame · refining-as-building · grid + free decoration · DSP island scale (1 big home town + 1–2 outposts/map).

### D. Automation *(System 3 + Mani 2)* — **SPLIT INTO 2 GAMES (decided 2026-06-10; design in `Mechanics/Automation/` 00–11)**
One risky core (Conjuration Engineering) = one proving game + the fusion. A standalone Surface-B/market game was considered and rejected — factory-without-the-signature fails the identity test ("the fusion minus its soul"); the factory genre is already proven by Factorio/DSP.
- **5 (①) The Conjuration Game** *(small, ~Opus Magnum scale; DEV SLOT 5)* — a Zachlike commission-puzzle game: pure Surface A (spec letter → build the in-gem process → fire in the Test Room → scored on spec-fit · part-cost · cast time · gem efficiency); ~30–50 commissions along the element matrix + sandbox; Akash-tease finale. **Proves the #1 gray-box** + builds the VFX template library, module sim, derived-stat model, and the spec-judge (= the bake-off engine). Mirrors Parry Combat: riskiest identity mechanic, wrapped small and shipped.
   - *uses:* 3 (Surface A only) + 2 + Tier 0
- **▶6 (②) The Spell-Shop (fusion; DEV SLOT 6)** — adds Surface B (warehouse factory + mega assembler), the living market + **competitor AI v1**, the two-currency progression, gear, plot — with **full-depth mixing (UNCAPPED — not the LS's 2-element cap)**. Reuses the Conjuration Game additively.
   - *uses:* 3 + 2 + 11 (economy) + Tier 0

### E. Procgen *(System 7)* — **NOT a standalone game.** It's the cross-cutting **Procgen Thread** (see top: v1 placement → v2 rooms → v3 dungeons → v4 towns/islands), grown inside whichever game first needs each version.

> **NO separate Dungeon/Roguelike integration game (removed 2026-06-10).** It was once slotted as game 7 (Systems 6 + 8 + Procgen v3 + city-defense — the dream game's threat loop). **Cut** because its only real de-risking value was procgen, and procgen is already a versioned thread maturing across the earlier games (v1 placement, v2 rooms, v4 towns/islands) → by the time the dream game needs v3 dungeons the tech is proven. So **System 6 (Dungeon), System 8 (Roguelike), Procgen v3, and the reserved city-defense/TD all build directly in the Dream Game.** Consequence: the deferred dream-game internal decisions (dungeon-break clock, extract-before-fall, clear-vs-defend balance, run granularity) get designed against the Dream Game build itself — which is already where they were deferred to.

### ▶ FINAL — The Dream Game *(combine everything)*
7. **The Dream Game** — combat + looter + city + automation + **the dungeon/roguelike threat loop (Systems 6 + 8) + city-defense/TD + procgen (v3 dungeons + v4 towns/islands)** + the dual-city structure + the immortal protagonist + story (9) + extraction (10). The full four-pillar fusion. *This is where the deferred dream-game internals (dungeon-break clock, extract-before-fall, clear-vs-defend, run granularity) finally get designed against a playable build.*

---

## Woven through (NOT their own testbeds)
- **9 Storytelling** — proven in the Looter Shooter (trust arc) + the dream game (Triangle mystery).
- **10 Extraction** — proven in the Looter Shooter + the dream game.
- **11 Economy / trading** — proven in the looter game + the spell-shop + the integrations.

---

## Build order (the ladder)
**1** Parry Combat → **2** Tactical Combat → **3** Looting & Crafting → **▶ 4 The Looter Shooter** → **5** Conjuration Game → **▶ 6 Spell-Shop (fusion)** → **7** Mani Logistics Game → **8** Cozy Mani-Town Builder → **▶ 9 City Builder (fusion)** → **▶ 10 The Dream Game**
> **DEV ORDER FINALIZED 2026-06-10 (planning step 4): AUTOMATION now precedes CITY** (combat → automation → city → dream). Driver = the cross-game threads: the **competitor-AI thread must be additive** (EC1 market-level in the Spell-Shop → EC2 embodied in the City fusion → EC3 dream faction), so the Spell-Shop must ship *before* the City fusion; and it keeps the **procgen ramp monotonic** (P1 placement → P2 single-screen grids [arenas + gem grids] → P3 islands → P4/P5 dungeons/archipelago). The Conjuration Game lands at slot **5** — the small breather right after the big Looter Shooter (the flagged "portable earlier" slot), front-loading the particle/VFX tech.
> *(Within each pillar the order is unchanged: combat 1→4 · automation 5→6 · city 7→9 — city still Logistics → Cozy Town → Fusion. Procgen woven through: P1/P2 in combat 2–4, P2 reused in automation 5–6, P3 in city 7–9, P4/P5 at the Dream Game.)*
> *Per-game elevator-pitch / setting / motivation concepts live in `Main Game/Games/` (see `00 Game Concepts Hub`). Parry Combat ("Last Rite") is the first locked concept; the rest are in progress.*

> **OPEN (refine when relevant):** exact ship-vs-internal per game. *(The city-vs-automation ordering is now DECIDED — automation first; see the dev-order note above.)*
> **DECIDED:** no "combined combat" game (the LS *is* that merge) · procgen is a cross-cutting thread, not a standalone game · **no separate Dungeon/Roguelike integration game (removed 2026-06-10 — its systems build directly in the Dream Game; procgen de-risks via the earlier games)** · **dev order finalized 2026-06-10 — automation before city (thread-driven; see ladder)** · all three big pillars now scoped + split (combat 2026-06-02 · city 2026-06-07 · automation 2026-06-10, see `Mechanics/Automation/10`–`11`).
