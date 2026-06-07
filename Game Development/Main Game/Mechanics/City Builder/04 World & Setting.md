# City Builder — World & Setting

> The fiction for the standalone Anno-lite Mani-world city game (build-ladder Game 5). A **fresh Mani-world locale** — same world as the Looter Shooter & dream game, a different era, **no plot connection** (callbacks only). Theme locked by user 2026-06-07; some specifics flagged PROPOSED pending design.

---

## Core fantasy (LOCKED)

A **Mani-using civilization where Mani is everyday infrastructure** — not just a weapon/spell substance, but the thing that **makes daily life easier, the way electricity did for us.** You administer and grow this civilization's settlements. Cozy magitech with real systemic depth.

**Tone (LOCKED):** cozy-builder *under siege* — a beautiful, optimistic infrastructure builder with a genuine threat layer (see Tower Defense below). Gentle-DSP / Against-the-Storm end of the spectrum, **not** Factorio-brutal, **not** pure zen-cozy.

## Era / frame (PROPOSED — working frame)

A Mani civilization's **colonial / expansion age**: a prosperous heartland chartering frontier colonies into raw, untamed, Mani-rich lands. The player is a colony administrator. Loosely separate from the **Mani Accord** (the LS federation) — ancestral or far-region. A confident, Mani-rich society here can quietly foreshadow later-era hubris (the Breach) without any plot link.

## Society — multi-species (LOCKED, 4 species 2026-06-07)

All four species coexist and thrive (the LS only shows them ruined/Husked; here they're alive — a warm callback). Species is a **mechanic**, not just flavor — each has distinct **needs**, a preferred **biome/element**, and a distinct **economic identity**:

| Species | Element | Preferred biome | Economic identity |
|---|---|---|---|
| **Human** | Bhu (earth) | Highlands (generalist) | Generalist anchor — construction, the Main City |
| **Amphibian** | Jal (water) | Wetlands | Water specialists |
| **Reptile** | Agni (fire) | Volcanic | Heat & high-heat-industry specialists |
| **Avian** | Vayu (air) | Sky-islands / plateau | **Air & logistics specialists** — run the wind-powered shipping/propulsion network |

→ Clean **4-element : 4-species** mapping. Bhu/Human = the generalist start anchor; each other species owns one element and a real economic role.

- **The Main City houses all four species** → it must satisfy all four needs-sets simultaneously (the hardest logistics problem).
- **Satellite / other colonies each focus on ONE species** → settled in that species' biome, specializing in its Mani + goods.
- **Avian = the 4th species (LOCKED "for now," 2026-06-07 — pivot if dev bloats).** Constraint to stay cheap: **humanoid bird-folk** (avian-*humanoid*), **shared one-rig** (+1 mesh + cosmetic wing accessory), **wings cosmetic, NO flight gameplay.** The expensive version (true avian rig + flight pathing) is forbidden.
  - **CANON RIPPLE (flag):** this promotes the world to **4 species** — the **dream game inherits this** (it reuses these species). Propagate to dream-game/LS species docs. Lore hook available: the Avians are a **"lost species,"** gone by the LS era (only 3 remain there) — a tragic callback, not a plot link.
- Reuses the locked **one-rig art discipline** (shared humanoid rig, per-species mesh).

## Map & colony structure (LOCKED 2026-06-07)

**Anno "sessions" model — each biome is its own MAP of multiple islands.** Biome-map set = **Bhu / Jal / Agni / Vayu** (Akash is the tech capstone, *not* a map — see above).

- **3-tier logistics ladder** (the Anno scaling the references love): (1) single island → (2) multi-island within a biome-map → (3) multi-map across biomes (inter-region shipping).
- **Unlocking a new biome-map advances three systems at once:** logistics scope + era progression + a new species/new Mani. One axis, triple payoff.
- **Main City** = your primary island in the **Bhu** starting map (all four species); **satellites** = single-species islands in the other biome-maps. The Main City can't self-supply → imports Jal/Agni/Vayu Mani + species goods → **mutual-dependency logistics web** (Anno engine).
- **Islands = procgen** (LOCKED). *(The procgen sophistication level + how it relates to the other games' procgen needs = deferred to a dedicated **cross-game procgen-planning discussion** — which will also inform dev-ordering across all games.)*
- **NO AI-controlled colonies** — every island/colony is player-managed (see Hard constraints).

## Build approach (LOCKED)

- The **full game features ALL designed biomes** (Bhu/Jal/Agni/Vayu + Akash capstone).
- **Prototyping starts with ONE biome (Bhu)** to gray-box the core loop, then scales up **additively** — same discipline as the combat gray-box. (There is no reduced-biome "v1"; there is the full design + a 1-biome first-playable.)

## Mani as civic infrastructure (PROPOSED — working model)

Each element keeps its **locked elemental nature** (the same nature behind its combat flavor), repurposed for daily life. **Mani is NOT an element-agnostic power grid** — each element does *specific jobs*, so a working city needs a **portfolio** of all of them, and each comes from a different biome. That portfolio-need is the engine of expansion + logistics.

- **Bhu (Earth)** — construction (grows building material from raw earth), soil fertility for farms, terraforming, roads. *The universal/starting material.*
- **Jal (Water)** — running water/taps (no plumbing network), sanitation, irrigation, refrigeration/cold-storage.
- **Agni (Fire)** — heating (stoves/hearths; cold biomes uninhabitable without it), cooking, high-heat industry (kilns/smelting/glass).
- **Vayu (Air)** — ventilation, pneumatic intra-city transport, **propulsion for the barges/ships that do inter-colony logistics**, cranes/lifting.
- **Akash (Ether)** — **the tech-tree capstone (LOCKED 2026-06-07): space-time manipulation, "potentially limitless."** NOT a biome-map you settle — **synthesized, not harvested**, *achieved* by mastering the other four elemental economies (the era-cap gate). Mechanics: **space-folding → instant inter-map transfer / portals between colonies** (the logistics endgame — conquering distance), and **time-bending → production acceleration / stasis (no decay)**. Fits locked LS lore (*Akash is produced at endgame, "learned not mixed," never just found*). **Callback:** Akash space-time portals = interdimensional gates = the Breach-echo — the city game's triumphant "limitless" capstone is the same phenomenon that becomes catastrophe in the LS era. Limitless **but dangerous** = the quiet Breach foreshadow.

## The threat — Tower Defense (LOCKED: included; specifics PROPOSED)

The threat is **wild Mani fighting back**: raw Mani surges that animate into hostile **elementals / Mani-constructs** besieging the colonies. **DSP-style automated tower defense** — you build **refined-Mani-powered turrets/walls** (Agni cannons, Vayu repulsors, Bhu walls…) to hold the line; you do **not** field a tactical squad (that's the combat games / dungeon delves).

- **Systemic loop (PROPOSED):** harvesting/expanding more raw Mani → draws bigger wild-Mani assaults → defend with refined-Mani infrastructure. (DSP's pollution→aggro, made Mani-native.) Defenses are *another* use of Mani-as-infrastructure.
- **Forward-map to the dream game:** wild-Mani besieging a colony → dungeon-beasts breaking out and besieging the island. **Same automated-defense loop, upgraded.** This game develops the dream game's *automated city-defense* system — distinct from the tactical dungeon combat, so it's non-redundant.

## Build model — grid + free decoration (LOCKED 2026-06-07, **CROSS-PORTFOLIO**)

**Grid is the universal substrate; decorations are free-build.** Decided after researching player/designer discourse (free-build is divisive, not an improvement — loved for cozy aesthetics, but actively breaks logistics/automation: gridless pathfinding choke-points, alignment chores, automation communities openly asking for grids).

- **Functional buildings = GRID.** Clean adjacency, logistics, and pathfinding. The grid is the **universal substrate across the whole portfolio** — city + combat (already locked grid) + automation (factory-inside-gem) + dungeons + **procgen** (discrete tiles = cheap procgen). **Solo-dev win:** ONE placement system, ONE pathfinding, ONE procgen tile-system, reused across all five games (pure additive carry-forward).
- **Decorations = FREE-BUILD.** The Town-to-City *wants*/happiness layer places semi-freely *on top of* the functional grid (or on a fine sub-grid). Cozy expression where it matters, grid discipline where the simulation needs it.
- **Cozy feel comes from presentation, not gridlessness** — an **organic/irregular grid look + smart auto-adapting tiles + satisfying placement feedback** (the Townscaper model: Townscaper is itself a grid). You don't trade coziness for logistics.

> **PROPAGATE (cross-portfolio):** this is a portfolio-level lock — the **automation game, dungeon, and procgen thread all inherit "grid substrate + free decoration."** Fold into `07 Testbed Build Plan` / `06 List Of Game Mechanics` / CLAUDE.md at the next checkpoint. Combat is already grid, so this is consistent (no re-open).

## Hard constraints (LOCKED)

- **NO AI-controlled colonies** — friendly or hostile. All colonies are **player-managed directly** (Anno multi-island style, all player-owned). No rival civs, no diplomacy, no AI allies. The only "opponent" is the emergent wild-Mani threat.

---

## Systems in scope (LOCKED list)

1. **Production-chain management** (Mani refining + goods)
2. **Logistics management** (intra- and inter-colony)
3. **Multi city / island colonies** (Main City + single-species satellites)
4. **Tower defense** (automated, vs wild-Mani constructs)
5. **Era progression**

> Depth allocation + per-system scoping → `05 Systems & Scope.md` (in progress).
