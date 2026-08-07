# Game 1 "Last Rite" — Combat Iteration Log

> A running record of where the combat **design + architecture moved, and why.** The specs hold *current* state — [[1 Parry Combat - Last Rite]] (design + D-locks), [[1a Last Rite - Code Architecture]] §AS-BUILT (engineering truth), [[1c Last Rite - Combat Core Spec]] (v0 intent), [[1d Last Rite - Reaction & Feints Spec]] (M1 slice). **This doc holds the *trail*** so an old decision is never mistaken for the live one.
>
> Newest first. Dates absolute (today's baseline: 2026-07-31).

## Why this log exists

The early specs (`1a`/`1c`, written pre-code in June 2026) describe a design the build has since pivoted away from in several **load-bearing** ways — the timing model, the animation pipeline, the player's offense, even a foundational invariant. Rather than delete the original reasoning, we log each pivot: the blueprint stays legible as *intent*, and every place reality diverged is explained here. If a spec sentence and this log disagree about *what was planned*, this log is the record; if they disagree about *what is true now*, the spec + `1a` AS-BUILT win.

---

## 2026-08-07 — Player weapons = **two transforming trick weapons** (supersedes the single Rite Blade)

- **Was:** a single **one-handed ritual blade**, one pack folder, moveset = 3-hit light chain + heavy ender (locked 2026-07-31, two entries below).
- **Now:** **two armaments, each with two forms** — Bloodborne's trick-weapon model. **The Rite Blade · *Samskara*** (Katana ↔ Halberd): the cord grip telescopes rearward in three ratcheted segments and the sword becomes a naginata; the blade never changes. **The Pyre Censer · *Dhupa*** (Club ↔ Maul): a censer forged into a war hammer whose head **slides along a fixed-length haft** and locks choked (one-handed) or extended (two-handed). **Transformation is a chain step costing AP** that switches movesets mid-combo, riding the `1d` §6b planning layer — it lives on the player's turn only, so it never competes with the enemy-phase parry read. **Moveset shape: 2 light (1 AP) + 2 heavy (2 AP) per form = 8 per weapon, 16 total.**
- **Why:** the developer rejected a wide flat weapon list as "too many sword types, no uniqueness" and asked for Bloodborne-style depth instead — few weapons, many movesets. Because every pack set is already a complete standalone moveset, a trick weapon costs nothing but the transform clip: four movesets for four owned sets and **two financed Kimodo clips**, which is the entire new-animation budget. No new pack sets purchased.
- **Design law recorded (learned across ~6 concept iterations):** transformations must **conserve mass** and must never "bloom" — opening a shape spreads its mass and reads as *lighter*, which destroys a heavy form's weight. Mechanisms that survive the eye: **split laterally · telescope the haft · slide the head.** Each weapon uses a different one so the tricks never blur. Two rejected pairings and why: *Katana ↔ Dual Swords* (two full-thickness blades cannot come out of one katana — needs half-thickness halves, half-moon guards, D-grips to read); *Dagger ↔ Scythe* (a blade cannot shrink, so the "dagger" is always an oversized curved knife).
- **Specced now:** [[1h Last Rite - Player Moveset & Animation Plan]] §1 + §4; [[1 Parry Combat - Last Rite]] §"D6 amendment" (weapon-identity and base-moveset bullets amended in place).

## 2026-08-07 — **Bhu gains a bespoke enemy family**; full weapon re-allocation

- **Was:** [[1g Last Rite - Enemy Roster]] stated Bhu was **deliberately excluded** from the bespoke roster — "a pure data remix of the existing roster," because "the real budget is moveset animation + telegraph design," which is why bespoke enemies were capped at nine. Guardian weapon folders were assigned in `1h` §5 as a proposal.
- **Now:** **four bespoke Bhu enemies** — Garland Husk (*Asthikumbha*), Mourner Husk (*Avarana*), Deadfall Husk (*Chitakashtha*) and the **Reliquary Husk (*Dhatugarbha*, boss, 3 phases)**. Roster grows 15 → 19. **Enemy moveset shape locked: guardians = 2 VFX specials + 2 light + 2 heavy (6); bosses = 2 VFX specials + 5 light + 5 heavy (12).** Specials are **particle systems layered over generic pack clips**, not bespoke animations — that is what keeps a whole new family at near-zero animation cost. The `1h` §5 assignment table was **replaced wholesale** (the player's four sets are now exclusive, which forced reassignment of SiltWeaver, DartWing, MireCrown, CinderScale and others).
- **Why:** the animation pack removed the cost that justified the exclusion, and Bhu was the only element with no identity of its own.
- **⚠ Two factual corrections to `1h` §2:** there is **no standalone one-hand sword folder** in the pack (Katana is the closest one-handed blade), and there are **no throw or grab clips anywhere** in it (only `Fishing - Throw`) — which is why the Garland's Urn Toss and the Mourner's Veil Embrace need custom clips. Clip names are **English**, not Spanish as previously recorded.
- **Kimodo returns in a limited role:** dropped for bulk combat animation (see 2026-07-31 below) but retained for **signature motions no weapon set expresses** — 8 clips across the Bhu family plus 2 player transforms, all batched in one session per the cold-start cost profile.
- **Specced now:** [[1i Last Rite - Bhu Mani Husks]] (new — designs, weapons, full movesets, concept art); [[1h Last Rite - Player Moveset & Animation Plan]] §5; [[1g Last Rite - Enemy Roster]] (Bhu element + 4 cards added; the "excluded" note and tier counts corrected).

## 2026-07-31 — Combat animation sourcing = **purchased pack** (Kimodo dropped for Last Rite)

- **Was:** combat clips generated by **Kimodo** (NVIDIA text→motion) via the AssetForge pipeline — the Art Bible §5.4 workflow; `1d` §5/§7's "Mixamo / marketplace / one Kimodo gen" sourcing.
- **Now:** **Kimodo is dropped for Last Rite combat animation** — output quality judged unacceptable by the developer. Clips come from the purchased **Mega Animation Pack v1.8** (Unity Asset Store, publisher Alcaboce, asset id 170897, ~$70; Humanoid rig; per-weapon folders of ~70 clips each; ships an Animator controller + avatar mask) → **Unity Humanoid retarget** onto the chibi rigs → **per-clip Blender fixes** (shoulder rotation to avoid head clipping · foot-curve pinning · ~10–15% amplitude reduction) → **re-time in Blender NLA/Graph Editor** to the AttackDef frame windows (130ms perfect / 330ms block; ≤15% speed change) → **Mecanim Animator** (shared link poses; cross-fades 0.1–0.15s chains / 0.05–0.08s parry deflect / 0.15–0.2s heavy wind-ups; Animation Events on impact frames — the clip-events timing model stands unchanged). License: standard Asset Store EULA — one purchase covers player + all enemies + future games.
- **Why:** the pack buys pro-quality, consistent single-attack clips for the whole roster at a flat one-time cost — exactly the abundant-single-clips supply the linked-clip combo model (D-d7) assumes; Kimodo's generation quality never cleared the bar for combat despite the local server being free. Kimodo stays available for non-combat / other uses, and its Tools docs are retained with status notes.
- **Specced now:** [[1h Last Rite - Player Moveset & Animation Plan]] (clip mapping, tiered list, validation checklist); [[1b Last Rite - Art Bible]] §5.4 status note; `Tools/00–03` deprecation notes.

## 2026-07-31 — Player weapon identity = **the Rite Blade** ("blade now, gauntlets later")

- **Was:** "blade offense" locked as a *lane* (D6) and expanded into the QTE/AP combo (2026-07-15), but the weapon itself had no identity.
- **Now:** a single **one-handed ritual blade ("Rite Blade")** — deliberate, readable strikes that fit the purifier mercy-kill fantasy. A second weapon (**elemental gauntlets**, punches projecting elemental strikes) is **future-proofed via the data-driven `MovesetDef`/`SOAttackDef` layer but NOT built for Game 1** — candidate for post-launch (Chaos Descent) or the Looter Shooter. Base moveset on the locked QTE/AP system: 3-hit light chain (1 AP/hit) · heavy ender (2 AP) · counterattack after Perfect parry.
- **Why:** one weapon keeps the animation + readability budget on the ~6 guardians (the real cost, per the design's cost-truth); the moveset-as-data layer makes a second weapon purely additive later, so deferring it costs nothing now.
- **Specced now:** [[1 Parry Combat - Last Rite]] §"D6 amendment — the Rite Blade & Mani Surge"; execution plan in [[1h Last Rite - Player Moveset & Animation Plan]].

## 2026-07-31 — Elemental layer added: **Mani Surge** (technique #1; provisional Mani meter)

- **Was:** elements were **enemy-side only** (rebirth overlays on guardians — fairness firewall); the ≤2 expressive techniques were unclaimed (candidates: perfect-dodge→counter, chain-parry stance).
- **Now:** the blade **automatically absorbs the rebirth's dominant element** (Descent 2=Bhu · 3=Jal · 4=Vayu · 5=Agni; Descent 1 = neutral) — diegetic, no unlock UI, no tree. **Passive:** elemental particle trails (pure VFX). **Active — Mani Surge:** one signature attack per element (stone shockwave / water wave / wind slash / lava arc) — one shared channel-and-release animation, four VFX sets. **Fuel:** a separate, explicitly **provisional dev-time "Mani meter" fed by Perfect parries ONLY** (Blocks don't feed it); full = one Surge; HUD shows it from Descent 2 only. **Mani Surge claims technique slot #1**; #2 stays reserved (candidate: elemental counter on Perfect parry with full meter).
- **Why:** wires player power-expression into the rebirth spine diegetically while complying with the ≤2-techniques / no-skill-tree lock; Perfect-only feed preserves the greed split; the meter is kept **independent from the Purge meter while that system is in flux** — merge/keep/cut is an explicit gray-box decision once both are playable. Risk logged: player elemental VFX must never obscure enemy telegraphs (keep Surge VFX brief/directional).
- **Specced now:** [[1 Parry Combat - Last Rite]] §"D6 amendment — the Rite Blade & Mani Surge" + the dated note under §"Elemental rebirth spine".

## 2026-07-18 — Timing truth moved onto the animation clip (**I3 inverted**)

- **Was:** attack timing — the five phases *and* the parry/dodge windows — lived as **frames in `SOAttackDef`** (`TimeWindow{StartFrame,EndFrame}`), read each frame by polling the Animator's playback position (`AnimController.GetCurrentAnimationFrame()` + `SOAttackDef.PhaseAt`). Invariant **I3** (`1c` §0) explicitly *forbade* this on the clip: "*gameplay timing must never be authored inside animation clips or anim events… anim events are allowed for pure cosmetics only.*"
- **Now:** phases and defense windows are **authored as animation events on the clip itself** (`AnimStartup / AnimTelegraph / AnimCommit / AnimHit / AnimRecovery`, plus defense-window open/close), fired frame-accurately during the animation update. A **`CombatAnimEventListener`** — a nested `MonoBehaviour` `AddComponent`-ed onto the Animator's GameObject at runtime — forwards each event (with the `SOAttackDef` + the entity) into `CombatEntity`, which raises the matching combat events. **I3 is inverted for windows/phases:** the clip is now the timing truth; the SO keeps only the *non-frame* data (`Damage`, the difficulty start-delay, AP cost, `AllowedDefences` / `NextAllowed`).
- **Why:** (1) **frame-accuracy** — polling in `Update()` can step over a 1-frame branch point on a hitchy frame; a clip event fires exactly once, inside the animation update. (2) Unity's built-in **FBX Events UI is already a scrub-to-author surface** — you scrub the preview and drop the event on the exact frame. (3) A human must place each event by eye so it lands on the right visual moment; **that act *is* the fairness verification**, so it can't be automated away.
- **Specced now:** [[1d Last Rite - Reaction & Feints Spec]] §3–§5, §7; [[1a Last Rite - Code Architecture]] §AS-BUILT.

## 2026-07-18 — Scrub-to-author tool dropped → code port instead

- **Was:** a planned **custom Editor window** (`m1-author`) to scrub an attack clip and stamp its frame windows into `SOAttackDef` (reserved as `1c` §9.3, sized as a paying-for-itself M1 task).
- **Now:** **dropped.** Authoring uses Unity's **built-in** FBX Events UI; the M1 task is retargeted to the **code port** — finish switching the runtime to the event model and migrate the existing CinderScale + Purifier attacks/clips off the old polled-frame path.
- **Why:** the tool would only re-wrap the built-in Events UI around the *same* unavoidable human step (eyeballing each clip so the event fires on the right frame) — pure overhead, no work removed.
- **Specced now:** ship-plan task `m1-author` (retargeted) + combo-plan `df-author`; [[1e Last Rite - Build Roadmap]] M1.

## 2026-07-18 — Difficulty = **delay the defense-window start**, not shrink its width

- **Was:** higher difficulty **shrinks the parry/dodge window width** via a per-tier multiplier.
- **Now:** higher difficulty **delays the window's OPEN** by a per-tier amount (held in the SO / a global `DefenseTuningDef`); the CLOSE stays anchored to the visual impact. Recommendation (playtest-open): **leave Perfect-parry un-delayed** — delay Block + Dodge only — so earned muscle memory stays valid across tiers and the tightening widens the skill gap.
- **Why:** the window close is pinned to the frame the blade lands — shrinking *from the close* makes a visually on-time parry whiff, which reads as unfair. Delaying the *open* punishes **anticipation / panic** presses while "parry when it arrives" stays truthful at every tier. And the delay is a **number in the SO**, so tiers scale by data (not by re-authoring the clip events).
- **Specced now:** [[1d Last Rite - Reaction & Feints Spec]] §3; also the combo difficulty axis in §6.2.

## 2026-07-15 — Combos = **linked pre-made clips**, not generated combo clips

- **Was:** a signature combo was to be generated as **one continuous Kimodo multi-prompt clip** per string (transition-frames blending each step).
- **Now:** combos are **linked pre-made single-attack clips** chained through the **Animator transition graph**. A string is a *data list* of `SOAttackDef`s (`SOAttackStringDef`); the seam between steps is a normal graph transition (`AnimController.Play(next)` / `AnimTransitionData`). **Authored** (fixed order) and **dynamic** (runtime pick from each attack's `NextAllowed` set) chaining are the *same* animation pipeline — only the order-picker differs.
- **Why:** **cost** — a new combo now ≈ **one data asset**, not one generation; single attacks are cheap and abundant (Mixamo / marketplaces / one Kimodo gen each); it unifies authored + dynamic on one pipeline; and the seams were validated empirically in the test combo system (no per-pair transition curation and no seam-gap enforcer were needed — Unity's transition blending handles them).
- **Specced now:** [[1d Last Rite - Reaction & Feints Spec]] §6/§7 (decisions **D-d7/D-d8**).

## 2026-07-15 — Player offense = **QTE-chained, AP-bounded combo**

- **Was:** the player's blade was deliberately **thin** — light strikes for proactive pressure + HP-chip only; "mastery *is* the parry, offense stays minimal, no skill tree."
- **Now:** the blade is a **QTE-chained combo bounded by Attack Points** (Expedition 33-shaped): the player picks *which* moves to chain and in *what order*, lands each with a **forgiving timed-press**, and the combo ends on **AP-exhaust or a missed QTE** (no infinite combos). **Economy split kept clean:** AP = offense input (spent on the player's turn) · Purge = parry-fed defense payoff (the ultimate).
- **Why:** offense expression the original kit lacked, expressed as a **planning** layer (which moves / what order / spend-now-or-bank) rather than twitch execution — and the turn boundary keeps it temporally separate from the parry read, so it *adds* instead of competing. Logged as a **conscious scope expansion** (reactive-half → two-sided combat).
- **Specced now:** [[1d Last Rite - Reaction & Feints Spec]] §6b (decision **D-d9**); the Combo System dev plan (child of the ship plan's `m1-strings`).

## 2026-07-15 — Purge payoff = **player-triggered ultimate** (D5 amendment)

- **Was:** the parry-fed purge meter fills → **auto-stagger → auto-purification-finisher** (D5, original lock).
- **Now:** a full meter **arms a player-triggered Purge ultimate**; firing it consumes the meter, **staggers** the guardian (it forfeits its next turn) and applies **+25% damage** while purged. The purification finisher **moves to the killing blow** (the mercy-kill cinematic).
- **Why:** Expedition 33-style **primed Break** — fill the bar, then *choose when to cash it* with a triggering attack — over Sekiro's auto posture-break; the player authors the burst window. Combos amplify it (a fully-parried string = a fat meter payday).
- **Specced now:** [[1 Parry Combat - Last Rite]] §"D5 amendment — the Purge ultimate".

## 2026-07-04 — Architecture: **MonoBehaviour / animation-integrated**, not a pure-C# sim

- **Was:** the `1a`/`1c` blueprint — a **pure-C# deterministic sim**: `ISimClock`, `CombatSession.Tick`, `IEventBus`, `HealthSystem`, `IAttackScheduler`, asmdef extraction, EditMode tests (invariants **I1** sim-is-pure-C#, **I2** ms-sim-clock, **I7** deterministic tick).
- **Now:** **MonoBehaviour-driven, animation-integrated**, a single `Assembly-CSharp` under `Assets/Scripts/` — no sim clock, no event bus, no asmdefs, no tests. **The animation is the clock** (windows first read from the Animator playback frame; then → clip events, per the 2026-07-18 entry). Turn flow is event + `StateMachineBehaviour` driven (`AttackStateBehaviour` → `CombatSystem.NextTurn`); planners (`IAttackPlanner` / `NPCAttackPlanner` / `PlayerAttackPlanner`) pick moves; `CombatTurnSequencer` handles the parry→counter turn.
- **Why:** solo-dev velocity + the portfolio's "**Unity-only, no pure-C#**" stance; sim and presentation share **one** timeline instead of being kept in sync across two.
- **Specced now:** [[1a Last Rite - Code Architecture]] §AS-BUILT (the blueprint below it stays the *aspiration* for later hardening/extraction).

---

## Confirmed (clarified, not reversed)

- **Telegraph before Commit + the re-telegraph fairness law (2026-07-18).** Phase order is `Startup → Telegraph → [branch: continue / switch / cancel] → Commit → Hit → Recover`. A feint **switch or cancel may only happen before Commit** (Commit = the point of no return, so an attack can only change before it). The switched-to attack must then play **its own** Telegraph → Commit → Hit — **read → switch → re-read** — so a fake can never become an unreactable sucker punch. Fairness stays sacred. See [[1d Last Rite - Reaction & Feints Spec]] §6.3.
- **Damage stays in `SOAttackDef`.** It's a *how much*, not a *when* — no frame identity, and it's what balancing tweaks most; it does not move onto the clip with the timing events.
