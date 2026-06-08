# Automation — Spell System: Conjuration & Elements

> The signature mechanic in full: **Conjuration Engineering** (Surface A), the **particle tech** that renders it, and the **element matrix** (multi-Mani, full depth). **Locked 2026-06-08**; the element-matrix specifics are a strong proposal pending final confirm; exact operations deferred to pre-prototyping.

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

**Form = function = visual, all one thing.**

### Depth source (the Modulus-fix)
A **tech tree of in-gem operations** — new emitters, **shapers** (new forms), **drivers** (new behaviors: homing, multi-stage, orbit, delayed-detonate), and **multi-element merging** — is the deliberate patch for Modulus's missing depth. **Gem-scale** (bigger gem = **more nodes, purer nodes, and more internal build-space** → bigger, more-complex, higher-quality constructs) and **market pressure** compound on top. **Gem grade is the *quality* axis and lives entirely in Surface A** — Surface B is quantity/throughput only (see `05`).

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

> **Status:** count, **names, and roles LOCKED 2026-06-08** (base unchanged; pairs = Steam / Magma / Lightning / Mud / Frost / Crystal; triples = Eruption / Tempest / Detonation / Blizzard; all-4 = Akash, forbidden). Exact spell *numbers* firm up alongside the particle exploration.

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
