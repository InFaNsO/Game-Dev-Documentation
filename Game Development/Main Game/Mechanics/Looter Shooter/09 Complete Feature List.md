
# Complete Feature List — Standalone Prototype

Legend: **[MVP]** ship-blocker · **[V1]** first polish pass · **[Stretch]** nice-to-have.

Combat model = **Sparks of Hope × XCOM × Expedition 33** (kinetic free-form movement with rescued XCOM tactics and reactive timing).
Setting & content = lore-driven (Lithic Mow / Grinders / Mani).
See [[08 Standalone Prototype - Design]] for design intent and [[10 Unity Code Architecture]] for systems layout.

> Lore reference stack: [[14 Naming Glossary]] · [[12 The Akashic and The Bleed]] · [[11 Factions and Species]] · [[13 Campaign Structure]] · [[15 Grinder Trust Arc]]

---

## A. Player & Movement (Real-time exploration)

- [MVP] Top-down 3D character (or 2.5D), WASD movement, mouse-aim
- [MVP] Sprint with stamina drain
- [MVP] Crouch (reduces noise + detection radius)
- [MVP] Interact prompt (loot containers, doors, evac)
- [MVP] Carry weight affects move speed
- [V1] Footstep noise tied to surface type
- [V1] Lean (peek corners)
- [Stretch] Vaulting / mantling low cover

## B. Stealth & Detection

- [MVP] Enemy vision cone (range + FOV)
- [MVP] Enemy hearing radius (scales with player noise)
- [MVP] 3-state AI: Idle → Suspicious → Alerted (encounter trigger)
- [V1] Distraction throwables (rocks, noisemakers)
- [V1] Line-of-sight breaks reset suspicion over time
- [Stretch] Body hiding, silent takedowns from behind

## C. Combat — Tactics + Timing Layer

Triggered when an `Alerted` enemy gets LOS within combat range. Time freezes for enemies, camera blends, encounter UI fades in. **Player stays in continuous world coords — no grid snap.** Scope = current room + adjacent rooms within ~15m.

### C1. Turn Structure (Sparks of Hope action economy)
- [MVP] **Initiative order** based on agility stat
- [MVP] **Per turn:** free-form movement within visible dome + **2 actions** (weapon attack / technique / item)
- [MVP] **No movement after using your weapon** — commit-to-the-shot rule
- [MVP] Dash and Team Jump (main game) do NOT consume movement budget
- [MVP] **Reactives free** during enemy turn (cost stamina / timing, not actions)
- [V1] Turn order UI shows next 5 combatants with portraits

### C2. Tactical Space (continuous, NOT a grid)
- [MVP] **No grid snap** — combat preserves explore-mode positions
- [MVP] **Movement dome** rendered on ground during your turn (visible range circle)
- [MVP] NavMesh real-time pathing — you drive the character with the stick
- [MVP] Cover anchors authored as discrete metadata on room prefabs
- [MVP] LOS via raycasts against world colliders + cover anchor cones
- [V1] **Height layers** (multi-level rooms with up/down traversal)

### C3. Cover System (XCOM crispness rescued)
- [MVP] **Full cover** — −66% damage
- [MVP] **Half cover** — −33% damage
- [MVP] **Cover anchor facing cones** — shooting from outside the cone = **flanked** (no cover bonus + crit chance). Preserves XCOM flanking in continuous space.
- [MVP] **Height advantage** — +10% damage, ignores half cover
- [V1] **Destructible cover** — high-cal rounds & grenades degrade cover over hits
- [V1] **Mani veins as destructible cover** (lore tie — see C13)
- [V1] **Smoke** — blocks LOS, grants soft cover

### C4. Damage Model — Deterministic (Expedition 33 × Sparks of Hope)
- [MVP] Weapon shots **always land** for calculated damage (base ±10% variance)
- [MVP] Cover modifies **damage**, not hit chance
- [MVP] **No hit% on basic shots.** RNG lives in: Mani Effect rolls, status proc, crit windows
- [MVP] Crits earned via: flanking, perfect parry counter, status combos
- [V1] Penetration math: bullet pen value vs. armor class
- [V1] Body-part hit zones — Head / Torso / Limbs with multipliers

### C5. Movement-as-Mechanic (Sparks of Hope core)
- [MVP] **Free-form stick-driven movement** within dome — feels real-time
- [MVP] **Dash-through** enemies on path → chip damage; limited count per turn
- [MVP] **Authored traversal points** in room prefabs (pipes, vents, ledges)
- [V1] **Movement combos** — dash → traversal → flank in one move phase
- [Stretch] **Team Jump** — disabled in prototype (solo); ships in main game

### C6. Intent System (STS2 / Expedition 33)
- [MVP] **Intent icon** above each enemy at start of their turn
- [MVP] Icon shows: **action type + target + predicted damage**
- [MVP] Player sees this *before* committing their own action
- [V1] Intent reveals scale with player Perception stat (low perception = vague icons)
- [V1] **Knowledge Log integration** — examining enemy lore unlocks richer intent reveals (lore = combat power loop)

### C7. Reactive Layer (Expedition 33)
- [MVP] **Parry** — timed input minigame on incoming attack
  - [MVP] Perfect = 0 damage + counterattack (free shot)
  - [MVP] Good = 50% reduction
  - [MVP] Miss = full damage
- [MVP] **Dodge** — held stamina, opposed agility roll → full miss on success
- [MVP] Reactives are **free** (no action cost) but consume stamina or require timing skill
- [V1] **Parry assist** mode (accessibility) — widens window
- [V1] **Reactive abilities** via skill tree (riposte chain, dodge → counter-roll)

### C8. Abilities (JRPG layer)
- [MVP] **2–3 signature abilities** per weapon class (cooldown-based)
- [MVP] **1 Ultimate** per character — gauge fills via damage taken/dealt + **Mani pickup**
- [V1] Ability upgrades via skill tree branches
- [V1] Mod-granted abilities (rare weapon mods unlock unique actions)

### C9. Status Effect Deck (Sparks of Hope Super Effects)
- [MVP] **Burn** (DoT) · **Freeze** (skip turn) · **Push** (forced move) · **Honey / Root** (no movement)
- [V1] **Vamp** (steal HP) · **Ink** (can't shoot) · **Suppressed** (move only) · **Stagger** (reduced actions next turn)
- [MVP] Applied via: ammo types, abilities, **Mani grenades**, **Mani vein breaks**
- [V1] **Combo bonuses** — e.g., Burn + Push → enemy moves through fire for extra dmg

### C10. Overwatch (XCOM, adapted for continuous space)
- [MVP] End turn with held action → triggers on enemy entering LOS
- [MVP] **Continuous LOS check** during enemy real-time movement (raycast per tick)
- [V1] **Overwatch ambush** — full-stealth overwatch = guaranteed crit on trigger

### C11. Enemy AI (Goal-Oriented, faction-aware)
- [MVP] Goals: Patrol / Investigate / Engage / Flank / Regroup / Retreat
- [MVP] **Intent picker** weights goals against cover state + ally positions
- [MVP] **Faction behavior overlay** — Grinders use pack-tribe AI (see Section J)
- [V1] Squad behavior — flanking, suppressive fire callouts
- [V1] **Reinforcements drift in** from adjacent rooms if alerted by gunfire (later turns)
- [V1] Boss archetype per map theme (Grinder Chief / Shaman)
- [Stretch] Faction reputation visibility — Grinders react differently if you've been spotted before

### C12. Encounter Lifecycle
- [MVP] Trigger → freeze enemies → camera blend → first turn (~0.8s transition, no grid snap)
- [MVP] **Flee action** — opposed agility vs. enemy alertness; success = brief invuln, real-time resumes, enemies investigate
- [MVP] Victory → corpses spawn as lootable; real-time resumes
- [V1] Combat replay log

### C13. Mani System (LORE-DRIVEN, the prototype's signature mechanic)
- [MVP] **Mani Grenade — real-time pressure**
  - [MVP] Pick up Mani shard → live detonation timer in hand (~5–8 sec configurable)
  - [MVP] Free movement, dash, traversal during timer countdown
  - [MVP] Throw or hold-to-detonate before expiry
  - [MVP] **Auto-detonate at 0** (in hand = self-damage; great risk/reward)
- [MVP] **Mani Effect Table** — roll on detonation
  - [MVP] Common (60%): Burn / Freeze / Push / Stagger
  - [V1] Uncommon (30%): Honey / Vamp / Ink / Suppressed
  - [V1] Rare (10%): Phase (teleport caster), Summon wildlife, Anti-grav lift, Create cover
- [MVP] **Mani-Infused Ammo** — per-shot proc chance triggers common-tier Effect roll
- [V1] **Mani Veins** in mine walls — destructible cover, drops shards when broken, triggers radial random effect on break
- [V1] **Ultimate fueling** — Mani pickup adds to Ultimate gauge faster than damage alone
- [Locked / Main Game] **Refined Mani** — chosen effects (not rolled), unlocked by Mega Structure progression

---

## D. Weapons

- [MVP] Categories: Pistol, SMG, Shotgun, AR, Sniper, Melee
- [MVP] Per-weapon: damage, range, action cost, mag size, recoil curve, durability
- [MVP] Durability — degrades per shot, breaks at 0
- [MVP] **Ammo types** per caliber: FMJ / AP / HP / **Mani-infused** (status proc on hit)
- [V1] **Mod slots** — barrel, optic, mag, grip, stock; mods alter stats / abilities
- [V1] **Grinder-exclusive weapons** unlocked via vendor rep (drills, mining charges, scavenged scrap-guns)
- [V1] Jamming if condition < threshold
- [V1] Weapon mastery XP per weapon family → unlock perks
- [Stretch] Crafted unique weapons with random affixes

## E. Armor & Equipment

- [MVP] Slots: Helmet, Vest, Backpack, Rig (chest pouches)
- [MVP] Armor class (1–6), durability, weight, coverage zones
- [MVP] Backpack & Rig define **grid inventory size**
- [V1] Penetration math: bullet pen value vs. armor class
- [V1] Helmets with face/ear coverage zones

## F. Inventory — Grid (Tarkov-style)

- [MVP] Grid-based (items take WxH cells)
- [MVP] Drag/drop, rotate, stack
- [MVP] Containers within containers (cases, ammo boxes, med-bags)
- [MVP] Quickslots (hotbar 1–4) bound to rig pouches
- [MVP] Item context menu: equip, use, examine, drop, split
- [MVP] **Mani shards take dedicated slot** (cannot stack with normal items — represents instability)
- [V1] Sort & auto-arrange
- [V1] Search filter in stash
- [V1] **Knowledge Log** examine reveals stats only after found N times (knowledge progression)

## G. Loot & Containers

- [MVP] World loot: containers, corpses, ground loot (placed by **loot director** per room type + depth)
- [MVP] Search timer with progress bar (vulnerable while searching)
- [MVP] Loot tables per container tier, weighted by raid difficulty
- [MVP] Rarity tiers: Common / Uncommon / Rare / Epic / Legendary
- [MVP] **Mani shards as world spawns** — rare drops in Mani vein chambers + Grinder Shaman drops
- [V1] Locked containers requiring keys/lockpicks (minigame)
- [V1] Loot rumor system — vendor hints at rare spawns
- [V1] **Loot only in real-time** (cannot loot mid-encounter)

## H. Raids & Maps — Procedural Assembly (Lithic Mow Outskirts)

- [MVP] **3 raid map seeds** (small / medium / large) — each procedurally assembled per raid
- [MVP] **30–50 hand-authored room prefabs** across 3 sub-themes:
  - **Mining tunnels** — tight LOS, ambush corridors
  - **Drill machinery rooms** — multi-level vertical
  - **Outdoor camps / Mani vein chambers** — open / chaotic-reward
- [MVP] Multiple spawn + evac points per assembled map
- [MVP] Raid timer (~25 min) — late raid spawns scavenger patrols / cave-in events
- [MVP] Map difficulty selector affects loot quality + enemy gear + Mani vein density
- [V1] **Cave-in events** — environmental hazard mid-combat, blocks passages, deals AOE
- [V1] Weather & time-of-day variation (dust storms reduce visibility)
- [V1] Boss room at high difficulty (Grinder Chief / Shaman encounter)

## I. Procedural Level Assembly

- [MVP] **Room prefab contract** — every prefab ships metadata:
  - Cover anchors (Full / Half / None) **with facing cones** (for flanking)
  - Spawn anchors (player, enemy, loot, **Mani vein**, boss)
  - Door sockets with directional connection rules
  - Traversal points (pipe, vent, ledge)
  - Height-layer markers
  - **Environmental hazard markers** (cave-in trigger zones)
  - Theme tag, size tag, role tag (combat / loot / corridor / boss / evac)
- [MVP] **Assembler** — layout grammar / WFC stitches rooms via door sockets
  - Rules: ≥1 evac on outer edge, boss behind objective room, no dead-end corridors
- [MVP] **Encounter director** — places enemies by cover-cluster + sightline heuristics, **faction-aware** (Grinders spawn as cohesive squads)
- [MVP] **Loot director** — places loot by room role + raid depth, includes Mani shard spawns at vein anchors
- [MVP] **Runtime navmesh bake** at raid load (async, `NavMeshSurface.BuildNavMesh`)
- [V1] **Mission templates** dictate macro layout (linear / hub-and-spoke / loop)
- [V1] **Library expansion tools** — editor validators for prefab metadata
- [Stretch] Player-detectable seed re-roll (rare in-game item)

## J. Enemies, Factions & World

### J1. Grinders (Primary Hostile Faction)
- [MVP] **Pack-tribe AI overlay** — callouts, regrouping, never alone, shared blackboard
- [MVP] **Mining-aesthetic weapon palette** — drills, mining charges, scavenged firearms
- [MVP] **Mani-ignorance mechanic** — Grinder-thrown raw Mani grenades randomly affect everyone, including themselves. Player can bait self-harm via positioning.
- [MVP] Archetypes:
  - [MVP] **Grinder Scout** — light, fast, primitive bow + Mani grenade thrower
  - [MVP] **Grinder Driller** — heavy melee, mining drill can break cover
  - [V1] **Grinder Chief** — buff aura for nearby Grinders, mining-charge artillery (delayed AOE)
  - [V1] **Grinder Shaman** (rare) — attempts to refine Mani, signature: **Mani Nova** (high-risk-high-reward area burst)

### J2. Bleed-Touched Wildlife
- [V1] Passive faction, hostile if provoked
- [V1] Minor combat presence; rare encounters
- [V1] Drops Mani-related materials

### J3. Rogue Grinder Defector (Vendor NPC)
- [MVP] Single NPC at player's base
- [MVP] Lore source — audio logs + dialogue about Mani, the Mani Accord, Mega Structures, Grinder society
- [V1] Reputation track — buying / selling / quest completion → unlock Grinder-exclusive gear

### J4. Faction System (Architectural)
- [MVP] **`FactionDef`** SO drives: AI overlay, weapon palette, archetype roster, reputation track
- [V1] Reputation visible in vendor UI + reflected in vendor pricing
- [Stretch] Faction-on-faction events (e.g., Grinder patrol attacks wildlife mid-raid)

## K. Base / Hideout

- [MVP] Stash (large grid storage)
- [MVP] **Rogue Grinder vendor** (buy/sell, dynamic prices)
- [MVP] Crafting station
- [MVP] Loadout screen
- [V1] Vendor reputation tiers — Grinder-exclusive gear unlocks
- [V1] Craft minigame — success → rarity bump
- [V1] Workbench upgrades unlock recipe tiers (including **Mani-infused ammo recipes**)
- [Stretch] Hideout cosmetic customization

## L. Economy & Crafting

- [MVP] Currency (scrip / scrap)
- [MVP] Buy/sell with vendor at dynamic prices
- [MVP] Recipes consume materials → produce items
- [MVP] **Mani-infused ammo recipe** — consume raw Mani shard → produce ammo with proc chance
- [V1] Blueprint items unlock new recipes permanently
- [V1] Salvage station — break items into materials
- [Stretch] Auto-sell rules

## M. Progression (Meta)

- [MVP] Character XP & level
- [MVP] Stash persists across runs
- [MVP] **Skill tree** — branches: Tactics (combat/cover/overwatch), Reflexes (parry/dodge windows), Looting, Crafting, **Mani Affinity** (Effect Table re-roll, longer timers, ammo proc bonus)
- [V1] Weapon mastery perks per family
- [V1] **Knowledge Log** — unlocks examine info + intent clarity as encounters repeat
- [Stretch] Prestige / NG+ with modifiers

## N. Death & Risk

- [MVP] On death: drop everything carried; stash safe
- [MVP] Insurance: pay scrip pre-raid → recover specific items if lost
- [V1] Secure container (small grid that survives death)
- [V1] Karma — killing neutrals affects vendor prices

## O. UI / UX

- [MVP] Explore HUD: health, stamina, weight, ammo, quickbar, mini compass
- [MVP] Combat HUD: turn order, action counter, intent icons, action wheel, parry timing ring
- [MVP] **Movement dome visualization** on ground during your turn
- [MVP] **Cover anchor indicators** — show full/half + facing cone direction when you hover/approach
- [MVP] **Mani grenade timer** prominent in HUD when shard is live in hand
- [MVP] Inventory / stash / vendor / craft / map / loadout screens
- [MVP] Tutorial raid (small map, scripted first encounter + scripted Mani pickup)
- [V1] Tooltips with comparison (equipped vs. hovered)
- [V1] Damage log & combat replay
- [V1] **Knowledge Log screen** (lore + enemy + item entries)
- [Stretch] Accessibility — colorblind, parry assist mode, text scaling

## P. Audio

- [MVP] Footsteps, gunshots, reload, hit feedback
- [MVP] Combat encounter musical sting (mode shift cue)
- [MVP] **Mani shard pickup audio cue + ticking timer SFX** (real-time tension)
- [V1] Spatialized Grinder chatter / barks (pack-callouts on flank / overwatch / reload)
- [V1] Adaptive music (tension layers based on alert state)

## Q. Saving / Persistence

- [MVP] Save profile (character, stash, vendor rep, skill tree, knowledge log, faction reputations)
- [MVP] No mid-raid save (commit to the run)
- [V1] Cloud save slot
- [Stretch] Multiple character slots

## R. Tech / Engine (Unity-specific)

- [MVP] Unity 6 (URP)
- [MVP] New Input System
- [MVP] Addressables for room prefabs + item data
- [MVP] ScriptableObject-driven content (items, weapons, enemies, recipes, loot tables, room prefab metadata, **Mani effects, faction defs**)
- [MVP] NavMeshSurface runtime baking
- [V1] Cinemachine for camera transitions (explore ↔ combat)
- [V1] Unity AI Navigation for enemy patrols + combat pathing
- [Stretch] DOTS for large enemy counts (probably not needed at prototype scope)
