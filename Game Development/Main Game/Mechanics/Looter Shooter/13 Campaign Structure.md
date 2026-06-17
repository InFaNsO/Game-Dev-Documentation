
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

Each hub is hand-authored within its biome theme. Mini-locations are small, focused, single-objective. *(Combat arenas within and around hubs are procedurally generated for run variety — see §6.5.)*

---

## 2. Connecting Regions Detail

### The Riverwild (Lithic Mow → Genesis Vats)
**Vibe:** Forested / rocky river corridor between the mountain valley and the lake. Roaming hostile scavengers (rival Grinders) and scattered Bleed-pool hazards. Old Accord transport infrastructure (rusted barges, feeder pipes).

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
| **The Riverwild** | Mild scavenger threat + Bleed pools | Player capability |
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
- **Within each hub** = open exploration of the safe zone + raid zones (the looter-shooter loop). *(In the **prototype** these are hand-authored; **main-game** raid zones may use procgen — a dream-game feature.)*

### Why this is right for solo dev
- 10 authored zones is achievable vs. 10s of kilometers of seamless world
- Fast travel keeps the "river as spine" feel without forced repetition
- Prototype zones are **hand-authored**; main-game raid zones can layer in procgen (dream-game dungeon tech) later

---

## 5. Story Acts

Four acts, mapped to geography. Each act = a hub + the connecting region after it. **Player wakes already knowing Bhu-Mani refinement** (cryo-memory) — the campaign is about teaching it back to a forgotten world and then learning the other three (Jal, Vayu, Agni) from the named Husks, before culminating in producing the long-lost fifth (Akash) correctly.

| Act | Region | Beat |
|---|---|---|
| **Tutorial** | Headwaters cryo pod | Wake. Cryo-memory of Bhu-Mani refinement intact. Learn movement, stealth, combat. Encounter first Grinder. |
| **Act 1 — Trust** | Lithic Mow + Riverwild | Earn Grinder trust ([[15 Grinder Trust Arc]]). **Teach Bhu-Mani refinement** to the Shaman during the Drill Assault. Reclaim Lithic Mow. Establish player base. Bhu-Mani gear unlocks river travel. First travel down Riverwild — meet Cryo-Survivor 1. |
| **Act 2 — Revelation** | Genesis Vats | Reclaim Vats. **Vats Husk 1** (entry hub) → teaches **Jal-Mani**. **Vats Husk 2** (inner sanctum) → teaches **Vayu-Mani**; both Teaching Husks believe the *official lie* (an accident while refining) and carry only unease. **Vatlord** (manager boss; requires Jal + Vayu to defeat) → on defeat, attempted purification → **confesses the reality** (it was a doorway experiment) + reveals the **sealed deep chamber's location** beneath the Vats — **then dies before teaching the "how."** This is the mid-game reveal; the back half now plays under dramatic irony. |
| **Act 3 — Conviction** | Drowned Reach + Prism Forge | Travel south (now knowing the chamber exists, but unable to reach or enter it yet). **Forge Husk 1** (entry hub) → teaches **Agni-Mani** (the 4th and final Mani). **Sun-King** (manager boss; requires Agni to defeat) → forced-purified → **drops the Prime Focus Lens**, the apparatus needed to enter and power the sealed Akash chamber. Player now holds both endgame keys. |
| **Act 4 — Reckoning & Choice** | Return to Genesis Vats → player base | With all four Manis (to *reach*) and the Prime Focus Lens (to *enter*), the **sealed deep chamber beneath the Vats — the Breach site — opens.** Descend. **Final boss: the Head of Accord Research** (Amphibian, the boss the managers answered to; requires all four Manis). Forced purification → the **endgame reckoning** (full truth + guardian revelation + the **"how to refine"** Vatlord died before giving). Return to base with him as an ally. **Council Scene → The Choice.** Good path: **Akash Mani produced** at the base. Endings play. |

---

## 6. The Choice (the endgame)

### The endgame sequence (full flow)

1. All three structures reclaimed. Player holds all four elemental Manis (**Bhu, Jal, Vayu, Agni**) AND the **Prime Focus Lens** dropped by Sun-King.
2. The **two-key gate** opens the **sealed deep chamber beneath Genesis Vats — the actual Breach site, ground zero**: all four Manis to *reach* it (the descent unseals), the Prime Focus Lens to *enter and power* it. The player returns to the Vats and descends. (The chamber's location was revealed by Vatlord back in Act 2; only now can the player act on it.)
3. **Final boss: the Head of Accord Research** (Amphibian; the boss Vatlord and Sun-King answered to). Requires all four Manis to defeat — the culmination of everything learned.
4. **Forced purification** → he is restored → the **endgame reckoning** (full truth, the guardian revelation that his Husk spent a thousand years unconsciously guarding against Akash re-refinement, the **"how to refine"** Vatlord died before giving, and the framing of the Choice). See [[12 The Akashic and The Bleed#9 The Endgame: The Head of Research & The Final Reckoning|the reckoning]].
5. He returns to the **player base** as an ally.
6. **Council Scene → The Choice → (good path) Akash Mani production**, all at the base. The three-part endings play.

### Framing

By the time the Choice arrives, the player has **forced-purified five of the six named Husks into allies** (3 teaching Husks + Sun-King + the head of research). **Vatlord is the exception — he died delivering the truth in Act 2.** Every player arrives with the same context: all four Manis learned, the full truth known, the Prime Focus Lens in hand, and — via the head of research's mandatory purification — **the safe Akash-refinement protocols.** The question is never *can* you refine Akash, only *should* you.

**The Choice is a pure values declaration:** rebuild civilization centered on humans alone, or rebuild as a renewed multi-species federation.

| Choice | What it means in practice |
|---|---|
| **Humans Only** | Refuse the multi-species path. The handful of purified Amphibians and Reptiles can stay as honored guests; no more rescue missions. The vast majority of remaining Husks stay Husks forever. **Akash Mani is not produced** — the player rejects the very experiment that destroyed the Accord, even in its corrected form. |
| **Multi-Species Cooperation** | Continue purifications as a generational project. The rebuilt civilization is structurally three-species. Husks across the world are gradually transmuted — mercy at scale. **Akash Mani IS produced**, correctly this time, using all four elemental Manis + multi-species cooperation + the head of research's now-lucid guidance. This is the climactic act of the good ending. |

Both choices are honest answers to *"what kind of world do we build?"*. The bad ending is not "humans hate other species" — it is "humans decided their survival was enough, and chose not to take on the burden of either rescuing the other species OR risking another Breach by re-attempting Akash." Recognizable real-world ethics, not a strawman villain.

### The Akash Mani Production Sequence (good-ending climax)

If the player chooses Multi-Species, the council shifts into a climactic refinement sequence at the player base. All four elemental Manis are brought together. The purified Amphibians and Reptiles operate the protocols only their species can perform. The now-lucid Head of Research guides the procedure he once performed in secret — this time in the open, with cooperation. The player conducts the final refinement. **Akash Mani manifests as a stable jewel** — the act the Accord botched, done right at last — and the rebuilt civilization reaches for the "next frontier."

Because the head of research is a mandatory final boss, the protocols are *always* complete — there is no "tense improvised" variant. The drama lives in the *Choice itself* (will you do it at all?), not in whether you collected the right optional NPC.

### The Council Scene — full composition

Triggers when player returns to base after Forge reclamation. Single authored sequence at the player base. **All recruited allies present:**

- **Grinders**: Chief, Shaman, Scout
- **Cryo-Survivors**: every recruit from river-journey mini-locations
- **Purified named Husks**: 5 (Vats 1, Vats 2, Forge 1, Sun-King, Head of Research) — every player has all five via forced purification. **Vatlord died in Act 2** and is honored by an empty seat.

Each ally speaks 1-2 lines making their case (~25-35 lines total). Speakers naturally split:

| Tend toward Humans-Only | Tend toward Multi-Species |
|---|---|
| Chief (possibly — pragmatic, tribal-loyalty) | The Shaman (haunted by knowledge of the dead) |
| Cryo-Survivors who lost everyone | The purified named NPCs (most damning case) |
| | Some Cryo-Survivors who saw what cooperation built |

**The purified NPCs are the most powerful voices.** They look at the player and describe specific Husks they personally knew — by name — still walking the Vats and the Forge, who could be saved. Choosing humans-only means looking these allies in the eye and telling them no more.

### Hybrid scene authoring

**Council scene structure is fixed**, but specific allies' dialogue varies based on personal arcs:
- **Named NPCs** — Chief, Shaman, Scout, and the **5 purified named Husks** (Vats 1, Vats 2, Forge 1, Sun-King, and the Head of Research) — get **arc-aware lines** referencing player choices throughout the campaign. The Head of Research, as the last and most senior voice, anchors the case for the multi-species path. **Vatlord is absent — he died in Act 2 delivering the truth; an empty seat at the council marks him.**
- **Cryo-Survivors and generic recruits** have **fixed lines** that fit the council shape

All 5 surviving named Husks are always present (forced purification means every player has them as allies); Vatlord's absence is constant. Variance comes from Grinder Trust Arc completion + Cryo-Survivor recruits along the river journey + dialogue choices throughout.

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

**Scope target (current estimate): ~3-4 hours of main-path content per structure.** Total main game ~12-15 hours when combined with Trust Arc, connecting regions, and the endgame Choice sequence. **Final duration will be set during development** — these numbers are a working target, not a hard commitment.

### Locked design dimensions

| Dimension | Lock | Why |
|---|---|---|
| **Puzzle complexity** | Light environmental puzzles, 3-5 per structure | Solo-dev realistic. Makes structures feel distinct from raid zones. |
| **Lore distribution** | Hybrid — audio logs primary + purified-NPC dialogue for emotional landing | Audio logs cheap to produce; named NPCs already locked content. Each method plays to its strength. |
| **Refinement-knowledge gating** | Boss drops blueprint object + cryo-memory flashback on pickup + companion reaction | Refinement feels *earned*. Three small systems each ~1 min of authored content per structure. |
| **Reclamation mechanic** | Multi-phase reactivation sequence (4 phases: locate → restore → defend → activate) | Reclamation feels *engineered*, not just "boss died, structure works again." |
| **Procgen vs authored** | *(MAIN-GAME.)* Authored: hub safe zone, boss arena, puzzle rooms, reactivation phase areas. Procgen: regular raid/exploration zones (themed per structure). | **NOTE (updated 2026-06-10):** the prototype's **explore world is hand-authored**, but its **combat arenas are procedurally generated** (contents within the locked single-screen frame, Encounter-Arena model). Full level/dungeon/raid-zone procgen remains a dream-game / main-game feature. |
| **NPC presence** | Locked from prior topics: named purifiable Husks + generic Husk enemies + audio logs as "ghost voices" | No new design — existing systems cover this. |

### Per-structure mechanical hooks

Each structure gets one defining mechanic that scales across the visit.

#### Lithic Mow — Vertical drill reactivation
- **Hook**: vertical descent. Drill is the literal centerpiece. Bhu-Mani-gear required to repair collapsed routes and shore up unstable shafts.
- **Named Husks**: **none.** Husks don't appear in the Grinder valley. Player already knows Bhu-Mani from cryo-memory (cryo-amnesia preserved this one refinement). The Grinders learn it from the player during the Trust Arc.
- **Puzzle gate examples**: route around a flooded mine section by reactivating an old pump; shore up an unstable shaft with Bhu-Mani-crafted supports; map a maze of corridor splits using vibration patterns.
- **Inner sanctum**: drill core (where the Demonstration beat happens — locked via Topic 6).
- **Audio logs**: pre-Breach mining operations, Grinder ancestor castes (degraded workers who survived in the valley), early hints that the drill was producing more raw Mani than the Accord publicly reported.
- **Boss (main game)**: Elder Chieftain — a rival Grinder splinter-faction leader who rejects the player's refinement knowledge as heresy. NOT a Husk. Different combat flavor — human enemy with mining-gear loadout.

#### Genesis Vats — Vapor management
- **Hook**: vapor management. Toxic Bleed vapor in corrupted zones (status DoT). Healing vapor in old refinement halls (regen). Player redirects vapor flows via valves to clear paths and create temporary safe zones.
- **Named Husks**:
  - **Vats Husk 1** (entry hub, mandatory mini-boss) — teaches **Jal-Mani** refinement. First named-Husk encounter of the entire campaign. Establishes the mini-boss-and-teaching mechanic.
  - **Vats Husk 2** (inner sanctum, mandatory mini-boss) — teaches **Vayu-Mani** refinement. **Does NOT know the truth** — believes The Breach was an accident while refining, repeats the official story with buried unease. (The reveal comes from Vatlord, not here.)
- **Puzzle gate examples**: route healing vapor into a corrupted refinement hall to make it safe enough to enter (using **Jal-Mani** learned from Vats Husk 1); cut off vapor flow to a Husk-infested rig to weaken enemies; seal a leak bleeding contaminated water into a clean section.
- **Inner sanctum**: deep refinement chambers (partially submerged).
- **Boss**: **Vatlord** (the Vats site manager — he *knew* the truth). Multi-phase, spawns Vat-Brood, cascading status. **Requires Jal + Vayu to defeat** (gives no Mani). Defends the central vapor regulator. On defeat, attempted purification → **confesses the reality (doorway experiment) + reveals the sealed chamber's location, then DIES** before teaching the "how." The mid-game reveal; the one named Husk who does not survive; NOT the campaign final boss.
- **Mani knowledge unlocks**: Jal-Mani (via Husk 1, mid-visit) + Vayu-Mani (via Husk 2, end-visit). Both learned from the teaching Husks; Vatlord then tests mastery of both.
- **Endgame note**: the sealed deep chamber below the Vats (the Breach site) is visible but inaccessible on this first visit — foreshadows the return for the final boss.
- **Audio logs**: Accord-era Amphibian rituals, vapor refinement protocols, the inner-circle Akash doorway research, fragments of the final day before The Breach.

#### Prism Forge — Light redirection + heat zones
- **Hook**: light/laser redirection (aim mirrors/prisms to crack sealed doors, activate sequences, redirect tower beams). Heat zones (timed exposure, Agni-Mani gear extends safe time).
- **Puzzle gate examples**: redirect three tower beams to crack a Reptile ceremonial seal; align mirrors so a single beam passes through three sequential prisms to ignite a forge; survive a heat zone by hopping between shade pillars using timing.
- **Named Husk**:
  - **Forge Husk 1** (entry hub, mandatory mini-boss) — teaches **Agni-Mani** refinement (the 4th and final elemental Mani). Does NOT know the truth — same official story, same buried unease. Forced-purified → ally.
- **Inner sanctum**: top of a glass tower, sun-peak chamber.
- **Boss**: **Sun-King** (the Forge site manager — he knew the truth). Phase 1 = sun pillars (LOS hazards constantly sweeping the arena), phase 2 = burn-everything ultimate. **Requires Agni to defeat** (gives no Mani). Defends the **Prime Focus Lens.** Forced-purified → ally; confirms Vatlord's account and **hands over the Prime Focus Lens** — the second endgame key, required to enter and power the sealed Akash chamber beneath the Vats. **NOT the campaign final boss** — that is the Head of Research, fought afterward beneath the Vats.
- **Mani knowledge unlocks**: Agni-Mani (via Forge Husk 1). Sun-King then tests mastery of it. **Key item:** Prime Focus Lens (from Sun-King).
- **Audio logs**: Accord-era Reptile hierarchy, heat refinement, the doorway experiment in its final days, the names of those who knew vs those who didn't.

#### The Breach Site (endgame final-boss arena, beneath Genesis Vats)
- **Access (two-key gate)**: **all four elemental Manis** to *reach* it (the descent unseals) + the **Prime Focus Lens from Sun-King** to *enter and power* it. Location revealed by Vatlord in Act 2; requires a return trip to the Vats once both keys are held.
- **Hook**: ground zero — the chamber where Akash was originally refined and The Breach occurred. Heaviest Bleed contamination in the game; the environmental shader/audio presence peaks here.
- **Final boss**: **Head of Accord Research** (Amphibian; requires all four Manis to defeat). Forced purification → the endgame reckoning. See §6 and [[12 The Akashic and The Bleed#9 The Endgame: The Head of Research & The Final Reckoning|the reckoning]].
- **No new biome cost**: reuses the Vats environment + one authored boss arena; not a fourth hub.

### Why this design scales solo-dev

- One 8-beat shape × 3 structures = author the rhythm once, vary content.
- One signature hook per structure = three mechanical systems total, each playable in ~3-4 hours of content.
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
| Combat arenas | Lithic Mow themed, procgen within the locked frame | + Vats / Forge themed arenas; full procgen dungeons = dream-game only |

The prototype proves the **combat system, the loot + crafting (Mani-economy) loop, the Grinder trust arc, and Bhu-Mani refinement** — plus **constrained single-screen arena procgen** (the explore world stays hand-authored; full level/dungeon procgen is a dream-game feature). **Every system carries forward into the main game with additive layers only — no rewrites.**

Every system carries forward into the main game with additive layers only — no rewrites (the carry-forward discipline; see [[Games/1a Last Rite - Code Architecture]] for the concrete extraction-ready seams).

---

## 8. Open Questions to Resolve Later

- Exact placement of the named-NPC purification encounters in each biome
- Specific Bhu-Mani / Jal-Mani / Vayu-Mani / Agni-Mani gear pieces (and their gating roles)
- Audio log distribution strategy — how many per region, what they reveal in what order
- Cinematics for: cryo wake, first refinement ceremony, first purification reveal, the Choice itself, both endings
- Music / audio palette per biome
- Cryo-Survivor NPCs — how many, what roles, distinct personalities or generic recruits?
