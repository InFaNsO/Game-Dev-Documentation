# Game Concepts Hub

> One concept doc per shippable game in the portfolio: **elevator pitch · setting · player motivation · scope**, viewed through the **$5–10 Steam** lens. The dev order + system decomposition live in [[07 Testbed Build Plan]]; the pillar *mechanics* live in `Mechanics/<Pillar>/`. **This folder = the creative concept of each game as a product.**

## Shared principles (apply to every game)

- **Same universe, different corner.** Every game lives in the **Mani world** — callbacks, never hard plot-links. This is what lets the one-rig art, the corruption shader, Mani VFX, enemy meshes, and tech flow between games.
- **Era is chosen for reuse.** Each game sits in whichever era pays off most: combat games = **post-Breach** (Husks/Grinders/ruins → feed the Looter Shooter); automation = **pre-Breach Accord golden age**; city = its own fresh locale; dream = a later era.
- **$5–10 means mechanic-forward.** A sentence of setup, a few lines per beat — no campaign. **Replayability is the value, not first-clear length.** Structure (boss-rush ranking, roguelike runs, diegetic NG+ loops) carries playtime, not content volume.
- **Scope discipline:** thin story, one rig + accessory/shader reuse, the expensive budget is *moveset/animation + bespoke systems*, not art.

## The 10 games (dev order)

| # | Game (working title) | Pillar | One-line pitch | Concept |
|---|---|---|---|---|
| 1 | **Parry Combat — "Last Rite"** | Combat | Descend a post-Breach ruin; parry-purify its corrupted guardians to claim a Husk's secret of immortality — then be reborn at the top, harder and madder, each cycle. | ✅ **LOCKED** — [[1 Parry Combat - Last Rite]] |
| 2 | **Tactical Combat — "Salvage Run"** *(WT)* | Combat | A lone scavenger fights through procgen single-screen grid arenas to extract from the dead zone; die-and-retry roguelike. | ⬜ TODO |
| 3 | **Looting & Crafting** *(WT)* | Looter | Loot raw Mani, run the refine + research minigames, manage inventory + a vendor economy. | ⬜ TODO |
| ▶4 | **The Looter Shooter** | Integration | The flagship: cryo-survivor at Lithic Mow, the Grinder trust arc, full combat + loot + story. | ✅ designed in `Mechanics/Looter Shooter/` (product-concept polish: TODO) |
| 5 | **The Conjuration Game** | Automation | Zachlike commission-puzzle: build the in-gem conjuration process to fulfil spell-spec contracts, scored on fit/cost/cast-time/efficiency. | ◐ pitch in `Mechanics/Automation/11`; setting/motivation pass: TODO |
| ▶6 | **The Spell-Shop** | Integration | Design spells, mass-produce them, and out-compete rival AI workshops in a living market. | ✅ designed in `Mechanics/Automation/` (product-concept polish: TODO) |
| 7 | **Mani Logistics Game** *(WT)* | City | An Anno route-network with abstract demand: build the Vayu-freight web to keep demand nodes fed. | ⬜ TODO |
| 8 | **Cozy Mani-Town Builder** *(WT)* | City | Intimate, household-granular town planning: needs + desirability + Mani-as-infrastructure. | ⬜ TODO |
| ▶9 | **The City Builder** | Integration | The city fusion: cozy towns + multi-biome network + era staircase + Akash Synthesis. | ✅ designed in `Mechanics/City Builder/` (product-concept polish: TODO) |
| ▶10 | **The Dream Game** | Final fusion | The immortal ruler, the Triangle's portal-dungeon islands, the dual-city roguelike. | ✅ pitch in `Main Game/04 Game Pitch` |

✅ done · ◐ partial · ⬜ to do · ▶ integration/fusion (its design already lives in the pillar folder; only the product-concept framing is pending).

## ⚑ TODO — finish the remaining concepts

Run the **same pass we did for Parry Combat** (setting · player motivation · the replayability engine · the scope/length model · callbacks) for each game that still needs it. Priority = the **proving games** (their identity isn't locked yet), then a light product-concept polish on the integrations.

- [x] **1 — Parry Combat ("Last Rite")** ✅
- [ ] **2 — Tactical Combat** — settle the setting/motivation (working: scavenger extraction roguelike), the run/meta structure, the reward.
- [ ] **3 — Looting & Crafting** — the hardest "what's the fun loop as a *standalone*" question (it's the least self-evident as a $5–10 game); setting + motivation + replay.
- [ ] **5 — The Conjuration Game** — wrap the Zachlike commission puzzle in a setting + motivation + the campaign/sandbox/replay structure.
- [ ] **7 — Mani Logistics Game** — setting + motivation for an abstract-demand network game; how it stays "a game, not a spreadsheet."
- [ ] **8 — Cozy Mani-Town Builder** — setting + motivation + the cozy hook that keeps it distinct from the fusion.
- [ ] *(light)* **4 / 6 / 9 / 10 integrations** — product-concept polish only; the full designs already exist in the pillar folders.

> Method reminder: decide **setting + motivation first**, then **scope it** (the replay engine + length model), then lock. Keep each game a complete, replayable $5–10 product with thin story + heavy reuse.
