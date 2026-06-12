# Game 1 — Parry Combat: "Last Rite"

> The combat pillar's **reactive-half** proving game (dual-camera parry/dodge/counter), shipped as a complete $5–10 product. **Concept LOCKED 2026-06-10.** Dev slot 1 (first game; builds Tier 0 foundation). Working title "Last Rite." Touchstones: Furi · Sekiro gauntlets · Expedition 33 (the parry reference the user named).
> **Engineering blueprint:** [[1a Last Rite - Code Architecture]] (assembly map, abstraction seams, Tier-0 concrete v1 — written 2026-06-10, pre-scaffold).
> **Combat core spec (v0):** [[1c Last Rite - Combat Core Spec]] (implementation-level: classes, events, diagrams, tick law, tests — written 2026-06-12, pre-code).
> **Art bible:** [[1b Last Rite - Art Bible]] (style lock, colour governance, Meshy + Kimodo pipeline, rig amendment — the PORTFOLIO art lock, written 2026-06-10).

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

- **Player kit (LOCKED 2026-06-10):** **Blade offense** (light strikes — proactive pressure + HP chip; a conscious *addition* to the original list, since "you master the blade" needs offense) · Parry — **Block** (wide window, small purge, chip) + **Perfect** (tight window, big purge, guaranteed **counter**, **ranged-deflect** sends projectiles back) · **Dodge** (i-frames for perilous attacks; safe, no purge — the greed-split) · **Purification finisher** (purge-break → mercy-kill). **≤2 unlockable techniques** — *expressive, not power*, unlocked **diegetically at rebirth milestones**, never a skill tree (**mastery IS the progression**). Candidates (gray-box-open): perfect-dodge→counter; chain-parry stance.
- **Enemy attacks pose a "which response?" read:** parryable (deflect) · unparryable/perilous (must dodge) · ranged (Perfect-parry deflects it back) · **feints/mix-ups** (the core depth — punish premature parries) · **phases** (HP-threshold moveset shifts = the cheapest depth multiplier). Telegraphs are **colour/icon-coded and fairness is sacred** (Sekiro's perilous tell).
- **Resolution (LOCKED 2026-06-10): HP + a parry-fed purge meter** (Sekiro-style). Blade + counters chip HP; **Perfect parries fill purge fastest**; a broken purge meter staggers the guardian into the **purification finisher** ("take my pain" — the mercy-kill). **Parrying is the win condition, not just defense** → wires the purifier theme into the mechanics and **seeds the LS's forced-purification of named Husks.** Purge decays if you stop parrying.
- **Camera (LOCKED 2026-06-10): a single smart action-cam** that dynamically pushes into OTS/close framing on each telegraphed attack, then pulls back (Furi/Sekiro/E33 do this with ~one camera). **NOT the LS's two-mode dual-camera rig** — that switch only earns its keep with a grid to switch away from, so it **moves to Game 2 (Tactical Combat)**. Game 1's milestone = the **reaction layer + reaction-cam *feel*.** Multi-enemy stays **readable** (attacks resolve one at a time → the depth of mixed fights is the *planning between reads*, not parsing simultaneous chaos).
- **Movement model (LOCKED 2026-06-10):** Expedition 33-style **near-stationary timing duels** for v1 (cheapest, matches the reference, lowest-risk gray-box). Free-arena movement stays a possible *later* experiment, not v1.

## Replayability — four stacked layers

1. **Difficulty ascension (the rebirth loop)** — each cycle remixes patterns + escalates; the diegetic NG+.
2. **Per-duel ranking** — no-hit / time / style; the mastery chase.
3. **"The Rite" gauntlet** — all guardians back-to-back, one healthbar, for score (unlocks after first clear; near-free to build).
4. **★ The mystery as the replay engine** — the truth behind the immortality unfolds in *layers* across rebirths/difficulty, building to a **true ending**. You replay to *learn more*, not just to score.
- **Future free update:** an **Endless Dungeon mode** once procgen matures to that level (a later P-tier) — remixed/procedural descents. v1 ships fully authored; the update lands when the tech exists (great for the Steam algorithm). **Stepping stone (LOCKED 2026-06-11): the Chaos Descent** — post-true-ending randomized descent via random *selection* of authored content (no procgen), lands first as v1-stretch / first content update; see "Elemental rebirth spine."

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

## Design-lock pass — LOCKED 2026-06-10

A full design-detail pass settled the structural decisions (numbers stay reference-only, per portfolio discipline — feel-dependent values get their real answer from the gray-box):

- **D1 — Immortality framing:** the protagonist **gains** immortality at the **first beast's heart** (the inciting rebirth). Death-retry (respawn at duel start) is on from minute one regardless.
- **D2 — Loop structure:** **full re-descent every rebirth** — all 6 guardians re-fought but **remixed** (feints / new attacks / warped ruin) = a re-learn, never bigger numbers. **The rebirth spine is elemental (LOCKED 2026-06-11): 5 descents = 1 neutral tutorial descent + 4 Mani-themed rebirths (Bhu → Jal → Vayu → Agni)** — each rebirth themes one Mani as an **overlay** on the guardians' base identities (corruption flavor, never replacement), driving that cycle's palette LUT, remix overlays, telegraph VFX, and lore/narration (see "Elemental rebirth spine" below). The **final rebirth (Agni) transforms into the true-ending sequence** (mind-gone protagonist walks out a Husk → hands the baton to the LS); the true ending itself stays **element-neutral**. Late-loop-drag mitigations (ranking + a "fast-clear once mastered" valve) = playtest-revisit.
- **D3 — Movement:** **E33 near-stationary timing duels** for v1 (free-arena = possible later experiment).
- **D4 — Camera:** **single smart action-cam** (push-to-OTS on telegraph, pull back). ⚠ **Roadmap correction:** the LS two-mode dual-camera rig **moves to Game 2 (Tactical Combat)**; Game 1 proves the reaction layer + reaction-cam feel. Supersedes the older "dual-camera gray-box = Game 1's first milestone" framing.
- **D5 — Resolution:** **HP + parry-fed purge meter → purification finisher** (Sekiro-style; parrying is the win condition; seeds the LS forced-purification).
- **D6 — Kit:** blade offense + Parry (Block/Perfect) + Dodge + counter + ranged-deflect + finisher; **≤2 expressive (non-power) techniques** unlocked diegetically at rebirth milestones (no tree). Blade offense is a conscious addition to the original concept's kit list.
- **D7 — Replay / sanity:** 4 stacked layers (difficulty-ascension · per-duel ranking · "The Rite" gauntlet · the layered mystery → true ending); **sanity = atmospheric only, fairness sacred**; Endless mode = future free update; "embrace the hallucination" toggle = deferred stretch.
- **D8 — Reference-target numbers (ALL playtest-open):** player ~4 hits to death; Perfect parry ~8f (~130ms @60fps) inside a ~20f (~330ms) Block window; dodge ~12–15f i-frames; purge-to-finisher ~5–8 Perfects on a basic guardian (decays when idle); duel ~30–90s; ranking thresholds = deferred.

### Elemental rebirth spine — LOCKED 2026-06-11

Each cycle themes one Mani as an **overlay on the guardians' base identities** (corruption flavor — D2's "re-learn, never replace"), expressed through enemy attack flavor + telegraph VFX + palette LUT + lore/narration:

| Descent | Theme | Guardian flavor (overlay) | Palette | Canon tie |
|---|---|---|---|---|
| 1 | **none** | base identities, no overlay | neutral grade | tutorial descent; ends at the first heart (D1) |
| 2 | **Bhu** (Earth) | heavy, grounded, *readable* — ground-slam perilous, guard-pressure | stone / ochre / dust | the ruin's own stone-corruption |
| 3 | **Jal** (Water) | flowing, *delayed*, feint-heavy — the "slow" is the **enemy's** tempo | deep blue, drowned light | **Amphibian-Husk** corruption ascendant |
| 4 | **Vayu** (Air) | fast, evasive, gust-lunges; guardians may dart within the stationary frame | pale white-out | wind / sky — the lost-Avian echo |
| 5 | **Agni** (Fire) | fastest, most relentless; telegraphed burn-DoT punishes greed; **terminal** → true-ending sequence | ember / ash / orange | **Reptile-Husk** corruption ascendant |

- **Fairness firewall (D7):** elements re-flavor *enemy* attacks + VFX + palette + lore — they **never** touch the player's reaction window. Jal/Vayu's canonical player-debuffs (slow/push) express as *guardian behavior* (tempo, the guardian's own displacement), never as player status.
- **The true ending stays element-neutral** — Agni is the climax; Akash is deliberately NOT invoked.
- **Chaos Descent (post-true-ending; v1-stretch / first content update, NOT core v1):** an unlockable randomized descent — random *selection* of authored guardians/lessers + a random element theme + seeded random remix overlays. Pure data-shuffle of authored content, zero procgen — the bridge between v1's authored cycles and the eventual full-procgen Endless mode. Immortality means the cycle never truly ends.

### Still deferred to the gray-box (per portfolio discipline)
- All D8 numbers (feel-dependent → tune in the playable build).
- The exact 1–2 unlockable techniques (you'll know what the loop is missing once it's playable).
- Micro-feel of the smart action-cam (**the #1 thing to validate first**).
- Movement: whether free-arena ever earns its way in.
