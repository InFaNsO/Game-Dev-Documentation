# City Builder — References & Design DNA

> The reference stack for the City pillar (build-ladder Game 5, the Anno-lite Mani-world standalone). Captures **what we want to steal from each**, then the **cross-cutting DNA** that falls out of the three picks. This is Phase 0 of the city design pass — decide what we like *before* designing.
> Analogous to the combat pillar's reference stack (SP:FbW × Into the Breach × Expedition 33 × Valkyria Chronicles).

---

## The three references

### 1. Cities: Skylines — *the deep-simulation logistics sim*
What we like:
- **Logistics maintenance, especially traffic** — flow/throughput as a living, breakable system you constantly tend.
- **Many biomes & maps** — varied starting conditions and terrain.
- **Full customization** (including Workshop mods) — player expression, deep moddability.
- **Detailed simulation** — systems run deep, emergent behaviour.
- **Economy management** — taxes, revenue generation, balancing *greed vs happiness*.
- **Production pipelines for specialized industries** — dedicated industry chains.

### 2. Town to City — *the cozy free-build needs sim*
What we like:
- **Cozy feel** — low-stress, inviting tone.
- **Free-build, not grid-based** — organic placement, aesthetic freedom.
- **Decoration system tied to needs & happiness** — beautification is a *mechanic*, not just dressing; part of what the player enjoys for its own sake.
- **Wide variety of needs** — many distinct demands to satisfy.
- **Anno-like multi-town** — different towns fulfill different needs.

### 3. Anno series — *the production-chain & scaling-logistics sim*
What we like:
- **The production pipeline** — multi-step resource chains.
- **Building expansion increases output** — upgrade a building → more production (vertical growth, not just more buildings).
- **Intra-city logistics + needs being met** — moving goods to where demand is.
- **Each biome has a different hardship to expand into** — biomes gate/challenge growth differently.
- **Logistics that scale in tiers** — 1 island → multi-island → multi-map with multiple islands. Growth = widening logistics scope.

---

## Cross-cutting DNA (what the three picks have in common)

These are the patterns that appear in **multiple** references → the strongest signal for our core design.

1. **Production pipelines are the non-negotiable core verb.** All three list it (CS specialized industries · Anno multi-step chains · T2C goods for needs). → Our **Mani refine-and-supply economy** maps onto this directly (raw Mani → refined → goods/needs).
2. **Logistics is the central, escalating challenge.** CS = traffic flow; Anno = intra-city → multi-island → multi-map; T2C = inter-town specialization. The recurring shape is **logistics scope widening as you grow** — and Anno's 1→multi-island→multi-map ladder is *literally* the dream game's Main↔Satellite spine. **This is the heart of the game.**
3. **Needs + happiness as the demand engine, in tension with economy/greed.** CS (taxes vs happiness) · T2C (variety of needs, decoration→happiness) · Anno (needs being met up the population tiers). The push-pull that drives the whole loop.
4. **Biome-differentiated expansion.** CS many biomes · Anno "different hardship per biome" · and our locked **Mani-element-per-biome**. Strong convergence → biomes should be *mechanically distinct* (each its own hardship + its own Mani element you can't get elsewhere = the reason to expand).
5. **Vertical growth via building upgrades** (Anno: expand a building → more output). A specific, stealable upgrade mechanic — growth isn't only sprawl.

## Tensions to resolve (where the references *disagree*)

These are real forks the design pass has to settle — the references pull in different directions:

- **Free-build (T2C, cozy) vs grid/structured (CS, Anno detailed).** You like *both* the cozy organic placement AND the detailed simulation. Biggest open fork.
- **Cozy tone (T2C) vs deep hardcore simulation (CS).** Where do we sit on the cozy ↔ sim-depth axis? This sets the whole feel and the scope ceiling.
- **Logistics fidelity.** Full CS-style *traffic* simulation is enormous scope for a solo dev. Anno's abstracted route/transport logistics is far cheaper and still delivers the "logistics is the challenge" fantasy. How granular do we go?
- **Customization / mods (CS).** Loved, but Workshop-grade moddability is almost certainly out of scope for a testbed — note as aspiration, not a target.

## Implications for our game (preliminary)

- The **production pipeline + escalating logistics** is the spine — and it conveniently *is* the dream-game's Main↔Satellite system, so the testbed proves exactly the right thing.
- **Biomes = Mani elements = reasons to expand** is a clean three-way fusion of the references and our locked lore.
- The **cozy ↔ deep-sim** axis and **free-build vs grid** fork are the two decisions that most shape scope — settle these early.

> **Next:** resolve the tone axis (cozy ↔ sim-depth) and the build model (free vs grid), then return to Phase 1 (premise) and Phase 2 (scope).
