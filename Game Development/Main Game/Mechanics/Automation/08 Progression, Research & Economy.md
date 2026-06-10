# Automation — Progression, Research & Economy

> The systems spine — how a playthrough grows. The **two-currency model**, the **analysis-machine** research engine, and the three growth axes. **Locked 2026-06-08** (relationships; all numbers deferred to pre-prototyping). First output of the **Systems Spec** phase. Builds on `05` (loop), `06` (spells / gear-slots), `07` (market).

---

## The two-currency spine

| Driver | Earned by | Spends on |
|---|---|---|
| **Money** | selling finished spell-gear | the *operating economy* — procure gems, raw materials, machines, gear; pay build costs |
| **Research Points (RP)** | **analysis machines** (below) | the *capability engine* — operations tech-tree, element/compound recipes, machine tiers, expansion-gating |

Plus one **non-spendable progress meter**:
- **Reputation / frontier-tier** — rises with success (sales, contracts won). It **widens what the market offers** (new element gems, bigger/purer gems, higher-slot gear, richer bake-offs). Gates *availability*, never spent. Grounds the frontier-boom escalation (`04`) and answers "why don't I start with all four elements."

## Research = analysis machines (the capability engine)

- **Analysis machines** are a Surface-B machine type that sit on the warehouse floor → they **compete for space with the production line** (another layer of the bounded-warehouse spatial puzzle).
- **They consume FINISHED spell-gear, not raw gems.** The stake is a **forgone sale** — a real, legible opportunity cost (a raw gem is a shrug; a finished product is money you're giving up). On-brand: R&D teardown — you study your own best work.
- **RP yield scales with the product's GRADE** (grade = Surface A quality: gem + design). First-time analysis of a new element / combination / grade = a **breakthrough bonus** — rewards analyzing *variety*, keeps research feeling *discovered* rather than just bought.
- **Aesthetic mirror:** the **mega assembler** *builds* the product; the **analysis machine** *breaks it down* for knowledge. A structural bookend. (Distinct from the Looter Shooter, where research is a manual *minigame* — here it's automated, fitting the pure-machine identity.)

**RP unlocks:**
- the **operations tech-tree** (new emitters / shapers / drivers / merging = spell *depth*; the deliberate Modulus depth-fix)
- **element & compound recipes** — analyzing advances element branches; RP unlocks the **pair/triple recipes** (how you climb the element matrix). Two-layer combination gate: **research = "do I know the recipe," gear slots = "can my gear hold N gems."**
- **machine tiers** (better analyzers, the 2nd/3rd mega assembler)
- **workshop expansion** — *working default:* **research unlocks the tech/right, money pays the build.** (Revisit.)

## Tech-surface structure (LOCKED 2026-06-10)

The tech surface splits in two, and the recipe half gets the distinctive UI:

1. **The Element Matrix IS the recipe-research screen.** The 14-cell matrix (4 base + 6 pairs + 4 triples + the grayed-out forbidden **Akash** cell) is literally the research UI — you **unlock cells**. A cell needs **RP + having analyzed at least one finished product of each parent element** (the breakthrough rule made visible — research stays *discovered*, and the forbidden cell taunts you all game).
2. **A conventional 3-branch tree for everything else:**
   - **Conjuration** — new emitters / shapers / drivers / merge tiers (Surface A depth; **mold** as the capstone, see `09`)
   - **Workshop** — machine tiers, belt speed, analyzer tiers, the 2nd/3rd mega assembler
   - **Expansion** — warehouse floor, higher-slot gear licenses, bigger-gem handling

## The core fork — and why it isn't grind

Every finished good is a **fork: SELL (money now) or ANALYZE (capability later).**
- **Grade ties research to your best work** → the sharp choice is *sell your masterpiece for top money, or study it for top RP.*
- **Emergent pacing (free):** RP rate rises with craft quality, so **capability compounds** — research is naturally slow early (only low-grade goods) and accelerates as you improve.
- **Self-regulating:** operating costs force you to keep *selling*, so you analyze the surplus / a few strategic pieces — you can't grind everything into RP.

## The three growth axes (what "getting better" means)

| Axis | = | Driven by |
|---|---|---|
| **Capability** (what you *can* make) | operations + recipes + gear slots | **RP** (+ the gear-slot gate, locked `06`) |
| **Quality** (how *good*) | gem grade (size / purity) + design | **Surface A** (locked) |
| **Quantity** (how *much*) | warehouse throughput | **Surface B** (locked) |

## Deferred to numbers / pre-prototyping

All RP yields, costs, drop rates, and prices; the **gem-scarcity balance** (gems serve quality + quantity, and research taxes finished output → RP is *expensive*, a pacing lever); the expansion pay-model confirm.
