
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

After all three Mega Structures are reclaimed and the player has had the opportunity to purify named NPCs of each species, the player returns to the base for the endgame Choice.

### The Choice itself

Presented in-fiction as a council with all recruited allies present: Grinders, Cryo-Survivors, and any purified Amphibian / Reptile representatives. The player chooses:

| Choice | Ending | Tone |
|---|---|---|
| **Humans-only — rebuild with our species alone** | **Bad ending.** Refuse to revive the other species. Short-term human flourishing as the Mega Structures are restored for human use only. Long-term: no multi-species refinement chain possible, knowledge stagnates, civilization peaks within one generation and slowly declines. The last human dies alone in a quiet base, surrounded by an empty world. The Bleed outlasts humanity. | Tragic, isolating, "we kept the world for ourselves and lost it." |
| **Multi-species cooperation — rebuild the federation together** | **Good ending.** Continue purifying named NPCs of each species. The three species rebuild together, slowly, learning from the Accord's mistakes. **The lost knowledge of Akash refinement stays lost, by choice.** Civilization heals. The world enters a new era. | Hopeful, earned, "we chose the harder path together." |

### Gating logic

The Choice unlocks once:
- All three Mega Structures reclaimed
- At least one named NPC of each non-human species has been encountered (player has *seen* the option to purify)
- Player returns to base and triggers the council scene

The Choice is **not reversible** within a save. Players can replay to see the other ending.

### Why this works narratively

- No artifact to chase = the climax is **emotional and ideological**, not collectible
- The bad ending is a *consequence* not a punishment — players who choose humans-only see exactly why it fails
- The good ending is *earned*, not handed over — requires actively purifying named NPCs throughout the game
- Sequels can pick up either ending and continue (humans-only canon = sequel about isolated humanity's last try; multi-species canon = sequel about the rebuilt federation reaching the "next frontier")

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
