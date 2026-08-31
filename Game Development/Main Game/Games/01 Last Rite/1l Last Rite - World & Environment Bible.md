# Game 1 "Last Rite" — World & Environment Bible

> **The canonical architecture, layout and environment art direction for "Mani Kalpa: One Last Rite". Environment direction LOCKED 2026-08-12.** All four wing exteriors have approved concept art. This doc owns **where the game physically is** — the complex, its layout, the four wings, and the laws that govern how the environment is coloured and animated.
>
> Companion to [[1k Last Rite - Lore Bible]] (the fiction — **it wins on any story detail**), [[1b Last Rite - Art Bible]] (the style lock + colour governance this doc amends), [[1j Last Rite - Shroud, Mani & Sanity]] (the corruption loop as systems), [[1i Last Rite - Bhu Mani Husks]] (who lived here), [[1 Parry Combat - Last Rite]] (the design locks).
>
> **World-level canon is owned elsewhere and is unchanged by this doc:** [[12 The Akashic and The Bleed]] (the Breach, the Bleed, Akash, what a Husk is) and [[14 Naming Glossary]] (the two-tier naming law). This doc adds a **local layer** — one building — inside that world.
>
> **⚠ This doc is NOT inert.** It carries four amendments to previously locked decisions, all resolved 2026-08-12 and traced in §6: **strata order is now free** (§1.7), **the environment palette is "colourful but dead"** (§2.5), **threshold crystals are element-coloured, not teal-violet** (§2.4), and **the mani-aura exception** — reserved gameplay colours govern *surfaces*, never mani itself (§2.5). The fourth one closes Agni's crystal hue and legalises Agni's live amber.

---

# PART ONE — THE WORLD

## 1.1 The franchise frame

All games share **one world, different eras**, with no plot connection between them. Callbacks and shared lore only — never a continuing story.

- **Franchise:** Mani Kalpa
- **This game:** *Mani Kalpa: One Last Rite* (the COMBAT pillar)
- **Era:** post-Breach, millennia after the catastrophe. The Looter Shooter holds deep world canon and sets the era; this game must not contradict it.

**Naming is two-tier** — common English in gameplay UI, formal Sanskrit in the lore codex ([[14 Naming Glossary]]):

| Common (UI) | Formal (codex) |
|---|---|
| The Bleed | The Akashic |
| Earth / Water / Air / Fire | Bhu / Jal / Vayu / Agni |
| Ether (forbidden fifth) | Akash |

**Mani** = the magical jewel-substance refined from the elements. **Kalpa** = a cosmic cycle.

## 1.2 The Breach *(summary — canon lives in [[12 The Akashic and The Bleed]])*

Civilization ran on mani refined from the five elements. The **Accord** refined mani for energy — that was the public truth. Its inner circle was using **Akash-mani** to force open a doorway to the dimension Akash. It opened. Something pushed back. The door collapsed, the crystal shattered, and Akashic radiation leaked for a few seconds before sealing.

Creatures that absorbed the radiation became immortal, unaging, mind-gone: the **Husks**.

**The Breach closed millennia ago. The cosmic horror is past tense.** This game is set in the long, quiet aftermath — the ruin, not the disaster.

## 1.3 This facility

Before the Breach, this complex was the Accord's **mani-medicine division** — already chasing a cure for death.

After the Breach, its researchers watched the Husks and saw not a plague but a prize: creatures that could not die. They reverse-engineered Husk-immortality into a deliberate **medicine**. *"We can make it a gift."*

It went wrong. A bad batch — or an engineered strain that did exactly what it was built to do — turned the entire staff into Husks. But these are not raw Breach-Husks. They are **concentrated, engineered, mutated**: stronger, stranger, and more extreme than anything the Breach made on its own. The facility sealed itself as a tomb.

**Why this works:** it respects Looter Shooter canon (the Breach still makes the original Husks) while giving this game its own bespoke roster. And it is a *chosen* sin — the facility repeats the Accord's hubris deliberately, with full knowledge of what the first attempt cost.

## 1.4 The protagonist

A **purifier** who has undergone the **Kavach Rite** — mani-powered near-instant regeneration channelled through the **Shroud**, an ablative mani armour. The Rite keeps her alive. It also **leaks**: she needs a constant mani feed to survive, a prisoner of her own cure.

She has heard that this dead facility cracked *permanent* immortality. She dives in to steal it, so she never needs the feed again. She does not know that the cure she is chasing is exactly what hollowed everyone here into Husks. **Her solution and her doom are the same substance.**

## 1.5 The last rite

Husks are immortal — they do not age — but they can take damage. The **mani-gem** inside each one is the core of its immortality. Extract or destroy it and the endless life ends: the Husk crumbles instantly to dust, centuries overdue.

This is a **mercy**. A last rite, not a slaughter. It is where the game gets its name.

Harvested gems feed her Shroud and sharpen her — and pull her toward the facility's fate. **The keystone:** *player mastery rises as the protagonist's mind falls.*

## 1.6 Framing — tragedy, not mystery

The player will guess the corruption early. **Do not fight this.** Structure the game as **slow dread**, not as a twist. The question is never *"what happened here?"* — it is *"can she stop in time? can I?"* The moral choice lands at the very end, with full knowledge. *(Full reasoning: [[1k Last Rite - Lore Bible]] §9.)*

## 1.7 The descent

A tutorial under her **Mentor** (who performed her Rite), then four elemental strata, each a "rebirth" that re-teaches the fight — remixed movesets, new grading LUT, new telegraph VFX, new lore — rather than raising numbers.

> **⚠ AMENDMENT 2026-08-12 — STRATA ORDER IS NOW FREE.** Previously locked as a fixed sequence **Bhu → Jal → Vayu → Agni** ([[1k Last Rite - Lore Bible]] §6, [[1a Last Rite - Code Architecture]] §4.3 "Elemental spine", [[1e Last Rite - Build Roadmap]]). **The four wings are now playable in any order.** Each yields a **key / seal**; all four together open the sealed sanctum. The physical layout (§2.2) is what makes this natural — four wings branching off one spine, all reachable from the entrance.
>
> **Three consequences that are load-bearing and must not be skipped:**
> 1. **There is no terminal stratum.** `StratumDef.isTerminal` is void — terminality moves to **the sanctum**, gated by four seals. See [[1a Last Rite - Code Architecture]] §4.3.
> 2. **Difficulty cannot be a per-stratum ladder.** A free-order player can walk into any wing first. **Recommended resolution (⚠ OPEN — confirm at gray-box): key the difficulty ascension to `strataCleared`, not to `stratumIndex`.** The ramp then survives free order intact, and each wing authors a *character*, not a rung. Touches [[1d Last Rite - Reaction & Feints Spec]] §3 (the defense-window start-delay baseline) and D9.
> 3. **Pacing anchors that named a specific stratum break.** "Fraying onset must be felt by the Jal stratum (the second)" ([[1j Last Rite - Shroud, Mani & Sanity]] §3.1) becomes **"by the second stratum, whichever it is."**

She is not the first. The ruin is littered with **prior purifiers who became Husks**. The player walks past their future repeatedly before understanding it.

## 1.8 The heart & the choice

At the heart: the **lead researcher** who first self-tested the immortality-medicine. She is literally the protagonist's future — the one who already won the prize being chased. **The heart is the sealed sanctum** at the end of the processional spine (§2.2) — the thing that has been on screen since the entrance.

After the heart, the fork — **three endings, not two** ([[1k Last Rite - Lore Bible]] §8 owns this):

- **TAKE it.** Become deathless and mind-gone. Walk out to begin an endless wandering. The tragic TRUE ending — and the baton-pass to the Looter Shooter era.
- **RENOUNCE it.** Drop the gem-crutch, walk away, keep the leaking mortal Rite.
- **HUSK OUT.** *(failure, mid-dive)* Sanity hits zero and the next death turns her where she stands. The same becoming, blind. Authored content, not a game-over screen.

**How scarred she is by then decides how much the choice costs.**

## 1.9 The corruption loop *(fiction only — systems live in [[1j Last Rite - Shroud, Mani & Sanity]])*

Two-layer health: the **Shroud** (ablative armour) over a short **body bar** that mani cannot heal. When the Shroud is spent, body hits cost **sanity**.

**FRAYING — recoverable.** Tactical give-and-take. As sanity frays, HUD and combat cues dampen and the world reads obscure. In exchange: damage up, elemental resistance up.

> **The information never lies.** The picture gets noisy; a parry window is still a real parry window. Fraying degrades *presentation*, never *truth*.

**SCARRING — permanent.** A one-way ratchet, earned by greedy play: taking body damage with the Shroud spent, over-leaning on gem buffs, pushing sanity past threshold. Each scar **permanently lowers the sanity ceiling.** The recoverable maximum shrinks. The walls close in. Dread expressed as a mechanic, not as a second death-meter.

Max scarring ends the run. Reaching the finale with sanity intact is the true victory; a no-scar clean run is a genuine flex.

**Why it bites:** over-reliance makes you statistically stronger but worse at *reading* the fight — in a parry game, that is a real cost. Greed is not flavoured, it is priced.

---

# PART TWO — THE ENVIRONMENT

## 2.1 Identity — a temple with no god

Science here got so advanced it became indistinguishable from miracle, so people worshipped it **as a temple**. But there is **no god at the centre**. The object of worship is **mani itself** — the substance, the work.

**The design engine:** everywhere a deity's murti would sit, there is a **crystal** instead. Devotional geometry funnels toward a growing crystalline mass. The temple is a refinery wearing a devotional skin.

**Visual gradient:** near the entrance the crystal is small, contained, holy. Deeper in it metastasizes through the walls — the temple eaten by what it enshrines.

> **⚠ Register reconciliation (2026-08-12).** [[1k Last Rite - Lore Bible]] §3 calls this place **"a hospital built like a temple"** and derives the funerary enemy family ([[1i Last Rite - Bhu Mani Husks]] — Garland, Mourner, Deadfall, Reliquary) from that. **Both registers are true and they stack:** the *building* is a devotional refinery that worships mani (this doc); the *rites practised inside it* were medical, and the staff who died mid-work left funerary grammar behind — consecration halls, reliquaries, burial terraces. **Temple is the architecture. Medicine is the function. Funeral is the residue.** Neither doc is stale; they describe three layers of the same place.

> **Hard production rule:** no legible real-world text, no fake Devanagari, and **no deity idols**, anywhere. The fiction needs no god and no legible doctrine. Kick back any generation that adds them.

## 2.2 Scale & layout

**One structure**, not a campus or a city. Scope deliberately smaller than the Looter Shooter. But it resolves as a single **monolithic temple-complex** — a devotional pilgrimage site, not a walled compound of separate huts.

```
                    [ SEALED SANCTUM ]
                     shikhara tower
                    always on screen
                           |
        [BHU] ---- PROCESSIONAL SPINE ---- [JAL]
                           |
        [VAYU] ----        |        ---- [AGNI]
                           |
                      [ ENTRANCE ]
              beneath the sanctum:
              THE CHAOS DESCENT (endless mode)
```

- One grand **processional hall / spine**. Entrance at one end.
- The **sealed sanctum** at the far end, **in constant view**. The prize is always on screen — seduction made architectural.
- **Four elemental wings** branch off the sides, tackled in **any order** (§1.7). Each yields a **key / seal**. All four open the sanctum.
- **Beneath the sanctum**: the **Chaos Descent**, the endless roguelike pit — the only deep-underground part; the campaign spreads horizontally at ground level. *(The mode itself is already locked and promoted to core v1 — [[1j Last Rite - Shroud, Mani & Sanity]] §5, [[1a Last Rite - Code Architecture]] §4.3. **New here: it now has a physical location**, which is also the fiction of why it repeats when nothing else does.)*

**First impression target:** *ominous but inviting.*

> **⚠ Environment kit re-price (supersedes [[1b Last Rite - Art Bible]] §5.1).** The kit was priced against *"four strata + a tutorial space"* with Agni as the "sealed core." That is wrong on two counts: Agni is a **ground-level courtyard wing**, and the sealed core is a **separate seventh space**. The real list is **entrance · processional spine · four wings · sealed sanctum · Chaos Descent pit · tutorial space** — eight distinct places, not five. **Reuse across them is more load-bearing than ever**: lean on shared structural pieces (arched doorways, banded mouldings, pillar/bracket sets, plinths) + per-wing dressing + per-wing LUT.

## 2.3 The wings are medical facilities

Each wing is **not** an earthly temple with an element theme. Each is an advanced **medical-arcane facility** researching the healing magic of one element, wearing a devotional skin.

**Reveal balance: temple first, tech on closer look — but visible tech on the exterior too**, not hidden away in interiors.

| Element | Medical domain |
|---|---|
| **Bhu** (Earth) | Growth & the physical body — tissue, bone, flesh, cultivation |
| **Jal** (Water) | Blood & fluids — circulation, purification, transfusion |
| **Vayu** (Air) | Breath & spirit — prana, consciousness, soul-tether |
| **Agni** (Fire) | Energy, life-force, metabolism — the body's furnace, the vital spark |

## 2.4 Threshold grammar — LOCKED

The single most important reusable rule the environment establishes.

> **A crystal in the wing's own element colour, set in a carved arched doorway = you can enter here.**

> **⚠ AMENDED 2026-08-12 — the crystal is element-coloured, NOT teal-violet.** The original direction gave every wing a **teal-violet** threshold crystal as a through-line to the sanctum. That **directly violated the colour law** in [[1b Last Rite - Art Bible]] §3 — teal/white-violet is the reserved **purge / purification** gameplay channel, and the law is *"a gameplay colour never appears as decoration."* **Resolution: each wing's threshold crystal takes its own element's hue.** Teal stays 100% gameplay-reserved, the wings become monochromatic in their own element, and the grammar gets *stronger* — the crystal is now the apex of the colour the player has been reading all along.

**Rules:**

1. **Always in line of sight** the moment the player enters the building's vicinity. It does **not** have to be climbed to.
2. It sits in an **actual doorway**, not flush in a wall — so the seal can visibly break when the wing is cleared.
3. **One crystal per wing.** With the crystal and the veins now sharing a hue, "only object with that colour" no longer does the work — **value and saturation do.** See the separation law below. This rule is non-negotiable: if the crystal doesn't pop, the grammar is dead.
4. **The approach shows the function.** The player walks past the wing's specimens or apparatus on the way in. Environmental storytelling, no audio log needed.

**The separation law (the rule that replaces "only object with that colour"):**

| | Veins / décor | Threshold crystal |
|---|---|---|
| **Hue** | the wing's element colour | the same element colour |
| **Value** | **dark** | **the brightest thing in the wing** |
| **Saturation** | **low, matte** | **maximum** |
| **Behaviour** | broad, spread across the structure | a single concentrated point |

**Four placements, one rule** — this is what keeps the wings from feeling like reskins:

| Wing | Crystal hue | Placement | Direction |
|---|---|---|---|
| Bhu | bright emerald / jade | High on the mountain face | **Up** |
| Jal | bright indigo | At the floor of the stepwell | **Down** |
| Vayu | bright **green** ⚠ see note | Suspended mid-air, seen through the lattice | **Through** |
| Agni | bright amber / fire-gold | Ground level, across the courtyard | **Into** |

> **✅ OPEN #1 RESOLVED 2026-08-12 — Agni's crystal is amber / fire-gold.** The old blocker was that Agni's element colour taken to maximum brightness lands on amber-orange, colliding with the reserved Agni-corruption channel; garnet was the proposed dodge. **The mani-aura exception (§2.5) dissolves the problem** — a threshold crystal *is* a mani gem, so it is entitled to mani's own glow. Agni's crystal takes its natural amber/fire-gold and the separation law (§2.4, value + saturation + behaviour) does the work against the corruption channel, as it already does in every other wing. Garnet is dropped.

> **⚠ NEW OPEN — Bhu and Vayu are now both green** (developer decision 2026-08-12: Vayu's crystal is green, not silver-lilac). Two green wings need separation or the threshold grammar blurs. **Recommendation: Bhu = deep warm jade / mineral emerald; Vayu = pale cool mint / celadon** — which also suits Vayu being *"the palest wing"* (§3.3). **Confirm at gray-box.** Vayu's *veins* stay pale and very low-saturation either way (§2.5).

> **⚠ OPEN #2 — what ties the wings to the sanctum now?** The teal-violet crystal used to be the through-line ("refined mani, the immortality substance, present in every wing"). With crystals gone element-coloured, that link is cut. **Recommendation: give teal-violet exclusively to the sanctum** — the one place where all four elemental functions were combined into the immortality-medicine ([[1k Last Rite - Lore Bible]] §8). The wings then read as four raw elements and the sanctum as the only *refined* thing in the building, which is both truer to the fiction and a stronger reveal. It also keeps teal meaning exactly one thing everywhere — *purification* — across décor and gameplay alike. **Blocked on the sanctum interior being designed (§6 open item 5).**

> **⚠ PRODUCTION CONSEQUENCE — the approved concept art no longer matches.** All four wing images were generated with **teal-violet** crystals. They remain approved for **form, silhouette, layout, palette and dressing**; the crystal needs a **recolour pass** per the table above before they are used as Meshy input or as art-direction reference for the threshold. Do not treat the current PNGs as colour-accurate at the threshold. See §5.

## 2.5 Colour governance — LOCKED

> **⚠ AMENDMENT to [[1b Last Rite - Art Bible]] §3 (2026-08-12).** The Art Bible reads *"dark, desaturated environments"* and *"because the world is grey, any saturated colour reads instantly as meaning."* **The environment is no longer grey.** It is **colourful and dead** — and the information channel survives intact, because it was never really carried by saturation alone. It is carried by **value separation + motion**. This amendment is written into [[1b Last Rite - Art Bible]] §3.

**The dead world is colourful.** Colour comes from **material and light**, not from saturation: a warm faded mineral palette — sandstone gold, dusty rose, ochre, verdigris copper-green, basalt, terracotta. Weathered, sun-baked temple stone. Colourful, but dead.

**Reserved gameplay channels — these NEVER appear as décor:**

| Channel | Colour |
|---|---|
| Jal corruption | saturated blue |
| Agni corruption | saturated orange-red |
| Perilous telegraph | hot magenta / red |
| Parryable | white / gold |
| Purge / purification | cool teal / white-violet |

> **⚠ THE MANI-AURA EXCEPTION — added 2026-08-12, developer decision. Read this before rejecting any generation on colour grounds.**
>
> **The reserved-channel law above governs *décor*. It does NOT govern mani itself.** Wherever a colour is the **aura of a mani gem, or of something directly powered by / connected to a mani gem**, it is entitled to that element's true hue — including hues that appear in the reserved table. Mani is the substance the entire building worships and runs on; forbidding it its own colour would be forbidding the world its own physics.
>
> **In practice this legalises:** the amber-orange glow of live mani in Agni's braziers, lamp-cups, hearth cavities and gem racks; Agni's threshold crystal at full amber (§2.4); saturated blue on live Jal mani; and the equivalent on any wing. **It does not legalise painted-on orange stone, orange banners, orange grime, or "warm mood lighting."** The exception attaches to *mani*, never to *surfaces*.
>
> **The separation that keeps combat readable is unchanged, and it was never saturation:** décor-mani is **dim, low-value and static-or-slow**; gameplay hues are **bright, high-value and sharply momentary, in motion, on an enemy.** A guttering brazier can never be mistaken for a telegraph — not because it is a different colour, but because it does not move like a threat. This is the same value + motion separation §2.5 already relies on everywhere else; the exception simply stops the colour table from over-reaching into it.
>
> **Standing test for any generation:** *is this colour coming from mani, or from a surface?* Mani → allowed at true hue, held dim and slow. Surface → the reserved table applies, no exceptions.

**Two-tier glow — every wing:**

- **Threshold crystal** = the wing's element colour at maximum brightness and saturation (§2.4).
- **Raw mani veins in the structure** = the same element colour, dark and matte. Raw, unrefined element-mani.

**Per-wing vein colours:**

| Wing | Vein colour | Collision risk & guardrail |
|---|---|---|
| Bhu | deep mineral emerald / jade | low — keep any gameplay green brighter, acid |
| Jal | deep indigo | sits near reserved blue — décor stays **dark and low-saturation**; gameplay blue is **bright, saturated, in motion on enemies**. If it muddies in playtest, push décor toward violet. |
| Vayu | pale **mint-green**, very low saturation ⚠ follows the crystal change (§2.4) | watch against white/gold parryable flash — décor is dim and static, the parry cue is bright and momentary. Also hold it clearly **cooler and paler than Bhu's jade** |
| Agni | deep dark **antique bronze** | Veins are *raw, unrefined* mani, so they stay deep bronze — the mani-aura exception does **not** promote them to live amber. **Live** amber belongs to the gems, lamp-cups and braziers (§3.4). Must stay clearly dimmer and slower than any Agni corruption VFX. |

> **Standing discipline:** **value separation does the work, and motion backs it up.** Décor is dark, desaturated and *static*; gameplay hues are bright, saturated and *moving*. Anything orange in an Agni room is **either live mani or a threat** — and the two are told apart by behaviour, not hue: mani sits still and gutters slowly, threats move fast and strike. *(Amended 2026-08-12 by the mani-aura exception above; previously read "must be unambiguously a threat.")*

**Agni's dead hearths glow teal-violet — ⚠ VOID under the §2.4 amendment.** Teal-violet is now purge-only and appears nowhere in the wings.

> **⚠ REVISED AGAIN 2026-08-12 under the mani-aura exception.** The interim ruling was *"Agni's dead hearths do not glow at all."* That over-corrected. The ruling now: **the overwhelming majority of hearths, lamp-cups and gem racks are dead** — cold ash, black glassy slag, dull grey burnt-out gems, drifts of clinker. **A bare scattered few still hold live mani and glow true amber**, dim, static, and guttering out of sync with one another. The wing's thesis — *"still trying to burn, and failing"* — is carried by the **ratio**, not by total darkness: a hundred dead lamps and three live ones says it far louder than a hundred dead ones alone, because the live ones prove the rest *could* have burned. **Guardrail: live-mani points are dim, small, static and slow. If a generation returns a lit, warm, blazing courtyard, reject it — the failure mode is quantity and brightness, not the colour itself.**

## 2.6 Motion — "living structure via shader, NEVER mesh"

**The rule.** Do not deform, rig, or blend-shape environment meshes to make buildings move. It breaks silhouette, breaks collision, fights the silhouette-first flat-toon style, and is the single most over-budget thing a small team can chase.

**Achieve life through shader, light, and small-prop animation instead.** Three tiers, cheapest first — stop at any point:

1. **MUST — pulse the emissive veins.** Animate glow *intensity* on a slow curve. Nearly free. The eye latches onto rhythmic light in a dead, still world; this alone reads as alive.
2. **SHOULD — vertex-displacement surface swell.** Millimetre in-out wobble on *organic bits only* (bulging masses, vat membranes). Silhouette unchanged, so collision and style stay safe. No rig, no new geometry.
3. **PAYOFF — real animation on small props.** Specimens drifting in vats, fluid caustics, flexing membranes. Cheap to animate, and it lands at inspection distance where the player leans in.

**Build it once for Bhu, reskin per wing.** Same shader, four different rhythms — and the rhythm is characterization:

| Wing | Motion | Rhythm | Reads as |
|---|---|---|---|
| **Bhu** | Swells outward | slow regular heartbeat | Growth. Alive. |
| **Jal** | Flows downward | continuous trickle | Circulation. Still running. |
| **Agni** | Rises upward | **irregular guttering** | Dying. Cannot finish dying. |
| **Vayu** | Nothing physical moves | **pulse with holds** | Holding its breath. |

**Agni detail.** Thin columns of ash and dim embers lift continuously from every dead hearth, from the cavities beneath the treatment couches, from the lamp-cups — no flame, no heat, just endless convection. The building is *still trying to burn, and has been for millennia.* Veins gutter irregularly; surviving mani gems in the racks wink out of sync with each other. Regular rhythm reads as alive; irregular reads as dying.

**Vayu detail.** All the movement is trapped *inside* the tower; nothing on the outside moves at all.

- A travelling pulse of light runs the **nadi channels** — climbing ida, crossing at each chakra ring, descending pingala. The lotus-rings ignite in sequence.
- The rhythm has **holds**: rise, *stop dead*, descend, *stop dead*. Pranayama, not a smooth cycle. Vayu is the only wing that pauses.
- Inside each sealed bell-jar, a faint shimmer slowly swirls. **Bhu's horror is what's in the tank; Vayu's is that the tank looks empty and isn't.**
- **One bellows still works** — weakly half-inflating and collapsing on its own timing, out of sync with everything else. Something down there is still trying to breathe alone.
- Everything else is frozen. Banners, chains, bells: never a flicker. **Dust motes hang suspended without falling** — air so dead it won't let dust settle.

All four stay inside the rule. Emissive masks, small-prop animation, particles. **Nothing touches the architecture mesh.**

---

# PART THREE — THE FOUR WINGS

Each has approved concept art, filed in `_attachments/Last Rite/`. **⚠ All four predate the §2.4 crystal amendment — approved for form, not for threshold colour.**

| Wing | Reference |
|---|---|
| Bhu | `Env - Bhu Facility.png` · `Env - Bhu Facility Living Concept.png` |
| Jal | `Env - Jal Facility.png` |
| Vayu | `Env - Vayu Facility.png` |
| Agni | `Env - Agni Facility.png` |
| Aerial | `Env - Main Complex.png` ⚠ stale — see §6 open item 1 |

## 3.1 BHU — growth & the physical body

**Form.** A rocky, overgrown mountain-shrine. Bulging boulder-masses, real height and mass. Rises.

**Threshold.** High on the rock face — a **bright emerald** crystal in a carved arched doorway, reached by a **ruined, broken path**: cracked flagstones, uneven worn steps, moss through the gaps, loose fallen stone across the route. Trespassed upon, not maintained.

**The medical reveal.** Two or three **monumental specimen-vats** in carved devotional stone housings, set asymmetrically at different heights, holding suspended half-formed biological specimens, partly overgrown. Around the base and on the ledges, **clusters of smaller specimen jars** — some toppled, some cracked, tangled in roots. A facility abandoned mid-work.

**Signature detail.** The **scale mix** — monumental vessels plus scattered small ones — is more interesting than either alone, and tells you the facility worked at multiple scales. Pale **fungal growth** on the upper slopes fits Bhu's decay-and-regrowth and distinguishes it from generic moss-and-vines.

**Veins.** Deep mineral emerald, spreading like a circulatory system across the rock, converging on the crystal.

**Palette.** Ochre, sandstone gold, mossy green, umber, rocky brown.

**Motion.** Breathing veins on a slow heartbeat. Specimens drifting in the vats.

## 3.2 JAL — blood & fluids

**Form.** A monumental **sunken stepwell** (baoli / vav), descending into the ground in tiers. Sinks. The perfect foil to Bhu: Bhu is a mass that rises, Jal is a void that sinks.

**Architecture is BOLD AND OPEN** — only two or three grand staircases, widely spaced, with broad terraces, pillared galleries and **large uninterrupted stone wall faces** between them. *(Learned the hard way: a dense stair lattice becomes wallpaper and leaves the veins nowhere to travel and the terraces nowhere to hold pools.)*

**Threshold.** At the floor of the well — a **bright indigo** crystal in a carved arched doorway, reflecting in a pool of dark fluid. Visible in straight line of sight from the rim. **Line of sight is built into the form.**

**The medical reveal.** The descent *is* the filtration. A **single cascade**: fluid collects in carved catch-basins on each terrace and spills to the next, stepping down to the pool at the crystal's feet. **One fluid, one colour, no mixing** — deliberately, to stay cheap.

The purification story lives entirely in the **vessels**, not the fluid: upper basins caked, crusted, heavily stained → lower stages progressively finer and cleaner → the glass nearest the crystal almost pristine. A material gradient, not a fluid sim.

**Avoiding gore.** Treat it as **alchemy, not surgery.** Apothecary glass, settling vessels, mineral crusts, dark resinous residue, stained tide-marks. Blood implied through **stain and sediment**, never splatter.

**Veins.** Deep indigo, running *within the stone* as branching cracks of light — unmistakably light in rock, never liquid. **This separation is the wing's key lesson: emissive glow = mani only; the blood theme is non-emissive stain.**

**Palette.** Warm sandstone gold, ochre, verdigris copper-green, umber, dusty rose, weathered basalt.

**Motion.** Flowing fluid, continuous trickle.

**⚠ Open note.** The cascade fluid in the approved art reads bright saturated red — push it to **deep oxblood, matte, non-emissive, near-black in shadow** at material time, so the Perilous telegraph stays the brightest red in any room.

## 3.3 VAYU — breath & spirit

**Form.** A **tall, slender, hollow tower-shrine**. Open skeletal structure, heavy negative space, walls of pierced stone lattice (jaali). Wind passes straight through. Clearly much taller than wide.

**Threshold.** A single **bright pale mint-green** crystal *(changed from silver-lilac, 2026-08-12 — §2.4; hold it cooler and paler than Bhu's jade)* **suspended free in mid-air** at the tower's heart, on chains — seen **through** the lattice from outside. A spiral of ramps, stairs and thin bridges winds up inside toward it, ending in a bridge whose final span has **fallen away**. The route was complete once and the ruin took it.

**The medical reveal — yogic breath-anatomy as architecture:**

- **The nadis.** Three main vein channels run the tower's full height — two spiralling around the central shaft in a **double helix** (ida and pingala), wrapping a third that runs dead straight up the middle (sushumna) into the crystal. *This is the wing's whole thesis: the veins are not ornament, they are the building's breath-anatomy.*
- **Chakras.** Carved stone lotus-rings threaded up the central core, one per tier, veins passing through each.
- **Bhastrika bellows.** Monumental paired bellows in the tower flanks like collapsed lungs. *Bhastrika* literally means "bellows" — authentic pranayama equipment.
- **Breath-retention vessels.** Person-sized glass bell-jars, sealed and **completely empty** save a faint shimmer.
- **Practice apparatus.** Empty meditation seats worn smooth, mala bead-strings for counting breath cycles, graduated timing vessels for kumbhaka.

**The horror.** Bhu's vats are full of something terrible. **Vayu's are empty** — breath and spirit leave nothing behind to see. The empty seats do the quiet work: someone sat in every one of them.

**Colour.** The palest wing, but **NOT monochrome white** — warm pale sandstone, cream, bleached ochre with dusty rose banding. Colour arrives via **faded textiles** (saffron, indigo, madder-red banners, sun-bleached, hanging **dead limp**) and **metal patina** (verdigris copper-green, tarnished brass), which ties it back to Jal's palette.

**Debris.** Drifts of dust and dry dead leaves banked into every corner — debris the wind *would* have cleared, piling up because there is none.

**Motion.** See §2.6. The stone breathes and nothing else does.

## 3.4 AGNI — energy, life-force, metabolism

**The most intense wing in the game.** Its dressing and enemy design should feel like a final trial.

> **⚠ 2026-08-12 — "the hardest area" is now a TUNING target, not a POSITION.** With strata order free (§1.7), Agni **cannot assume it is played last** — a player may open with it. Its *character* stays maximal (density of dressing, elaborateness of the shrine mass, roster intensity); its *difficulty baseline* must come from `strataCleared`, not from being Agni. See §1.7 consequence 2.

**Form.** A carved **Hindu temple courtyard complex** — devotional stone architecture FIRST, never industrial. Pillared mandapa porches, ornate columns and brackets, deep arched niches, banded mouldings, stepped plinths. Walled, asymmetric, irregular, built around a great raised **stepped fire-altar**, now dead and heaped with cold ash.

> **Production warning.** The word "furnace" drags generations industrial — an early attempt came back as a brick kiln with no temple in it at all. **Name the temple before you name the fire, or the fire wins.**

**Threshold.** Ground level, across the courtyard — a **bright amber / fire-gold** crystal *(resolved 2026-08-12, §2.4)* in the arched doorway of the main shrine mass, head-on in unbroken line of sight.

**Lamp-towers.** Two tall carved **deepstambha** flank the courtyard, ringed with dozens of protruding stone lamp-cups. Every lamp extinguished, black soot streaks smeared up the stone above each cup, a bare few holding one faint ember.

> **The image that ties fire to health:** one lamp per patient, burning as long as that life burned. **A wall of dead lamps is a wall of dead people.** Devotional, distinctly Hindu, instantly legible.

**The medical reveal — clinical, NOT athletic.** *(An akhara / wrestling-gymnasium version was tried and rejected: too literal, too mundane for a place chasing immortality.)*

- Rows of carved stone **treatment couches**, each with an open hearth-cavity beneath it — bodies were warmed from below. Iron mounts and straps at the corners.
- Small domed **sweat-chambers** set into the walls, doors hanging open, ash spilling out.
- Bronze braziers, censers, standing cauldrons.
- Carved stone friezes of **musculature and sinew** — anatomical votive reliefs of the body's inner heat.

**Mani as fuel, mostly spent.** Open racks and stone trays of dull lifeless **grey burnt-out mani gems**, a scattered few still faintly alive and glowing **true amber** at *different intensities from one another* (legal under the mani-aura exception, §2.5 — dim, small, static). Drifts of burnt grey-black mani clinker in every dead hearth. Tipped baskets spilled across the ground. **Every one of those dead stones was somebody's treatment.**

**Lived-in traces.** Bhu and Jal feel alive because something *kept happening* after the people left — moss kept growing, fluid kept trickling. Agni has only static ash, so it needs the other kind of life: **traces of the people who worked there.** Black soot **handprints** on the walls, grooves worn into the steps by generations of feet, oil-darkened floor patches, tools and cloth dropped mid-work.

**Veins.** Deep dark antique bronze, branching like a nervous system, feeding directly into every hearth, couch and lamp-tier. Brightness **uneven and irregular** — some stretches faint, others dead and filled with black glassy slag. A fire banked low and guttering out.

**Palette.** Dark terracotta, oxblood brick-red, warm soot-black, verdigris copper and tarnished brass, ochre, ash-grey, umber, worn gold.

> **⚠ AMENDED 2026-08-12 — the old line read "No orange, no fire anywhere."** That is now **"no orange *surfaces*, no fire."** Under the mani-aura exception (§2.5), **live mani may glow true amber** in gems, lamp-cups, braziers, hearth cavities and the threshold crystal. What stays banned is **orange as a material or as mood**: no orange-painted stone, no orange banners, no orange grime, no warm firelight washing the courtyard. And still **no flame** — Agni's whole thesis is that nothing here burns any more. The glow is *residual mani*, not fire.

**Motion.** See §2.6. Rising ash; irregular guttering.

---

# PART FOUR — PRODUCTION CONVENTIONS

## 4.1 Current phase

**Mood and identity exploration in 2D — NOT Meshy production input yet.**

Wing concepts are generated as **isolated structure studies on a clean neutral background**, to judge silhouette honestly and to give cleaner 3D input later.

Pipeline when the time comes: 2D concept sheet FIRST → Meshy image→3D (never text→3D) → mandatory unifying post-pass (shared toon shader → corruption overlay → grading LUT). This is the same discipline as [[1b Last Rite - Art Bible]] §5.2/§5.3, and the **all-sides sheet rule** applies when a piece goes to production.

## 4.2 Every prompt bakes in

- Stylized chibi-toon, flat-shaded, silhouette-first, bold and chunky, no greebling *(the §1 style lock in [[1b Last Rite - Art Bible]] — unchanged)*
- **No legible text. No deity idols. No human or divine figures.**
- Warm faded mineral palette — colourful but dead
- The **only** emissive saturated glows: element-colour raw-mani veins (dark, matte) + the element-colour threshold crystal (bright) + **a rare few live mani gems / lamp-cups** (small, dim, static — §2.5 mani-aura exception). Nothing else emits.
- Ruined and abandoned, never maintained or pristine
- Devotional temple read FIRST; medical-arcane facility on closer look

## 4.3 Generator behaviours learned

| Problem | Lever |
|---|---|
| Symmetry creeps in the moment you say "staircase" | Say **asymmetric**; add **loose fallen stones across the path** — debris is what breaks it |
| Stairs multiply into wallpaper | Cut to **"one grand staircase"**; generators treat stairs as free ornament |
| Veins come back as a sparse accent | Say **"like a nervous system"**, "covering the structure"; they need explicit permission to dominate |
| Veins read as decoration, not life | Demand **uneven brightness** — a bright patch fading in both directions reads as something *travelling*; an even glow reads as ornament |
| Gems multiply | State **"ONE SINGLE SACRED CRYSTAL ONLY, no other gems anywhere"** |
| Structures come back pristine | Explicitly list damage: cracks, missing panels, grime, rubble, snapped fittings |
| Agni drifts industrial | Lead with **"HINDU TEMPLE COMPLEX, carved devotional stone architecture FIRST"** before any mention of fire |
| Reserved colours creep into décor | Specify **dim, dark, metallic, matte, non-emissive** for **surfaces**; never "glowing orange/red" *stone*. Live **mani** is exempt (§2.5 mani-aura exception) — but keep it **small, dim, static and rare** |

## 4.4 Motion in still frames

Motion can't appear in a still, but its **evidence** can — ash caught mid-drift, a bright patch partway along a vein channel, a bellows caught half-open, lower lotus-rings lit and upper ones dark, dust hanging where it shouldn't.

## 4.5 Shading & surface model — LOCKED 2026-08-12

**The environment is LIT. Characters are UNLIT.** Same shader (`BGamer/AnimeToonLit`), per-material `_Unlit` toggle — environment materials leave it at **0**. Full reasoning and the table: [[1b Last Rite - Art Bible]] §1.1. Unlit architecture is flat cardboard, and it would have voided this bible's own per-wing grading LUT.

**Surface stack, in order:**

1. **Flat palette colour.** One hand-authored ~512² atlas of ~30 swatches drawn from §2.5's mineral palette (sandstone gold, ochre, dusty rose, verdigris, basalt, terracotta, soot-black…). Meshy's generated PBR is used as **colour reference only and then discarded** — this is how §5.3's *"never ship a raw Meshy texture"* rule gets enforced structurally instead of by discipline. Whole kit lands on one material.
2. **Real-time lights** carry form. This is what makes carving read as carved.
3. **Baked vertex colour**, three channels: **R = ambient occlusion** (contact darkening in creases and inside corners, which no directional light gives you) · **G = cavity** (grime pooling, soot in carving) · **B = convexity** (stone rubbed pale on exposed edges — the worn step-grooves of §3.4 and Vayu's smooth meditation seats). Same kit piece reads clean in the spine and filthy in Agni by tinting G and B.
4. **Per-wing grading LUT** last.

> **⚠ Never bake directional dust into a kit piece.** "Dust settles on upward faces" breaks the instant the piece is rotated, and a modular kit rotates constantly. Bake AO and cavity (orientation-independent); compute up-facing dust **in the shader from the world normal.**

**Detail resolution rule:** vertex colour resolves at vertex density (~8 cm on a 4 × 3 m panel at 2–3 k tris) — fine for soft gradients, useless for crisp marks. §3.4's **soot handprints need decals**, not vertex paint.

**Meshy input discipline (verified against Meshy docs 2026-08-12):** **ONE object per image, always.** Multi-object images are a documented failure mode — *"AI merges multiple objects into one distorted mesh."* To get a matched set, author one master sheet with all the pieces together as the **human consistency anchor**, then crop each piece out, remove backgrounds, and run them through **Batch Images to 3D** (up to 10 per batch, one model each). Multi-View is per-piece only, ≤3 images, consistent camera distance and scale, and needs Meshy 6 + Pro/Studio. **Polycount is a setting** (Smart Topology target 100–15,000), never something to control by cramming objects into one generation.

---

# PART FIVE — CONCEPT ART STATUS

| Asset | File | Status |
|---|---|---|
| Bhu wing | `Env - Bhu Facility.png` | ✅ approved — form/palette. ⚠ crystal recolour → emerald |
| Bhu wing (living) | `Env - Bhu Facility Living Concept.png` | ✅ approved — motion/overgrowth study |
| Jal wing | `Env - Jal Facility.png` | ✅ approved — form/palette. ⚠ crystal recolour → indigo · ⚠ cascade fluid → oxblood |
| Vayu wing | `Env - Vayu Facility.png` | ✅ approved — form/palette. ⚠ crystal recolour → **pale mint-green** |
| Agni wing | `Env - Agni Facility.png` | ✅ approved — form/palette. ⚠ crystal recolour → **amber / fire-gold** · ⚠ verify veins stay bronze |
| Aerial / complex | `Env - Main Complex.png` | ✅ **REGENERATED 2026-08-11** — now carries all four correct wings + the dark shikhara sanctum. Approved for layout/massing. ⚠ three fixes before use as Meshy input: **strip the fake carved glyphs** from the canyon walls (§2.2 hard rule) · Vayu + Jal crystal recolour · **square off the spine's entrance terminus** (below) |

> **⚠ CONCEPT-ART CORRECTION — the spine's entrance terminus (developer, 2026-08-12).** In the aerial, the processional spine's near end flares into a **stepped apron with angled flanks that reads as a tapered triangular head**. **It should be a box** — a squared rectangular terminus. The greybox is already correct here (`Plaza_Entry` is a plain 40 × 24 m block); it is the *art* that needs the fix, so it does not get baked into a Meshy generation later. The far/sanctum end of the spine is fine as drawn.

All filed in `_attachments/Last Rite/`, matching the enemy concept-art convention. Originals live outside the vault in `D:\Code\Claude\Concept art work\Enviornment\`.

---

# PART SIX — OPEN ITEMS

1. ~~**The aerial no longer matches.**~~ **✅ CLOSED 2026-08-11** — `Env - Main Complex.png` was regenerated and now carries the four correct wings (Vayu lattice tower, Agni courtyard, Bhu rock-shrine, Jal stepwell) around the dark shikhara sanctum, with the spine-and-branches layout intact. Two residual fixes tracked in §5: **strip the fake carved glyphs from the canyon walls** (§2.2 hard rule — developer confirmed removal 2026-08-12) and the Vayu/Jal crystal recolour.
2. ~~**Agni's threshold crystal hue.**~~ **✅ CLOSED 2026-08-12** — resolved to **amber / fire-gold** by the mani-aura exception (§2.5). Garnet dropped.
2b. **⚠ NEW — Bhu and Vayu are both green.** Vayu's crystal moved from silver-lilac to green (developer decision 2026-08-12). Recommendation: Bhu deep warm jade, Vayu pale cool mint/celadon. **Confirm at gray-box.**
3. **What ties the wings to the sanctum** — §2.4 OPEN #2. Recommendation: teal-violet becomes sanctum-exclusive. Blocked on item 5.
4. **Crystal recolour pass** on all four approved wing images + the aerial (§5) before they are used as Meshy input. Final hues: **Bhu emerald/jade · Jal indigo · Vayu pale mint-green · Agni amber/fire-gold · sanctum teal-violet.**
5. **Jal's cascade fluid** → push to deep oxblood, matte, non-emissive.
6. **Agni's veins** → verify they stay dark bronze, not amber-orange, at material time.
7. **Jal's dirty→clean vessel gradient** didn't survive to the exterior. Not load-bearing there, but it's the best vehicle for the purification story — carry it into Jal interior concepting.
8. **Difficulty ascension under free order** — §1.7 consequence 2. Recommendation: key to `strataCleared`. Confirm at gray-box; touches [[1d Last Rite - Reaction & Feints Spec]] §3 and D9.
9. **Not yet designed:** the central sanctum / boss-heart interior (where all four functions were combined into the immortality-medicine; the researcher-boss = her future); the Chaos Descent pit; per-stratum audio-log and lore arc; all interior concept sheets.
10. **Franchise timeline plant — NOT propagated.** This doc's era note (§1.1) originally read *"a few years before the Looter Shooter's cryo-waking."* That would be the **first time Last Rite is dated on the shared timeline**, and world-level canon is owned by [[12 The Akashic and The Bleed]] / [[14 Naming Glossary]] — which currently date the waking but never date this game. **Deliberately left out of §1.1 and out of the LS docs (scope decision, 2026-08-12).** If adopted, it belongs in the LS canon docs first, and [[1k Last Rite - Lore Bible]] §10 second.

---

*End of document.*
