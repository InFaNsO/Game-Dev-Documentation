# Game 1 "Last Rite" — Echoes (learned passives on the mani budget)

> **The player's build layer: passives learned from the Husks and bound with mani — designed 2026-10-11.** The developer asked whether Last Rite had anything like Expedition 33's Pictos / Lumina. It had nothing: the docs had ruled it out ("no inventory", "the reserve is one scalar + an element tag"). This doc adds it. Companion to [[1j Last Rite - Shroud, Mani & Sanity]] (§2 the mani economy this rides on, §3.2 greed scars, §4 kalpa), [[1i Last Rite - Bhu Mani Husks]] (§1.4 Husk passives), [[1a Last Rite - Code Architecture]] (§0 / §2 / §4.7 / §6 amended), [[1e Last Rite - Build Roadmap]] (M3 / M4 tasks), [[1f Last Rite - Combat Iteration Log]] (the 2026-10-11 entry).
>
> **Four developer decisions anchor this doc (2026-10-11):**
> 1. The "no inventory" rule is lifted for exactly one structure: the learned Echo list.
> 2. **Husks gain passives of their own**, and each Echo is the player-side form of one.
> 3. Echoes **ship at launch**, across **all four wings**.
> 4. The name is **Echo** (*Smriti*).
>
> Numbers are reference values, **ALL playtest-open**, per portfolio discipline.

---

## 0. The short version

Every Husk family carries a passive: a habit that outlived the person inside. Purify enough of one family and she **remembers** it as an **Echo**. At a shrine she **binds** Echoes by holding mani in them, as many as she can afford. Mani is also the only thing that repairs the Shroud, and **healing burns it**.

**So every heal is paid for out of the build.** A player who parries cleanly keeps their mani and runs a fuller build; a player who patches mid-fight trades power for survival. *The healing IS the disease* ([[1j Last Rite - Shroud, Mani & Sanity]] §0), turned into a choice on the shrine screen.

**The E33 reference, and what we kept.** In Expedition 33 you equip a Picto (gear: stats + a passive); after 4 battles its passive is learned as a Lumina, which any character can equip by spending Lumina points. **We keep only the learned half** — no gear, no item drops, no stats on equipment — and **replace Lumina points with mani**, the same currency that heals.

---

## 1. Naming (the two-tier law — [[14 Naming Glossary]])

| Common (UI) | Formal (lore, codex) | Meaning |
|---|---|---|
| **Echo** | ***Smriti*** (SMRI-tee) | "that which is remembered" |

**Verbs:** an Echo is **remembered** (learned) · at a shrine you **bind** it (mani goes in) and **release** it (the mani comes back) · Shroud repair **burns** mani.
**UI labels:** *Echoes* · *Bind* · *Release* · the *Binding* bar · *the Brink* (§3.3).

**Rejected names — logged so they are never relitigated:**
- *Samskara* — already the Rite Blade's formal name ([[1h Last Rite - Player Moveset & Animation Plan]] §1.1).
- *Relic* — collides with the Reliquary Husk and the boss-relic question ([[1j Last Rite - Shroud, Mani & Sanity]] §8.3).
- *Remnant* — a candidate name for the endless score ([[1j Last Rite - Shroud, Mani & Sanity]] §5).
- *Vasana* — right in philosophy (latent impressions carried across lives), but means "lust" in everyday Hindi.
- *Threads · Sutra* — fits the woven Shroud and the knot countdown, but English players hear "Kama Sutra".
- *Vows · Vrata* — fits the restraint theme, but a vow is chosen, not inherited from the dead.

**It is already in the lore.** The Vayu wing's researchers "produced echoes of personhood without proof of a person inside" ([[1k Last Rite - Lore Bible]] §5, Vayu). Vayu is the natural place for the game to name the mechanic in-world (⚠ open, writing pass).

---

## 2. Fiction

- **Every Husk was a person** — this facility's own staff ([[1k Last Rite - Lore Bible]] §3, [[1i Last Rite - Bhu Mani Husks]] §1). **Its passive is the habit that outlived them:** the gravedigger still keeps time; the mourner never left the grave.
- **Tearing out the gem completes their funeral** ([[1i Last Rite - Bhu Mani Husks]] §1.1). **Carrying their Echo is how she remembers them — and how she becomes them, one habit at a time.** Mani is Husk-substance; an Echo is a Husk's *way*, held in it.
- **Dramatic irony, not a reveal** ([[1j Last Rite - Shroud, Mani & Sanity]] §8.2): the player knows from the first Echo what she is collecting. The tension is whether she can stop.
- **⚠ Open lore hook:** at the **Renounce** ending she releases every Echo — the bound mani goes back to the ground. A free, wordless beat if the writing pass wants it.

---

## 3. The economy

### 3.1 Where mani sits

| State | How it gets there | On death | Comes back? |
|---|---|---|---|
| **Carried** | picked up from purifications | **drops where she fell, one retrieval** ([[1j Last Rite - Shroud, Mani & Sanity]] §4) | by retrieval |
| **Bound** | bound into Echoes at a shrine | **safe** — counts as converted, like the worn Shroud | in full, on release at a shrine |
| **Burned** | spent on Shroud repair (field patch or shrine re-vest) | — | **never** (gone for the kalpa) |

### 3.2 Rules

- **Bind and release only at shrines** — the same beat as the re-vest, the only place to switch element ([[1j Last Rite - Shroud, Mani & Sanity]] §2). **The shrine is the build screen.** No build changes in the field.
- **No slots.** Bind any number of Echoes; each has its own cost, so strong Echoes are expensive.
- **The Rite allowance:** a small free binding capacity granted by the Kavach Rite. It can't be burned or dropped. **Nobody ever runs with zero Echoes.**
- **Selling the build:** at a shrine with an empty reserve, she can release Echoes to pay for a re-vest. The hardest choice the system offers, and the one that makes it bite.
- **Binding ignores element (v1).** Raw and elemental mani bind at face value. Element still matters only for the Shroud re-weave ([[1j Last Rite - Shroud, Mani & Sanity]] §2).
- **Binding costs no sanity.** Burning keeps its existing cost (a field patch costs fraying, [[1j Last Rite - Shroud, Mani & Sanity]] §3.1).

### 3.3 The Brink

- **A marked line on the Binding bar.** A build past it is **"over-leaning on gem buffs"** — the greed-scar trigger [[1j Last Rite - Shroud, Mani & Sanity]] §3.2 named but never defined. Above the Brink: more power, and it ticks toward scars. This keeps a zero-scar clean run meaning restraint, and stops skilled players snowballing for free.
- **⚠ Launch status (2026-10-11): whether sanity and scars ship at launch is NOT decided.** So the Brink is built with a mode switch:
  - **`Cap`** (default until scars ship) — binding stops at the line.
  - **`ScarTrigger`** — binding past the line is allowed and counts toward greed scars.
  - Flip the switch when scars ship; no other change needed.

### 3.4 Kalpa and endless

- **Remembered Echoes and purify tallies carry into the next kalpa** (MetaSave). **Bound and carried mani reset, and so does the Rite allowance.** This stays inside "light meta, not a roguelite grind" ([[1j Last Rite - Shroud, Mani & Sanity]] §4): Kalpa 2 starts with knowledge, not power — she remembers more than she can afford.
- **Endless (Chaos Descent):** every remembered Echo is available; the run's own mani funds them. ⚠ Open: does binding past the Brink speed the sanity timer ([[1j Last Rite - Shroud, Mani & Sanity]] §5)?
- **Leaderboards:** record the bound Echo set with each endless run, alongside the existing metadata ([[1j Last Rite - Shroud, Mani & Sanity]] §5.1). Boards do **not** segregate by build.

---

## 4. Learning Echoes

- **Husk families:** remembered after **N = 3** purifications of that family. **Variants share their family's Echo** — one Echo per family, not per variant.
- **Bosses:** remembered on the **first** purification.
- **Rite starters:** **three**, given by the mentor in the tutorial ([[1k Last Rite - Lore Bible]] §4). They teach binding before the first Husk Echo is remembered.
- **Why 3, not E33's 4:** Bhu holds ~50 Husks over 26 encounter sites, so N = 3 lands each family's Echo early in the wing, **while the player is still fighting that family**. Playtest-open.
- **Feedback (launch):** a toast on each purification — *"Garland Husk 2/3"* — then *"Echo remembered: Gravedigger's Rhythm"* and the codex line unlocks. *(Stretch: the gem's light visibly flows into her marks.)*

---

## 5. Husk passives — the enemy side

No Husk has a passive today (2026-10-11). Each family gets one; its Echo is the same idea turned to her side, **with the same name** — you fight it first, then you wear it.

**Laws:**
1. **One passive per family, fixed.** All variants carry it.
2. **It shows on the body** — the gem's pulse or brightness, the adornment, or a visible stack. The gem is already the telegraph ([[1i Last Rite - Bhu Mani Husks]] §1.1); the passive speaks the same language.
3. **It may change the Husk's own tempo, chain length or economy pressure** — that is where the game puts its depth ([[1j Last Rite - Shroud, Mani & Sanity]] §6). **It may never narrow a window, hide a telegraph or delay a real cue** — the fairness firewall ([[1j Last Rite - Shroud, Mani & Sanity]] §3.4).
4. **First contact stays readable:** a family's passive debuts in a solo fight ([[1j Last Rite - Shroud, Mani & Sanity]] §6 rule 1).

---

## 6. Echo content laws — the player side

1. **No Echo touches parry, dodge or jump windows, telegraph timing, or cue visibility.** Windows move only through the menu difficulty ([[1d Last Rite - Reaction & Feints Spec]] §3). Echoes change what a defence *gives* you, never how hard it is.
2. **Offense only through existing payouts** — AP, Purge, counter damage, combo length. **No new attack, no new meter, no elemental player offense.** The [[1j Last Rite - Shroud, Mani & Sanity]] §2 scope guard stands: Purge is still the only offensive meter, AP the only offense input.
3. **No free healing.** An Echo may make repair cheaper or stronger per mani; it may never repair the Shroud from nothing, and **never heals the body** (shrine rest only, [[1j Last Rite - Shroud, Mani & Sanity]] §1.1).
4. **A flat list, not a tree** — the [[1a Last Rite - Code Architecture]] §6 "no skill tree" guardrail stands.
5. **Each wing's set covers different jobs** — Parry · Economy (AP) · Purge · Shroud · Element · Risk (Ashform / fraying) — so the best builds mix wings and every wing is worth clearing.
6. **One codex line per Echo**, about the person it came from. Cheap lore delivery.
7. **Readable procs:** a proc is a Cue → a small flash on her marks. **Never over a telegraph** (the juice fairness law).

---

## 7. The catalog

**Target at launch: ~20–24 Echoes** — 3 Rite starters + one per Husk family + one per boss, across four wings. *(All names below are working names; each gets a formal Sanskrit name in the writing pass.)*

### 7.1 Rite starters (from the mentor)

| Echo | Effect | Cost | Job |
|---|---|---|---|
| **Steady Hand** | a Perfect parry adds +5 Purge to the attacker (on top of the base) | 5 | Purge |
| **Thrift** | field patches cost 25% less mani | 5 | Shroud |
| **Ember Rim** | while the Shroud is Unbound or Ashform, counters deal +15% | 5 | Risk |

### 7.2 Bhu (worked examples)

Husk side is canon in [[1i Last Rite - Bhu Mani Husks]] §1.4; numbers live here.

| Family | On the Husk | Her Echo | Cost | Job |
|---|---|---|---|---|
| **Garland** · *the Metronome* | **Gravedigger's Rhythm** — each consecutive hit it lands unparried deals +10% (max +30%); a parry resets it; the gem brightens per stack | **Gravedigger's Rhythm** — each consecutive Perfect parry in one Enemy Phase adds +10% to your next counter (max +30%); taking a hit resets it | 10 | Parry |
| **Mourner** · *the Off-Beat* | **Long Vigil** — each time it is dodged, its next attack hangs longer (max 2 stacks); the censer-gem's swing visibly slows | **Long Vigil** — dodging a *hanging* attack refunds 1 AP (dodges normally pay nothing — this blunts the economy attack) | 15 | Economy |
| **Deadfall** · *the Liar* | **Splinter Crown** — each feint the player bites on adds a leaf to its crown; at 3 leaves its next string is one attack longer; holding through a feint strips a leaf | **Seen Through** — holding through a feint adds +5 Purge to that enemy | 10 | Purge |
| **Reliquary** · *the Multiplier* (boss) | **Consecration** — its existing identity: everyone else's chains run longer while it lives | **Consecration** — each Husk purified this encounter gives your next AP combo one free step (max 2) | 20 | Economy |

*(Deadfall's Echo keeps a different name because the player's side of a lie is seeing through it.)*

### 7.3 Jal — 5 slots

Roster per the 2026-10-10 decision: **BogShambler, BogHerald, SiltWeaver, MireSpitter** are lesser families (3 variants each); **MireCrown** is the boss. Identity seeds for the design pass: SiltWeaver's ghost→real afterimage cuts · BogHerald *the Liturgist*'s held overheads · MireSpitter's ranged spit · MireCrown *the Drowned Regent*'s Crown Sweep. Jal's tempo leans delayed and feint-heavy ([[1i Last Rite - Bhu Mani Husks]] §6), so its set should lean **Economy and Parry**.

### 7.4 Vayu and Agni — 4–6 slots each

Bosses **GaleTalon** (Vayu) and **PyreMaw** (Agni) per [[1h Last Rite - Player Moveset & Animation Plan]] §5; families **not final** — one Echo per family once each roster is built. Agni's tempo is the fastest ([[1i Last Rite - Bhu Mani Husks]] §6) — a natural home for **Risk** Echoes; Vayu is the natural home for the in-world naming of the mechanic (§1).

---

## 8. Reference numbers (ALL playtest-open)

**Unit:** 1 mani ≈ one Husk purified (raw or elemental). **A wing ≈ 50 Husks** (Bhu r3: 50 over 26 sites).

| Item | Reference |
|---|---|
| Learn count | **N = 3** per family · bosses 1 |
| Field patch | **5** mani per Shroud state (~25%) |
| Shrine re-vest (full restore) | **10** |
| Echo costs | starters **5** · Husk Echoes **10–15** · boss Echoes **20** |
| Rite allowance | **10** (two starters, free) |
| The Brink | **60** bound (≈ the allowance + 4–5 Echoes) |

**The intended shape:** a clean player spends ~15–20 of a wing's ~50 mani on repair, so ~30 per wing goes into binding — reaching the Brink around the second wing. A struggling player spends ~40 per wing on repair, so they bind ~10: one Husk Echo per wing on top of the allowance. **That gap is the design. If it is much wider at gray-box, the death spiral is showing** — the fix is the allowance and the patch price, not the Echo costs.

---

## 9. Presentation and UI

- **Shrine Binding board** (`BGamer.UI`): Echoes grouped by wing; each card shows the name, the formal name, cost, job, a one-line effect and the codex line. **The Binding bar** shows the Rite allowance · bound · carried, with the Brink marked. Bind / Release.
- **In combat:** no HUD beyond proc flashes on her marks and the *"N/3"* toast on purify. The Shroud stays the health readout.
- **Husk side:** each family's stack reads on its body — gem brightness (Garland), the censer-gem's swing (Mourner), crown leaves (Deadfall). Per-family art, kept small.

---

## 10. Architecture

Built to the target architecture ([[1a Last Rite - Code Architecture]] §T, LOCKED 2026-10-09).

**Above the seam — `LastRite`** ([[1a Last Rite - Code Architecture]] §0: *mechanics below, economies above*; Echoes are Last Rite policy):
- **`SOEchoDef`** (ScriptableObject) — id, common + formal name, source (family / boss / Rite), learn count, cost, job, effect text, codex line, **`PlayerEffects`** and **`HuskEffects`** lists. One asset holds both sides of the mirror.
- **`SOEchoEffect`** (ScriptableObject base) — one subclass per trigger kind: graded defence · held feint · purify · Shroud state change · damage step · cost modifier. **A new Echo is a new asset, not a new script** (the same rule as the T4 maintainers).
- **`EchoLoadout`** (MonoBehaviour, on `Player/Combat`) — applies the bound Echoes' effects on enable, removes them on disable.
- **`HuskEcho`** (MonoBehaviour, on each enemy prefab's Combat object) — applies its family's Husk effects.
- **`EchoLedger`** (MonoBehaviour) — the remembered set + per-family purify tallies; raises `UnityEvent`s for the toast and the "remembered" moment; its memento goes into MetaSave.

**Below the seam — generic hooks in `BGamer.Combat`** (Combat never learns what an Echo is):
- **Already built:** `CombatSystem.OnDefenseResolved(defender, grade, success)` · the parry's +1 AP refund (`CombatSystem.Parry`) · the enemy Purge meter (`EnemyCombatEntity.AddPurge`, `OnPurgeChanged`, `OnPurgeFull`) · `CombatEntity.OnDied` · `IDamageAbsorber` (the Shroud, `PlayerShroud`).
- **To add:**
  - **`IDamageStep`** — the damage-modifier chain [[1j Last Rite - Shroud, Mani & Sanity]] §7 and [[1e Last Rite - Build Roadmap]] M1 already planned (`AshformMult`, `PurgedMult`). Echoes are its third client. Not built yet.
  - A public **AP grant** command on `PlayerCombatEntity`.
  - A **feint-resolved** state event (Seen Through, Splinter Crown), if the feint system doesn't raise one.
  - An attack **tag** on `SOAttackDef` for hanging (delayed) attacks (Long Vigil).
  - A **chain-length** hook on enemy strings (Splinter Crown, Consecration).

**Events (T2):** `UnityEvent<>` only — no C# `event` or `Action` fields. Echo procs raise Cues from an `EchoCues` emitter → a `Moment` → `MaterialOneShot` on her marks. "Echo remembered" is screen-wide, so it goes through a `CueChannel` → `HudBanner`. Shrine buttons are Commands into `LastRite`.

**House rules:** MonoBehaviour / ScriptableObject only · `Debug.Assert`, never silent returns · one MonoBehaviour per file.

**Saves:** `RunSave` += bound Echo ids + carried mani · `MetaSave` += remembered Echo ids + per-family tallies. **⚠ There is no progress save system in code yet** (2026-10-11: only `GameSystem`'s two settings in PlayerPrefs). Echoes need one; so does the launch.

---

## 11. Launch build plan

**Fixed dates:** launch build to Steam review **Nov 2** · date lock **Nov 5** · launch **Thu Nov 19, 2026**.

| Phase | Work | Window |
|---|---|---|
| **E0 — Mani economy + saves** | reserve · mani from the gem-tear finisher · field patch · shrine re-vest · death drop + one retrieval · RunSave / MetaSave | Oct 12–16 |
| **E1 — Echo framework** | `SOEchoDef` / `SOEchoEffect` / `EchoLoadout` / `HuskEcho` / `EchoLedger` · `IDamageStep` + the Combat hooks · the Brink with its mode switch | Oct 19–21 |
| **E2 — Bhu set + starters** | 4 Husk passives + 4 Echoes + 3 Rite starters, tested in the encounter lab | Oct 21–23 |
| **E3 — Shrine + feedback** | Binding board · N/3 toast · "remembered" banner · codex lines | Oct 26–28 |
| **E4 — Jal set** | 5 Husk passives + 5 Echoes | Oct 28–30 |
| **E5 — Vayu + Agni sets** | one per family + boss, as each wing's enemies land | before Nov 2 |
| **E6 — Tuning** | §8 numbers · death-spiral check · fixes | Nov 2–5 |

**⚠ Schedule risk — logged honestly:**
- E0–E3 alone was estimated at **15–19 working days**, against **~15** before Nov 2, shared with all other launch work.
- **Vayu and Agni enemies are not built** (2026-10-11); their Echoes can only follow them.
- **Cut order if it slips:** (1) diegetic feedback stays a toast · (2) Husk-side stack readouts reduce to gem brightness only · (3) Vayu / Agni ship their boss Echo first, families after.
- **The demo** (planned Oct 27) goes out without Echoes; they arrive in a demo patch — a natural "you asked, I built it" beat.

---

## 12. Open

- §8 numbers: learn count, costs, the allowance, the Brink.
- **Brink mode** — `Cap` until the sanity/scars launch decision.
- Endless: does binding past the Brink speed the sanity timer?
- The Renounce ending releasing every Echo (§2) — writing pass.
- Formal names and codex lines for every Echo — writing pass.
- Vayu as the place the mechanic is named in-world.
- Jal, Vayu and Agni Echo designs (§7.3–7.4).
- The death-spiral check at gray-box (§8).
