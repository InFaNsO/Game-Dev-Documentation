
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
| **Vatlord** (manager boss) | — | Genesis Vats reclamation boss = **the Vats site manager** under the Accord. Multi-phase, spawns Vat-Brood, cascading status. **Gives no Mani — requires Jal + Vayu (learned earlier in the Vats) to defeat.** Forced-purified; becomes ally; gives a *hint* toward the deeper truth. |

### Reptile Husks (Prism Forge)

Combat flavor: **predator pressure + burn.** Agni-Mani affinity (fire/burn).

| Archetype | Role | Hook |
|---|---|---|
| **Cinder-Stalker** | Special (assassin) | Fast melee. Stiff predator dash — covers huge distance in one move, ignores half-cover on flank approach. *Lore: the working-rank reptiles, weaponized.* |
| **Ember-Speaker** | Ranged | Stationary caster. Lobs Agni-Mani bolts that apply **Burn**; rare refined fire-AOE. Lowest HP. *Lore: the senior refiners.* |
| **Forge-Sentinel** | Frontline / Tank | Heavy armored, slow, high HP. Damage-reduction aura. Dies to penetration / flank. *Lore: the ceremonial Forge guards, still in their armor.* |
| **Sun-King** (manager boss) | — | Prism Forge reclamation boss = **the Forge site manager** under the Accord. Phase 1 = sun pillars (LOS hazards), phase 2 = burn-everything ultimate. **Gives no Mani — requires Agni (learned earlier in the Forge) to defeat.** Forced-purified; becomes ally; gives a *hint* toward the deeper truth. |

### The Head of Research (Final Boss)

| Entity | Role | Hook |
|---|---|---|
| **Head of Accord Research** (Amphibian) | **Campaign final boss** | The most senior researcher of the entire Mani Accord — **the boss the managers (Vatlord, Sun-King) answered to.** Led the secret Akash doorway work; was the most bio-attuned and closest to The Breach, hence the most thoroughly corrupted. Found in a **sealed deep chamber beneath Genesis Vats — the actual Breach site / ground zero** — accessible only after all three structures are reclaimed and the player holds all four elemental Manis. **Requires all four Manis (Bhu/Jal/Vayu/Agni) to defeat.** His Husk, even mindless, has spent a thousand years *unconsciously trying to prevent anyone from re-refining Akash.* Forced-purified → delivers the **endgame reckoning** (the full truth + the guardian revelation + framing the Choice). See [[12 The Akashic and The Bleed]] and [[13 Campaign Structure#6 The Choice (the endgame)|the Choice]]. |

### Wildlife — CUT (Topic 9)

**No wildlife faction exists in the game.** Cut deliberately to preserve the one-rig art discipline (wildlife would be the only enemy category requiring a non-humanoid rig pipeline) and to reinforce the dead-world cosmic-horror tone. The world is ecologically emptied — what lives is human (Grinders) or corrupted (Husks).

Non-player, non-Husk threats are covered by:
- **Hostile rival Grinders / scavengers** — splinter groups who reject the player's refinement knowledge as heresy (see the Elder Chieftain rival faction and the "Wild Driller" side quest in [[15 Grinder Trust Arc]]). Reuse the human rig — no new assets.
- **Environmental Bleed-pool hazards** — residual contamination zones that damage over time, not combatants.

Ambient non-combat life (insects, distant silhouettes, set-dressing fauna) may exist for atmosphere but is never a combat entity and never needs a combat rig.

---

## 4. Boss Strategy: Asset Approach

**Bosses use the base species mesh, scaled 1.5×, with unique accessory loadouts and unique abilities.** No bespoke boss meshes. Specifically:

- **Elder Chieftain** = upscaled Human mesh + chieftain crown + ceremonial weapon (rival Grinder, Lithic Mow)
- **Vatlord** = upscaled Amphibian mesh + crown of crystalline growths (Vats manager)
- **Sun-King** = upscaled Reptile mesh + Forge ceremonial armor + sun-disc accessory (Forge manager)
- **Head of Research** = upscaled Amphibian mesh + unique inner-circle accessories, distinct from Vatlord (final boss, beneath the Vats). Heaviest corruption-shader treatment in the game.

> 📝 **Open design note — multiple bosses per biome (Duckov-style):**
> Escape from Duckov places multiple boss-tier encounters across each map (not all story-critical — many are elite spawns with unique loot, gimmicks, or named identity). Consider extending each biome with **2–3 additional mini-bosses** beyond the single story boss. Examples for shape:
> - **Lithic Mow:** Elder Chieftain (story) + *The Drill-Mother* (unique Driller variant with cover-shredding drill), *Whisper-Shaman* (a Shaman who almost succeeded at refining Mani — drops first chosen-element raw shard)
> - **Genesis Vats:** Vatlord (story) + *The Drowned Choir* (3 fused Choirs sharing HP), *Tide-Captain* (Tidefall with extended detonation radius + multi-phase)
> - **Prism Forge:** Sun-King (story) + *Ash-Marshal* (elite Forge-Sentinel commanding lesser Sentinels), *Mirror-Speaker* (Ember-Speaker that reflects projectiles)
>
> **Asset cost** is low (reuses base archetype + 1–2 accessories + tuned stats). **Design cost** is real — each needs a unique mechanical hook to feel boss-tier. Revisit after [[13 Campaign Structure]] details solidify.

---

## 5. The Purification System (hybrid)

### Two tiers of Husks

| Tier | Who | Combat role | Purification |
|---|---|---|---|
| **Common Husks** | Workers, soldiers, refinery technicians who **didn't know the Accord's secret**. | Generic enemies (Vat-Brood, Tidefall, Choir, Cinder-Stalker, Ember-Speaker, Forge-Sentinel). | Not purifiable. Killed in normal combat — **"killing" them is mercy** (severs the radiation thread, lets millennia-old prisoners finally rest). |
| **Named / Inner-Circle Husks** | Accord researchers and site managers — **the ones who knew the doorway secret.** Their unusual intelligence vs other Husks comes from their conscious minds being trapped intact when their bodies were corrupted. | **Mini-bosses and bosses.** Combat-tier encounters, unique abilities, distinct silhouettes. | **Forced purification, no kill option** (see below). |

### The Named-Husk Hierarchy (org chart, preserved in corruption)

The Accord's command structure survives in the Husks. Three tiers:

```
        Head of Research  (Amphibian) ──────────── FINAL BOSS (beneath the Vats)
          /                       \
   Vats Manager = Vatlord     Forge Manager = Sun-King
   (Amphibian, boss)          (Reptile, boss)
          |                          |
   Teaching Husks             Teaching Husk
   (Vats 1, Vats 2)           (Forge 1)
```

**No named Husks at Lithic Mow** — Husks don't appear in the Grinder valley. The player already knows Bhu-Mani from cryo-memory; the named-Husk mechanic begins at the Vats.

**Tier 1 — Teaching Husks** (researchers; mini-bosses that teach a Mani):

| # | Location | Type | Teaches | Truth tier |
|---|---|---|---|---|
| 1 | **Vats — Entry Hub** | Amphibian mini-boss | **Jal-Mani** | First named encounter; introduces the mechanic. *Hint* only. |
| 2 | **Vats — Inner Sanctum** | Amphibian mini-boss | **Vayu-Mani** | **Mid-game factual reveal** — the Accord secretly tried to open a doorway to Akash; that's what caused The Breach. Powers Act 3 with dramatic irony. |
| 3 | **Forge — Entry Hub** | Reptile mini-boss | **Agni-Mani** | *Hint* only — references "someone above us," "the deep Vats." |

**Tier 2 — Manager Bosses** (the heads of each structure; reclamation bosses):

| Boss | Location | Type | Mani gate | Truth tier |
|---|---|---|---|---|
| **Vatlord** | Vats reactivation | Amphibian boss | Requires **Jal + Vayu** to defeat. Gives no Mani. | *Hint* — "I only managed this place. He… he was below us all." |
| **Sun-King** | Forge reactivation | Reptile boss | Requires **Agni** to defeat. Gives no Mani. | *Hint* — "The Forge was never the heart. Go back to the water." |

**Tier 3 — Final Boss** (the head of the whole Accord research effort):

| Boss | Location | Type | Mani gate | Truth tier |
|---|---|---|---|---|
| **Head of Research** | **Sealed deep chamber beneath Genesis Vats — the Breach site** (opens after all 3 structures reclaimed + all 4 Manis held) | Amphibian final boss | Requires **all four Manis (Bhu/Jal/Vayu/Agni)** to defeat | **The endgame reckoning** — full truth, the guardian revelation, frames the Choice. |

### Purification mechanic (all named Husks) — **mandatory, no kill option**

Named Husks **cannot be killed**. The player must defeat them in combat *and then* purify them by force, taking away their suffering whether they resist or accept. *"Mercy without consent"* — consistent with the cosmic-horror tone (millennia of half-life ended whether the prisoner is ready or not).

1. **Combat phase**: mini-boss/boss fight with unique abilities. Manager bosses and the final boss require the relevant Mani(s) to defeat.
2. **Transmission / revelation** (at critical state): the Husk weakens and speaks.
   - *Teaching Husks*: **teach their Mani refinement** as a dying-act knowledge transmission (player receives the recipe here) + a hint, or (Vats Husk 2) the mid-game factual reveal.
   - *Manager bosses*: deliver a **hint** toward the deeper truth.
   - *Final boss*: delivers the **full endgame reckoning** (see [[12 The Akashic and The Bleed]]).
3. **Forced purification QTE**: player uses the relevant Mani to anchor the transmutation. (Vats Husk 1, the first, uses the Bhu-Mani the player already knows; the Husk teaches Jal during this beat.)
4. **Resolution**: Husk reverts to pre-Breach form, joins the player base as an ally, becomes a named character with arc-aware dialogue at the endgame Council Scene.

> *"Take my knowledge, then take my pain."* For teaching Husks the encounter is one unified beat: fight → transmit → purify. The Mani recipe is unlocked **as part of** the encounter.

### Two-tier truth (information design)

- **Hints** (Vats Husk 1, Forge Husk 1, Vatlord, Sun-King): fragments that build intrigue — "someone above us," "go back to the water," "he's still down there."
- **Mid-game factual reveal** (Vats Husk 2): the conspiracy — *what* happened. Powers Act 3.
- **Endgame reckoning** (Head of Research, final boss): the *why* + the guardian revelation (his Husk spent a thousand years unconsciously trying to stop Akash re-refinement) + framing the Choice. This can only land at the end.

### What is and is not gated

- **Mani refinement recipes**: unlocked automatically through the forced teaching-Husk encounters. NEVER gated by player choice.
- **Boss progression**: each manager boss requires its biome Mani; the final boss requires all four — so the gating is *capability*, enforced by what you've learned.
- **Council Scene roster**: every named Husk is forced-purified, so all of them (3 teaching Husks + 2 managers + the head of research = **6 Husk allies**) are guaranteed present at the endgame Council Scene for every player. Variance comes from Grinder Trust Arc completion + Cryo-Survivor recruits + dialogue choices.

### Why this works

- The forced-purification framing makes the mercy theme *load-bearing* — the player can't opt out of compassion
- The org-chart hierarchy gives the world structural coherence — you climb the Accord's chain of command in reverse, ending at the man who caused everything
- Capability-gated bosses make the Mani-learning feel mechanically meaningful (you can't brute-force past a manager without its biome's element)
- The two-tier truth protects the back half (mid-game reveal) AND delivers a genuine climactic bomb (endgame reckoning) — see [[13 Campaign Structure]]

---

## 6. Faction Reputation (high level)

- **Grinders:** Trust arc gates camp access, vendor stock, companion availability. See [[15 Grinder Trust Arc]].
- **Amphibian / Reptile species (purified ally side):** Recruited via story purifications. Each purified named NPC counts toward the species-cooperation endgame path.
- **Rogue Grinder Defector vendor:** Friendly by default. Possibly folds into the main Grinder vendor once trust arc completes.
