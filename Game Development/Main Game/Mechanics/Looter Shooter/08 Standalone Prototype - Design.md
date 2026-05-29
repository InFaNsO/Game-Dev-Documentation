
# Standalone Prototype — Design Brief

> Slice of the [[00 Looter Shooter|Looter Shooter]] pillar, shipped as its **own polished game** before integration into the [[01 Concept & Idea|main game]].
>
> **Combat inspirations:** **Mario + Rabbids: Sparks of Hope** (free-form movement within a dome, real-time-during-your-turn feel, deterministic damage, status effect deck, real-time grenade pressure) + **XCOM 2** (explicit cover math, flanking via anchor facing, overwatch) + **Expedition 33** (parry/dodge reactive layer, intent telegraphs).
>
> **Setting:** The outskirts of [[05 Mega Structures - Game Worlds#Lithic Mow|Lithic Mow]] in the post-[[12 The Akashic and The Bleed|Breach]] world, alongside [[07 Civilization - The Grinders|the Grinders]] — the player's first allies in the journey to rebuild what the [[14 Naming Glossary#The Federation|Mani Accord]] left behind.
>
> **Strategic rationale:** the prototype's procgen + tactical combat tech transfers **directly** into the main game's dungeon system. Lore is the same lore — the prototype is canonically the player's first hours after waking from cryo.

> Lore reference stack: [[14 Naming Glossary]] · [[12 The Akashic and The Bleed]] · [[11 Factions and Species]] · [[13 Campaign Structure]] · [[15 Grinder Trust Arc]]

---

## 1. One-line Pitch

A stealth-extraction looter set in the post-Breach valley of Lithic Mow — earn the trust of the [[07 Civilization - The Grinders|Grinders]], teach them refinement, read enemy intents, parry, flank with cover, and weaponize raw [[06 Element in Looter Shooter|Mani]] in turn-based duels that feel real-time because you actually drive your character.

## 2. Design Goals (Prototype Scope)

1. **Prove the combat model** — Sparks-of-Hope × XCOM × Expedition 33 fused into a single cohesive system that feels readable, skill-expressive, and never RNG-frustrating.
2. **Prove the procgen room tech** — modular prefabs + assembler produce tactically coherent levels. This is the dream game's hardest unknown; the prototype de-risks it.
3. **Prove the Mani loop** — unpredictable raw Mani grenades create memorable moments without feeling unfair; refined Bhu-Mani becomes the carrot pulling players forward.
4. **Prove the social loop** — the [[15 Grinder Trust Arc|Grinder trust arc]] earned through demonstration, not handed over via quests.
5. **Prove the loot tension** — going in light vs. heavy, deciding when to extract.
6. **Prove the meta loop** — base → raid → return → upgrade → harder raid.
7. **Be shippable in isolation.** Anything that requires the city builder / automation pillars is out of scope.

## 3. Core Loop

```
Base (stash, Grinder vendor [Allied tier+], craft, load out)
   ↓ pick raid map + difficulty
Raid load → procedural assembler stitches rooms from Lithic Mow library
   ↓
Exploration (real-time top-down stealth across stitched rooms)
   ↓ Grinder / scavenger alerted + LOS → encounter triggers
Tactical Combat — turn-based but kinetic
   • Free-form movement within visible dome (Sparks of Hope)
   • 2 actions per turn — no movement after weapon fire
   • Cover anchors with facing cones for flanking (XCOM)
   • Raw Mani grenades — live detonation timer, random Effect Table
   • Parry / Dodge / Overwatch (E33 + XCOM)
   • Deterministic damage; RNG only in Mani rolls & status procs
   ↓ win → loot (Mani shards, weapons, mods), real-time resumes
   ↓ flee → reposition + invuln, real-time resumes
   ↓ lose → drop carried loot, run ends
Extract at evac point  →  back to Base
```

## 4. Pillars

| Pillar | Player Feeling | Mechanic |
|---|---|---|
| **Extraction tension** | "Do I push one more room?" | Weight, raid timer, evac points, on-death loot drop |
| **Tactical kinetic combat** | "I read that attack and dashed into flanking position." | Free-form movement dome, intent icons, cover + facing, parry timing |
| **Mani unpredictability** | "Did I just freeze the whole room?" | Live-timer raw Mani grenades, Effect Table rolls, destructible Mani veins |
| **Social earning** | "I'm one of them now." | [[15 Grinder Trust Arc|Trust arc]] — gates everything, earned through three discrete moments |
| **Loot identity** | "This gun is *mine*." | Grid inventory, rarity, mods, condition |
| **Meta progression** | "Next run I'm ready." | Stash, vendor rep, craft tiers, blueprints, skill tree |

## 5. Combat Model Summary

Full design in [[04 List of things required]] and [[10 Unity Code Architecture]]. Headline rules:

- **Turn:** free-form movement within a visible **dome** (real-time stick input, Sparks of Hope), then **2 actions** (weapon / technique / item). **No movement after weapon fire** — commit to the shot.
- **Damage:** deterministic — shots always land, cover reduces damage. **No hit%.**
- **Variance:** lives in parry timing (Expedition 33), crit windows (flank / perfect parry / status combos), status effect proc chances, and **raw Mani Effect Table rolls** (lore-justified by [[12 The Akashic and The Bleed#5 The Akashic / The Bleed — the corrupting force|Akashic contamination]] in every grain of Mani).
- **Intent:** every enemy shows next action + target + predicted damage at start of their turn.
- **Cover:** Full −66%, Half −33%, Flank +crit. **Anchors have facing cones** — shooting outside the cone = flank. Preserves XCOM flanking without a grid.
- **Movement:** free-form within dome; **dash-through chips enemies, costs no movement**; authored traversal points (pipes/vents/ledges) per room.
- **Raw Mani grenade:** pick up shard → live timer in hand → free movement during timer → throw / hold-detonate → roll on Mani Effect Table.
- **Status deck:** Burn, Freeze, Push, Honey, Vamp, Ink, Suppressed, Stagger — applied via ammo, abilities, raw Mani grenades, shattered Mani veins.

## 6. Setting — Lithic Mow Outskirts

Single biome: **the outskirts of [[05 Mega Structures - Game Worlds#Lithic Mow|Lithic Mow]]**, controlled by [[07 Civilization - The Grinders|The Grinders]]. The player wakes at the [[14 Naming Glossary#World Locations|Headwaters]] (a forgotten Accord research outpost upriver) and ventures into the valley.

**Sub-themes for room library:**
- **Mining tunnels** — tight LOS, ambush corridors, dense cover anchors
- **Drill machinery rooms** — multi-level vertical fights, height-advantage gameplay
- **Outdoor camp areas** — open ground with scavenged shelter, longer sightlines
- **Mani vein chambers** — destructible cover walls, high reward / high chaos

Three raid map *seeds* (small / medium / large) — each procedurally assembled per raid from the shared 30–50 room library. Story beats minimal in raid zones; story happens at the Grinder camp via the trust arc.

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
- **Raw Mani Grenades** with live detonation timers create real-time pressure inside turn-based combat
- **Mani Effect Table** rolls deliver Burn / Freeze / Push / Honey / Vamp / Ink / Suppressed / Stagger / rarer effects (Phase, Summon Mani-construct, Anti-grav lift, Create cover)
- **Mani Veins** in mine walls are destructible cover that drop shards and trigger radial random effects on break
- **Refined Bhu-Mani = chosen effects** — unlocked after teaching the Grinders. This is the carrot.

### Grinders are a faction, not a stat block
Their lore (~15–20 hunter-gatherers, mining-proficient, Mani-ignorant) drives:
- **Pack-tribe AI** — coordinated callouts, regrouping, never alone
- **Mining-aesthetic weapons** — drills (cover-breakers), mining charges (delayed AOE), scavenged firearms
- **Mani ignorance is a mechanic** — Grinder grenades randomly affect everyone including themselves; players can bait self-harm via positioning
- **Archetypes:** Scout · Driller · Chief · Shaman (rare Mani-Nova caster)

### Lithic Mow is a combat environment, not a backdrop
- Tunnel-density LOS, drill-rooms verticality, **Mani veins**, cave-in mid-combat events
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

- Combat system (Sparks × XCOM × E33) — gains party orchestration on top
- Mani system — gains Jal/Vayu/Agni refinement tiers + purification anchor mechanic
- Faction system — gains Amphibian Husks + Reptile Husks
- Room prefab library + assembler + encounter director — becomes the dungeon procgen for Vats and Forge
- Cover system with facing cones, AI goal selector, status effect system
- Grid inventory, item definitions, crafting recipes
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
- No raid feels like the same raid twice (procgen passes the "novelty test").
- Inventory tetris feels *fun*, not chore.
- Players describe combat as "tactical" and "kinetic" — never "lucky" or "static."
