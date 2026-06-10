# Automation — Core Mechanics & Loop

> The mechanical skeleton: the **two automation surfaces**, the **core loop**, and the resolved supporting decisions. **Locked 2026-06-08** (exact operations/belts deferred to pre-prototyping). Spell-design detail lives in `06`; market/competitor in `07`.

---

## The two surfaces (the load-bearing structure)

The game has **two distinct automation surfaces**. Keeping them separate is what avoids The Good Company's hated worker/machine tangle — and they're coupled by a **shared part vocabulary**, so it's one material economy, not two games.

### Surface A — Spell Design ("Conjuration Engineering") — *deep, signature*
You build a **modular process on the gem grid** that **produces and launches a 3D elemental construct**. The form comes from **shapers (process-operations), never hand-placement**. Full detail in `06`. *This is the #1 gray-box.*

### Surface B — Workshop Production — *the "regular automation" layer*
A belt/machine line that **manufactures the parts + gear** that Surface A consumes. **Its throughput = how much you can put on the market** — it's the production bottleneck and the main scaling lever. **LOCKED 2026-06-08: fully MACHINE-automated — one production system, no human/staff layer** (cleanest reading of The Good Company anti-lesson; the artisan fantasy is delivered in Surface A, where *you* are the master conjuration-engineer).

**The coupling:** Surface A *arranges* parts; Surface B *manufactures* them. A fancier spell (more / rarer parts) costs more production → lower throughput unless you scale B. One economy, two views.

**Scope leash for B:** a **curated, bounded part catalog** (~15–25 part types, not Factorio's hundreds) and **shallow-but-real chains**. Side benefit: A handles **continuous particle flow**, B handles **discrete item belts** — mechanically distinct, so they never blur. B is the comfort-familiar layer; A is the exotic signature.

### Surface B — locked design (2026-06-08)

**Human vs machine → fully MACHINE.** One automated production system (no worker/staff sim → cleanest adherence to the no-overlap anti-lesson, no agent-sim cost, and A's "continuous particle flow" stays mechanically distinct from B's "discrete item belts"). Pure-machine's only weakness — no payoff moment — is solved by the **mega assembler**, not by an artisan layer.

**The mega assembler — the hero node + the payoff.** The line terminates in a **mega assembler** that *visibly builds each finished spell-gear in real time* (the DSP "watch it take shape" dopamine). It is deliberately **oversized relative to the tiny spell-focus it outputs** — an elaborate magitech contraption making a delicate little wand (on-brand cozy-automation charm).
- **Feeding it is the puzzle.** The assembler is hungry; routing parts to it across a cramped warehouse is the core spatial problem, and it **stalls if any required part runs dry** — that stall is the coupling's teeth.
- **Topology / scaling lever:** **one upgradeable hero assembler early** (centerpiece + forces the feeding puzzle), with a **2nd/3rd unlockable late** as the "Grow" lever once the warehouse expands.
- **Spectacle scales gracefully:** early = slow, you watch each build; late = fast / many → it becomes an ambient "the factory is *thrumming*," so watching never gates throughput.

**Parts model — the synergy engine (makes A's choices physically reshape B).**
- **Generic parts (backbone)** — every spell needs them (conduit, frame, core-casing); a steady baseline line that keeps the factory humming.
- **Specialized parts, per element (differentiator)** — medium/hard spells of each element need their own (Agni heat-cores, Jal chill-lattices, …), **tiered by spell difficulty** (medium → tier-1; hard → tier-2 that consumes tier-1 → the "shallow-but-real chains"; hard spells are genuinely production-expensive).
- **The tension this creates:** **specialize deep** (mass one element's parts → dominate that market segment) vs **spread wide** (a little of everything → flexible but never cheapest anywhere). It couples straight to market segments + the competitor's flood/undercut behavior (`07`) — *this* is what keeps B strategic instead of grind.

**The warehouse — the bounded stage.** All of B lives inside the **shop's warehouse**: small machines (the goods are small) → a charming **mini-factory-in-a-room**. The bounded floor is the **difficulty** (space-constrained routing), not only charm; **warehouse expansion is a "Grow" lever.** Structurally the warehouse is the **scope container for B** exactly as the gem is for A — two nested, fully-readable, single-space stages (B's analog of the combat arena's single-screen frame). Fits the shared **Duckov / Universim** stylized art.

**Quality vs quantity (where quality lives).** **Quality = Surface A:** a better-engineered process **+ a bigger / purer gem → more nodes, purer nodes, and more internal build-space** to shape the Mani. **Surface B = pure quantity / logistics** (throughput only). So competition stays two-axis — **your design quality vs your price / volume** — and there is **no quality choice inside B** (keeps the two surfaces cleanly distinct, no finishing sub-system).

## The core loop

1. **Design** a spell — Conjuration Engineering on the gem grid (Surface A). *Produces a blueprint.*
2. **House** it — pick the **gear** (gauntlet / wand / sword / armor); the gear adds delivery method, durability, and **gem slots** (= multi-element capacity, see `06`).
3. **Produce** — feed the workshop (Surface B): mass-produce the blueprint's parts and route them to the **mega assembler**, which builds the finished spell-gear. **Throughput caps your market output.**
4. **Sell** — put products on the **competitive market** (`07`): demand varies by element / behavior / gear; **rival workshops** supply the same market → supply & demand → prices move.
5. **Special requests** — periodic head-to-head **bake-offs**; best submission wins big, weak ones eat losses (you go directly against the rival AI on a fixed spec).
6. **Grow** — money → expand the workshop (stations, machines, **bigger/purer gems, better gear**) + climb the **tech tree** (new operations, elements, multi-Mani recipes, gear types). Growth (and market demand) pulls you toward **multi-Mani** spells for maximum power.

## Resolved supporting decisions

- **Make-or-buy → procure refined gems.** Gems are **bought pre-refined**, graded by **element / size / purity** (size = node count, purity = particle rate/quality). They are the **substrate** you build on, slotted into the gear. **No in-house refining in the spell loop** — Surface A is *design*, not material prep. Surface B makes **parts + gear**, not gems.
- **Three procurement inputs:** refined **gems** (substrate, by element/size/purity) · **raw materials** (feed Surface B to make parts + gear) · plus the **tech** you've unlocked (capability).
- **Scope principle (locked by the user):** *this* game is the **deep, fully-featured, micro-managed reference implementation** of Mani spell-engineering. The **dream game decides later how much to abstract** — this one does not pre-abstract.
- **Combat reuse:** the spells designed here **share the combat pillar's spell-data model** — they are literally the spells cast in combat (fire lance = projectile, earth fist = melee slam, ice wall = barrier). The automation game's *output* is the combat game's *input*.

## Machine framework — RESOLVED (2026-06-10)

The **operation set, machines, and belt system** are now locked as a framework in [[09 Machine Framework - Modules, Parts & Production|09]] (keystone: modules = parts; 5 op categories; cast-time-from-flow; recipe-gated merging; B taxonomy + Vayu-belt transport + blueprint = BOM), grounded in the particle deep dive (`06`). Remaining defer: exact op/part lists + all numbers → the gray-box. The first build milestone is the **Conjuration Engineering gray-box** (= the ① Conjuration Game's first milestone, see `11`), not the shop economy.
