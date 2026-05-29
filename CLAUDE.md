# Project: 2026 Gaming Vault

Obsidian vault for YouTube content planning and game design. The flagship work is the design of a "dream game" and its first shippable pillar, the **Looter Shooter** (`Game Development/Main Game/Mechanics/Looter Shooter/`).

This file is the shared, version-controlled brain across machines (home PC + office laptop). It syncs via git. The two sections below are maintained by Claude.

---

## Current Plan of Action

> Next few goals we're working toward. Claude keeps this current — update/check off as work lands.

**Topics already locked (this design pass):**
- [x] **Topic 1** — Naming (Sanskrit-rooted: Mani / Panchamani / Mani Accord / The Breach / The Akashic / The Bleed)
- [x] **Topic 2** — Species design (3 species — Humans, Amphibians, Reptiles — Duckov/Universim art, 1 minimal rig + 3 meshes + corruption shader)
- [x] **Topic 3** — Enemy roster (Grinders 4 archetypes + 3 Amphibian Husk + 3 Reptile Husk + 3 bosses; mini-bosses-per-biome question deferred to Topic 10)
- [x] **Topic 4** — The Akashic / The Bleed lore (conspiracy mechanism, historical residue not active threat, ambiguous Akash inhabitants)
- [x] **Topic 5** — River journey (3 hubs + player base, no Old Capital, hub-and-spoke fast travel after first visit)
- [x] **Topic 6** — Grinder Trust Arc (3 compressed phases, Drill Assault as climactic sequence carrying both trust-tier shifts)
- [x] **Topic 7** — The Endgame Choice (pure values declaration, both endings always available, full council scene with hybrid authoring, 3-part hybrid endings with playable epilogue)
- [x] **Topic 8** — Mega Structure interiors (8-beat visit shape, multi-phase reactivation, per-structure signature mechanics: drill descent / vapor management / light + heat)

**Topics still queued:**
- [ ] **Topic 9** — Wildlife scope (keep / cut / fold into Husks only — likely merges into Topic 8)
- [ ] **Topic 10** — Revisit mini-bosses per biome (deferred from Topic 3 — Duckov-style multi-boss setpieces per biome)

---

## Claude Memory

> Curated, human-readable project context plus a digest of recent changes. `git log` is the authoritative change record; this section is the fast cross-machine handoff summary. Claude reconciles this at session start (the `SessionStart` hook surfaces recent commits via `git log --oneline -n 15`) and whenever a meaningful change lands. **After Claude updates this file, commit & push so the other machine sees it.**

### Project context

- **Development strategy — ship one pillar first, then carry forward.** The dream game is a four-pillar fusion: roguelike storytelling + city builder + automation + looter-shooter/dungeon-crawler (`Main Game/01 Concept & Idea.md`, `04 Game Pitch.md`). Plan: build the hardest, most self-contained pillar — the Looter Shooter — as its own polished standalone game first, then fold its tech into the main game with **additive changes only, no rewrites** (`Looter Shooter/10 Unity Code Architecture.md` §22 Carry-Forward Checklist). Rationale: procgen room assembly + the Sparks of Hope × XCOM × Expedition 33 tactical combat model are the riskiest unknowns; the standalone de-risks them and ships a real product.

- **Scope firewall.** City-building, automation, party combat, other biomes (Genesis Vats / Prism Forge), and the endgame Choice are explicitly scope-cut from the prototype (`Looter Shooter/08 Standalone Prototype - Design.md` §10). Don't pull them into prototype work. Features are tagged MVP / V1 / Stretch (`09 Complete Feature List.md`) — honor that prioritization.

- **Tech stack.** Unity 6 + C#, single-player, single project. Data-driven (ScriptableObjects) + decoupled (service locator, typed event bus, state machines). Likely solo/very small indie; no multiplayer.

- **Locked design — substance & naming (Topic 1).** Mani (common, all gameplay UI) / Panchamani (formal — "Five-Jewel", in lore / audio logs). Federation = **The Mani Accord** (transactional, not utopian — Sanskrit "Accord" = signed treaty). Catastrophe = **The Breach** (lasted only seconds). Source dimension = **Akash**. Corrupting force = **The Akashic** (formal) / **The Bleed** (survivor speech). Refined elemental forms: Bhu-Mani (Earth, Lithic Mow) · Jal-Mani (Water, Vats) · Vayu-Mani (Air, Vats) · Agni-Mani (Fire, Forge) · Akash-Mani (Ether — destroyed in The Breach, lore-only, NEVER refined in this campaign). See `14 Naming Glossary.md`.

- **Locked design — cosmic horror mechanism (Topic 4).** The Mani Accord *secretly* tried to refine Akash-Mani as a doorway to the Akash dimension. Doorway opened. Something on the other side pushed back. Doorway collapsed inward, original Akash-Mani destroyed in the same instant. Interdimensional radiation (The Akashic / The Bleed) leaked through for the few seconds the Breach was open, then The Breach sealed itself. Bio-attuned Amphibians (Vats) and Reptiles (Forge) were corrupted into **immortal Husks** — they have been wandering for millennia. **The truth is hidden from the working castes and from the player at game start** — revealed at the first named-NPC purification. **No active threat in the player's timeline; all horror is historical residue.** See `12 The Akashic and The Bleed.md`.

- **Locked design — species & art direction (Topic 2).** Three species: Humans, Amphibians (pear-shape, slimy shader), Reptiles (lean, procedural tail, head frill, scaly shader). Art style = **Escape from Duckov / Universim** stylized simplicity. **One minimal humanoid rig (~8-10 bones)** shared across all three. Three base meshes. Single parameterized corruption shader (Jal blue for Amphibian Husks, Agni orange for Reptile Husks). Archetypes via equipment swaps + color palettes + bolt-on accessories — never new rigs. ~6 shared animations cover the entire enemy AND ally pipeline. See `11 Factions and Species.md`.

- **Locked design — enemy roster (Topic 3).** 10 enemy types + 3 story bosses total across prototype + main game. **Grinders** (humans, primary prototype faction): Scout (MVP), Driller (MVP), Chief (V1), Shaman (V1), Elder Chieftain (boss, main game). **Amphibian Husks**: Vat-Brood, Tidefall, Choir, Vatlord (boss). **Reptile Husks**: Cinder-Stalker, Ember-Speaker, Forge-Sentinel, Sun-King (boss). Bosses = same base mesh at 1.5× scale + unique accessories. Mini-bosses-per-biome (Duckov-style elite spawns) is an open question — deferred to Topic 10.

- **Locked design — campaign shape (Topic 5).** Single linear river spine. Headwaters cryo wake site → **Lithic Mow** (Bhu, prototype + Act 1) → **The Riverwild** (connecting region, 2 mini-locations) → **Genesis Vats** (Jal + Vayu, Act 2 — first named-NPC purification reveals the conspiracy) → **The Drowned Reach** (connecting region, 2 mini-locations) → **Prism Forge** (Agni, Act 3) → **The Glass Delta** (connecting region, 1 mini-location) → return to base → The Choice. **No Old Capital location** (originally proposed, dropped — original Akash-Mani is destroyed, no artifact to recover). 5 hub zones + 5 mini-locations = 10 distinct authored areas total. Hub-and-spoke fast travel after first visit. Player base grows at Lithic Mow alongside the Grinders. See `13 Campaign Structure.md`.

- **Locked design — combat model.** Sparks of Hope (free-form movement within visible dome, 2 actions per turn, NO movement after weapon fire) × XCOM (cover + flanking preserved via anchor facing cones, overwatch via continuous LOS) × Expedition 33 (parry/dodge reactive layer, intent telegraphs above every enemy). **Deterministic damage** — shots always land, cover reduces damage, no hit%. RNG lives only in raw Mani Effect Table rolls (Burn/Freeze/Push/Honey/Vamp/Ink/Suppressed/Stagger + rare effects) and status proc chances. **Raw Mani = unstructured chaotic potential (random effects). Refined Mani = structured, predictable, power scales with crystal size.** See `04 List of things required.md` and `10 Unity Code Architecture.md`.

- **Locked design — Grinder Trust Arc (Topic 6).** Compressed 3-phase arc with 3 named NPCs (Chief, Shaman, Scout). **Phase 1: Save the Scout** → Wary tier. **Phase 2: Prove and Prepare** (mini-quests, Shaman bonds) → still Wary. **Phase 3: The Drill Assault** — joint operation, mid-assault Demonstration beat (player demonstrates Bhu-Mani refinement on Accord equipment found in the drill) shifts tier to Allied, end-assault Mass Production beat shifts to Tribal Kin. **The Drill Assault is the prototype's load-bearing climactic sequence** — combat + revelation + mechanical unlock + social transformation in one converging operation. See `15 Grinder Trust Arc.md`.

- **Locked design — The Endgame Choice (Topic 7).** Pure values declaration, both endings always available. **2-3 named Husk purifications per species are MANDATORY main-campaign content** (built into Vats and Forge story beats) — no gating prerequisites. **Bad ending (Humans Only)** reframed: "we decided our survival was enough" — recognizable real-world ethics, not strawman villainy. **Council scene**: full composition, every recruited ally speaks (~25-35 lines). **Hybrid authoring**: fixed structure + arc-aware lines for named NPCs (Chief, Shaman, Scout, first-purified Amphibian, first-purified Reptile). **Hybrid endings format**: voiceover + time-lapse cinematic → small playable epilogue in transformed base (~5-10 min, no combat) → final cinematic shot + closing line. Same base layout reused for both epilogues, visually altered for scope. Save/reload before Choice allowed. See `13 Campaign Structure.md` §6.

- **Locked design — Mega Structure interiors (Topic 8).** Every structure visit follows a shared **8-beat shape**: Approach → Entry Hub → Exploration → Puzzle Gate → Inner Sanctum → Reactivation (multi-phase: locate → restore → defend → activate, boss is the defense phase) → Blueprint pickup → Outro. **Per-structure signature mechanics**: Lithic Mow = vertical drill descent + reactivation (Bhu-Mani gear); Genesis Vats = vapor management (toxic vs healing vapor flows, valve redirection); Prism Forge = light/laser redirection + heat zones (Agni-Mani gear extends safe exposure). **Lore distribution hybrid**: audio logs primary + purified-NPC dialogue for emotional landing. **Refinement-knowledge gating**: boss drops blueprint + cryo-memory flashback on pickup + companion reaction. **Light environmental puzzles** (3-5 per structure, using the signature mechanic). **Authored sections**: hub, boss arena, puzzles, reactivation phases, inner sanctum. **Procgen sections**: regular raid/exploration zones. Scope target: 5-8 hours of main-path content per structure. See `13 Campaign Structure.md` §6.5 and `05 Mega Structures - Game Worlds.md`.

- **Looter-shooter doc structure.** Numbered files 00–15 in `Looter Shooter/`. `00 Looter Shooter.md` is the hub. Lore: 11 Factions, 12 Akashic/Bleed, 13 Campaign, 14 Naming Glossary, 15 Grinder Trust Arc. Implementation: 08 Prototype Design, 09 Feature List, 10 Unity Architecture. `06 Element in Looter Shooter.md` is intentionally kept-named (filename preserved for Obsidian link stability) but its *content* is now the Mani System doc.

- **"Topic N" numbering.** The user refers to planning sessions by "Topic N" (a session sequence, NOT filenames). Topics 1–7 are now locked and documented. Topics 8–10 remain queued (see Plan of Action above).

### Recent changes (digest)

- **2026-05-29** — Topic 8 locked (this commit): Mega Structure interiors. 8-beat visit shape shared across all three structures. Multi-phase reactivation sequence (locate → restore → defend → activate). Per-structure signature mechanics: Lithic Mow drill descent, Vats vapor management, Forge light redirection + heat zones. Lore distribution: audio logs primary + purified-NPC dialogue for emotional landing. Refinement-knowledge gating: boss drops blueprint + cryo-memory flashback on pickup. Light environmental puzzles (3-5 per structure). Authored hub/boss/puzzle/reactivation sections + procgen exploration zones. Scope target: 5-8 hours per structure. Updates to `13 Campaign Structure.md` §6.5, `05 Mega Structures - Game Worlds.md`, and `04 List of things required.md`.

- **2026-05-29** — Topic 7 locked (commit `d3638c7`): The Endgame Choice. Purification of 2-3 named Husks per species made MANDATORY main-campaign content; both endings always available. Bad ending reframed as "we decided our survival was enough." Full council scene with all recruited allies; hybrid authoring (fixed structure + arc-aware named NPC lines). 3-part hybrid endings: voiceover+timelapse → small playable epilogue → final cinematic. Same base layout reused for both epilogues, visually altered.

- **2026-05-29** — Topic 6 locked (commit `99da80a`): Grinder Trust Arc. Compressed from 5 phases to 3 (Save the Scout → Prove and Prepare → The Drill Assault). 3 named Grinder NPCs (Chief, Shaman, Scout). First Bhu-Mani refinement ceremony happens AT the Lithic Mow drill site during the joint reclamation assault — both trust-tier shifts (Wary→Allied, Allied→Tribal Kin) happen inside this single climactic sequence.

- **2026-05-29** — Major lore lock batch (commit `d2b9ad4`): Full Looter Shooter design locked across 16 files. Sanskrit naming system, The Mani Accord, The Breach, The Akashic/Bleed two-tier naming, conspiracy mechanism (Accord secretly tried doorway to Akash dimension), 3 species design with Duckov/Universim art direction (1 rig + 3 meshes + corruption shader), 10 enemy archetypes + 3 bosses, campaign shape (3 hubs + Choice, no Old Capital), Sparks of Hope × XCOM × Expedition 33 combat model. New comprehensive lore docs added (11-15). Existing docs (00-07) rewritten with locked lore.

- **2026-05-29** — CLAUDE.md and `.claude/settings.json` (this commit): Created CLAUDE.md as cross-machine shared brain; configured `SessionStart` hook to surface recent commits at session start for memory reconciliation. Both Topic 6 and Topic 7 captured in this file.
