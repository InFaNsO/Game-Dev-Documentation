
# Factions and Species

This document covers the three species of the [[Mani Accord]], their current state, the enemy roster per faction, art-direction principles for assets, and the purification system.

> See [[14 Naming Glossary]] for all locked names. See [[12 The Akashic and The Bleed]] for the cosmic-horror lore underlying corruption.

---

## 1. Art-Direction Principles (the rules that make this shippable solo)

### Principle 1 — One rig, three meshes
All three species share a single **minimal humanoid rig** of ~8–10 bones total: head, torso, two stub-arms, two stub-legs, optional procedural tail/frill chain. No finger bones. No spine bend. No facial micro-rigging. Eyes are textures, mouth is a single blendshape if used at all.

### Principle 2 — Visual style: Escape from Duckov / Universim
Stylized simplicity. Large heads, small bodies, mitten hands, rounded silhouettes. Charm-forward read. Within this style, **silhouette and color do all the species-recognition work**, not anatomy.

### Principle 3 — Corruption is a shader, not a model
Echo-corrupted (Bleed-touched) variants of any species use the **same mesh** as their pre-Breach form, with a parameterized **corruption shader overlay**:
- **Jal-Mani blue glow + crystalline growths** for Amphibian Husks
- **Agni-Mani orange-red glow + cracked scales + smoke wisps** for Reptile Husks

### Principle 4 — Archetypes via accessories, not new rigs
Different archetypes within a species differentiate via **equipment swaps, color palettes, scale variants, and bolt-on accessory meshes** — never new rigs or new base meshes.

### Asset budget summary
- 1 humanoid rig (~8–10 bones)
- 3 base meshes (Human / Amphibian / Reptile)
- ~6 shared animations (walk, idle, attack, hit-react, death, throw/loot)
- 1 parameterized corruption shader
- ~10–15 equipment props (drills, bows, charges, refinement tools)
- ~3 bolt-on mesh accessories (Shaman robe, Reptile frill variants, Amphibian growth caps)
- ~10 color palettes for cosmetic differentiation

That covers the entire visual roster for prototype + main game.

---

## 2. The Three Species

### Humans

**Body plan:** Standard tall stylized humanoid for Duckov/Universim style — slightly oversized head, small body, mitten hands.

**Pre-Breach role:** Engineers, mathematicians, designers. Drafted the Mega Structure blueprints, ran central administration. **Worked at all three structures but were resident at none** — coordinators rather than operators. Their biology had no special Mani affinity, which is exactly why they were the most replaceable and the most dispersed.

**Post-Breach status:**
- Most died.
- Some survived in deep cryo facilities (the player is one of these).
- Some descendants survived in pockets across the continent and degraded over millennia into hunter-gatherers — **the Grinders.**
- **No bio-attunement to Mani meant minimal Bleed absorption** → mostly avoided corruption.

**Cosmetic differentiation in-game:**
- **Player / cryo-survivor**: Faded Accord jumpsuit colors (clean palette, geometric markings)
- **Grinder archetypes**: Same mesh, swapped equipment props (drill, bow, charges, shaman robe), different tribal earth-tone palettes, scale variation for Chief
- **Rogue Grinder Defector vendor**: Unique palette + small bolt-on apron/satchel

### Amphibians

**Body plan:** Bipedal pear-shape — wider lower body, narrower shoulders, oversized rounded head. Mitten hands implied webbed via texture. Slimy/wet shader carries enormous visual character with zero geometry cost.

**Pre-Breach role:** Vapor refiners at **Genesis Vats**. Bio-permeable amphibian skin let them work inside vapor refinement chambers that would kill humans in hours. **The Accord literally could not refine Jal-Mani or Vayu-Mani without them.** Cultural disposition: **collective, ritualistic, water-mystical.**

**Post-Breach status — corrupted en masse:**
The very bio-affinity that made them irreplaceable made them **the first and most thoroughly corrupted.** When The Breach happened, The Bleed poured through the Vats vapor directly into Amphibian respiratory systems. They didn't die. They became Husks — and have walked the Vats for millennia, immortal and unsleeping.

**Husk visual:** Same base mesh, corruption shader overlay — **Jal-Mani blue glow** in chest cavity and eye sockets, crystalline growths erupting from shoulders/back, beads of water that defy gravity.

### Reptiles

**Body plan:** Tall lean bipedal silhouette — elongated, narrow shoulders, smaller head with subtle snout extension. **Procedural tail** (animated via physics chain, no manual rig joints) + **head frill** (single blendshape, flares on aggression). Scaly shader, dry and dusty.

**Pre-Breach role:** Heat refiners at **Prism Forge**. Cold-blooded reptilian biology tolerated lethal forge temperatures. **The Accord could not refine Agni-Mani without them.** Cultural disposition: **hierarchical, formal, sun-ritualistic** — direct opposite cultural pole from the collective Amphibians.

**Post-Breach status — corrupted en masse:**
The Breach happened during the final Akash refinement attempt at the central refinement complex (the Forge was supplying heat at peak output). Reptiles were closest to the energy peak. Most died instantly. Those who didn't became Husks within minutes — and have stalked the Forge for millennia.

**Husk visual:** Same base mesh, corruption shader overlay — **Agni-Mani orange-red glow** seeping through cracked scales, smoke wisps from joints, eye-pits burned out and glowing.

---

## 3. Enemy Roster

Anchor for combat-role variety: **Frontline/Tank · Ranged · Support · Special**. Each species emphasizes different roles to keep encounters varied.

### Grinders (Humans — primary prototype faction)

| Archetype | Role | Hook | Tier |
|---|---|---|---|
| **Scout** | Ranged | Primitive bow + throws raw Mani grenades (random Effect Table — can self-harm) | MVP |
| **Driller** | Frontline / Tank | Mining drill melee, **breaks cover** on hit | MVP |
| **Chief** | Support | Buff aura for adjacent Grinders, lobs mining-charge artillery | V1 |
| **Shaman** | Special | Attempts to refine Mani live — signature *Mani Nova* (high-risk-high-reward AOE) | V1 |
| **Elder Chieftain** (boss) | — | Unique encounter, main game; commands the surviving tribe at Lithic Mow | Main game |

### Amphibian Husks (Genesis Vats)

Combat flavor: **collective swarm + disruption.** Jal-Mani affinity (water/freeze/push).

| Archetype | Role | Hook |
|---|---|---|
| **Vat-Brood** | Frontline (swarm) | Scaled-down mesh, come in groups of 4–6, fast melee, low HP each. *Lore: the children of the Vats, preserved as a collective by The Bleed.* |
| **Tidefall** | Special (exploder) | Slow walker, body crusted with Jal-Mani. On death → AOE **Freeze/Push** burst. *Lore: vapor-workers fused with their own crystallized refinement.* |
| **Choir** | Support | Stationary, applies **Confusion** to player + **Regen** to nearby Husks. Must be silenced first. *Lore: the collective ritual chanting persists, terribly.* |
| **Vatlord** (boss) | — | Genesis Vats reclamation boss. Multi-phase. Spawns Vat-Brood mid-fight, cascading status. Drops first **Refined Jal-Mani**. |

### Reptile Husks (Prism Forge)

Combat flavor: **predator pressure + burn.** Agni-Mani affinity (fire/burn).

| Archetype | Role | Hook |
|---|---|---|
| **Cinder-Stalker** | Special (assassin) | Fast melee. Stiff predator dash — covers huge distance in one move, ignores half-cover on flank approach. *Lore: the working-rank reptiles, weaponized.* |
| **Ember-Speaker** | Ranged | Stationary caster. Lobs Agni-Mani bolts that apply **Burn**; rare refined fire-AOE. Lowest HP. *Lore: the senior refiners.* |
| **Forge-Sentinel** | Frontline / Tank | Heavy armored, slow, high HP. Damage-reduction aura. Dies to penetration / flank. *Lore: the ceremonial Forge guards, still in their armor.* |
| **Sun-King** (boss) | — | Prism Forge reclamation boss. Multi-phase: phase 1 = sun pillars (LOS hazards), phase 2 = burn-everything ultimate. Drops first **Refined Agni-Mani**. |

### Element-Touched Wildlife

Passive cross-faction ambient threats. Wildlife touched by residual Bleed pools.
- **Stoneback Burrower** — armored herbivore, hostile if cornered, mines passively. Bleed-touched variant has crystal protrusions.
- **Silt Hound** — pack predator, uses sound. Bleed-touched variant is briefly incorporeal.
- **Drill Wyrm** — rare large worm. Mid-tier threat encounter. Bleed-touched variant erupts unpredictably.

---

## 4. Boss Strategy: Asset Approach

**Bosses use the base species mesh, scaled 1.5×, with unique accessory loadouts and unique abilities.** No bespoke boss meshes. Specifically:

- **Elder Chieftain** = upscaled Human mesh + chieftain crown + ceremonial weapon
- **Vatlord** = upscaled Amphibian mesh + crown of crystalline growths
- **Sun-King** = upscaled Reptile mesh + Forge ceremonial armor + sun-disc accessory

> 📝 **Open design note — multiple bosses per biome (Duckov-style):**
> Escape from Duckov places multiple boss-tier encounters across each map (not all story-critical — many are elite spawns with unique loot, gimmicks, or named identity). Consider extending each biome with **2–3 additional mini-bosses** beyond the single story boss. Examples for shape:
> - **Lithic Mow:** Elder Chieftain (story) + *The Drill-Mother* (unique Driller variant with cover-shredding drill), *Whisper-Shaman* (a Shaman who almost succeeded at refining Mani — drops first chosen-element raw shard)
> - **Genesis Vats:** Vatlord (story) + *The Drowned Choir* (3 fused Choirs sharing HP), *Tide-Captain* (Tidefall with extended detonation radius + multi-phase)
> - **Prism Forge:** Sun-King (story) + *Ash-Marshal* (elite Forge-Sentinel commanding lesser Sentinels), *Mirror-Speaker* (Ember-Speaker that reflects projectiles)
>
> **Asset cost** is low (reuses base archetype + 1–2 accessories + tuned stats). **Design cost** is real — each needs a unique mechanical hook to feel boss-tier. Revisit after [[13 Campaign Structure]] details solidify.

---

## 5. The Purification System (hybrid)

Locked from Topic 2 design discussions.

### Two tiers of Husks

| Tier | Who | Purification |
|---|---|---|
| **Common Husks** | Workers, soldiers, refinery technicians who **didn't know the Accord's secret**. Generic enemies. | Not purifiable. Killed in normal combat — but **"killing" them is mercy** (severs the radiation thread, lets millennia-old prisoners finally rest). |
| **Named / Inner-Circle Husks** | Accord scientists, doorway operators, "first-through" volunteers — **the ones who knew.** Rare, named, encountered at specific story beats. | Story-gated purification. Encounter triggers a unique combat opportunity. Player can kill or attempt purification. |

### Purification mechanic (named NPCs only)

1. Player encounters a named Husk in a biome (marked differently — slightly distinct silhouette, name displayed, doesn't immediately attack).
2. Combat begins; player can choose to kill normally or attempt purification.
3. Purification = consume **1 unit of Refined Mani matching the species** (Jal-Mani for Amphibian, Agni-Mani for Reptile) + complete a short timing / QTE sequence to "anchor" the transmutation.
4. **On success:** Husk reverts to pre-Breach form, joins the player base as a colonist or party member, gains a story arc. **The first such purification reveals the Accord's conspiracy** ([[12 The Akashic and The Bleed]]).
5. **On failure:** Husk dies; refined Mani consumed; reputation hit with the would-be ally faction.

### Why this is good

- Uses the refined-Mani progression as the *cost* — players have to *want* to save someone enough to spend the rare resource
- Named encounters become memorable
- Both outcomes (success / fail) tell a story
- Generic Husks remain straightforward combat — no scope creep
- **The first named-NPC purification is the act-one-to-act-two pivot for the entire main game** — see [[13 Campaign Structure]]

---

## 6. Faction Reputation (high level)

- **Grinders:** Trust arc gates camp access, vendor stock, companion availability. See [[15 Grinder Trust Arc]].
- **Amphibian / Reptile species (purified ally side):** Recruited via story purifications. Each purified named NPC counts toward the species-cooperation endgame path.
- **Rogue Grinder Defector vendor:** Friendly by default. Possibly folds into the main Grinder vendor once trust arc completes.
