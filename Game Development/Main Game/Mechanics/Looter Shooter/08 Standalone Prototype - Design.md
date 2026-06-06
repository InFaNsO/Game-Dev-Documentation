
# Standalone Prototype — Design Brief

> Slice of the [[00 Looter Shooter|Looter Shooter]] pillar, shipped as its **own polished game** before integration into the [[01 Concept & Idea|main game]].
>
> **Combat inspirations (locked 2026-06-02 — see [[06 Element in Looter Shooter|06]] "Combat" sections):** **South Park: The Fractured But Whole** (compact single-screen tile **grid**, perspective 3/4) + **Into the Breach** (telegraphed enemy intent + collision/hazard push) + **Expedition 33** (reactive parry/dodge on the enemy turn, the dual-camera parry moment) + **Valkyria Chronicles** (tactical ↔ OTS camera blend). *Sparks of Hope (free-form dome), XCOM cover %, height/verticality, firearms, and procgen arenas were all **cut** in the combat redesign.*
>
> **Setting:** The outskirts of [[05 Mega Structures - Game Worlds#Lithic Mow|Lithic Mow]] in the post-[[12 The Akashic and The Bleed|Breach]] world, alongside [[07 Civilization - The Grinders|the Grinders]] — the player's first allies in the journey to rebuild what the [[14 Naming Glossary#The Federation|Mani Accord]] left behind.
>
> **Strategic rationale:** the prototype proves the **combat + loot + story** core that carries directly into the main game (combat controller, Mani economy, faction/trust framework). Procgen is **not** in the prototype — it moves to the dream game's dungeon system later. Lore is the same lore — the prototype is canonically the player's first hours after waking from cryo.

> Lore reference stack: [[14 Naming Glossary]] · [[12 The Akashic and The Bleed]] · [[11 Factions and Species]] · [[13 Campaign Structure]] · [[15 Grinder Trust Arc]]

---

## 1. One-line Pitch

A stealth-extraction looter set in the post-Breach valley of Lithic Mow — earn the trust of the [[07 Civilization - The Grinders|Grinders]], teach them refinement, read enemy intents, **parry on the reaction-cam**, flank on a tile grid, and collect & refine raw [[06 Element in Looter Shooter|Mani]] into spells in self-contained tactical arenas.

## 2. Design Goals (Prototype Scope)

1. **Prove the combat model** — the grid-tactical + dual-camera + reactive-parry loop (South Park grid × Into the Breach intent × Expedition 33 parry), readable, skill-expressive, never RNG-frustrating. **The #1 thing to gray-box: the dynamic-camera reactive loop** (plan in tilted-3/4 → enemy attack → OTS reaction-cam → parry → snap back).
2. **Prove the loot + crafting loop** — since there are no guns/armor, **Mani is the loot**: collect raw Mani → the refining minigame → refined Mani → cast/research. *(Replaces the old "prove procgen room tech" goal — procgen is cut from the prototype.)*
3. **Prove the Mani loop** — chaotic raw-Mani launcher shots create memorable moments without feeling unfair; refined Bhu-Mani spells (via the refining/research minigames) become the carrot pulling players forward.
4. **Prove the social loop** — the [[15 Grinder Trust Arc|Grinder trust arc]] earned through demonstration, not handed over via quests.
5. **Prove the loot tension** — going in light vs. heavy, deciding when to extract.
6. **Prove the meta loop** — base → raid → return → upgrade → harder raid.
7. **Be shippable in isolation.** Anything that requires the city builder / automation pillars is out of scope.

## 3. Core Loop

```
Base (stash, Grinder vendor [Allied tier+], refine/research minigames, load out)
   ↓ pick raid map + difficulty
Raid load → hand-authored Lithic Mow explore space
   ↓
Exploration (real-time top-down stealth across the authored world)
   ↓ Grinder / scavenger alerted + LOS → transition into a self-contained ARENA
Tactical Combat — turn-based, side-based phases (Encounter Arena model)
   • Compact single-screen TILE GRID (~7×7), no panning, no cover, no height
   • Dual camera: tilted-3/4 tactical ↔ OTS reaction-cam on enemy attacks
   • AP economy (parry-fed, no carryover); melee free / spells + launcher cost Mani
   • Facing & flank arcs = the positional damage lever (Front/Side/Rear→crit)
   • Player Phase (plan/move/attack) → Enemy Phase (each attack = OTS cam + Parry/Dodge)
   • Raw Mani fired AoE via launcher → chaotic Effect Table; hazard-push collisions
   • Deterministic damage; RNG only in Mani Effect Table & status procs
   ↓ win → RETURN to the explore spot; enemies become lootable bodies (raw Mani + materials)
   ↓ lose → drop carried loot, run ends   (death-drop/insurance → task 2)
Extract at evac point  →  back to Base (refine raw Mani, research spells)
```

## 4. Pillars

| Pillar | Player Feeling | Mechanic |
|---|---|---|
| **Extraction tension** | "Do I push one more room?" | Weight, raid timer, evac points, on-death loot drop |
| **Tactical grid combat** | "I read that attack, repositioned to its rear, and parried the counter." | Tile grid, intent telegraphs, facing/flank arcs, dual-camera parry timing |
| **Mani unpredictability** | "Did I just freeze the whole arena?" | Raw-Mani launcher → Effect Table rolls, hazard-push collisions, Mani veins as big-ore deposits |
| **Social earning** | "I'm one of them now." | [[15 Grinder Trust Arc|Trust arc]] — gates everything, earned through three discrete moments |
| **Loot identity** | "This gun is *mine*." | Grid inventory, rarity, mods, condition |
| **Meta progression** | "Next run I'm ready." | Stash, vendor rep, craft tiers, blueprints, skill tree |

## 5. Combat Model Summary

Full design in **[[06 Element in Looter Shooter|06]] "Combat" sections** (the live spec). Headline rules (locked 2026-06-02):

- **Arena:** combat triggers a transition into a **self-contained arena** — a compact single-screen **tile grid** (~7×7), no panning, **no cover, no height/verticality**. Authored, not procgen. On win you return to the explore spot.
- **Camera:** one unified **perspective** rig; **tilted-3/4 tactical** view ↔ **OTS reaction-cam** during enemy attacks; tactical angle = OTS + 15–25° (smooth blend = the #1 identity bet).
- **Turn:** **side-based phases** — Player Phase (plan / move / attack) → Enemy Phase (each attack = OTS reaction-cam + **Parry/Dodge**).
- **Action economy:** **AP pool**, no carryover, **fed by base regen + parry** (parry = the skill engine). **Movement = separate splittable budget.** Three weapon lanes on an inverse cost↔range↔risk curve: **enchanted melee** (free/adjacent) → **refined-Mani spells** (mid, cost AP + refined Mani) → **Mani launcher** (rare, AP + big refined Mani, AoE).
- **Damage:** flat **deterministic** (no variance, no hit%). **Facing & flank arcs** = the positional lever (Front ×1.0 / Side ×1.25 / **Rear→crit**). **NO armor.** Crit earned (rear-flank · parry-counter · status-combo). RNG only in the raw-Mani Effect Table + status procs.
- **Reactive layer:** Dodge (safe, no reward) vs Parry (Block/Perfect). Both parry → immediate counter + flat AP; **Perfect → crit counter + drops raw Mani**; ranged: Block negates, **Perfect deflects**.
- **Intent:** every enemy telegraphs action + target + **exact** predicted damage (Into the Breach); round-locked for the prototype.
- **Raw Mani:** the unrefined weapon → **fired AoE via the launcher → rolls the Effect Table** (chaotic, can self-harm). The old live-timer "grenade" is retired. **Hazard-push** = forced movement + collision damage.
- **Status deck (prototype core):** Burn / Freeze / Stagger / Push — via Bhu enchant + the raw-Mani Effect Table; wired into the combo-crit (Freeze→Shatter).

## 6. Setting — Lithic Mow Outskirts

Single biome: **the outskirts of [[05 Mega Structures - Game Worlds#Lithic Mow|Lithic Mow]]**, controlled by [[07 Civilization - The Grinders|The Grinders]]. The player wakes at the [[14 Naming Glossary#World Locations|Headwaters]] (a forgotten Accord research outpost upriver) and ventures into the valley.

**Sub-themes (authored explore world + a pool of authored arenas):**
- **Mining tunnels** — tight ambush corridors in the explore world; tight arenas
- **Drill machinery areas** — industrial setpieces (single-plane — no verticality)
- **Outdoor camp areas** — open ground, longer engagements
- **Mani vein chambers** — big-ore deposits (launcher Mani); arena hazard ingredients

The explore world is **hand-authored** (full story density); combat uses a **pool of hand-authored, region-themed arenas** (no procgen). Mani veins / cave-ins are arena *ingredients*, not literal explore terrain. Story beats minimal in raid zones; story happens at the Grinder camp via the trust arc.

## 7. The Grinder Trust Arc (the prototype's social spine)

> Full design: [[15 Grinder Trust Arc]]. Summary here for prototype context.

The trust arc is the prototype's main story spine. It runs in parallel with raids and gates major content:

| Phase | Trust tier | Unlocks |
|---|---|---|
| 1. **Save the Scout** | (start) | First contact, camp known but not accessible |
| 2. **Provisional Wary** | Wary | Camp access, basic dialogue, prove-useful mini-quests |
| 3. **First Refinement Ceremony** | Allied | Vendor, crafting, blueprints, Grinder colonist recruitment |
| 4. **Joint Lithic Mow Reclamation** | Tribal Kin | Companions in combat, base co-building, full Grinder gear |
| 5. **Beyond** | (post-game / main-game seed) | Hint of the wider world (downriver glimpse, audio logs hinting at The Bleed and the conspiracy) |

The first Bhu-Mani refinement — taught by the player to the Grinder Shaman at the Headwaters outpost — is the watershed moment that gates raid expansion.

## 8. Lore × Combat Integration

### Mani drives combat unpredictability
The lore's "[[06 Element in Looter Shooter|Mani]] holds chaotic potential" *is* the central novel combat mechanic:
- **Raw Mani fired via the launcher** → AoE on impact → **Effect Table** roll: the chaotic, can-self-harm option (early/Grinder weapon). The old live-timer hand-grenade is retired.
- **Mani Effect Table** rolls deliver Burn / Freeze / Push / Stagger / rarer effects (the "Create cover" roll is removed — no cover; "Anti-grav" → displacement-only)
- **Mani Veins** are big-ore deposits → refine into **big refined Mani** = launcher fuel; in arenas they double as hazard-push targets
- **Refined Bhu-Mani spells** — made via the **refining + research minigames** after teaching the Grinders. The carrot. (Loot = raw Mani; you *make* refined Mani.)

### Grinders are a faction, not a stat block
Their lore (~15–20 hunter-gatherers, mining-proficient, Mani-ignorant) drives:
- **Pack-tribe AI** — coordinated callouts, regrouping, never alone (round-locked intent telegraphs)
- **Mining-aesthetic weapons** — **NO firearms** (a Mani-tech world never built gunpowder): crude **Mani-shard launchers** (chaotic raw-Mani AoE) + the Driller's drill (melee gap-closer)
- **Mani ignorance is a mechanic** — Grinder raw-Mani launcher shots randomly affect everyone including themselves; players can bait self-harm via positioning
- **Archetypes (roster DEFERRED — prototype builds ONE simple enemy first):** candidate doctrine Scout · Driller · Chief · Shaman

### Lithic Mow combat ingredients (arena-side, not literal explore terrain)
- Tunnel-tight arenas, **Mani veins** (big-ore + hazard targets), cave-in hazards, raw-Mani zones — authored arena ingredients, single-plane (no verticality)
- Genesis Vats / Prism Forge environmental setpieces deferred to main game

### Knowledge drives progression
- The player is **the only character who can refine Mani at game start**
- Each Mega Structure reclamation unlocks the next refinement tier
- After the trust arc's first refinement ceremony, **the player teaches the Grinders** — represented mechanically by Grinder colonists operating refinement stations in the background while the player raids
- **Knowledge is the gating power**, not stats or stamina

## 9. Faction Roster — Prototype Scope

- **The Grinders** — sole human faction, all four archetypes (Scout / Driller / Chief / Shaman). Trust arc transforms disposition from semi-hostile to tribal kin.
- **Hostile rival Grinders / scavengers** — splinter group rejecting the player's knowledge; reuse the human rig (no new assets). Wildlife was cut (Topic 9).
- **Rogue Grinder Defector** — single vendor NPC at the player's wake-site / base; potentially folds into the main Grinder vendor once trust arc completes

Amphibian Husks, Reptile Husks, and the purification mechanic are **deferred to the main game** — see [[11 Factions and Species]].

## 10. Scope Cut List (NOT in the prototype)

- City building, automation, party-based combat (solo only)
- Multiplayer / co-op
- Other biomes (Genesis Vats, Prism Forge stay in main game)
- Other species enemies (Amphibian Husks, Reptile Husks — main game)
- Purification mechanic (main game)
- Refined Mani above Bhu (Jal/Vayu/Agni — main game, Akash never in this game)
- Team Jump movement (party-only; ships in main game)
- The endgame Choice (main game)
- Vehicles, mounts

## 11. Prototype → Main Game Carryover

What ships forward into the dream game **with zero rewrite**:

- Combat system (grid × dual-camera × E33 parry) — gains party orchestration on top
- Mani system — gains Jal/Vayu/Agni refinement tiers + 2-element compound spells + purification anchor mechanic
- Faction system — gains Amphibian Husks + Reptile Husks
- Authored arena pool + encounter framework — main game adds procgen *later* (dream-game dungeon system), not in this prototype
- Facing/flank system, AI intent telegraphs, status effect system
- Grid inventory, item definitions, refining/research minigames, crafting recipes
- Trust arc framework — adaptable to purified-NPC ally reputation system in main game
- Save system, event bus, service locator

What changes for main game:

- Solo → 2–3 character party (Team Jump enabled, class differentiation)
- Lithic Mow only → all three Mega Structures + connecting regions
- Stash → per-character + shared stash
- Adds city builder, automation, the endgame Choice + two endings

## 12. Success Criteria

- A 20-minute first session ends with the player saying "one more run."
- Average raid length 8–15 min.
- Combat encounter resolves in 30–90 sec.
- Players describe Mani moments as "memorable" not "unfair."
- The first refinement ceremony lands emotionally — playtesters report it as "the moment the game clicked for me."
- The dual-camera parry loop "feels good" in the gray-box — the validation gate for the whole combat identity.
- Inventory tetris feels *fun*, not chore.
- Players describe combat as "tactical" and "skill-expressive" — never "lucky" or "static."
