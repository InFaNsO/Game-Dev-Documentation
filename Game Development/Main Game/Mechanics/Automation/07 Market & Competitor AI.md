# Automation — Market & Competitor AI

> The pressure engine: the **market substrate** and the **competitor AI**. This is the home of **v1 of the cross-portfolio competitor-AI thread**. **Locked 2026-06-08** (v1 framework); roster/personality detail and numbers deferred.

---

## Market = shared substrate (reuses across the portfolio)

The **market model** (demand, pricing, supply & demand, contracts) is built as **shared infrastructure**, not an automation-only feature. It has a home in nearly every game (LS vendors, the city economy, the dream-game Main↔Satellite economy), so it carries forward by default.

In the automation game it drives:
- **Demand** by **element / spell-behavior / gear type**, shifting over time and with competitor activity (a *living* market, not a fixed order board).
- **Pricing** via supply & demand — flooding a segment drops its price.
- **Special requests / contracts** — periodic high-stakes bake-offs (see below).

## The competitor — v1 of the AI thread

**Tone:** competitive. Rival workshops are an **economic** threat (lost sales, not destruction) — real stakes, non-violent texture.

**What v1 is:** a **market-level economic agent — NOT embodied.** You see its *effects* (supply, prices, contract bids), never its workshop. (Embodiment — an agent acting in a game world whose actions drive the market — is **v2**, in the city pillar. See the thread note below.)

**What the competitor does (v1 behaviors):**
- **Supplies the market** → its production pushes prices (flood the fire-wand segment → fire-wand prices fall).
- **Targets demand** → shifts production toward high-margin / high-demand niches, racing you for the lucrative segments.
- **Prices dynamically** → undercuts you to steal demand, or prices up on a quality edge.
- **Bids on special requests** → its submission quality is the bar you must beat — the head-to-head moment.
- **Escalates** → improves over time (better spells, eventually multi-Mani), so you can't coast.
- **Has personality** → **2–4 rival workshops, each with a specialty** (premium · cheap-mass-market · a single-element specialist · …) → a textured market and a clean **difficulty dial** (add rivals to raise the heat).

**Implementation guardrail (keeps v1 cheap):** a **utility / heuristic agent** on the shared market model — evaluate segments by margin/demand, allocate production, set prices, bid contracts. **No ML, no world-simulation.** This is the smallest version that makes the market feel alive, and it's the foundation v2 extends.

## Special requests (the bake-off) — format LOCKED 2026-06-10

Periodically the market (or a patron) posts a **contract with a spec**: constraints (element/compound · gear type · stat thresholds, e.g. "pierce ≥ X, cast ≤ Y" · quantity · deadline).

- **Opt-in** — you choose to compete (cozy-respecting; ignoring one just means a rival takes it).
- You design or adapt a spell, produce the batch, submit before the deadline.
- **Judged vs rival submissions on spec-fit + grade + price** — winner takes the payout + a **reputation jump**; losers **eat their production costs**.
- **Structural double duty:** bake-offs are the pacing skeleton (quasi-quests) AND the **plot-delivery vehicle** — the endgame **Akash-tease** requests ("the Accord seeks a new kind of Mani…") arrive through this channel (the plot session hangs its story here).
- **Build reuse:** the spec-judging engine is built first in the **① Conjuration Game** (its commission levels = bake-offs v0, see `11`) and carries into the Spell-Shop unchanged.

## The cross-portfolio competitor-AI thread

The competitor is **one versioned thread** (like procgen and logistics), not three separate problems. It's a **shared strategic "brain" + a swappable "action space"**:

- **v1 — Automation (here):** action space = **market levers** (price, supply, contract bids). Abstract, behind-the-curtain. Cheap. Proves *whether a rival is even fun.*
- **v2 — City builder (fusion):** action space = **world-actions** (build, claim territory/resources, produce) → those actions *drive* the market. Embodied, visible. Only built if v1 earns it. *(Re-opens the city's "NO AI colonies" lock — to be resolved before the game list; cozy-town stays rival-free.)*
- **dream game:** inherits v2-level (faction competitor in the Triangle) **additively**.

The reusable layer is the **market substrate + the strategic brain**; only the thin action-space layer is per-game. Full thread scoping (versions → per-game assignment → dev-order) happens in the **cross-game systems-thread session** (alongside procgen) after the full game list is compiled. See `CLAUDE.md` planning sequence.
