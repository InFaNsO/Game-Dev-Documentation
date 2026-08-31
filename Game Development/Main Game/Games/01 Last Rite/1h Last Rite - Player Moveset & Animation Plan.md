# Game 1 "Last Rite" — Player Moveset & Animation Plan

> **The execution plan for the player's weapons and the pack-based combat animation pipeline.** Companion to [[1 Parry Combat - Last Rite]] (§"D6 amendment — the player's weapons"), [[1b Last Rite - Art Bible]] §5.4, [[1d Last Rite - Reaction & Feints Spec]] (clip-event timing + QTE/AP combos) and [[1f Last Rite - Combat Iteration Log]]. Enemy movesets for the Bhu family live in [[1i Last Rite - Bhu Mani Husks]].
>
> **⚠ REVISED 2026-08-07 — §1, §2, §4 and §5 were rewritten.** The single one-handed Rite Blade became **two transforming trick weapons**; the guardian assignment table was replaced wholesale; two factual errors about the pack were corrected. The superseded text is recorded in [[1f Last Rite - Combat Iteration Log]].
>
> **Per I10 discipline:** every timing/blend number here is a playtest-open reference.

---

## 1. The weapons — two transforming trick weapons

The player's armaments are **transforming trick weapons, each with two forms**. Bloodborne's model: variety comes from transformation, not weapon count. Each form is a complete pack moveset, so two weapons yield **four distinct movesets** for four owned animation sets.

> **⚠ Acquisition (2026-08-07): the player starts with the RITE BLADE ONLY**, and unlocks its attacks progressively as they level rather than receiving all 8 at once. **The Pyre Censer is deferred** — it is fully designed and its sets are reserved, but when and how it enters the game is an open question. A second armament is purely additive through the `MovesetDef`/`SOAttackDef` layer, so deferring it costs nothing. **This means M2's vertical slice only needs the Katana and Halberd sets.**
>
> **⚠ No Mani-based player attacks.** The Mani Surge and the whole elemental player layer are **cut from Game 1 scope** — no elemental absorption, no signature elemental attack, no elemental trails, no Mani meter. Elements are enemy-side only; **Purge is the player's only meter.**

### 1.1 The Rite Blade · *Samskara* — Katana ↔ Halberd

![[Weapon Rite Blade Dual Form Concept.jpg]]

A slender ritual katana whose **cord-wrapped grip telescopes rearward** in three nested segments joined by silver ratchet collars, turning the sword into a naginata. **The blade never changes** — only the grip extends. Historically literal (naginata and katana blades were reforged into one another), so the mechanism needs no explaining.

- **Sword Mode** (one-handed, Katana set) — deliberate, readable single strikes. The purifier mercy-kill fantasy.
- **Polearm Mode** (two-handed, Halberd set) — sweeping arcs, reach, leverage.

### 1.2 The Pyre Censer · *Dhupa* — Club ↔ Maul

![[Weapon Pyre Censer Dual Form Concept.jpg]]

A temple censer forged into a war hammer. The head is a **solid pierced block** with embers burning inside — flat striking face one side, tapered spike the other. The haft is **one fixed length** with a silver track running down it; the head **slides along that track** and locks at two positions.

- **Choked Mode** (one-handed, Club set) — head locked mid-haft, gripped below it, spare haft protruding above. Fast and controlled.
- **Extended Mode** (two-handed, Maul set) — head slid to the top and locked, gripped at the base. Maximum leverage.

### 1.3 The transformation law (design rule, learned the hard way)

Every transformation must **conserve mass** and must never "bloom" — opening a shape spreads its mass and reads as *lighter*, which kills the weight of a heavy form. The three mechanisms that survive contact with the eye are **split laterally**, **telescope the haft**, and **slide the head**. Each of the two weapons uses a different one, so their tricks never blur together.

### 1.4 Transformation as a combat verb

Transforming is **a chain step, not a menu action**. It rides the AP-combo planning layer ([[1d Last Rite - Reaction & Feints Spec]] §6b): the transform-attack costs AP, deals damage, and switches form mid-combo so the remainder of the string draws from the other moveset. Open in Sword Mode for two heavy cuts, transform into Polearm Mode, finish with wide sweeps.

It lives entirely on the **player's turn**, so it never competes with the enemy-phase parry read — the same reason the AP combo and the reaction layer coexist fairly.

> **"Blade now, gauntlets later"** still stands: elemental gauntlets remain future-proofed via the data-driven `MovesetDef`/`SOAttackDef` layer and are **NOT built for Game 1**.

---

## 2. The source pack

| | |
|---|---|
| **Pack** | **Mega Animation Pack** — Unity Asset Store, publisher **Alcaboce**, asset id **170897** (logged in [[Production Cost Ledger]]) |
| **Location** | `games/01-Last-Rite/Assets/Mega Animation Pack` |
| **Rig** | Unity **Humanoid** — retargets onto the project rigs for free |
| **Structure** | **22 combat-capable per-weapon folders, ~79 clips each.** Also ships **meshes and prefabs for 30 weapons**, an Animator controller and an avatar mask. |
| **License** | Standard Asset Store EULA — **one purchase covers the player + all enemies + future games.** |

### 2.1 Pack facts that correct earlier assumptions

- **⚠ There is no standalone one-hand sword folder.** The earlier text listed one; the actual sets run 002 Greatsword → 003 Katana → 004 Sword and Shield. **Katana is the Rite Blade's set.**
- **⚠ There are no throw or grab clips anywhere in the pack** (only `Fishing - Throw`). Any throw, lob or grapple attack needs a custom clip.
- **Clip names are English**, not Spanish: `Attack Weak 01–07`, `Attack Strong 01–07`, `Attack Double`, `Attack Spin`, `Attack Finisher`, `Attack Running`, `Attack Walking`, `Attack Kick`, `Hit Weak/Strong` (+ 4 directions each), `Death`, `Paralysis A/B/C`, `Trembling`, `Confusion`, `Blinded`, `Deafened`, `Idle`, `Bored 01–05`.
- **`Paralysis A/B/C` and `Trembling` map directly onto `FighterPhase.Staggered`**, which `TurnOrderScheduler` already skips — the Purge-ultimate stagger state costs no new animation.
- **Tool sets are fully combat-capable.** Shovel and Pickaxe each carry 5 weak, 5 strong, spins, a finisher, full directional hit reactions and a death.

---

## 3. The pipeline (pack clip → game-ready attack)

1. **Pick the clip** from the per-weapon folder (mappings in §4 and §5).
2. **Unity Humanoid retarget** onto the target rig.
3. **Per-clip Blender fixes:** shoulder rotation to avoid head clipping · foot-curve pinning · ~10–15% amplitude reduction.
4. **Re-time in Blender NLA/Graph Editor** to the AttackDef frame windows (**130 ms perfect / 330 ms block** references; keep any speed change **≤15%** — beyond that, pick a different clip).
5. **Wire into the Mecanim Animator:** shared link poses between chain steps; cross-fades 0.1–0.15 s (chains) / 0.05–0.08 s (parry deflect) / 0.15–0.2 s (heavy wind-ups); Animation Events on impact frames — phases and defense windows stay authored as clip events per [[1d Last Rite - Reaction & Feints Spec]] §7.

### First-session validation checklist

- [ ] **Root-motion vs in-place** — confirm per clip; the near-stationary duels prefer **in-place**.
- [ ] **Retarget one full weapon folder** onto the player rig — check arm/head clipping and foot skate.
- [ ] **Retarget one attack onto a husk** — the Deadfall's hunch and the Mourner's stoop will be straightened by upright-human clips. Prove the additive-pose-layer fix on one attack before building four movesets.
- [ ] **Re-time tolerance** — confirm candidate clips hit the AttackDef windows within the ≤15% budget.

---

## 4. Player moveset — 8 attacks per weapon

**The structure (locked 2026-08-07):** every form carries **2 light + 2 heavy** attacks. Two forms per weapon = **8 attacks per weapon**; two weapons = **16 player attacks** at full build-out. Lights cost **1 AP**, heavies cost **2 AP**, per the QTE/AP system in [[1d Last Rite - Reaction & Feints Spec]] §6b.

**These 16 are the full ceiling, not the starting kit.** The player begins with the Rite Blade and a subset of its attacks, unlocking the rest by level; the Pyre Censer's 8 arrive later still (§1). Unlock pacing and the starting four are open — see §6.

![[Player Final Ref All sides.jpg]]

### 4.1 The Rite Blade — 8 attacks

| Form | Slot | Attack | Pack clip |
|---|---|---|---|
| **Sword** | Light 1 | **Rite Cut** | `Katana - Attack Weak 01` |
| Sword | Light 2 | **Ash Slip** | `Katana - Attack Weak 02` |
| Sword | Heavy 1 | **Severing Rite** | `Katana - Attack Strong 01` |
| Sword | Heavy 2 | **Drawn Silence** | `Katana - Attack Strong 02` |
| **Polearm** | Light 1 | **Reaching Cut** | `Halberd - Attack Weak 01` |
| Polearm | Light 2 | **Haft Sweep** | `Halberd - Attack Weak 02` |
| Polearm | Heavy 1 | **Wide Rite** | `Halberd - Attack Strong 01` |
| Polearm | Heavy 2 | **Pilgrim's Fall** | `Halberd - Attack Strong 02` |

### 4.2 The Pyre Censer — 8 attacks

| Form | Slot | Attack | Pack clip |
|---|---|---|---|
| **Choked** | Light 1 | **Quick Toll** | `Club - Attack Weak 01` |
| Choked | Light 2 | **Backhand Swing** | `Club - Attack Weak 02` |
| Choked | Heavy 1 | **Spike Drive** | `Club - Attack Strong 01` |
| Choked | Heavy 2 | **Censer Spin** | `Club - Attack Spin 01` |
| **Extended** | Light 1 | **Low Sweep** | `Maul - Attack Weak 01` |
| Extended | Light 2 | **Rising Strike** | `Maul - Attack Weak 02` |
| Extended | Heavy 1 | **Pyre Fall** | `Maul - Attack Strong 01` |
| Extended | Heavy 2 | **Ash Break** | `Maul - Attack Strong 02` |

### 4.3 Shared non-attack clips (per weapon set)

Combat idle (`Idle` + `Bored 01–05` fidgets) · block impact · **perfect-parry deflect (⚠ custom — the signature moment)** · counterattack after a Perfect parry (a fast `Attack Weak`) · dodge (`Roll`) · light hit react (`Hit Weak` + directionals) · stagger (`Hit Strong` + directionals) · collapse (`Death`).

### 4.4 Known custom-animation gaps

| Gap | Count | Source |
|---|---|---|
| **Transform — Rite Blade** (grip telescopes) | 1 | Kimodo |
| **Transform — Pyre Censer** (head slides) | 1 | Kimodo |
| **Perfect-parry deflect** | 1 | re-cut a pack block clip with sharper timing |
| Purification finisher camera sync | — | camera work, not animation |

**These two transform clips are the entire financed new-animation budget for the player.** No new pack sets are being purchased.

---

## 5. Enemy weapon allocation

Every bespoke enemy gets a **distinct pack set** so movesets read as visually distinct for free. The four sets in §1 are **player-exclusive** and appear nowhere below.

| Enemy | Element · Tier | Pack set | Why it fits |
|---|---|---|---|
| **PyreMaw** *the Furnace Warden* | Agni · boss | **Giant Sword** | The biggest silhouette in the pack for the last gate |
| **MireCrown** *the Drowned Regent* | Jal · boss | **Greatsword** | Slow heavy horizontal arcs = Crown Sweep; regal weight |
| **GaleTalon** *the Hollow Gale* | Vayu · boss | **Combat** (unarmed) | Talons, not weapons — punches and kicks sell the dive kit |
| **Reliquary Husk** *Dhatugarbha* | Bhu · boss | **Magic** | `Magic Heal 01/02` land exactly on Consecrate and Rite of Return |
| ~~**CinderScale**~~ *the First Guardian* | Agni · miniboss | **— parked —** | **⚠ Prototype only (2026-08-07)** — built as a combat test enemy, not shipped content. Model may be reused; weapon set unassigned. **Sword and Shield is free.** |
| **SearCoil** *the Molten Fang* | Agni · miniboss | **Axe** | Brutal one-handed chops read as the coil lash |
| **BogHerald** *the Liturgist* | Jal · miniboss | **Pickaxe** | Long haft, heavy held overheads — reskin as a ritual crozier |
| **SiltWeaver** *the Silt-Dancer* | Jal · miniboss | **Dual Swords** | Two blades double the afterimages for the ghost→real echo cuts |
| **DartWing** *the Streak* | Vayu · miniboss | **Dagger** | The fastest, most compact strikes in the pack |
| **ShriekMatriarch** *the Cracked Bell* | Vayu · miniboss | **Bow** | Her volleys become real animations instead of pure VFX |
| **Garland Husk** *Asthikumbha* | Bhu · husk | **Shovel** | A gravedigger's spade makes Grave-Scoop literal |
| **Mourner Husk** *Avarana* | Bhu · husk | **Spear and Shield** | Reach plus a guarded vigil posture; shield reskinned as a grave-slab |
| **Deadfall Husk** *Chitakashtha* | Bhu · husk | **Scythe** | Bone blade on a driftwood haft — reaper, harvest and funeral at once |

**The six regulars** share the **Combat** (unarmed) set with element VFX, or borrow one or two clips from their stratum's guardian. At 1 phase and one to two clips each, a unique set is wasted on them. **Slingshot** is available if MireSpitter's spit should be a real projectile.

**Unallocated slack:** Torch · Crossbow · Revolver · Shotgun · Slingshot.

### 5.1 Enemy moveset shape (locked 2026-08-07)

| Tier | Structure | Total |
|---|---|---|
| **Regular / husk** | 2 special (VFX-driven) + 2 light + 2 heavy | **6** |
| **Boss** | 2 special (VFX-driven) + 5 light + 5 heavy | **12** |

**Specials are particle systems, not animations.** The body motion comes from a generic pack clip — a spin, a ground slam, a kneel, a raised-arm cast — and the VFX carries the entire identity. This is what keeps a full enemy family at near-zero new animation cost. Every special is depicted on its enemy's weapon-and-moves reference sheet in [[1i Last Rite - Bhu Mani Husks]].

### 5.2 Bonus clip uses (cross-roster)

| Pack clips | Use |
|---|---|
| `Attack Kick 01/02` | Perilous/unparryable mix-ups (`AllowedDefenses = Dodge` steps inside strings) |
| `Paralysis A/B/C`, `Trembling` | The Purge-ultimate stagger state (enemy forfeits its turn, +25% damage taken) |
| Directional `Hit Weak/Strong` | Directional hit-reacts |
| `Attack Double`, `Attack Spin` | Multi-hit chain steps and area specials |

---

## 6. Open / playtest-open (I10)

Final clip picks per slot (best-linking is an Animator-graph judgement) · all cross-fade durations · whether the transform-attack costs 1 or 2 AP · whether `Attack Double` chains replace or supplement single weak steps for QTE doubles · the custom perfect-parry deflect's exact timing (must honor the 130 ms Perfect reference window) · whether the boss 5+5 shape holds or trims to 4+4 after the first boss is playable.

**Acquisition & progression (opened 2026-08-07):** which of the Rite Blade's 8 the player starts with · how attacks unlock against level, and whether "level" is even the right gate for a game whose stated progression is *mastery* · when and how the Pyre Censer is acquired · whether the transform itself is available from the start or is the first unlock.
