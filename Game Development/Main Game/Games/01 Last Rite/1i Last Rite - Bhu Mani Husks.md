# Game 1 "Last Rite" — Bhu Mani Husks (enemy family + movesets)

> **The Bhu (Earth) enemy family — concept art, weapons and full movesets.** Written 2026-08-07. Companion to [[1g Last Rite - Enemy Roster]] (the full roster), [[1h Last Rite - Player Moveset & Animation Plan]] §5 (weapon allocation + moveset shape) and [[1d Last Rite - Reaction & Feints Spec]] (defensibility flags, chains, feints).
>
> **⚠ This reverses [[1g Last Rite - Enemy Roster]]'s "Bhu deliberately excluded from the bespoke roster."** That exclusion was priced when moveset animation was the bottleneck — `1g` states outright that "the real budget is moveset animation + telegraph design," which is why bespoke enemies were capped at nine. The Mega Animation Pack removes most of that cost, so a fifth bespoke family is now affordable. Bhu also stops being the one element with no identity of its own.
>
> **Per I10 discipline:** all values are playtest-open.

---

## 1. The family

Three husks and a boss.

> **⚠ ORIGIN CORRECTED 2026-08-11 — [[1k Last Rite - Lore Bible]] §3.** They were **not** "people caught by the Bleed near raw Bhu deposits." They are **this facility's own staff**, turned by the engineered immortality-medicine that hollowed everyone inside — *concentrated* Husks, worse than anything the raw Breach ever made. The Bhu stratum is the facility's **burial terraces**, where a mani-medicine division that spent centuries failing to cure death did its interring.

> **⚠ PLACE SPECIFIED 2026-08-12 — [[1l Last Rite - World & Environment Bible]] §3.1.** The Bhu wing is now designed: a **rocky, overgrown mountain-shrine** whose medical domain is **growth & the physical body** — monumental specimen-vats in carved devotional housings, clusters of toppled smaller jars, pale fungal growth, emerald veins spreading across the rock toward the threshold crystal.
>
> **This does not retract the burial terraces — it explains them.** Per the three-layer register ([[1k Last Rite - Lore Bible]] §3): **the architecture is temple, the function is growth-medicine, and the funerary grammar is the residue** of what happened when the function failed. The terraces are where this wing's own staff were interred; the vats are what they were doing right up until they became the things being interred. **That is exactly why this family reads as failed funerals** — Garland, Mourner, Deadfall and the Reliquary are the *result* of a growth lab, laid out with rites, on the terraces of the building that killed them. The wing's approach-shows-the-function rule (§2.4 rule 4) means the player walks past the vats on the way in, then fights what came out of them.

The Bhu-wing staff went down saturated in earth-mani, and the ground took them: **entombed in clay, moss, wood and stone around their own gem** — but **the rite was never finished, so they walk.** Each one is a failed burial. Killing one completes its funeral.

*(Everything below this line — silhouettes, adornment, movesets, tempos — is unchanged. The origin swap costs no art and no design: a facility that buried its dead in its own earth produces exactly the same creatures.)*

### 1.1 Shared DNA

- **Something is always missing.** A sealed pot for a head, a veil with nothing behind it, a splintered hollow, no head at all. The body is a container with nobody inside — that is the "husk" read at a glance.
- **The shard is always visible.** A corrupted Bhu Mani lodged somewhere vital, glowing warm **amber**, with ochre mani-veins spreading from it like root filaments.
- **⚠ The gem is the deathlessness itself (CANONIZED 2026-08-11).** It is not decoration or a drop table — **it is the core of the husk's endless life, and tearing it out IS the kill**: the body crumbles instantly to dust, centuries overdue. That is the purification finisher, and it is why "lodged somewhere vital" is a design requirement rather than a styling note. See [[1k Last Rite - Lore Bible]] §5 and [[1j Last Rite - Shroud, Mani & Sanity]] §2.
- **The gem is the telegraph.** Slow attack → slow deep pulses; fast attack → rapid flicker; post-attack exposure → the gem dims. One shared readability language across the family. Honours the fairness law in [[1 Parry Combat - Last Rite]] — the gem never obscures a telegraph, it *is* one. **It therefore carries three jobs at once: the tell, the kill target, and the loot.**
- **Funerary adornment marks rank:** marigold garland → mourning veil → blood-red leaf crown → full vestments and a halo.
- **Silhouette spectrum:** squat → stooped → lanky → perfectly upright. Readable at combat distance under the unlit + black-outline treatment.

### 1.2 Naming

Two-tier, per the law in [[14 Naming Glossary]] — common English in all gameplay UI, formal Sanskrit in Accord-era texts and the lore codex.

| Common | Formal | Meaning |
|---|---|---|
| Garland Husk | **Asthikumbha** | bone-urn |
| Mourner Husk | **Avarana** | the veil, the covering |
| Deadfall Husk | **Chitakashtha** | pyre-wood |
| Reliquary Husk | **Dhatugarbha** | relic-chamber (the reliquary of a stupa) |

### 1.3 Moveset shape

Per [[1h Last Rite - Player Moveset & Animation Plan]] §5.1 — husks carry **2 special + 2 light + 2 heavy (6)**; the boss carries **2 special + 5 light + 5 heavy (12)**.

**Specials are particle systems, not animations.** The body motion is a generic pack clip; the VFX carries the identity. Each is depicted on that enemy's weapon-and-moves sheet below.

---

## 2. Garland Husk · *Asthikumbha* — the Metronome

![[Bhu - 01 Garland Concept.jpg]]

A squat, potbellied thing built like a cracked funeral urn given legs — sun-dried riverbed clay over a fired terracotta core, split by kiln-crack fissures leaking amber light. Its head is a **lidded clay pot fused shut with wax seals** — no face at all. Arms too long for the body, fingers fused into blunt spade-tips caked in grave-soil. Dead marigold garlands hang across it like a sash; a scrap of vermilion rite-cloth is pressed into its chest clay. The shard sits behind a grille of cracked clay ribs, glowing like a coal in a kiln.

**Combat identity — the honest one.** Longest telegraphs, widest windows, biggest damage. He is the rhythm baseline the rest of the family deviates from, and where the player learns the language.

![[Bhu - 01 Garland Reference.jpg]]

**Weapon — the Grave Spade** (`Shovel` set). A broad rusted iron blade caked with grave-soil, cracked wooden haft bound in vermilion rite-cloth matching his chest banner.

![[Bhu - 01 Garland Weapon and Moves.jpg]]

| Slot | Attack | Defense | Pack clip | Notes |
|---|---|---|---|---|
| Special | **Kiln Vent** | **Dodge-only** | `Shovel - Attack Spin 01` | Torso cracks blaze white-hot; a wide ring of embers and hot ash bursts outward. Longest recovery in the family — the fairest possible lesson that parry doesn't always work. |
| Special | **Grave Dust** | Parry \| Dodge | `Shovel - Attack Strong 02` | Spade driven into the ground; a column of ochre dust and marigold petals erupts. Leaves a dust cloud that **shrinks the player's parry window** for one attack. |
| Light | **Grave-Scoop** | Parry \| Dodge | `Shovel - Attack Weak 01` | The connector. Links into everything. |
| Light | **Spade Backhand** | Parry \| Dodge | `Shovel - Attack Weak 02` | Alternate light so repeated chain steps don't read as copy-paste. |
| Heavy | **Gravedigger's Fall** | Parry \| Dodge | `Shovel - Attack Strong 01` | Overhead spade slam. |
| Heavy | **Bellring Lunge** | Parry \| Dodge | `Shovel - Attack Running` | Chain ender. **Parry it and the pot rings like a struck temple bell — every other enemy is Staggered and loses its next turn slot.** |

**Authored spine:** Grave-Scoop → Kiln Vent → Bellring Lunge. Parry, switch to dodge, then parry for the payday — the response-switch taught in one string.

**Feint:** *Lid-Rattle* — shares Grave-Scoop's Startup+Telegraph, cancels to guard.

> **Bellring Lunge is the family's best mechanic and the cheapest to build** — `FighterPhase.Staggered` already exists and `TurnOrderScheduler` already skips staggered fighters. One correct parry deleting the enemy team's next round needs no new systems.

---

## 3. Mourner Husk · *Avarana* — the Off-Beat

![[Bhu - 02 Mourner Concept.jpg]]

A tall figure stooped like someone standing at a grave in the rain. Its entire front is a **veil of hanging moss, votive threads and white mourning flowers** falling from crown to shins — no face, only curtain. Beneath: a hollow ribcage of woven banyan roots, and inside that hollow the Bhu Mani hangs on root-strands like **a censer on chains, swinging as it walks.** Walk behind it and it is **concave — a front-only shell, like a discarded mask.**

**Combat identity — rhythm corruption.** His attacks *hang*: unusually long commits, so the hit lands later than the telegraph implies. He punishes muscle memory rather than bad reading, which also makes him the tempo the **Jal stratum** is written against ("the slow is the enemy's tempo").

![[Bhu - 02 Mourner Reference.jpg]]

**Weapon — the Funeral Standard and Grave-Slab** (`Spear and Shield` set). A tall root haft topped with a narrow carved stone grave-marker as its head; the shield is a broad weathered slab with worn script, gripped by root straps.

![[Bhu - 02 Mourner Weapons And Moves.jpg]]

| Slot | Attack | Defense | Pack clip | Notes |
|---|---|---|---|---|
| Special | **Grief Toll** | **Dodge-only** | `Spear and Shield - Attack Spin 01` | Bows deep; the pendulum gem swings and strikes his root ribs. Concentric shockwave rings expand outward, hitting every living fighter on the opposing team. |
| Special | **Root Snare** | Parry \| Dodge | `Spear and Shield - Attack Strong 02` | Standard slammed butt-first into the soil; grasping roots erupt in a spreading web. Applies **Rooted — the player's dodge window is halved for the next two attacks.** |
| Light | **Vigil Thrust** | Parry \| Dodge | `Spear and Shield - Attack Weak 01` | |
| Light | **Slab Shove** | Parry \| Dodge | `Spear and Shield - Attack Weak 02` | Shield bash with the grave-slab. |
| Heavy | **Mourner's Sweep** | **Jump** | `Spear and Shield - Attack Strong 01` | The canonical heavy sweep the `AllowedDefenses = Jump` flag was reserved for. Hangs before it lands. |
| Heavy | **Veil Embrace** | **Dodge-only**, unparryable | `Spear and Shield - Attack Finisher` | Drains HP **and Purge**. Dodge it and he is **Exposed** for a full turn. |

**Authored spine:** Root Snare → Mourner's Sweep. Rooted halves the dodge window, and the follow-up is Jump-flagged — which Rooted doesn't touch. The string teaches that **Jump is its own answer**, not a worse dodge, and stays fair while feeling like a trap.

---

## 4. Deadfall Husk · *Chitakashtha* — the Liar

![[Bhu - 03 Deadfall Concept.jpg]]

Bone-white bleached driftwood wrung into a human shape — the grain visibly spirals, like a body twisted while it petrified. Too-long forearms and shins; silent glides broken by settling-timber creaks. It has **no head**: the neck ends in a violently splintered hollow with the amber gem wedged inside like a coal in a broken pipe, and from that same hollow erupts a crown of **blood-red leaves, like a wound that flowered.** One arm is a cord-bound bundle of branches; the other hand carries the scythe.

**Combat identity — the fake.** Fastest telegraphs, longest chains, most feints. He attacks the player's *economy*: a baited parry is a full hit **and** zero AP, so every successful lie costs the player their next turn's combo. **Author him first in the enemy rotation** — he sets the tempo and drains parries before the heavy hitters arrive.

![[Bhu - 03 Deadfall Reference.jpg]]

**Weapon — the Bone Scythe** (`Scythe` set). A long curved shard of pale bone lashed to a twisted driftwood haft with the same dark cord that binds his arm bundle. Grown or buried with him, never looted.

![[Bhu - 03 Deadfall Weapons and Moves.jpg]]

| Slot | Attack | Defense | Pack clip | Notes |
|---|---|---|---|---|
| Special | **Quill Shed** | **Dodge-only** | `Scythe - Attack Strong 03` | Torso wrung around in a full twist, flinging a radial spray of splinters with red leaves scattering. Afterward his quills are visibly blunt — his next attack is weakened, a breather he telegraphs himself. |
| Special | **Ash Scatter** | — | `Scythe - Attack Weak 05` → `Paralysis A` | **The feint.** Plays Widowmaker's full telegraph, then collapses into an inert heap as a low burst of ash and dead leaves blows outward and the throat-gem dims to a faint ember. |
| Light | **Splinter Rake** | Parry \| Dodge | `Scythe - Attack Weak 01` | Fastest read in the game. |
| Light | **Reaping Cut** | Parry \| Dodge | `Scythe - Attack Weak 02` | |
| Heavy | **Widowmaker Lunge** | **Parry-only** | `Scythe - Attack Running` | Tight window, high damage. The step his fake imitates. |
| Heavy | **Harvest Arc** | Parry \| Dodge | `Scythe - Attack Strong 01` | Wide committed sweep. |

**Authored spine:** Splinter Rake → Splinter Rake → *Ash Scatter* → Widowmaker Lunge. The longest string in the family.

---

## 5. Reliquary Husk · *Dhatugarbha* — the Multiplier (BOSS · 3 phases)

![[Bhu - Reliquary (Boss) Concept.jpg]]

The deliberate outlier: where its kin are broken and hunched, this one is **serene, symmetrical and statue-still** — **the wing's rites-officer**, the Accord ritualist who consecrated this facility's dead and was entombed by them in turn, sealed in polished black temple basalt, every crack repaired with gold inlay like a restored idol. *(⚠ 2026-08-11: was "an Accord-era ritualist the earth entombed" — now specifically **facility staff**, per [[1k Last Rite - Lore Bible]] §3. It gains a reason to be here and a reason to keep raising the others: it never stopped doing its job.)* It has **no head**: the neck opens into a blooming lotus crown of stone petals cradling the bare Bhu Mani. Its chest is a **shrine niche** holding a row of oil lamps that ignite one by one as it escalates. Rising from its back on gold supports: a **broken stone halo**, a third of the ring missing.

**Combat identity — the force multiplier.** It barely fights. Without positioning, support means **turn economy and window manipulation** — it inflates everyone else's chains and undoes your kills. It never fights alone by choice, and killing it makes the whole encounter audibly relax.

![[Bhu - Reliquary (Boss) Reference.jpg]]

**Weapon — the Shrine Staff** (`Magic` set, wand reskinned). A full-length staff taller than its bearer: a black basalt shaft carrying the same gold kintsugi seams as its body, topped with a blackened-iron lotus cage holding live flame and its own **miniature broken halo** — a portable shrine that copies its bearer. Heavy spiked iron ferrule at the base. Gripped in the upper third so the wand-set casting clips never sling the far end through the floor.

![[Bhu - Reliquary (Boss) Weapon and Moves.jpg]]

| Slot | Attack | Defense | Pack clip | Notes |
|---|---|---|---|---|
| Special | **Consecrate** | — | `Magic - Magic Heal 01` | Drives the ferrule down and kneels; a ring of golden script unfurls across the ground. **Persistent aura: +1 chain step and tightened windows for every living husk, until it dies.** |
| Special | **Rite of Return** | — | `Magic - Magic Heal 02` | Staff raised, every lamp blazing; gold light pours into a destroyed husk, which rises at reduced HP. **Channels on one turn, resolves the next** — the player gets one full Player Phase to interrupt it. |
| Light | **Lamplight Lash** | **Parry-only** | `Magic - Attack Weak 01` | Tight, elegant window. The duelist read. |
| Light | **Censer Sweep** | Parry \| Dodge | `Magic - Attack Weak 02` | |
| Light | **Ferrule Jab** | Parry \| Dodge | `Magic - Attack Weak 03` | Spiked base thrust. |
| Light | **Ember Dart** | Ranged — Perfect deflects | `Magic - Attack Magic Weak 01` | Carries the family's ranged-deflect read. |
| Light | **Lampfall** | Ranged — Perfect deflects | `Magic - Attack Magic Weak 02` | |
| Heavy | **Halo Grind** | **Dodge-only** | `Magic - Attack Magic Strong 01` | Big damage, long recovery. Its one real commitment. |
| Heavy | **Pyre Column** | **Dodge-only** | `Magic - Attack Magic Strong 02` | |
| Heavy | **Benediction** | Parry \| Dodge | `Magic - Attack Magic Strong 03` | |
| Heavy | **Gilded Chain** | **Parry-only** | `Magic - Attack Magic Strong 04` | |
| Heavy | **Sanctum Break** | **Jump** | `Magic - Attack Magic Strong 05` | Phase-3 finisher. |

> **⚠ Rite of Return is the one mechanic here needing new plumbing** — re-inserting a dead fighter into the rotation is a scheduler concern `TurnOrderScheduler` does not currently handle (it removes Dead fighters from `_order`).

---

## 6. Encounter synergy

Without positioning, an encounter is **one continuous rhythm the player must hold**, and composition is how that rhythm is written.

**Each husk owns a different tempo.** Garland is honest and slow, Mourner is delayed and off-beat, Deadfall is fast and lying, Reliquary stretches everyone else. Four Garlands is a metronome a new player can learn. Garland + Deadfall is a rhythm test. Add the Mourner and the player is holding three incompatible tempos across one Enemy Phase.

**The economy war is the strategy.** Parry generates AP and feeds Purge; dodge generates nothing. So **every dodge-only step is an economy attack** — it costs HP *and* starves the next turn. Garland is the AP faucet with generous windows; Mourner and Deadfall are the drain. The player's real decision each Enemy Phase is how greedy to be, and composition sets the exchange rate.

**Rotation order is the main authoring dial.** Deadfall first to drain parries with fakes → Mourner to land dodge-only economy attacks while the player is rattled → Garland offering fat parryable payouts to a player who no longer trusts their read → Reliquary last so its buff lands going into the next round. Reordering the same four enemies produces a genuinely different fight for free.

**Every husk carries one counter-lever, so mastery is legible.** Parry Bellring Lunge and the whole team loses a turn. Dodge Veil Embrace and the Mourner is Exposed. Kill the Reliquary and every chain shortens. Watch for blunt quills and the Deadfall's next turn is weak. None are hidden.

**These four are the base tempos the rest of the game is written against.** The Jal stratum (delayed, feint-heavy) pushes everyone toward Mourner behaviour; the Agni stratum (fastest, most relentless) toward Deadfall. Designing the base set as four distinct tempos is what makes each later stratum feel like a **re-learn** rather than a stat bump — the "each stratum re-teaches the fight" law in [[1k Last Rite - Lore Bible]] §6.

> **⚠ 2026-08-11 — read this as *strata*, not *rebirth cycles*.** These tempos were originally the base layer that ~5 elemental re-descents would remix. The re-descent loop is cut ([[1 Parry Combat - Last Rite]] D2), so the Jal/Vayu/Agni versions are **their own families in their own places**, not overlays repainted onto these four. The overlay *vocabulary* still describes what each element does to a tempo — it now describes a neighbour rather than a repaint, and the Chaos Descent is where remixing actually happens ([[1j Last Rite - Shroud, Mani & Sanity]] §5).

---

## 7. Animation sourcing

**From the pack (free):** every light and heavy attack, all idles, locomotion, directional hit reactions, deaths, and the `Paralysis`/`Trembling` stagger poses.

**Custom (Kimodo) — signature motions no weapon set expresses.** Batch these in **one session**: cold start dominates, so eleven clips cost roughly what one costs.

| Clip | Enemy | Why the pack can't cover it |
|---|---|---|
| Urn Toss | Garland | **No throw animation exists anywhere in the pack** |
| Kiln Vent | Garland | A stationary chest burst; `Attack Spin` reads as a spin |
| Bellring Lunge | Garland | The pot headbutt — the move depends on the pot being struck |
| Grief Toll | Mourner | The deep bow that swings the pendulum into his own ribs |
| Veil Embrace | Mourner | **No grab clips exist anywhere in the pack** |
| Deadfall Collapse | Deadfall | A collapse to a prone heap; `Paralysis` is a stagger, not a collapse |
| Halo Grind | Reliquary | A shoulder-first charge; the Magic set only casts |
| Consecrate | Reliquary | Kneeling with a palm to the ground |

All must be **in-place** with horizontal root motion stripped and vertical kept, per [[1b Last Rite - Art Bible]]. Pack clips are hand-keyframed and Kimodo output reads looser — survivable precisely because Kimodo only covers signature moves, so the tonal difference lands on attacks that are meant to look unusual.

**Retarget risk:** pack clips are authored for upright human proportions. The Deadfall's hunch and the Mourner's stoop will be straightened. The fix is an Animator layer carrying an additive hunch/asymmetric-shoulder pose over the retargeted clips — **prove it on one attack before building four movesets.**

---

## 8. Open / playtest-open (I10)

All damage and timing values · whether the Reliquary's 5+5 boss shape holds or trims to 4+4 · the Rooted debuff's exact magnitude · whether Grave Dust's parry-window shrink is fair or reads as unreadable · the scheduler work for Rite of Return · final clip picks per slot.
