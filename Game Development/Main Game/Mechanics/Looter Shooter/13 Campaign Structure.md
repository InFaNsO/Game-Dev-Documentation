
# Campaign Structure

The macro arc of the main game, mapping geography, story acts, progression gates, and the endgame Choice. The prototype covers Lithic Mow and the trust arc only; main game extends to Vats and Forge.

> See [[14 Naming Glossary]] for locked names. [[11 Factions and Species]] for who lives where. [[12 The Akashic and The Bleed]] for cosmic-horror lore. [[15 Grinder Trust Arc]] for the social mechanic that gates the first act.

---

## 1. World Map — The River Spine

The world is a single continent crossed by a single great river that flows from a mountain valley to a desert delta. The river is the spine of the journey; the three Mega Structures are beads on it.

```
        [HEADWATERS]                  ← Player wake site (forgotten Accord research outpost)
              ↓                          Cryo pod, fragmentary memory, brief tutorial
       [LITHIC MOW]                   ← Player base grows here. Grinder camp.
              ↓                          Mines / refines Bhu-Mani.
   ╞══ The Riverwild ══╡              ← Connecting region (forest/rocks)
              ↓                          2 mini-locations
      [GENESIS VATS]                  ← Mid-game. Amphibian Husks.
              ↓                          Refines Jal-Mani + Vayu-Mani.
   ╞══ The Drowned Reach ══╡         ← Connecting region (flooded ruins)
              ↓                          2 mini-locations
      [PRISM FORGE]                   ← Late game. Reptile Husks.
              ↓                          Refines Agni-Mani.
   ╞══ The Glass Delta ══╡            ← Final outdoor region; river dies in desert
              ↓                          1 mini-location (optional Accord prototype site)
            ░░░                       ← End of map. No more eastward travel in this game.
```

### Why this map shape works

- **Single linear spine** = clean progression, easy to communicate
- **3 hubs + connecting regions** = scope-manageable (no Old Capital — see §3)
- **River travel** as visual motif justifies fast-travel cinematics
- **Each hub is further from civilization, deeper into the past** — symbolic arc

### Total authored area count

**5 hub zones + 5 mini-locations = 10 distinct authored areas across the entire main game.**

- 5 hubs: Headwaters · Lithic Mow · Genesis Vats · Prism Forge + the player base (grows at Lithic Mow)
- 5 mini-locations: 2 in Riverwild · 2 in Drowned Reach · 1 in Glass Delta

Each hub uses the [[10 Unity Code Architecture#7 Procedural Level Assembly|procgen room library]] within its biome theme. Mini-locations are small, focused, single-objective.

---

## 2. Connecting Regions Detail

### The Riverwild (Lithic Mow → Genesis Vats)
**Vibe:** Forested / rocky river corridor between the mountain valley and the lake. Bleed-touched wildlife at low intensity. Old Accord transport infrastructure (rusted barges, feeder pipes).

**Mini-locations:**
1. **Pre-Breach research outpost** — Accord cache. Lore reveals + first Mani blueprints reward.
2. **A cryo-pod survivor encounter** — recruit another human ally. First recruitable companion outside the Grinder tribe.

### The Drowned Reach (Genesis Vats → Prism Forge)
**Vibe:** The river exits the Vats lake and flows south. Flooded ruins from before the desert encroached. Half-submerged Accord cities. Mix of water and increasingly arid terrain.

**Mini-locations:**
3. **Submerged Accord city** — major lore drop + Bleed-pooled zone challenge. Possible named-NPC purification site (an Amphibian who fled the Vats during The Breach and got this far).
4. **Second cryo-pod / hidden Grinder shrine** — Grinder lore + reputation event. Reveals the Grinders descended from a specific Accord caste.

### The Glass Delta (Prism Forge → end of map)
**Vibe:** Where the river dies into the desert near Prism Forge. Sand-glassed riverbeds, mirages of pre-Breach figures.

**Mini-location:**
5. **A failed Accord prototype site** — a smaller, earlier refinement structure that never worked. Tutorial for the scale of pre-Breach ambition. Optional.

---

## 3. Progression Gates — capability-based

Each Mega Structure mechanically unlocks the next. Players *feel* the gate as "I need to refine X to get past Y."

| Region | Gated by | Unlocked by |
|---|---|---|
| **Lithic Mow** | (starting area) | — |
| **The Riverwild** | Mild wildlife threat | Player capability |
| **Genesis Vats interior** | **Bhu-Mani gear** (mining tools to clear collapsed entry, earth-shields vs. falling debris) | Lithic Mow reclamation completed |
| **The Drowned Reach** | **Jal-Mani gear** (rebreathers for vapor zones, water-skim boots for flooded passages) | Genesis Vats partial reclamation (Jal-Mani refining online) |
| **Prism Forge approach** | **Vayu-Mani gear** (heat-dispersal cloaks for tower interiors) | Genesis Vats full reclamation |
| **The Glass Delta** | **Agni-Mani gear** (heat-tolerant; sand-glass needs Agni-Mani resonance to crack) | Prism Forge reclamation completed |
| **End of campaign — The Choice** | All three Mega Structures reclaimed; option to continue purifying named NPCs before committing | Player decision |

No invisible walls. Every gate is justified narratively *and* mechanically. The world enforces its own pacing.

---

## 4. Travel Mechanics

**Hub-and-spoke fast travel after first visit.**

- **First trip to each region** = physical traversal of the mini-locations between hubs. Player experiences the world the first time, meets named NPCs, encounters major lore beats.
- **Subsequent trips** = fast travel between unlocked hubs. River-journey cinematic (single shared asset — a barge cutting through water with biome transition montage) plays as a brief transition.
- **Within each hub** = open exploration of the safe zone + procgen raid zones (the looter-shooter loop from the prototype).

### Why this is right for solo dev
- 10 authored zones is achievable vs. 10s of kilometers of seamless world
- Fast travel keeps the "river as spine" feel without forced repetition
- Each zone procgen-assembled from the room library inside its biome theme

---

## 5. Story Acts

Four acts, mapped to geography. Each act = a hub + the connecting region after it.

| Act | Region | Beat |
|---|---|---|
| **Tutorial** | Headwaters cryo pod | Wake. Learn movement, stealth, combat. Find first Bhu-Mani blueprint fragment. Encounter first Grinder. |
| **Act 1 — Trust** | Lithic Mow + Riverwild | Earn Grinder trust ([[15 Grinder Trust Arc]]). Teach refinement. First Bhu-Mani together. Reclaim Lithic Mow. Establish player base. First travel down Riverwild — meet Cryo-Survivor 1. |
| **Act 2 — Revelation** | Genesis Vats | Reclaim the Vats in phases. Refine Jal-Mani, then Vayu-Mani. **First named-NPC purification = conspiracy revealed.** Player worldview shifts. Vendor unlocks. |
| **Act 3 — Conviction** | Drowned Reach + Prism Forge | Travel south. Submerged city deepens lore. Reach the Forge. Reclaim it. Refine Agni-Mani. **Second named-NPC purification** — deeper grief, deeper resolve. |
| **Act 4 — Choice** | Glass Delta + return to player base | Final reflection at the failed Accord prototype site (optional). Return to player base. All allies gathered. **The Choice.** Ending plays. |

---

## 6. The Choice (the endgame)

After all three Mega Structures are reclaimed and the player has lived through the mandatory main-campaign purifications of 2-3 named Amphibians and 2-3 named Reptiles, the player returns to the base for the endgame Choice.

### Framing

**Purification of 2-3 named Husks per species is mandatory main-campaign content** — scripted into the Genesis Vats and Prism Forge arcs as story beats. Every player arrives at endgame with the same context: they have met purified survivors of both species, learned the Mani Accord's secret from them, and built personal relationships. Both endings must therefore always be available — gating the good ending behind optional purifications would punish players who simply played the campaign as designed.

**The Choice is a pure values declaration:** keep doing this work as the foundation of the rebuilt civilization, or stop here and let humanity rebuild alone from now on.

| Choice | What it means in practice |
|---|---|
| **Humans Only** | Stop further purification. Rebuild centered on humans. The handful of already-purified Amphibians and Reptiles can stay as honored guests, but no more rescue missions are launched. The vast majority of remaining Husks stay Husks forever. |
| **Multi-Species Cooperation** | Continue purifications as a generational project. The rebuilt civilization is structurally three-species. Husks across the world are gradually transmuted — mercy at scale. |

Both choices are honest answers to *"what kind of world do we build?"*. The bad ending is not "humans hate other species" — it is "humans decided their survival was enough, and chose not to take on the burden of the other species' rescue." Recognizable real-world ethics, not a strawman villain.

### The Council Scene — full composition

Triggers when player returns to base after Forge reclamation. Single authored sequence at the player base. **All recruited allies present:**

- **Grinders**: Chief, Shaman, Scout
- **Cryo-Survivors**: every recruit from river-journey mini-locations
- **Purified named NPCs**: every Amphibian and Reptile saved during the campaign (4-6 individuals)

Each ally speaks 1-2 lines making their case (~25-35 lines total). Speakers naturally split:

| Tend toward Humans-Only | Tend toward Multi-Species |
|---|---|
| Chief (possibly — pragmatic, tribal-loyalty) | The Shaman (haunted by knowledge of the dead) |
| Cryo-Survivors who lost everyone | The purified named NPCs (most damning case) |
| | Some Cryo-Survivors who saw what cooperation built |

**The purified NPCs are the most powerful voices.** They look at the player and describe specific Husks they personally knew — by name — still walking the Vats and the Forge, who could be saved. Choosing humans-only means looking these allies in the eye and telling them no more.

### Hybrid scene authoring

**Council scene structure is fixed**, but specific allies' dialogue varies based on personal arcs:
- **Named NPCs** (Chief, Shaman, Scout, the first purified Amphibian, the first purified Reptile) get **arc-aware lines** that reference player choices throughout the campaign
- **Generic recruits** (additional Cryo-Survivors, secondary purified NPCs) have **fixed lines** that fit the council shape

This balances replay depth with authoring scope — adds roughly 30-50% more lines than a flat scene but keeps the core structure stable.

### The Declaration

After all speeches, the player makes a public declaration. Two dialogue options:

- **"We rebuild alone. Humanity will rise by its own strength — and answer to no one."**
- **"The Mani Accord's mistake was hiding the truth, not the cooperation. We finish what they should have built."**

The chosen path triggers the ending sequence.

### Ending format — hybrid voiceover + time-lapse + small playable epilogue

Both endings use the same three-part structure. **Same playable area** (the player base, visually altered for each ending) — saves authoring cost while giving the player a final agency moment.

**Part 1 — Voiceover narration over time-lapse imagery** (~1-2 min)
- Narrator (Shaman or player VO) describes the years that follow
- Imagery shows the wide-scope consequences of the Choice

**Part 2 — Small playable epilogue** (~5-10 min)
- Player wakes in the base, much older
- Walks through the transformed (rebuilt or declining) base
- Brief interactions with named NPCs who remain
- No combat; pure exploration and dialogue
- Ends at a symbolic location (new central monument vs. empty council ring)

**Part 3 — Final cinematic shot + closing line**
- Single held shot of the world's final state
- One-line closing voiceover
- Credits

### Humans-Only Ending (bad)

**Voiceover beat**: one human generation flourishes — Lithic Mow drill running at scale, Bhu-Mani in abundance, a brief peak. Then decline: Genesis Vats falls silent without Amphibians, Prism Forge cracks without Reptiles. Husks still walk the Vats and the Forge — unsaved, immortal forever. Bleed-pooled zones never heal.

**Playable epilogue**: player wakes in an emptier base, much older. Walks through a sparser, sadder version. Most generic NPCs are gone (natural causes; non-human allies departed or never fully integrated). A handful of named survivors remain. A purified Amphibian (if they stayed) is silent at a window, looking toward the Vats. A Husk is visible at the edge of the valley, still walking, never released. Player ends at the empty council ring.

**Final shot**: aged player sitting alone where the council once met. Closing line: *"The Mani Accord left us a wound. Humanity sealed itself inside it."* Elegiac music.

### Multi-Species Ending (good)

**Voiceover beat**: slow generational work — Genesis Vats coming back online with Amphibian engineers, Prism Forge running with Reptile heat-workers. Husks across the world gradually transmuted via mass-purification — mercy at scale. Bleed-pooled zones fade slowly. Three species working together: cautious, scarred, but real.

**Playable epilogue**: player wakes in a thriving multi-species rebuilt base, older but well. Walks through a richer, busier version. The Chief is older but proud (or his successor if appropriate). The Shaman is the keeper of records. Purified Amphibians and Reptiles lead their own restoration projects. A new central monument honors all three species. Player ends at a council chamber being built for a new generation.

**Final shot**: three-species council at the new monument under construction. Closing line: *"The Mani Accord left us a wound. We chose, finally, to heal it together."* Hopeful but earned music.

### Subtle sequel hook (both endings)

Each ending closes with a one-line whisper of *"what comes next?"* without committing to anything. The world built (or failed) is the canvas for sequel possibilities — the player can imagine what comes after, and the developer keeps full freedom.

### Save / replay handling

The Choice is irreversible within a save. Save can be reloaded from before the Choice to see both endings. **No anti-cheese mechanics** — players who want both endings deserve both.

### Why this works narratively

- No artifact to chase = the climax is **emotional and ideological**, not collectible
- Both endings are equally earned in effort — distinguished only by conviction
- The bad ending is a *consequence* not a punishment — players see why it fails through the playable epilogue
- The playable epilogue gives the player final agency to *experience* the consequences of their choice rather than just watching them
- The hybrid council scene rewards careful play without gating
- Sequels can pick up either ending and continue (humans-only canon = isolated humanity's last try; multi-species canon = the federation reaching the "next frontier")

---

## 6.5. Mega Structure Interior Design (Topic 8)

Every Mega Structure visit follows a single shared 8-beat structure. Solo-dev authors the rhythm once, then varies content per location. Each structure has **one defining mechanical hook** that differentiates the experience from regular raid zones.

### The 8-beat visit shape

```
1. APPROACH       outdoor area leading to entry. Light combat. Audio-log foreshadowing.
2. ENTRY HUB      hand-authored safe zone. Allies deploy. First major lore drop.
3. EXPLORATION    procgen raid zones, mini-objectives (find audio logs, scout, gather
                  raw Mani, encounter common Husks).
4. PUZZLE GATE    structure's signature mechanic blocks progression. Solving it = first
                  full taste of the structure's identity.
5. INNER SANCTUM  hand-authored zone. Deeper lore. FIRST NAMED HUSK ENCOUNTER —
                  purification reveals conspiracy layer.
6. REACTIVATION   multi-phase sequence: locate → restore → defend (boss is the defense
                  phase) → activate.
7. BLUEPRINT      boss drops refinement blueprint. Cryo-memory flashback on pickup.
                  Recipe unlocked for player base.
8. OUTRO          short post-boss area. Allies regroup. Structure operational. Path to
                  next biome unlocked via newly-refined Mani gear.
```

**Scope target: 5-8 hours of main-path content per structure.** Total main game ~30-45 hours when combined with Trust Arc, connecting regions, and the endgame Choice sequence.

### Locked design dimensions

| Dimension | Lock | Why |
|---|---|---|
| **Puzzle complexity** | Light environmental puzzles, 3-5 per structure | Solo-dev realistic. Makes structures feel distinct from raid zones. |
| **Lore distribution** | Hybrid — audio logs primary + purified-NPC dialogue for emotional landing | Audio logs cheap to produce; named NPCs already locked content. Each method plays to its strength. |
| **Refinement-knowledge gating** | Boss drops blueprint object + cryo-memory flashback on pickup + companion reaction | Refinement feels *earned*. Three small systems each ~1 min of authored content per structure. |
| **Reclamation mechanic** | Multi-phase reactivation sequence (4 phases: locate → restore → defend → activate) | Reclamation feels *engineered*, not just "boss died, structure works again." |
| **Procgen vs authored** | Authored: hub safe zone, boss arena, puzzle rooms, reactivation phase areas. Procgen: regular raid/exploration zones (themed per structure). | Matches existing prototype pattern. |
| **NPC presence** | Locked from prior topics: named purifiable Husks + generic Husk enemies + audio logs as "ghost voices" | No new design — existing systems cover this. |

### Per-structure mechanical hooks

Each structure gets one defining mechanic that scales across the visit.

#### Lithic Mow — Vertical drill reactivation
- **Hook**: vertical descent. Drill is the literal centerpiece. Bhu-Mani-gear required to repair collapsed routes and shore up unstable shafts.
- **Puzzle gate examples**: route around a flooded mine section by reactivating an old pump; shore up an unstable shaft with Bhu-Mani-crafted supports; map a maze of corridor splits using vibration patterns.
- **Inner sanctum**: drill core (where the Demonstration beat happens — locked via Topic 6).
- **Audio logs**: pre-Breach mining operations, Grinder ancestor castes (degraded workers who survived in the valley), early hints that the drill was producing more raw Mani than the Accord publicly reported.
- **Boss (main game)**: Elder Chieftain — a rival Grinder splinter-faction leader who rejects the player's refinement knowledge as heresy. NOT a Husk. Different combat flavor — human enemy with mining-gear loadout.

#### Genesis Vats — Vapor management
- **Hook**: vapor management. Toxic Bleed vapor in corrupted zones (status DoT). Healing vapor in old refinement halls (regen). Player redirects vapor flows via valves to clear paths and create temporary safe zones.
- **Puzzle gate examples**: route healing vapor into a corrupted refinement hall to make it safe enough to enter; cut off vapor flow to a Husk-infested rig to weaken them (no more sustaining radiation in their area); seal a leak that's bleeding contaminated water into a clean section.
- **Inner sanctum**: deep refinement chambers (partially submerged). **First named Amphibian Husk purification → conspiracy revealed.** Audio logs around the inner sanctum start hinting at the Akash doorway research.
- **Boss**: Vatlord — multi-phase, spawns Vat-Brood mid-fight, applies cascading status. Defense phase of reactivation (Vatlord defends the central vapor regulator the player needs to activate).
- **Blueprints**: Jal-Mani in Act 2A (mid-visit), Vayu-Mani in Act 2B (post-boss). Two-phase reclamation matches refining two elements here.
- **Audio logs**: Accord-era Amphibian rituals, vapor refinement protocols, the inner-circle Akash doorway research, fragments of the final day before The Breach.

#### Prism Forge — Light redirection + heat zones
- **Hook**: light/laser redirection (aim mirrors/prisms to crack sealed doors, activate sequences, redirect tower beams). Heat zones (timed exposure, Agni-Mani gear extends safe time).
- **Puzzle gate examples**: redirect three tower beams to crack a Reptile ceremonial seal; align mirrors so a single beam passes through three sequential prisms to ignite a forge; survive a heat zone by hopping between shade pillars using timing.
- **Inner sanctum**: top of a glass tower, sun-peak chamber. **First named Reptile Husk purification → deepest conspiracy reveal** (actual doorway protocols, the Accord's plan to enter Akash, the names of inner-circle members).
- **Boss**: Sun-King — phase 1 = sun pillars (LOS hazards constantly sweeping the arena), phase 2 = burn-everything ultimate. Defense phase of reactivation (Sun-King defends the prime focus lens the player needs to recalibrate).
- **Blueprint dropped**: Agni-Mani refinement.
- **Audio logs**: Accord-era Reptile hierarchy, heat refinement, the doorway experiment in its final days, the names of those who knew vs those who didn't.

### Why this design scales solo-dev

- One 8-beat shape × 3 structures = author the rhythm once, vary content.
- One signature hook per structure = three mechanical systems total, each playable in 5-8 hours of content.
- Audio logs are the cheapest lore vehicle that still feels evocative; reusable across structures.
- Procgen handles the raid-zone bulk; hand-authoring concentrates on the beats players will remember.

---

## 7. Prototype Scope vs Main Game Scope

| System | Prototype | Main Game |
|---|---|---|
| Hubs | 1 (Lithic Mow only) | 3 (+ player base) |
| Mini-locations | 0 | 5 |
| Species enemies | Grinders only | Grinders + Amphibian Husks + Reptile Husks |
| Refined Mani | Bhu only | Bhu / Jal / Vayu / Agni |
| Akash-Mani | (lore only, never appears) | (lore only, never appears) |
| Story arc | Trust arc + Lithic Mow reclamation | Full 4-act campaign |
| Endgame | Tutorial completion / vertical slice end | The Choice + two endings |
| Procgen room library | ~30–50 Lithic Mow rooms across 3 sub-themes | + Genesis Vats theme + Prism Forge theme |

The prototype proves the combat system, procgen room tech, Grinder trust arc, and Bhu-Mani refinement loop. **Every system carries forward into the main game with additive layers only — no rewrites.**

See [[10 Unity Code Architecture#20 Carry-Forward Checklist (Prototype → Main Game)|carry-forward checklist]] in the architecture doc.

---

## 8. Open Questions to Resolve Later

- Exact placement of the named-NPC purification encounters in each biome
- Specific Bhu-Mani / Jal-Mani / Vayu-Mani / Agni-Mani gear pieces (and their gating roles)
- Audio log distribution strategy — how many per region, what they reveal in what order
- Cinematics for: cryo wake, first refinement ceremony, first purification reveal, the Choice itself, both endings
- Music / audio palette per biome
- Cryo-Survivor NPCs — how many, what roles, distinct personalities or generic recruits?
