# Game 1 "Last Rite" — Player Moveset & Animation Plan

> **The execution plan for the Rite Blade moveset and the pack-based combat animation pipeline — written 2026-07-31.** Companion to [[1 Parry Combat - Last Rite]] (§"D6 amendment — the Rite Blade & Mani Surge" — the design decision this doc executes), [[1b Last Rite - Art Bible]] §5.4 (the pipeline amendment), [[1d Last Rite - Reaction & Feints Spec]] (clip-event timing model + QTE/AP combos) and [[1f Last Rite - Combat Iteration Log]] (the 2026-07-31 pivot entries). Supersedes Kimodo as the combat-animation source for Last Rite; Kimodo docs (`Tools/00–03`) are retained with status notes for reference/other uses.
>
> **Per I10 discipline:** every timing/blend number here is a playtest-open reference.

---

## 1. The weapon (recap — the decision lives in the D6 amendment)

- **The Rite Blade:** a single **one-handed ritual blade** — deliberate, readable strikes; the purifier mercy-kill fantasy.
- **"Blade now, gauntlets later":** elemental gauntlets (punches project elemental strikes) are future-proofed via the data-driven `MovesetDef`/`SOAttackDef` layer, **NOT built for Game 1** — candidate for post-launch (Chaos Descent) or the Looter Shooter.
- **Base moveset** on the locked QTE/AP system ([[1d Last Rite - Reaction & Feints Spec]] §6b): 3-hit light chain (1 AP/hit) · heavy ender (2 AP) · counterattack after a Perfect parry.

## 2. The source pack

| | |
|---|---|
| **Pack** | **Mega Animation Pack v1.8** — Unity Asset Store, publisher **Alcaboce**, asset id **170897**, ~$70 (logged in [[Production Cost Ledger]]) |
| **Rig** | Unity **Humanoid** — retargets onto the chibi rigs for free (the Art Bible §2 rig amendment pays off here) |
| **Structure** | **Per-weapon folders, ~70 clips each** (sword, katana, greatsword/Mandoble, sword+shield/Espada y Escudo, spear+shield/Lanza y Escudo, halberd/Alabarda, scythe/Guadaña, giant sword/Espada Gigante, …); ships an **Animator controller + avatar mask**. Clip names are Spanish (Ataque/Defensa/Impacto/…). |
| **License** | Standard Asset Store EULA — **one purchase covers the player + all enemies + future games.** |

## 3. The pipeline (pack clip → game-ready attack)

1. **Pick the clip** from the per-weapon folder (mapping in §4).
2. **Unity Humanoid retarget** onto the chibi rig.
3. **Per-clip Blender fixes** (the chibi-proportion pass): **shoulder rotation** to avoid head clipping · **foot-curve pinning** · **~10–15% amplitude reduction**.
4. **Re-time in Blender NLA/Graph Editor** to the AttackDef frame windows (**130ms perfect / 330ms block** references; keep any speed change **≤15%** — beyond that, pick a different clip).
5. **Wire into the Mecanim Animator:** shared link poses between chain steps; **cross-fades 0.1–0.15s** (chains) / **0.05–0.08s** (parry deflect) / **0.15–0.2s** (heavy wind-ups); **Animation Events on impact frames** — phases + defense windows stay authored as clip events per [[1d Last Rite - Reaction & Feints Spec]] §7 (nothing in the timing model changes).

### First-session validation checklist (before building the Animator)

- [ ] **Root-motion vs in-place** — confirm per clip; the near-stationary duels (D3) prefer **in-place** (strip/bake root motion where needed).
- [ ] **Retarget one full weapon folder onto the chibi purifier rig** — check arm/head clipping + foot skate (the reduced form of the Art Bible §7 chibi-retarget risk).
- [ ] **Verify the Magia folder** covers the Mani Surge channel-and-release (else log the custom-clip gap, §5).
- [ ] **Re-time tolerance** — confirm candidate clips hit the AttackDef windows within the ≤15% speed-change budget.

---

## 4. Player animation list (tiered) + pack clip mapping

Spanish clip names are the pack's own (per-weapon folder, e.g. the sword/katana folder).

### Tier 1 — core loop (12 animations)

| # | Animation | Pack source | Notes |
|---|---|---|---|
| 1 | Combat idle | **Reposo** (+ **Aburrido** fidget variants) | fidgets are Tier-3 polish |
| 2–4 | Light slash ×3 (the AP chain) | pick the best-linking 3 from **Ataque Debil 01–05** | **Ataque Doble 01–03** are pre-built 2-hit chains — usable for QTE doubles |
| 5 | Heavy ender (2 AP) | **Ataque Fuerte 01–05** (pick one) | the chain capstone |
| 6 | Block impact | **Defensa** | |
| 7 | Perfect-parry deflect | **⚠ GAP — custom:** re-cut **Defensa/Impacto** with sharper timing + VFX/hit-stop | the signature moment — worth the custom work |
| 8 | Counterattack (post-Perfect) | fast **Ataque Debil** or **Ataque Remate** | |
| 9 | Dodge | **Rodada** | |
| 10 | Light hit react | **Impacto Debil** + directionals | |
| 11 | Stagger | **Impacto Fuerte** + directionals | |
| 12 | Collapse / defeat | **Muerte** | |

### Tier 2 — signature (3)

| # | Animation | Pack source | Notes |
|---|---|---|---|
| 1 | Purification finisher | player **Ataque Remate 01–02** + enemy **Muerte**, camera-synced | **not a true paired clip** — the camera sells the sync |
| 2 | Mani Surge channel & release | **verify the Magia folder**; else custom | one shared clip, four VFX sets (D6 amendment scope trick) |
| 3 | Purge ultimate trigger | **try re-timing the Surge channel clip with different VFX first** | only author a bespoke clip if that fails |

### Tier 3 — polish

Blade draw (**Inicio**) · victory pose (**Fin** or a bored variant) · extra dodge directions · block-held loop · idle fidgets (**Aburrido**).

### Known gaps needing custom work

**Perfect-parry deflect** (1 clip) · possibly the **Surge channel cast** (if Magia doesn't fit) · **finisher camera sync** (camera work, not animation).

---

## 5. Guardian weapon-archetype assignments (PROPOSAL — playtest-open)

Each guardian gets a **distinct pack weapon folder** so the ~6 minibosses read as visually distinct movesets for free. Names below are the locked roster ([[1g Last Rite - Enemy Roster]]); folder picks are swappable at zero pipeline cost (everything shares the Humanoid retarget path).

| Guardian (roster) | Pack weapon folder | Why it fits |
|---|---|---|
| **CinderScale** (Agni · chain teacher, built) | one-hand sword (the player's folder) | the first guardian mirrors the player's weapon language — the tutorial read; already on sword-type Mixamo clips |
| **SearCoil** (Agni · telegraphed burn) | **Guadaña** (scythe) | wide low sweeps read as the coil/tail lash; big slow arcs fit the full-beat glow telegraph |
| **BogHerald** (Jal · variable holds) | **Lanza y Escudo** (spear+shield) | pole thrusts/slams read as the staff; the shield sells the liturgist's guarded, held-telegraph posture |
| **SiltWeaver** (Jal · echo feints) | **Katana** | the fastest, most flowing folder — sells the silt-dancer and its ghost→real echo cuts |
| **DartWing** (Vayu · telegraph-displacement) | **Espada y Escudo** (sword+shield) | compact fast strikes between darts; the shield gives a readable "re-set" pose after displacement |
| **ShriekMatriarch** (Vayu · shriek volleys) | **Alabarda** (halberd) | long-reach melee spacing between projectile volleys (volleys themselves are VFX, not clips) |
| *Bosses:* **MireCrown** (Jal) | **Mandoble** (greatsword) | slow, heavy horizontal arcs = Crown Sweep; brute holds |
| **GaleTalon** (Vayu) | unarmed set + **Ataque Patada** kicks | talons, not weapons — kicks + displacement clips sell the dive/rush kit |
| **PyreMaw** (Agni) | **Espada Gigante** (giant sword) | the last gate — the biggest silhouette in the pack |

### Bonus clip uses (cross-roster)

| Pack clips | Use |
|---|---|
| **Ataque Patada** (kicks) | **perilous/unparryable mix-ups** (dodge-only steps inside strings — `AllowedDefenses = Dodge`) |
| **Espada Atorada** (sword stuck) | **whiff-punish / overcommit recovery** states (the exposed Recover on a baited heavy) |
| **Paralysis A/B/C + Temblor** | the **Purge-ultimate stagger state** (guardian purged — forfeits its turn, +25% damage taken) |
| Directional **Impacto** clips | **flank reads** / directional hit-reacts |

---

## 6. Open / playtest-open (I10)

Final clip picks per slot (best-linking is an Animator-graph judgement) · all cross-fade durations · the guardian folder assignments above (§5 is a proposal) · whether Ataque Doble chains replace or supplement single Debil steps for QTE doubles · the custom perfect-parry deflect's exact timing (it must honor the 130ms Perfect reference window).
