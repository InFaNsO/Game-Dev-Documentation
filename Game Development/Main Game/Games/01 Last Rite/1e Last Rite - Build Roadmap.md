# Game 1 "Last Rite" — Build Roadmap (path to ship)

> **The task ladder from the current combat prototype to a shippable game.** Created 2026-07-04, after the combat prototype landed. Anchored on the AS-BUILT architecture (see [[1a Last Rite - Code Architecture]] §"AS-BUILT") and the locked design (see [[1 Parry Combat - Last Rite]]). Each milestone has concrete tasks, an **exit criterion**, and its dependency. Do them in order — later milestones assume earlier ones feel right.

**Definition of done (scope reminder):** a complete, shippable boss-rush — thin explore between duels · **13 bespoke enemies (4 bosses + 9 guardians) + 6 regulars** *(⚠ revised 2026-08-07 from "~6 bespoke + ~5 lesser" — the Bhu family was added once the animation pack removed the moveset-animation cost; see [[1i Last Rite - Bhu Mani Husks]])* · **⚠ REVISED 2026-08-11:** a one-way descent — tutorial + **4 elemental strata in ANY ORDER** (Bhu · Jal · Vayu · Agni, each granting a seal) → **the sealed sanctum → the heart and the Take/Renounce ending fork** *(was: a 5-cycle death/rebirth loop with elemental overlays → a layered true ending; the re-descent loop is cut, see [[1k Last Rite - Lore Bible]] §6 and [[1 Parry Combat - Last Rite]] D2. **⚠ 2026-08-12: the fixed Bhu→Jal→Vayu→Agni sequence is void** — order is free and the sanctum is gated on four seals; see [[1l Last Rite - World & Environment Bible]] §1.7)* · ranking + "The Rite" gauntlet. ~3–5 hr core + heavy replay. No grid / spells / inventory. **⚠ 2026-08-09 additions:** the **Shroud** two-layer health + defensive mani economy · fraying/scarring sanity with the **Husk ending** + **kalpa loop** · **Chaos Descent promoted to core v1** (the endless pillar). See [[1j Last Rite - Shroud, Mani & Sanity]].

---

## M0 — Combat prototype ✅ DONE (2026-07-04)
Playable 1v1 (extensible to #-vs-#): turn-alternating; fighters auto-pick a random moveset attack + target, play the anim, damage at the Hit frame; player parry (RMB) / dodge (LMB) timed to the boss's live **animation-frame** windows; `DebugCombatHUD` shows HP + the current attack window. Rigged Humanoid characters (Purifier, CinderScale), grounded Mixamo anims, consolidated `*_Full.fbx`. Framework: `GameManager` locator · `ISystem`/`IGameEntity` · `AnimController` · `CombatSystem`/`CombatEntity`/`SOAttackDef`/`SOFighterDef` · `AttackStateBehaviour`.

---

## M1 — Combat core complete & feels good  ⟵ **DO NEXT**
> The validation gate. The prototype *moves* but the reaction loop pays nothing out — closing it is the whole game's heartbeat.

- [ ] **Reactions pay out.** Wire `CombatSystem.OnEntityDefense` + `CombatEntity.GetDefenseResult` → outcomes: **parry** negates the incoming hit + feeds the purge meter (**Block = 1×**); **perfect parry** = **2× purge** + hitstop + opens the **counter window**; **dodge** = i-frame the hit (no purge — the greed split). **No stagger on parry** — stagger comes only from the Purge ultimate (2026-07-15 rework, below). (Today it only *classifies* the defense.)
- [ ] **Damage gated by defense.** The Hit-frame `ApplyDamage` must be cancelled by a valid parry/dodge, not fire unconditionally.
- [ ] **Counter input.** During the counter window, player Strike = counter hit (input, not auto — mastery game).
- [ ] **Fight flow / win-lose.** Death ends the duel → result state → restart. There is no end state today.
- [ ] **Purge meter → Purge ultimate** (D5 keystone — amended 2026-07-15, see [[1 Parry Combat - Last Rite]] §"D5 amendment"). Parry-fed only (Block 1× / Perfect 2×, decays idle) → full meter **arms the ultimate** → using it consumes the meter and **purges** the guardian: `Staggered` (forfeits its next attack turn — `TurnOrderScheduler` already skips `Staggered`) + **+25% damage taken** while purged (duration playtest-open; implement the multiplier as an `IDamageStep`, e.g. `PurgedMult`). Purification finisher moves to the killing blow (mercy-kill cinematic). Parry = the win condition.
- [ ] **Combo system — chaining + QTE/AP (combo attacks).** Implement [[1d Last Rite - Reaction & Feints Spec]] §6/§6b: combos are **linked pre-made single-attack clips** chained via the Animator graph (`SOAttackStringDef` = a data list of `SOAttackDef`s; the seam is a normal graph transition — a new combo ≈ one data asset). **Enemy chains** (authored signature spine + dynamic runtime recombination; each step its own read + parry feeds the meter; difficulty = longer chains + tighter windows) are the **M2 CinderScale-feint dependency — build first.** The **player QTE-chained, AP-bounded combo** (§6b; forgiving E33 timed-press; AP = offense input, distinct from the parry-fed Purge meter) is the offense expansion — build after. **Full breakdown: the [Combo System dev plan](https://claude.ai/code/artifact/03408d56-a27b-43ad-bcf4-698c4a5f3b67) (child of the Ship Plan's `m1-strings`).**
- [ ] **Port combat to the animation-event window system** (supersedes the scrub-to-author tool, dropped 2026-07-18). Windows/phases are authored as **animation events on the clip** (Unity's built-in FBX Events UI is the scrub-to-author surface — a human must eyeball each clip regardless, so a custom tool only re-wraps the required step). The runtime already fires clip events that open/close the parry & dodge windows and drive phases, passing `(entity, attackDef)` to `CombatAnimEventListener`, with **difficulty delaying the defense-window start**. Work = finish the code switch + **port the existing attacks/clips** (CinderScale + Purifier) off the old polled-frame path (`GetCurrentAnimationFrame` + `SOAttackDef` `TimeWindow` polling).
- [ ] **Smart action-cam.** Push-to-OTS on telegraph → snap back (Cinemachine). *Design's #1 must-playtest.*
- [ ] **Cleanups.** Fix the inverted `PickNextAttack` `Debug.Assert`; keep the "enemy-only" `NextTurn` test line out; add `CombatEntity.OnDestroy` unregister.

**Exit:** *"you immediately want to do it again."* Until this hits, everything below is premature.

---

## M2 — Vertical slice: one full guardian + ranking
> Prove the data-only guardian pattern and the full feel on a real fight before mass-producing.

- [ ] **CinderScale → real multi-phase boss:** telegraph cues (color/SFX), ≥1 feint/mix-up branch, phase swap at an HP threshold, dedicated finisher sequence.
- [ ] **Per-duel rank:** metrics recorder (hits taken, time, style) → grade → result screen.
- [ ] Validate reaction + camera + purge together on this fight.

**Exit:** one boss fight that's fun start-to-finish and grades you. **Dep: M1.**

---

## M3 — Run / descent spine (makes it a *game*)

> **⚠ RESTRUCTURED 2026-08-11.** This milestone was built around the rebirth loop (guardian-kill → NG+ → re-descend ~5 cycles). **That loop is cut** ([[1 Parry Combat - Last Rite]] D2). The FSM gets *simpler*; the strata content gets heavier (M4).

- [ ] **Top-level game FSM:** Boot → Title → Descent(Explore ⇄ Duel) → **Stratum transition** → … → **Heart → ending fork** → Credits. *(Was: → Rebirth → … → TrueEnding. There is no Rebirth state.)*
- [ ] **Thin explore mode:** third-person walk between duels + interactables (lore, doors, shrines, the fallen purifiers). No stealth/loot/platforming.
- [ ] **⚠ Stratum progression — REVISED 2026-08-12 (free order).** Death = retry at duel start. **All four wings are open from the spine at once**; clearing one grants its **seal** (palette/LUT swap per wing, 4 wings + tutorial). **The sanctum unlocks on all four seals held** — not on a stratum counter. **No NG+ re-descent.** *(Was "clearing a stratum opens the next.")* Structural consequence: `DescentDef` becomes **spine + four independently-enterable branches + a gated sanctum**, closer to a hub than a corridor — [[1a Last Rite - Code Architecture]] §4.3.
- [ ] **⚠ NEW — difficulty keys off `strataCleared`, not `stratumIndex`.** With any wing playable first, a per-stratum authored ladder would hand a new player the hardest wing. ⚠ Recommended, confirm at gray-box — [[1d Last Rite - Reaction & Feints Spec]] §3, [[1l Last Rite - World & Environment Bible]] §6 item 8. Same axis drives the fraying onset curve ([[1j Last Rite - Shroud, Mani & Sanity]] §3.1).
- [ ] **⚠ NEW — the ending fork:** the heart encounter resolves to **Take it** / **Renounce it**, plus the **Husk out** path armed by zero scars. Three authored terminal states, each with its own credits flow ([[1k Last Rite - Lore Bible]] §8, [[1j Last Rite - Shroud, Mani & Sanity]] §3.3).
- [ ] **⚠ 2026-08-09 — Shroud health + death economy + kalpa:** two-layer health (Shroud states + body bar) · shrine checkpoints (re-vest = full Shroud restore + element switch · fraying reset · body heal) · death drops unconverted mani (one retrieval) + the scar countdown · Husk-arming at zero scars → the Husk ending · kalpa reset with light carry-forward ([[1j Last Rite - Shroud, Mani & Sanity]] §1–§4).
- [ ] **Elemental strata** — `None` (tutorial) + an **unordered set** {Bhu · Jal · Vayu · Agni} — as **enemy-side** data only (fairness law: never touch player status). *(These are places now, not per-cycle overlays — the remix path survives only in the Chaos Descent. ⚠ 2026-08-12: written as an ordered ladder `None→Bhu→Jal→Vayu→Agni` until the order was freed.)*
- [ ] **⚠ NEW — the greed-scar triggers** (body damage · gem-buff leaning · sanity-floor pushes) alongside the death countdown, and the **fraying buff** (damage + elemental resistance) — [[1j Last Rite - Shroud, Mani & Sanity]] §3.1/§3.2.
- [ ] **Save system:** RunSave (stratum/room/cleared/seed) + MetaSave (ranks, unlocks, **endings seen — Take / Renounce / Husk**).

**Exit:** a full run playable start → **an ending**, even on placeholder content. **Dep: M2.**

---

## M4 — Content fill
- [ ] **Enemy roster:** **13 bespoke (4 bosses + 9 guardians) + 6 regulars** — data + animation only. Moveset shape: guardians **2 VFX specials + 2 light + 2 heavy (6)**; bosses **2 VFX specials + 5 light + 5 heavy (12)**. Specials are particle systems over generic pack clips, not bespoke animation. **⚠ 2026-07-31: animation sourcing changed — Kimodo dropped for bulk combat clips (quality); the roster animates from the purchased Mega Animation Pack (per-weapon folders → Unity Humanoid retarget → Blender fix/re-time pass). ⚠ 2026-08-07: Kimodo returns in a limited role for ~8 signature motions the pack cannot express (it has no throw and no grab clips at all) — batch them in one session.** Full weapon allocation: [[1h Last Rite - Player Moveset & Animation Plan]] §5. Bhu family designs + movesets: [[1i Last Rite - Bhu Mani Husks]].
- [ ] **Per-stratum elemental sets** (attack flavor/telegraph/palette). *(⚠ 2026-08-11: authored per place, not as per-cycle remix tiers.)*
- [ ] **Descent/room layouts** — **⚠ REVISED 2026-08-12: eight spaces, not five.** Entrance · **processional spine** (the hub; the sanctum is in line of sight from it the whole game) · four wings · **sealed sanctum** · **Chaos Descent pit** · tutorial. **This milestone absorbed the cut re-descent loop's runtime** — the strata now carry the ~3–5hr on their own, so layout volume here is the main length risk, and it just grew. Check against [[1j Last Rite - Shroud, Mani & Sanity]] §6. Layouts follow [[1l Last Rite - World & Environment Bible]] §2.2/Part Three.
- [ ] **⚠ NEW 2026-08-12 — the environment kit + threshold grammar.** Shared structural set (arched doorways · banded mouldings · pillar/bracket sets · stepped plinths · carved niches) reused across all eight spaces + per-wing dressing + per-wing LUT ([[1b Last Rite - Art Bible]] §5.1). **The threshold crystal is a gameplay-legible object, not décor** — one per wing, element-coloured, in an actual doorway so the seal visibly breaks on clear ([[1l Last Rite - World & Environment Bible]] §2.4).
- [ ] **⚠ NEW 2026-08-12 — the environment motion shader.** Emissive vein pulse (tier 1, mandatory) + organic-only vertex swell (tier 2) + small-prop animation (tier 3). **Build once for Bhu, reskin per wing with four different rhythms.** Hard law: **never deform the architecture mesh** — [[1b Last Rite - Art Bible]] §10.
- [ ] **⚠ NEW 2026-08-12 — concept-art debt before any Meshy input:** crystal recolour pass on all four approved wing images (they were generated teal-violet) · Agni's crystal hue decision · Jal's cascade fluid → oxblood · regenerate the stale aerial. [[1l Last Rite - World & Environment Bible]] §5/§6.
- [ ] **⚠ NEW — the fallen purifiers:** set dressing / codex beats along the route (she isn't the first) — the cheapest repeatable environmental storytelling in the game ([[1k Last Rite - Lore Bible]] §6).
- [ ] **⚠ 2026-08-09 — the Shroud asset + mani drops:** dissolve+emissive ablation states on the purifier ([[1b Last Rite - Art Bible]] §9) · raw/elemental mani drop tables across the roster · `AshformMult` + element resist/weakness as `IDamageStep`s ([[1j Last Rite - Shroud, Mani & Sanity]] §2, §7).

**Exit:** full authored content in. **Dep: M3 (spine), M2 (pattern).**

---

## M5 — Meta & polish (shippable)
- [ ] **Sanity/narration:** presentation-only degradation (post-volume weights, audio snapshots, narration variants). Cannot write to the sim — fairness sacred. **⚠ 2026-08-09:** + **sensory narrowing** (fraying-scaled; enemy/telegraph channels exempt) + **HUD unreliability** (abstract widgets only; steady-HUD toggle) + scar-countdown presentation (death-screen knots). The "embrace the hallucination" toggle stays a post-playtest stretch ([[1j Last Rite - Shroud, Mani & Sanity]] §3).
- [ ] **Ranking + "The Rite" gauntlet** (a descent variant: all guardians, one healthbar, score). **⚠ 2026-08-09:** + the promoted **Chaos Descent endless** (sanity-as-run-timer · mixed per-encounter elements · depth × sanity score · unlocked at the first ending; [[1j Last Rite - Shroud, Mani & Sanity]] §5) — no longer post-ship stretch. Campaign ending screen reports kalpa count · deaths · **scars carried** · score, with a named badge for a **zero-scar clear** (⚠ 2026-08-11 — the clean run is the campaign's replay pillar now).
- [ ] **Real UI:** replace `DebugCombatHUD` (IMGUI) with a proper HUD + menus (title/slots/pause/result/lore).
- [ ] **Audio pass · VFX · feel** (hitstop/slow-mo on parry/finisher).
- [ ] **Balance + playtest polish.**

**Exit:** shippable v1.

---

## Optional hardening (only if Game 2 reuse is pursued)
The as-built combat is a single `Assembly-CSharp`. If/when Game 2 needs the reaction tech, revisit the [[1a Last Rite - Code Architecture]] extraction story: split into `BGamer.Core` / `BGamer.Combat` asmdefs, add EditMode tests for the timing math. **Not required to ship Game 1.**

---

## Dependency chain
`M1 (feel) → M2 (one guardian + rank) → M3 (run spine) → M4 (content) → M5 (polish)`. Meta layers (sanity, gauntlet) can slot into M5 or overlap M4. If any task can't be placed on this ladder, re-scope it.
