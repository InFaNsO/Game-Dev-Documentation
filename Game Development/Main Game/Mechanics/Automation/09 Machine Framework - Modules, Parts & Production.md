# Automation — Machine Framework: Modules, Parts & Production

> The machine layer of both surfaces — the **keystone coupling**, the **Surface A module vocabulary + flow model**, and **Surface B's machines, transport, and blueprint→production pipeline**. **LOCKED 2026-06-10** (framework + reference vocabulary; the *exact* op list, part catalog, and all numbers firm up at the gray-box, per the defer-line). Grounded in the **particle-system deep dive** (findings in [[06 Spell System - Conjuration & Elements]]).

---

## The keystone — modules ARE parts (LOCKED)

The in-gem **operation-modules (Surface A) are the physical parts Surface B manufactures.** One unified part vocabulary:

- A **finished spell-gear = gear housing + gem(s) + an embedded conjuration-process** — a miniature machine, built from modules, that conjures & launches the construct when cast.
- Surface A **arranges** modules once → a design's **blueprint = its bill of materials (BOM)**.
- Surface B **mass-produces** the module-parts + housing and assembles each unit at the mega assembler.
- The A↔B synergy is **physical**: a fancier spell needs more/rarer modules → more Surface-B production. **Generic parts ↔ basic modules** (conduits, frames, core-casings); **specialized parts ↔ advanced modules** (element-specific shapers/drivers).

---

## Surface A — the module vocabulary (A1, LOCKED)

**Five operation categories** (staging/conditions live inside Drive — no 6th category):

| Category | Role | Reference starter ops | Feasibility (per the particle dive) |
|---|---|---|---|
| **Emit** | pull substance from a gem node (rate = size × purity) | per-element emitter · pulse emitter | cheap |
| **Shape** | form the flowing substance | taper · broaden · hollow · edge · ring · compress · **mold** (advanced — free-form construct shapes) | parametric = cheap; mold = moderate, **signature** |
| **Drive** | motion & behavior | thrust · arc · orbit · homing · **stage** (delayed detonate, on-impact spawn — GPU-event chains) | all confirmed feasible |
| **Split/Route** | fork a flow | splitter (volley) · router (multi-part constructs) | cheap |
| **Merge** | combine flows | same-element merge (bigger construct) · **compound merge** (recipe-gated, see A3) | sim-side, trivial |

- **Mold is the mid/late tech-tree capstone** — free-form construct shaping (runtime-SDF conform): the moment spells stop looking like presets and start looking like *yours*.
- The **~8 render templates** (Projectile · Volley · Beam · Burst/Detonation · Ground Zone · Conjured Construct · Aura/Channel · Weather/Arena) are the *visual* vocabulary the sim projects onto — see `06`. Rough behavior mapping: thrust→Projectile · split→Volley · sustained→Beam · stage→Burst · zone-drives→Ground Zone · mold→Construct · weather-scale triples→Weather.
- The tech tree's **Conjuration branch** content = new shapers + drivers (see `08`).

## A2 — Flow & connection model (LOCKED)

- Modules are **grid-placed inside the gem** (cross-portfolio grid lock), connected by **conduits** — which are themselves a manufactured generic part (the keystone at work).
- Substance moves as a **continuous flow with rates**: a gem node emits X substance/sec (size × purity); each module has a throughput cap; the slowest stage is the bottleneck.
- **Casting = charging the pipeline until the construct is complete, then launch.** Therefore:

> **Cast time is a DERIVED stat = construct volume ÷ process flow rate.** (LOCKED 2026-06-10)

Process efficiency becomes combat-legible: a bottlenecked process casts slow; a purer gem casts the same construct faster; a well-engineered process beats a sloppy one. This is the fourth derived stat (joins damage / damage-type / delivery in `06`) and gives the Modulus "optimize the process" joy a real payoff beyond part-cost.

## A3 — Multi-element merging (LOCKED)

- A compound spell = one Emit chain **per element gem**, joined at a **Merge module**.
- **Recipe-gated only:** have the researched recipe (`08`) + the gear slots (`06`) → the merge just works; the recipe fixes ratios internally. Compounds are a **progression unlock, not an engineering puzzle**.
- *Considered & rejected:* a flow-balancing puzzle (match input rates or purity drops) — rejected for sim simplicity. Noted as a possible later **additive** (with researchable stabilizer modules as the QoL counterpart) if compounds feel flat in playtest.

---

## Surface B — machine taxonomy (B1, LOCKED)

A bounded set (~6 types), per the curated-catalog leash in `05`:

| Machine | Role | Consumes |
|---|---|---|
| **Fabricator** | generic backbone parts (conduits, frames, core-casings) | generic raw materials |
| **Infuser** | specialized per-element parts, tier-1 (heat-cores, chill-lattices, …) | generic parts + **per-element raw materials** |
| **Assembler** | tier-2 specialized parts (consumes tier-1 → the "shallow-but-real chains") | tier-1 specialized parts |
| **Mega Assembler** | the hero — final spell-gear (housing + gem + module set per blueprint) | the blueprint's BOM |
| **Analysis Machine** | research (locked, `08`) | finished spell-gear |
| **Belts + arms** | transport (B2) | — |

**Per-element raw materials (LOCKED):** specialized parts require **element-flavored raws bought on the market** (~4, one per element — reference names, placeholder: Emberstone/Agni · Chillglass/Jal · Galesilver/Vayu · Loamiron/Bhu). This couples B procurement to the **specialize-deep vs spread-wide** strategy and gives the market more surface.

## B2 — Transport (LOCKED)

**Discrete item belts + simple feeder arms**, grid-placed, skinned as **Vayu-driven conveyor rails** (cozy magitech; ties transport to the element fiction like the city pillar's Vayu-freight). No trains, no bots — the warehouse is one room; space-constrained routing is the difficulty (per `05`).

## B3 — Blueprint → production (LOCKED)

Finishing a design in Surface A produces a **blueprint = bill of materials** (module list + gear housing + gem spec). Queue it on the **mega assembler**, which pulls the BOM from belts/storage and **stalls if any part runs dry** (the coupling's teeth, locked `05`). Design choices in A literally re-shape B's production demands — the keystone made playable.

---

## Defer-line (unchanged)

The framework above is locked **on paper**. Firm up at the gray-box: the exact op list per category, the exact part catalog (~15–25), module footprints/throughputs, template parameter surfaces, and all numbers.
