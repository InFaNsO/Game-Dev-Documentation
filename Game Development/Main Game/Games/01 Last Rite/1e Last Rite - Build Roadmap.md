# Game 1 "Last Rite" — Build Roadmap (path to ship)

> **The task ladder from the current combat prototype to a shippable game.** Created 2026-07-04, after the combat prototype landed. Anchored on the AS-BUILT architecture (see [[1a Last Rite - Code Architecture]] §"AS-BUILT") and the locked design (see [[1 Parry Combat - Last Rite]]). Each milestone has concrete tasks, an **exit criterion**, and its dependency. Do them in order — later milestones assume earlier ones feel right.

**Definition of done (scope reminder):** a complete, shippable boss-rush — thin explore between duels · ~6 bespoke multi-phase guardians + ~5 lesser · 5-cycle death/rebirth loop (elemental overlays Bhu→Jal→Vayu→Agni) → layered true ending · ranking + "The Rite" gauntlet. ~3–5 hr core + heavy replay. No grid / spells / inventory.

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

## M3 — Run / rebirth spine (makes it a *game*)
- [ ] **Top-level game FSM:** Boot → Title → Descent(Explore ⇄ Duel) → Rebirth → … → TrueEnding → Credits.
- [ ] **Thin explore mode:** third-person walk between duels + interactables (lore, doors, the heart). No stealth/loot/platforming.
- [ ] **Rebirth loop:** death = retry at duel start; guardian-kill = NG+ (palette-shift, ~5 cycles).
- [ ] **Elemental overlays** (None→Bhu→Jal→Vayu→Agni) as **guardian-side** data overlays only (fairness law: never touch player status).
- [ ] **Layered true ending** across the cycles.
- [ ] **Save system:** RunSave (tier/room/cleared/seed) + MetaSave (ranks, unlocks, true-ending flags).

**Exit:** a full run playable start → true ending, even on placeholder content. **Dep: M2.**

---

## M4 — Content fill
- [ ] **Guardian roster:** ~6 bespoke multi-phase + ~5 lesser (data + animation; the AssetForge→Kimodo pipeline is proven — see game brain). **⚠ 2026-07-31: animation sourcing changed — Kimodo dropped for Last Rite combat clips (quality); the roster animates from the purchased Mega Animation Pack v1.8 (per-weapon folders → Unity Humanoid retarget → Blender fix/re-time pass). Clip mapping + proposed guardian weapon archetypes: [[1h Last Rite - Player Moveset & Animation Plan]].**
- [ ] **Per-tier elemental remix sets** (attack flavor/telegraph/palette overlays).
- [ ] **Descent/room layouts** for all strata.

**Exit:** full authored content in. **Dep: M3 (spine), M2 (pattern).**

---

## M5 — Meta & polish (shippable)
- [ ] **Sanity/narration:** presentation-only degradation (post-volume weights, audio snapshots, narration variants). Cannot write to the sim — fairness sacred.
- [ ] **Ranking + "The Rite" gauntlet** (a descent variant: all guardians, one healthbar, score).
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
