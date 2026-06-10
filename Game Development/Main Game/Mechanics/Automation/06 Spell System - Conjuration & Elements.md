# Automation — Spell System: Conjuration & Elements

> The signature mechanic in full: **Conjuration Engineering** (Surface A), the **particle tech** that renders it, and the **element matrix** (multi-Mani, full depth). **Locked 2026-06-08**; element matrix confirmed; **module vocabulary + flow model LOCKED 2026-06-10** (see [[09 Machine Framework - Modules, Parts & Production|09]]); exact op lists + numbers firm up at the gray-box.

---

## Conjuration Engineering — the core verb

A spell is a **conjured 3D elemental construct** that you **design by building a process**, not by hand-sculpting. The defining rule:

> **You never place the final shape — you build the machine that produces it.** Modules are **process-operations** (emit / shape / drive), never static shape-bricks. *(This is what keeps it automation and not a voxel builder.)*

Model: **B-paradigm on C-substrate** — the *form emerges from a process* (Modulus-true depth), but the process is built from **discrete, grid-placed, manufacturable modules** (readable, scope-bounded, grid-lock-compliant, and the modules *are* Surface B's manufactured parts).

### The three design axes

| Axis | What you set | Fire Lance | Earth Fist |
|---|---|---|---|
| **Form** (shape) | the conjured 3D object | long pointed shaft | giant fist |
| **Substance** (element) | which Mani fills it | Agni → burn + light | Bhu → blunt + stagger |
| **Behavior** (motion) | what it does | thrust forward, fast | punch, short range |

→ **Fire(substance) + Lance(form) + Thrust(behavior) = a piercing fire projectile.** And it composes with the element matrix: **Steam + Cloud + Expand = a scalding AoE.**

### How a construct is built (the fire lance, concretely)
1. **Emitter** — pulls fire-substance from the Agni gem node (rate = gem size × purity).
2. → **Shaper** (taper) — forms the flowing substance into an elongated point. *The shape comes from the shaper, not from placement.*
3. → **Driver** (thrust, forward, high-speed).
4. **Run it** (test room / combat): substance flows → shapes into a lance → launches. You *watch it produced and fired.*

Because the form is **produced by a process**, novel operation combinations give **emergent results** (a splitter + two drivers → a forked lance volley) — the "plan the process, see what comes out" Modulus joy that hand-sculpting can't give.

### Deriving balanced stats from a player-made form
Stats come from **measurable properties** of the form + motion (so shape *matters* and stays balanceable):
- **Volume / mass** (voxel count) → damage & part-cost
- **Shape profile** → damage type (pointed → pierce · broad → blunt/AoE · edged → slash)
- **Motion** (driver + speed) → delivery (projectile / melee / AoE / barrier) + speed
- **Element** → status & damage flavor · **Purity** → power multiplier
- **Process flow rate** → **cast time** (casting = charging the pipeline; cast time = volume ÷ flow rate — LOCKED 2026-06-10, see `09`). Engineering efficiency is combat-legible.

**Form = function = visual, all one thing.**

### Depth source (the Modulus-fix)
A **tech tree of in-gem operations** — new emitters, **shapers** (new forms), **drivers** (new behaviors: homing, multi-stage, orbit, delayed-detonate), and **multi-element merging** — is the deliberate patch for Modulus's missing depth. **Capstone: mold** — free-form construct shaping (runtime-SDF conform) arrives mid/late tech-tree: the moment spells stop looking like presets and start looking like *yours* (locked `09`). **Gem-scale** (bigger gem = **more nodes, purer nodes, and more internal build-space** → bigger, more-complex, higher-quality constructs) and **market pressure** compound on top. **Gem grade is the *quality* axis and lives entirely in Surface A** — Surface B is quantity/throughput only (see `05`).

### The Test Room
A **proving ground** in the workshop where you **fire the spell and watch the real output** — instant visual feedback. It's the **dopamine** (DSP "watch it work"), the **debugger** (see what your process actually makes), and the **design bench** (iterate before committing to mass production) in one.

---

## Particle tech (validated)

**Approach:** **VFX Graph (GPU) + a small library of preset behavior-templates + runtime-set exposed parameters.** Verified against Unity docs (2026-06-08).

- **Separate simulation from visuals.** A custom **node-graph flow-sim** (logic: nodes, flows, throughput → spell stats) is the **source of truth**. Unity particles are only the **skin**, driven by the sim via `SetFloat/SetGradient/SetMesh/...` on exposed properties.
- **Templates × parameters = thousands of distinct spells from ~6–10 authored graphs.** Behavior picks the template; element sets color + mesh; throughput sets density + size + light; combination-tier adds layers.
- **Two parameter layers:** **functional** (derived from the sim → the spell looks like what it is) + **cosmetic** (optional player flourish → the shop's signature look = brand).
- **Power & cost:** GPU → millions of particles; the real cost is **overdraw**, not count — mitigated by mesh particles + the **stylized art direction** (Duckov/Universim) which keeps overdraw low. Requires URP/HDRP (fine on Unity 6).
- **Hard rule:** GPU particles aren't read back for logic — **the sim is authoritative for all gameplay; VFX is cosmetic only.**
- **Two visual budgets:** the **in-gem process** view favors *readability* (design clearly); the **fired spell** gets the lavish hero VFX.

*(Sources captured at validation: Unity VisualEffect scripting API, VFX Graph Parameters & Events, Performance & Optimization docs.)*

### Deep dive findings (2026-06-10 — the user-directed particle deep dive; grounds the `09` module vocabulary)

Researched against the Unity 6 / VFX Graph 17.x package docs. **The validated architecture survives — and is stronger than assumed:**

- **Runtime SDF baking is officially supported** (`MeshToSDFBaker`, runtime + editor; bake once at *spell-compile* time, output = a 3D-texture parameter). With **Conform/Attract-to-SDF**, a particle cloud can *hold an arbitrary player-built shape* → **the player's process output can literally define the construct's visual** (not just pick a preset). This powers the **mold** capstone. Caveats: resource-intensive (low res ≤64³ if frequent), normalized output (handle scale), must `Dispose()`.
- **GraphicsBuffer sampling** — exposed GraphicsBuffer properties + `[VFXType]` structs let the CPU sim upload arbitrary per-spell point/attribute lists the template reads per-particle. **The standard sim→skin data pipe** (supersedes point caches, which are editor-only).
- **GPU Events chain systems natively** (Trigger Event On Die / Rate → child system inherits position/velocity/color): projectile → explosion → lingering zone is first-class → **multi-stage and split-in-flight Drive ops are confirmed feasible.** (Gameplay forks still live in the CPU sim — default to firing new pooled instances; GPU events for cosmetic debris.) Still behind an "experimental" toggle but long-stable; since 17.1 compatible with instancing.
- **Strips/trails** (lances, beams — bendable via noise/vector-field params), **URP Lit Decal output** (ground zones: magma/mud), **six-way lit smoke**, mesh/skinned-mesh sampling (CPU-readable meshes), turbulence/vector-fields (Texture3D = runtime-swappable) — all available in URP on Unity 6.
- **The hard wall (confirmed absolute):** graph **topology** is compile-time — systems, blocks, wiring, output types, blend modes, **per-system capacity** are fixed; only exposed property *values* change at runtime. Bool/branch params allow 2–4 behavior variants per template. → The **~8-template × parameters** plan sits exactly on the right side of this wall.
- **URP gaps that don't bite:** no distortion output, no per-particle lights (use Output-Event-spawned pooled light prefabs), **mobile unsupported** (→ desktop-only, recorded in `10`). Instancing is on by default — prefer float/vector/gradient per-instance params; reserve texture/SDF swaps for the spell *archetype*, not per-cast.
- **Performance claim confirmed:** overdraw/fillrate (not particle count) is the dominant cost; stylized small/opaque-ish mesh particles genuinely de-risk it; VFX Graph 17 ships a profiling/debug panel.

**The 8 starter render templates (reference set, locked as framework in `09`):** Projectile (quad/mesh + strip trail + homing/orbit branch) · Volley · Beam (strip) · Burst/Detonation (GPU-event chain) · Ground Zone (loop + decal) · **Conjured Construct** (Conform-to-SDF — the signature) · Aura/Channel · Weather/Arena.

**Prototype-first order (for the ① Conjuration Game gray-box):** (1) one Projectile template × 30 parameter presets — does one template read as 30 spells? (the #1 design risk: template *coverage*; mitigated by shape-source params) → (2) `MeshToSDFBaker` → Conform "earth fist" pipeline → (3) GraphicsBuffer pipe from a stub sim → (4) GPU-event chain → (5) 20 pooled instances + profiling panel + overdraw view.

---

## The element matrix (multi-Mani — full depth)

The Looter Shooter capped combinations at 2 elements *(LS scope-limit only)*. Here we go to the **3-Mani ceiling**. With the **4 base elements**, the math is clean and bounded: **4 base + 6 pairs + 4 triples = 14 craftable elements.**

**Naming / design principle (locked):** a compound **fuses its parents into a *new* behavior**, never just intensifies one parent; names are English *common* terms (gameplay-legible, per the Mani / Panchamani common-vs-formal rule).

**Base (4):**
| Element | Flavor / role |
|---|---|
| **Bhu** (Earth) | Stagger / armor-break — control + defense |
| **Jal** (Water) | Freeze / slow — control |
| **Vayu** (Air) | Push / knockback — displacement |
| **Agni** (Fire) | Burn (DoT) — damage |

**Pairs (6):**
| Combo | Result | Behavior |
|---|---|---|
| Agni + Jal | **Steam** | Scalding AoE cloud — burn + obscure |
| Agni + Bhu | **Magma** | Lava terrain + heavy DoT — area denial |
| Agni + Vayu | **Lightning** | Fast chaining burst — high single-target |
| Jal + Bhu | **Mud** | Root + difficult-terrain **zone** — area-denial trap (immobility, vs Magma's damage) |
| Jal + Vayu | **Frost** | **AoE** freeze — the area version of base Jal's single-target freeze; sets up mass-Shatter |
| Bhu + Vayu | **Crystal** | **Wind-driven piercing shards** — ranged pierce |

**Triples (4):**
| Combo | Result | Behavior |
|---|---|---|
| Agni + Jal + Bhu | **Eruption** | Volcanic — massive AoE + lava terrain + DoT |
| Agni + Jal + Vayu | **Tempest** | Weather-scale storm — lightning + rain + wind |
| Agni + Bhu + Vayu | **Detonation** | Single huge explosive burst + shrapnel |
| Jal + Bhu + Vayu | **Blizzard** | Weather-scale freeze + push — hard control |

*Structural mnemonic: each triple **omits one element**, and the omission defines it — Eruption (no Vayu) = grounded · Tempest (no Bhu) = airborne · Detonation (no Jal) = dry blast · Blizzard (no Agni) = pure cold control, no damage.*

**The forbidden 5th — the callback:** the **all-4 mix → Akash, "the new Mani"** — the Accord's secret obsession, **teased in endgame requests but never craftable** ("it never happens"). The crafting ceiling *is* the fiction (see `04`).

**Mechanical basis for combining (ties three systems together):** gear **housings have gem slots** → **slot count = how many element-gems you combine = combination tier** (1 → base · 2 → pairs · 3 → triples). So **gear progression = element-combination capacity**, and Surface A's process merges the multiple gem streams into the compound.

> **Status:** count, **names, and roles LOCKED 2026-06-08** (base unchanged; pairs = Steam / Magma / Lightning / Mud / Frost / Crystal; triples = Eruption / Tempest / Detonation / Blizzard; all-4 = Akash, forbidden). **The matrix doubles as the recipe-research UI** — you unlock cells with RP + analyzed-parent breakthroughs; the forbidden Akash cell sits visibly grayed-out all game (LOCKED 2026-06-10, see `08`). Exact spell *numbers* firm up at the gray-box.

---

## Gear — the spell housing (LOCKED 2026-06-08)

Every spell is sold **housed in gear** — the device a mage casts through. Gear has **two axes**:

**Type** = delivery role + the customer segment it sells to:

| Gear | Delivery role | Combat lane / buyer |
|---|---|---|
| **Wand** | ranged projectile | caster lane / mages |
| **Gauntlet** | rapid close-range / utility | hand-cast / delvers |
| **Sword** | enchanted melee | every-turn melee lane / frontline |
| **Armor** | defensive / reactive (barriers, buffs) | survival / guards |

**Slots (1 / 2 / 3)** = the **combination tier** (locked): how many element-gems the gear holds = base / pair / triple. Each type comes in 1/2/3-slot versions; higher-slot gear costs more to produce and needs the **researched compound recipe** to fill the extra slots.

So **gear type → combat role → market segment**, and **gear slots = element-combination capacity** — a "3-slot Frost wand" is at once a spell, a combat tool, and a market bet. Surface B manufactures the gear (parts + housing). *(Type list is a strong default; staff / amulet / etc. can be added later.)*
