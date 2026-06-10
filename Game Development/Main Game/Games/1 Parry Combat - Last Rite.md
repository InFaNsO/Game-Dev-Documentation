# Game 1 — Parry Combat: "Last Rite"

> The combat pillar's **reactive-half** proving game (dual-camera parry/dodge/counter), shipped as a complete $5–10 product. **Concept LOCKED 2026-06-10.** Dev slot 1 (first game; builds Tier 0 foundation). Working title "Last Rite." Touchstones: Furi · Sekiro gauntlets · Expedition 33 (the parry reference the user named).

---

## Elevator pitch

A lone purifier descends a sealed, dead-world ruin, duelling its corrupted guardians one by one — read each wind-up, parry or dodge, strike back — to reach the **beast at the heart** and the secret it guards: **immortality.** Claim it, and you wake again at the ruin's mouth, the world a shade *wrong*. Each rebirth: harder guardians, deeper truth, and a little more of your mind gone. You master the blade as you lose yourself.

## Setting & era

- **Mani world, post-Breach, BEFORE the Looter Shooter.** A dead, residue-soaked world of corrupted Husks (Amphibian/Jal-blue, Reptile/Agni-orange) and ruined Accord structures. Same art set, tone, and enemies that flow into the LS.
- **One ruin, ~3 themed strata** (e.g. upper ruin → flooded depths → sealed core), ~2 guardians each. The same authored space, re-skinned and re-arranged by the curse on every rebirth — *feels* varied without authoring separate ruins.
- **The protagonist:** a lone purifier, drawn ruin to ruin to lay the corrupted to rest ("take my pain" — reusing the LS purification theme; every duel is a mercy, which gives thin-story weight).

## The core hook — the curse IS the replay loop

The immortality justifies **both** kinds of "coming back," diegetically:

- **Die mid-ruin → respawn** (you can't die) = the normal combat retry.
- **Beat the beast / claim the heart → reborn at the entrance, the world subtly wrong** = the NG+ tier.

So there's never a "why am I replaying this?" — the player *is* the immortal, trapped, reliving the descent. Roguelike retry and difficulty ascension are one fiction.

**The keystone marriage:** *the player's mastery rises as the protagonist's mind falls.* Getting good and losing yourself to the curse are the same arc — the hook that sells a thin-story game.

## What changes each rebirth (so it never feels like "same ruin again") — all cheap

- **Palette shift = the difficulty/sanity HUD** — the world's hue tells the player how deep in the curse (and how hard) they are, at a glance.
- **Remixed movesets** — guardians gain feints, new attacks, mixed compositions: a real *re-learn*, not bigger numbers.
- **The ruin warps** — hidden rooms open, others seal, props decay, guardians grow more corrupted. Same space, evolving canvas.
- **A new lore layer** + the protagonist's own narration degrades — the reward for pushing deeper.

## Combat model — the reaction-layer *subset* of the locked design

Uses only what `Mechanics/Looter Shooter/06` locked for the reactive layer — **no spells / launcher / tactical grid** (those are Tactical Combat + later games):

- **Player kit:** Parry (Block / Perfect) · Dodge · Counter · ranged-deflect. 1–2 unlockable techniques *max* — **mastery is the progression, not a skill tree.**
- **Enemy attacks pose a "which response?" read:** parryable (deflect) · unparryable/perilous (must dodge) · ranged (Perfect-parry deflects it back) · **feints/mix-ups** (the core depth — punish premature parries) · **phases** (HP-threshold moveset shifts = the cheapest depth multiplier).
- **Dual-camera reaction-cam** fires per incoming attack → multi-enemy stays **readable** (attacks resolve one at a time); the depth of mixed fights is in the *planning between reads* (who to face/punish/deny), not parsing simultaneous chaos.
- **Movement model — working default:** Expedition 33-style near-stationary timing duels (cheapest, matches the reference; confirm vs. free-arena movement at gray-box).

## Replayability — four stacked layers

1. **Difficulty ascension (the rebirth loop)** — each cycle remixes patterns + escalates; the diegetic NG+.
2. **Per-duel ranking** — no-hit / time / style; the mastery chase.
3. **"The Rite" gauntlet** — all guardians back-to-back, one healthbar, for score (unlocks after first clear; near-free to build).
4. **★ The mystery as the replay engine** — the truth behind the immortality unfolds in *layers* across rebirths/difficulty, building to a **true ending**. You replay to *learn more*, not just to score.
- **Future free update:** an **Endless Dungeon mode** once procgen matures to that level (a later P-tier) — remixed/procedural descents. v1 ships fully authored; the update lands when the tech exists (great for the Steam algorithm).

## The reward & the ending

- **What's at the bottom:** the **beast's heart = the secret to immortality.** The "reward" is the curse — you gain it, and it loops you.
- **The truth, layered:** each cycle reveals more of *why* the world died and what immortality really is — a small, self-contained mystery that **rhymes with the Breach/Akashic** (callback, not plot-link).
- **The true ending (target):** the immortality *is* the corruption that makes Husks. The protagonist, now mind-gone, walks out to begin their own thousand-year wandering — cyclical, devastating, and it hands the baton straight to the Looter Shooter.

## Sanity / madness — atmosphere, not unfairness

Mostly **atmospheric** (visual/audio distortion, degrading narration, hallucinated whispers) with only *fair* mechanical touches. **Fairness is sacred** — telegraphs stay readable always. *(Optional, later: a risk/reward "embrace the hallucination for power at a handicap" toggle.)*

## Scope

| Element | Budget | Notes |
|---|---|---|
| Bespoke guardian duels | ~6 | Multi-phase (2–3 each) → ~15 effective movesets. **These ARE the LS's named-Husk mini-bosses** (not throwaway). |
| Lesser enemies | ~5 | Single-identity, 2–3 attacks each, for mixed connective encounters (combinatorial padding). |
| Total encounters | ~15–18 | 6 guardian duels + ~10 mixed connective fights. |
| First descent | ~30–60 min | Faster as the player improves. |
| Rebirths → true ending | ~4–6 | ≈ 3–5 hr core; gauntlet + ranking + future Endless mode on top → 12–20+ hr for engaged players. |
| Player kit | parry/dodge/counter/deflect | Locked reaction-layer subset; ≤2 optional techniques. |

**Cost truth:** art is near-free (rig + accessory + corruption shader reuse); the real budget is **moveset animation + telegraph design** — that's why bespoke count stays ~6 and phases/mixing/rebirths do the multiplying.

## What it proves / forward-maps (dev purpose)

- Proves the **#1 identity bet** — the dual-camera reactive parry loop — in isolation, the **first internal milestone** of the whole portfolio (gray-box this loop first).
- Builds **Tier 0 foundation** (state machine, service locator + event bus, SO data layer, camera, input, UI) carried into every later game.
- The ~6 guardian movesets become the LS's **named-Husk mini-bosses**; the reaction layer becomes the LS combat's defensive half.

## Canon callbacks (record as Mani-world canon)

- **→ Looter Shooter:** Husks are immortal, mind-gone, wandering for millennia. Last Rite is the player **experiencing the Husk-making process from the inside** — by the true ending the protagonist has essentially become one. Deepens every LS Husk encounter.
- **→ Dream Game:** the immortal protagonist. The dream game's immortality is "the Husk-making force **with mind intact**"; Last Rite is the **mind-lost** counterpart. Deliberate thematic pair — Last Rite seeds the immortality motif years early.

## Open / deferred (settle at gray-box)

- Movement model: E33-stationary (default) vs free-arena.
- Exact rebirth count + how aggressively later loops compress/escalate.
- Whether the protagonist starts immortal or *gains* it at the first heart (leaning: gains it → the first beast-kill is the inciting rebirth; death-retry is on throughout regardless).
- All numbers (HP, posture/parry windows, ranking thresholds).
