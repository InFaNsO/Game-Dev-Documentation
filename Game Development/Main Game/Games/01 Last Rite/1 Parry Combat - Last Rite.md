# Game 1 — Parry Combat: "Mani Kalpa: One Last Rite"

> The combat pillar's **reactive-half** proving game (dual-camera parry/dodge/counter), shipped as a complete $5–10 product. **Concept LOCKED 2026-06-10.** Dev slot 1 (first game; builds Tier 0 foundation). **Full title LOCKED 2026-08-03: "Mani Kalpa: One Last Rite"** — the "one last" is the protagonist's doomed promise (there is never a last one; the curse loops), and the referent is deliberately double: every guardian gets one last rite, and the purifier believes each descent is *theirs*. Vault files/folders keep the short name "Last Rite." See [[14 Naming Glossary]] → The Franchise for the kalpa rationale + why "Chronicles" was dropped. Touchstones: Furi · Sekiro gauntlets · Expedition 33 (the parry reference the user named).
> **Engineering blueprint:** [[1a Last Rite - Code Architecture]] (assembly map, abstraction seams, Tier-0 concrete v1 — written 2026-06-10, pre-scaffold).
> **Combat core spec (v0):** [[1c Last Rite - Combat Core Spec]] (implementation-level: classes, events, diagrams, tick law, tests — written 2026-06-12, pre-code).
> **Art bible:** [[1b Last Rite - Art Bible]] (style lock, colour governance, Meshy + Kimodo pipeline, rig amendment — the PORTFOLIO art lock, written 2026-06-10).
> **Reaction & feints spec (M1):** [[1d Last Rite - Reaction & Feints Spec]] (attack timeline, defense windows, combos/QTE-AP, animation-event model). **Build roadmap:** [[1e Last Rite - Build Roadmap]] (M0→M5 task ladder).
> **Player moveset & animation plan:** [[1h Last Rite - Player Moveset & Animation Plan]] (Rite Blade moveset · pack clip mapping · guardian weapon archetypes — written 2026-07-31; the pack pipeline supersedes Kimodo for Last Rite combat animation).
> **⚑ Combat iteration log:** [[1f Last Rite - Combat Iteration Log]] — *what was planned → what it is now → why*, for every load-bearing combat pivot since the pre-code specs (architecture, timing model, combos, offense, difficulty). **Read it before trusting any dated detail in `1a`/`1c`.**

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

- **Player kit (LOCKED 2026-06-10):** **Blade offense** (light strikes — proactive pressure + HP chip; a conscious *addition* to the original list, since "you master the blade" needs offense) · Parry — **Block** (wide window, small purge, chip) + **Perfect** (tight window, big purge, guaranteed **counter**, **ranged-deflect** sends projectiles back) · **Dodge** (i-frames for perilous attacks; safe, no purge — the greed-split) · **Purification finisher** (purge-break → mercy-kill). **≤2 unlockable techniques** — *expressive, not power*, unlocked **diegetically at rebirth milestones**, never a skill tree (**mastery IS the progression**). Candidates (gray-box-open): perfect-dodge→counter; chain-parry stance. **⚠ Amended 2026-07-31 — the weapon's identity (the Rite Blade) + the elemental layer (Mani Surge = technique #1) are now decided; see "D6 amendment — the Rite Blade & Mani Surge" below.**
- **Enemy attacks pose a "which response?" read:** parryable (deflect) · unparryable/perilous (must dodge) · ranged (Perfect-parry deflects it back) · **feints/mix-ups** (the core depth — punish premature parries) · **phases** (HP-threshold moveset shifts = the cheapest depth multiplier). Telegraphs are **colour/icon-coded and fairness is sacred** (Sekiro's perilous tell).
- **Resolution (LOCKED 2026-06-10): HP + a parry-fed purge meter** (Sekiro-style). Blade + counters chip HP; **Perfect parries fill purge fastest**; a broken purge meter staggers the guardian into the **purification finisher** ("take my pain" — the mercy-kill). **Parrying is the win condition, not just defense** → wires the purifier theme into the mechanics and **seeds the LS's forced-purification of named Husks.** Purge decays if you stop parrying. **⚠ Payoff amended 2026-07-15 — see "D5 amendment — the Purge ultimate" below (fill → player-triggered ultimate, not auto-finisher).**
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
- **→ Franchise canon plant (LOCKED 2026-08-03): the name "Mani Kalpa" is diegetic — someone in-game names the great cycle as an actual kalpa.** Plan: a late-rebirth **lore layer** (Descent 4–5) delivers an Accord-era inscription at the sealed core that names the world-cycle formally (reference draft: *"the Mani Kalpa turns; what breaks is kept"*), and the protagonist's degrading narration **echoes** it in the Agni descent (reference draft: *"The kalpa does not end. It turns."*) — the purifier realizing their rebirth loop is one small turning of the world's own. Exact wording/placement = writing pass at gray-box; the **rule** (the term appears in-game, formal register, revealed late — never a tutorial popup) is locked. See [[14 Naming Glossary]] → The Franchise.

## Design-lock pass — LOCKED 2026-06-10

A full design-detail pass settled the structural decisions (numbers stay reference-only, per portfolio discipline — feel-dependent values get their real answer from the gray-box):

- **D1 — Immortality framing:** the protagonist **gains** immortality at the **first beast's heart** (the inciting rebirth). Death-retry (respawn at duel start) is on from minute one regardless.
- **D2 — Loop structure:** **full re-descent every rebirth** — all 6 guardians re-fought but **remixed** (feints / new attacks / warped ruin) = a re-learn, never bigger numbers. **The rebirth spine is elemental (LOCKED 2026-06-11): 5 descents = 1 neutral tutorial descent + 4 Mani-themed rebirths (Bhu → Jal → Vayu → Agni)** — each rebirth themes one Mani as an **overlay** on the guardians' base identities (corruption flavor, never replacement), driving that cycle's palette LUT, remix overlays, telegraph VFX, and lore/narration (see "Elemental rebirth spine" below). The **final rebirth (Agni) transforms into the true-ending sequence** (mind-gone protagonist walks out a Husk → hands the baton to the LS); the true ending itself stays **element-neutral**. Late-loop-drag mitigations (ranking + a "fast-clear once mastered" valve) = playtest-revisit.
- **D3 — Movement:** **E33 near-stationary timing duels** for v1 (free-arena = possible later experiment).
- **D4 — Camera:** **single smart action-cam** (push-to-OTS on telegraph, pull back). ⚠ **Roadmap correction:** the LS two-mode dual-camera rig **moves to Game 2 (Tactical Combat)**; Game 1 proves the reaction layer + reaction-cam feel. Supersedes the older "dual-camera gray-box = Game 1's first milestone" framing.
- **D5 — Resolution:** **HP + parry-fed purge meter → purification finisher** (Sekiro-style; parrying is the win condition; seeds the LS forced-purification). **⚠ Amended 2026-07-15 — see "D5 amendment — the Purge ultimate" below.**
- **D6 — Kit:** blade offense + Parry (Block/Perfect) + Dodge + counter + ranged-deflect + finisher; **≤2 expressive (non-power) techniques** unlocked diegetically at rebirth milestones (no tree). Blade offense is a conscious addition to the original concept's kit list. **⚠ Blade offense amended 2026-07-15** → the player's blade is a **QTE-chained, AP-bounded combo** (the offense mirror of enemy chaining; see the combo note in the D5 amendment above + [[1d Last Rite - Reaction & Feints Spec]] §6b). **⚠ Amended 2026-07-31 — see "D6 amendment — the Rite Blade & Mani Surge" below** (the blade gains its identity + the elemental layer; Mani Surge claims technique slot #1).
- **D7 — Replay / sanity:** 4 stacked layers (difficulty-ascension · per-duel ranking · "The Rite" gauntlet · the layered mystery → true ending); **sanity = atmospheric only, fairness sacred**; Endless mode = future free update; "embrace the hallucination" toggle = deferred stretch.
- **D8 — Reference-target numbers (ALL playtest-open):** player ~4 hits to death; Perfect parry ~8f (~130ms @60fps) inside a ~20f (~330ms) Block window; dodge ~12–15f i-frames; purge-to-finisher ~5–8 Perfects on a basic guardian (decays when idle); duel ~30–90s; ranking thresholds = deferred.

### D5 amendment — the Purge ultimate (2026-07-15)

Supersedes D5's payoff chain (break → auto-stagger → finisher). The meter and its parry-fed identity stand; the payoff becomes **player-triggered**:

- **Fill:** parries only — **Block parry = 1×, Perfect parry = 2×** (reference values, playtest-open). Decays when idle (unchanged). Dodge still pays nothing — the greed split stands.
- **Full meter arms the Purge ultimate** (a player attack input; binding + animation TBD at gray-box). Using it **consumes the meter** and **purges** the guardian: **staggered — it forfeits its next attack turn** — and it **takes +25% damage while purged** (duration playtest-open).
- **Parrying remains the win condition:** the only road to the ultimate runs through parries.
- **The purification finisher is no longer purge-triggered** — it moves to the **killing blow** (the mercy-kill cinematic; "take my pain" fiction unchanged). The ultimate is the *mid-fight* purification act; the finisher is the last rite itself.
- **Reference-design note:** this is deliberately closer to Expedition 33's *primed* Break (fill the bar, then **choose when to cash it** with a triggering attack) than Sekiro's auto posture-break — the player authors the burst window. Combos amplify it: **enemy** attack-strings ([[1d Last Rite - Reaction & Feints Spec]] §6) make each hit its own parry read, so a fully-parried string is a fat meter payday.
- **Combo system (LOCKED 2026-07-15 — see [[1d Last Rite - Reaction & Feints Spec]] §6/§6b):** combos are **linked pre-made single-attack clips** chained via the Animator graph (cheap — a new combo ≈ one data asset), on two sides: **enemy chains** (authored signature spine + dynamic runtime recombination; difficulty = longer chains + tighter windows; the rebirth-remix multiplier) and a **player QTE-chained, AP-bounded combo** (Expedition 33-style forgiving timed-press offense — a *planning* layer, not twitch execution). **Economy split stays clean: AP = offense input · Purge = parry-fed defense payoff.** The player combo is a conscious offense expansion (reactive-half → two-sided); build the enemy-string core first (it's the M2 CinderScale dependency), the player AP-combo after. Full build breakdown in the **Combo System dev plan** (child of the Ship Plan's `m1-strings`).
- **D8 retarget:** the old "purge-to-finisher ~5–8 Perfects" reference now reads "meter **fill** ≈ 5–8 Perfects (or mixed Block/Perfect equivalent)."
- **Still open (gray-box):** +25% duration (through the stagger turn vs fixed ms) · whether the ultimate can whiff or is guaranteed on a staggerable target · per-guardian meters in mixed encounters (default: per-enemy, fed only by parries against that enemy) · meter carryover between duels (default: resets).

### D6 amendment — the Rite Blade & Mani Surge (2026-07-31)

Extends D6's locked "blade offense" lane with the weapon's identity and a new elemental layer. Nothing in D6 is reversed — the QTE/AP combo (D5 amendment), the ≤2-expressive-techniques rule, and the fairness firewall all stand.

- **Weapon identity — the Rite Blade:** a single **one-handed ritual blade**. Deliberate, readable strikes — the purifier mercy-kill fantasy, not a flurry fighter. **"Blade now, gauntlets later":** a second weapon (**elemental gauntlets** — punches project elemental strikes) is **future-proofed via the data-driven `MovesetDef`/`SOAttackDef` layer but NOT built for Game 1** — a candidate for post-launch (the Chaos Descent) or the Looter Shooter.
- **Base moveset** rides the locked QTE/AP system ([[1d Last Rite - Reaction & Feints Spec]] §6b): **3-hit light chain (1 AP per hit) · heavy ender (2 AP) · counterattack after a Perfect parry** (already locked). Full tiered animation list + pack clip mapping: [[1h Last Rite - Player Moveset & Animation Plan]].
- **Mani Surge — the elemental layer (technique #1 of the ≤2 budget; #2 stays reserved — candidate: an elemental counter on Perfect parry with a full meter):**
  - **Element source (diegetic):** the blade **automatically absorbs the current rebirth's dominant element** (Descent 2 = Bhu · 3 = Jal · 4 = Vayu · 5 = Agni; Descent 1 = neutral) — "the curse seeps into your blade." **No unlock UI, no skill tree** — complies with the locked ≤2-techniques rule.
  - **Passive:** normal attacks gain **elemental particle trails** per rebirth. Pure VFX; the fairness firewall is untouched (never affects reaction windows).
  - **Active — the Surge:** **ONE signature elemental attack per element** (stone shockwave / water wave / wind slash / lava arc). **Scope trick: one shared "channel and release" animation, four VFX sets.**
  - **Fuel (⚠ PROVISIONAL, dev-time):** a **separate "Mani meter", fed by Perfect parries ONLY** (Blocks do not feed it). Full meter = one Surge. Explicitly provisional — kept **independent from the Purge meter while that system is in flux**; once both are playable in gray-box, **decide: merge / keep / cut** (recorded open below).
  - **HUD:** the Mani meter **only appears from Descent 2 onward** — the Descent 1 HUD stays clean.
  - **⚠ Risk note:** player elemental VFX must **never obscure enemy telegraphs** — keep Surge VFX brief/directional; test in gray-box.
- **Still open (gray-box):** the Mani meter's fate — **merge into Purge / keep separate / cut** (decide once both are playable) · Surge damage & values (I10) · technique #2.

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
- **⚠ 2026-07-31 — the rebirth element now also seeps into the *player's blade*** (Mani Surge — see the D6 amendment): passive elemental trails + one signature Surge per element, keyed to this table's Descent theme (Descent 1 = neutral, no Surge). The fairness firewall stands untouched — the player-side layer is pure VFX + one signature attack, and **never** affects reaction windows.
- **The true ending stays element-neutral** — Agni is the climax; Akash is deliberately NOT invoked.
- **Chaos Descent (post-true-ending; v1-stretch / first content update, NOT core v1):** an unlockable randomized descent — random *selection* of authored guardians/lessers + a random element theme + seeded random remix overlays. Pure data-shuffle of authored content, zero procgen — the bridge between v1's authored cycles and the eventual full-procgen Endless mode. Immortality means the cycle never truly ends.

### Still deferred to the gray-box (per portfolio discipline)
- All D8 numbers (feel-dependent → tune in the playable build).
- The exact 1–2 unlockable techniques (you'll know what the loop is missing once it's playable). **⚠ 2026-07-31: technique #1 = Mani Surge (see the D6 amendment); #2 stays reserved — candidate: an elemental counter on Perfect parry with a full Mani meter.**
- Micro-feel of the smart action-cam (**the #1 thing to validate first**).
- Movement: whether free-arena ever earns its way in.
