# Game 1 "Last Rite" — Build Roadmap (path to ship)

> **The task ladder from the current combat prototype to a shippable game.** Created 2026-07-04, after the combat prototype landed. Anchored on the AS-BUILT architecture (see [[1a Last Rite - Code Architecture]] §"AS-BUILT") and the locked design (see [[1 Parry Combat - Last Rite]]). Each milestone has concrete tasks, an **exit criterion**, and its dependency. Do them in order — later milestones assume earlier ones feel right.

**Definition of done (scope reminder):** a complete, shippable boss-rush — thin explore between duels · ~6 bespoke multi-phase guardians + ~5 lesser · 5-cycle death/rebirth loop (elemental overlays Bhu→Jal→Vayu→Agni) → layered true ending · ranking + "The Rite" gauntlet. ~3–5 hr core + heavy replay. No grid / spells / inventory.

---

## M0 — Combat prototype ✅ DONE (2026-07-04)
Playable 1v1 (extensible to #-vs-#): turn-alternating; fighters auto-pick a random moveset attack + target, play the anim, damage at the Hit frame; player parry (RMB) / dodge (LMB) timed to the boss's live **animation-frame** windows; `DebugCombatHUD` shows HP + the current attack window. Rigged Humanoid characters (Purifier, CinderScale), grounded Mixamo anims, consolidated `*_Full.fbx`. Framework: `GameManager` locator · `ISystem`/`IGameEntity` · `AnimController` · `CombatSystem`/`CombatEntity`/`SOAttackDef`/`SOFighterDef` · `AttackStateBehaviour`.

---

## M1 — Combat core complete & feels good  ⟵ **DO NEXT**
> The validation gate. The prototype *moves* but the reaction loop pays nothing out — closing it is the whole game's heartbeat.

- [ ] **Reactions pay out.** Wire `CombatSystem.OnEntityDefense` + `CombatEntity.GetDefenseResult` → outcomes: **parry** negates the incoming hit + staggers the attacker + opens a **counter window**; **perfect parry** = bigger reward/hitstop; **dodge** = i-frame the hit. (Today it only *classifies* the defense.)
- [ ] **Damage gated by defense.** The Hit-frame `ApplyDamage` must be cancelled by a valid parry/dodge, not fire unconditionally.
- [ ] **Counter input.** During the counter window, player Strike = counter hit (input, not auto — mastery game).
- [ ] **Fight flow / win-lose.** Death ends the duel → result state → restart. There is no end state today.
- [ ] **Purge meter → purification finisher** (D5 keystone). Parry-fed meter; break → `Stagger` → finisher micro-sequence. Parry = the win condition.
- [ ] **Window tuning + authoring tool.** Scrub-to-author editor helper: stamp frame windows (Hit / Parry) into `SOAttackDef` from the anim preview; tune each attack to its real impact frame.
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
- [ ] **Guardian roster:** ~6 bespoke multi-phase + ~5 lesser (data + animation; the AssetForge→Kimodo pipeline is proven — see game brain).
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
