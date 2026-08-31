# Game 1 "Last Rite" — Combat Iteration Log

> A running record of where the combat **design + architecture moved, and why.** The specs hold *current* state — [[1 Parry Combat - Last Rite]] (design + D-locks), [[1a Last Rite - Code Architecture]] §AS-BUILT (engineering truth), [[1c Last Rite - Combat Core Spec]] (v0 intent), [[1d Last Rite - Reaction & Feints Spec]] (M1 slice). **This doc holds the *trail*** so an old decision is never mistaken for the live one.
>
> Newest first. Dates absolute (today's baseline: 2026-08-12).
>
> **⚠ Fiction vs. combat:** this log tracks *combat* design. Since 2026-08-11 the **story** has its own authority — [[1k Last Rite - Lore Bible]]. Where a spec's story detail disagrees with the Lore Bible, the Lore Bible wins; the 2026-08-11 entry below records what it superseded.
>
> **⚠ Environment vs. combat:** since 2026-08-12 the **place** has its own authority too — [[1l Last Rite - World & Environment Bible]] (the complex, the wings, the threshold grammar, environment colour + motion law).

## Why this log exists

The early specs (`1a`/`1c`, written pre-code in June 2026) describe a design the build has since pivoted away from in several **load-bearing** ways — the timing model, the animation pipeline, the player's offense, even a foundational invariant. Rather than delete the original reasoning, we log each pivot: the blueprint stays legible as *intent*, and every place reality diverged is explained here. If a spec sentence and this log disagree about *what was planned*, this log is the record; if they disagree about *what is true now*, the spec + `1a` AS-BUILT win.

---

## 2026-08-12 — **The environment is locked; the strata order is FREED**

*The place the game happens in gets designed for the first time, and it changes the shape of the run. New canon: [[1l Last Rite - World & Environment Bible]] (authoritative for architecture, layout, environment colour and environment motion).*

**The environment direction is new content, not a pivot** — the complex, the four wings, the threshold grammar, the per-wing palettes and the shader-motion law had no prior owner. Four wing exteriors have approved concept art. What follows is only the part that **overturned something already locked.**

- **1. Strata order: FIXED → FREE.** *(the structural one)*
  - **Was:** `Bhu → Jal → Vayu → Agni`, a sequence, Agni terminal (locked 2026-06-11, restructured 2026-08-11).
  - **Now:** four wings **in any order**, each granting a **seal**; all four open the **sealed sanctum**, which holds the heart and the ending fork.
  - **Why:** it follows the building. The complex is **one processional spine with four wings branching off it** and the sanctum at the far end, in constant view from the entrance — the prize is on screen the whole game and the player picks their own road to it. Free order isn't bolted on; it's what that architecture *is*.
  - **Cost, and where it is paid:** `StratumDef.isTerminal` is **void** — terminality moves to the seal-gated sanctum, and `DescentDef` becomes **spine + four independently-enterable branches + a gated sanctum**, closer to a hub than a corridor ([[1a Last Rite - Code Architecture]] §4.3). **Difficulty must key off `strataCleared`, not `stratumIndex`** — any wing can be first, and Agni is the most intense ([[1d Last Rite - Reaction & Feints Spec]] §3, ⚠ open at gray-box). Same axis fixes the fraying-onset anchor, which named **Jal** as "the second" ([[1j Last Rite - Shroud, Mani & Sanity]] §3.1). **"Agni is the climax stratum" is void** — the climax is the element-neutral sanctum, which *strengthens* the element-neutral ending rather than weakening it.

- **2. Environment colour: GREY → COLOURFUL.**
  - **Was:** *"in a dead world, saturation = information"* — desaturated earth/stone neutrals, "because the world is grey, any saturated colour reads instantly as meaning" ([[1b Last Rite - Art Bible]] §3).
  - **Now:** **"the dead world is colourful"** — a warm faded mineral palette (sandstone gold, dusty rose, ochre, verdigris, basalt, terracotta) where colour comes from **material and light**, not saturation.
  - **Why it doesn't break the information channel:** the channel was never carried by saturation alone. **Value + motion carry it** — décor is dark, matte and static; gameplay hues are bright, saturated, momentary and on an enemy. Two new décor channels are added (element-colour veins, element-colour threshold crystal), each with a per-wing collision guardrail.

- **3. Threshold crystals: TEAL-VIOLET → ELEMENT-COLOURED.** *(the colour law working as designed)*
  - **Was proposed:** a teal-violet crystal in every wing's doorway, as a visual through-line to the sanctum.
  - **Rejected**, because [[1b Last Rite - Art Bible]] §3 reserves teal/white-violet for **purge/purification** and forbids gameplay colours as décor — and this was décor in a reserved colour in *every wing*.
  - **Now:** each wing's crystal takes **its own element's hue**, at maximum brightness and saturation, with the veins as the same hue kept dark and matte. **Value separation replaces "only object with that colour."** The grammar got stronger: the crystal is the apex of the colour the player has been reading all along.
  - **Two things this left open:** Agni's hue (bronze taken to max saturation lands on the reserved orange — recommendation: garnet), and what now ties the wings to the sanctum (recommendation: teal-violet becomes sanctum-exclusive, since the sanctum is literally where purification was manufactured). Both ⚠ open, [[1l Last Rite - World & Environment Bible]] §6.
  - **Production debt:** all four approved wing images were generated teal-violet. **Approved for form, not for threshold colour** — they need a recolour pass before any Meshy input.

- **4. New art law — the environment must never deform its mesh.** Life comes from **shader, light and small-prop animation**: pulse the emissive veins (mandatory, nearly free) → organic-only vertex swell → animated small props. Build once for Bhu, reskin per wing with four rhythms, and the rhythm is characterization (Bhu heartbeat · Jal trickle · Agni irregular guttering · Vayu pulse-with-holds). Filed as [[1b Last Rite - Art Bible]] §10 because it is a **budget guardrail**, not a wing note.

- **5. Kit re-priced — five spaces → eight.** "Four strata + tutorial" was wrong twice: **Agni is not the sealed core** (it's a ground-level temple courtyard; the sanctum is separate), and the placeholder wing names bore no relation to the designed forms. Real list: entrance · processional spine · 4 wings · sealed sanctum · **Chaos Descent pit** · tutorial. Layout volume was already the main length risk in M4; it just grew ([[1e Last Rite - Build Roadmap]]).

**Nothing retracted:** the §1 style lock, the rig amendment, the Shroud ablation law, the fairness firewall, elements-are-enemy-side, the three endings, and the tragedy-not-mystery framing all stand untouched. The "hospital built like a temple" register was **sharpened, not replaced** — temple is the architecture, medicine is the function, funeral is the residue; all three are true at once ([[1k Last Rite - Lore Bible]] §3).

**Deliberately NOT done:** the draft placed this game *"a few years before the Looter Shooter's cryo-waking"* — the first hard date Last Rite would have on the shared timeline. **Left out** (scope decision): world canon is owned by [[12 The Akashic and The Bleed]] / [[14 Naming Glossary]] and a franchise plant belongs there first. Logged as open in [[1k Last Rite - Lore Bible]] §11.

---

## 2026-08-11 — **The story spine is locked; the rebirth loop is CUT**

*The largest single pivot in the design's history — it removes a structural pillar that had been load-bearing since the 2026-06-10 concept lock. New canon: [[1k Last Rite - Lore Bible]] (authoritative for all fiction).*

- **Was:** a purifier wandered ruin to ruin laying the corrupted to rest. She descended a generic sealed ruin (~3 strata), killed the **beast at the heart**, claimed **immortality as a pickup**, and **woke at the ruin's mouth to re-descend the same ruin ~5 times** — 1 neutral tutorial cycle + 4 elemental rebirths (Bhu→Jal→Vayu→Agni) painted on as remix overlays. That NG+ curse loop was the replay engine, the difficulty ladder, the content multiplier, *and* the delivery vehicle for a **layered mystery** ending in a single true ending (D1/D2/D7).
- **Now, five changes:**
  1. **The place is specific.** The Accord's **mani-medicine division** — pre-Breach it chased a cure for death; post-Breach it reverse-engineered Husk-deathlessness into a medicine, and a bad batch turned **its entire staff** into engineered Husks. It sealed itself as a tomb. Every enemy is its staff, or a purifier who came hunting the same prize and lost.
  2. **The motive is specific.** The **Kavach Rite leaks** — she needs a constant mani feed or she dies. She dives to **steal** permanent immortality so she can stop feeding it, not knowing the cure is what hollowed everyone inside. *(This answers the open question the 08-09 session flagged as too thin to leave unresolved.)*
  3. **The loop is cut.** One **one-way dive**: tutorial → four elemental **strata** → the heart. The strata *are* the rebirths — each **re-teaches the fight rather than raising numbers**, which is the one law that survives intact from D2.
  4. **The ending forks three ways:** **Take it** (tragic true ending → LS baton) · **Renounce it** (walk out mortal, mind intact) · **Husk out** (sanity-zero failure, blind). Scar count prices the fork. The **beast's heart is gone**; the heart is the **lead researcher who tested the medicine on themselves.**
  5. **Tragedy replaces mystery.** The player sees the doom coming; the staged reveals are moved to the front and the tension becomes dramatic irony. The replay pillar "the mystery unfolds across rebirths" is replaced by **the clean run** — finish with sanity intact; a no-scar clear is the flex.
- **Also changed, in the same pass:** **fraying now buys power** (damage + elemental resistance) instead of being pure cost, making it a real tactical spend; **scarring gains greed triggers** (body damage · gem-buff leaning · sanity-floor pushes) **alongside** the every-Nth-death countdown — *developer decision: keep both sources*; and **the gem IS the kill** — tearing the mani-gem out of a Husk is the purification finisher, not loot that drops after.
- **Why:** the loop justified replay but not *dread*. With a curse cycling her forever, "one last rite" was never last and greed had no terminal price — the player always got another lap. A one-way dive makes the descent itself the doom, puts the theme (greed vs. restraint) on a ratchet the player can watch closing, and moves the replay value from *withheld information* to *demonstrated restraint*, which survives a second playthrough where a mystery cannot.
- **⚠ Costs to watch (logged so they are not rediscovered):** the ~5× re-descent was a **content multiplier** — the four strata must now carry the whole 3–5hr on their own (M4 layout volume is the length risk); the **environment kit amortises over four distinct places on one pass instead of five re-skins**; and **kalpa reset is now the only repeat structure**, making scar tuning the sole pacing control on run length.
- **Specced now:** [[1k Last Rite - Lore Bible]] (new, authoritative) · [[1 Parry Combat - Last Rite]] (elevator pitch, setting, core hook, replayability, "The heart & the choice", scope, D1/D2/D7, the elemental **strata** spine) · [[1j Last Rite - Shroud, Mani & Sanity]] (§0 fiction, §2 gem-as-kill, §3.1 fraying trade, §3.2 greed scars, §3.3 the fork, §4 kalpa, §8.1 answered, §8.2 rewritten) · [[1i Last Rite - Bhu Mani Husks]] (origin + gem canon) · [[1b Last Rite - Art Bible]] (§4, §5.1 four-stratum facility kit) · [[1e Last Rite - Build Roadmap]] (M3 descent spine, M4, M5).

---

## 2026-08-09 (b) — **Menu difficulty ships**; endless gains **Steam leaderboards**; the hallucination toggle becomes a playtest instrument

- **Was:** difficulty was **purely diegetic** — the rebirth ladder (`delay[tier]`, 2026-07-18) with **no menu difficulty setting** anywhere in the design; "embrace the hallucination" was a deferred post-launch stretch; leaderboards were never specced (only per-duel ranking + gauntlet score).
- **Now, three developer decisions:**
  1. **A menu difficulty setting ships**, driving the **same** mechanism as the rebirth ladder — the defense-window **start-delay** ([[1d Last Rite - Reaction & Feints Spec]] §3). Two axes now write one number; **composition is open** (recommended: menu difficulty **scales the ascension curve** rather than adding a flat offset, so each rebirth still escalates at every setting). All §3 guardrails hold — Perfect stays un-delayed by default, and the zero-width assert is now load-bearing at the *easiest×deepest* and *hardest×deepest* corners both.
  2. **Endless ships Steam leaderboards.** Rationale: the endless mode holds level layout and difficulty constant — only enemy selection and element mixing vary — so runs are comparable in a way the campaign never was. **Hard requirement: boards segregate by difficulty setting** (and by hallucination-toggle state until that toggle's scoring policy is settled), or easy-tier runs dominate a single board.
  3. **The hallucination toggle is promoted from post-launch stretch to a build item, shipped ON/OFF for playtesting** — the developer wants to test the fake-cue sanity experience both ways before deciding its final status. Its craft rule is now recorded (below); the *decision* about whether it stays, becomes default, or earns a reward is explicitly deferred to playtest.
- **Craft rule recorded for the toggle (the thing that would otherwise be lost):** when ON, corruption may **add false-positive cues (phantom sparks, early/ghost tells) — never remove or delay real ones**, and it may **never touch window timing**. Corrupt *information*, never the contract. Absent cues read as a bug; false cues read as her mind.
- **Why:** the developer wants real accessibility/approachability options (the vault's diegetic-only ladder assumed players accept the game's fixed bar) and wants endless to carry competitive replay value. The determinism caveat is recorded in [[1j Last Rite - Shroud, Mani & Sanity]] §5.1 — the as-built MonoBehaviour/animation-as-clock architecture **cannot** support replay-verified boards, so validation is heuristic and the board is honest about that.
- **Specced now:** [[1j Last Rite - Shroud, Mani & Sanity]] §3.4 + §5.1 + §8; [[1 Parry Combat - Last Rite]] D7 + D8 notes; [[1d Last Rite - Reaction & Feints Spec]] §3 (menu axis).

## 2026-08-09 — **Mani returns to the player as DEFENSE ONLY** (the 08-07 cut narrowed, not reversed)

- **Was:** the 2026-08-07 cut removed the elemental player layer entirely — "no Mani-based attacks of any kind… no Mani meter. Purge is the player's only meter." Elements enemy-side only.
- **Now:** **mani re-enters player-side as a defensive economy**: Husks drop raw/elemental mani; it repairs the **Shroud** (the player's new ablative armor layer — next entry) and sets its **element** (a resist + a paired weakness). **Everything offensive stays cut:** no Mani attacks, no absorption, no trails; **Purge remains the only offensive meter**, AP the only offense input; the reserve is one scalar + an element tag (no inventory). Elements still never touch the player's reaction windows.
- **Why:** the developer's Shroud/health design (2026-08-09 session) needed a diegetic repair resource, and mani-as-Husk-substance turns healing into the game's thesis — the cure IS the disease (see the sanity entry below). The 08-07 cut's actual rationale (no animation source for a Surge; a second *offensive* meter in flux) applies only to offense — defense needs no animation and no offensive meter. Confirmed by the developer 2026-08-09.
- **Specced now:** [[1j Last Rite - Shroud, Mani & Sanity]] §2; [[1 Parry Combat - Last Rite]] D6-amendment + elemental-spine cut notes annotated; [[1a Last Rite - Code Architecture]] §6 guardrail note.

## 2026-08-09 — Player health = **the Shroud** (two-layer ablative armor) + death economy + the **kalpa loop**

- **Was:** single HP pool (D8 "~4 hits to death"); death = retry at duel start with no further economy; no failure ending — replay pressure was all victory-side (rebirths).
- **Now:** **the Shroud** — a mani-woven second skin granted by the pre-game **Kavach Rite** — is the primary health layer, ablating through **four visible states (Vested → Kindled → Unbound → Ashform)** rendered as a dissolve+emissive pass on one mesh; beneath it a short **body bar** that mani cannot heal (shrine-only). **Ashform is risk/reward, not a fear state:** execution-gated damage amp on Perfect-parry counters + the Purge ultimate (considered-and-cut: passive sanity drain for merely being in Ashform). **Death:** the scar countdown ticks (next entry) + **unconverted mani drops at the fall, one retrieval**. **Husk-out ends the kalpa** → campaign resets as **Kalpa N+1** with light carry-forward (unlocks, lore, knowledge) — diegetic per the locked kalpa canon plant.
- **Why:** the developer wanted health made *visible on the body* (the 2026-08-09 ablation concept study) and real teeth on failure without mid-campaign run-deletion; at the locked 3–5hr core a Sifu-shaped reset is mastery pressure, not content-deletion. The Shroud also gives encounter tuning a readable instrument (state-at-boss-door).
- **Specced now:** [[1j Last Rite - Shroud, Mani & Sanity]] §0–§1, §4; [[1 Parry Combat - Last Rite]] D1 + D8 amendments; [[1b Last Rite - Art Bible]] §9 (visual law); [[1a Last Rite - Code Architecture]] §2.6 save note.

## 2026-08-09 — **Sanity gains teeth**; the fairness firewall narrows to "combat information never lies"

- **Was:** D7 — "sanity = atmospheric only, fairness sacred"; presentation-only degradation with no mechanical consequence; "embrace the hallucination" toggle a deferred stretch.
- **Now:** two-pool sanity mirroring the health layers. **Fraying** (recoverable — drained by Ashform body damage + field mani use; restored at shrines) expressed through **sensory narrowing** (world ducks/desaturates; the enemy + telegraph VFX stay full-contrast) and **HUD unreliability** (abstract UI flickers; diegetic reads always true; shrines honest). **Scarring** (permanent per kalpa — every Nth death blackens a notch; death-screen countdown; no recovery) → **zero arms the Husk ending** on the next death. **Cue corruption (false sparks, phantom tells) is default-off forever** — it lives only behind the opt-in toggle, tuned/enforced after playtesting. Accessibility: cue audio preserved by construction; steady-HUD toggle; a degradation floor.
- **Why:** sanity with no teeth is a number players ignore — but at a ~130ms Perfect window a false cue is a reflex hijack, not atmosphere. The firewall was right *about combat information* (same DNA as the 07-18 difficulty entry: earned muscle memory stays valid); stakes + the wrapper make sanity *felt* without the game ever lying mid-duel. Both channels confirmed by the developer 2026-08-09.
- **Specced now:** [[1j Last Rite - Shroud, Mani & Sanity]] §3; [[1 Parry Combat - Last Rite]] D7 amendment + sanity-section note; [[1a Last Rite - Code Architecture]] §4.5 note; [[1e Last Rite - Build Roadmap]] M5 note.

## 2026-08-09 — **Chaos Descent promoted to core v1**; the endless mode becomes the replayability pillar

- **Was:** Chaos Descent = post-true-ending v1-stretch / first content update; Endless = future free update "once procgen matures"; replay centered on the four authored layers.
- **Now:** **core v1.** Still zero procgen (random selection of authored content), extended to **mixed per-encounter element assignment**; **sanity is the run timer** (only falls; mani use accelerates it; the run ends when she turns); curated composition via role tags; **score = depth × sanity kept**; unlocks at the **first ending reached** (true or Husk — proposed: the Husk ending may flow straight into it as an epilogue). The campaign ending screen reports kalpa count · deaths · score; the full combat-scoring pass stays a deferred design session.
- **Why:** the developer re-scoped the campaign as the ~3–5hr authored core with replayability explicitly centered on endless; bolted-on endless modes get ignored, so it must be reachable from every ending and share the campaign's scoring spine.
- **Specced now:** [[1j Last Rite - Shroud, Mani & Sanity]] §5; [[1 Parry Combat - Last Rite]] replay-layers + Chaos notes; [[1e Last Rite - Build Roadmap]] definition-of-done + M5 notes.

## 2026-08-07 — **Mani Surge CUT**; elements are enemy-side only again

- **Was:** the blade absorbed the rebirth's dominant element (Descent 2=Bhu … 5=Agni), gaining elemental particle trails plus **one signature Surge per element**, fuelled by a separate provisional **Mani meter** fed by Perfect parries. Mani Surge held technique slot #1 of the ≤2 budget (locked 2026-07-31).
- **Now:** **removed from Game 1 scope entirely.** The player has **no Mani-based attacks of any kind** — no absorption, no Surge, no elemental trails, **no Mani meter**. **Purge is the player's only meter.** Both ≤2 technique slots are unclaimed again; remaining candidates are non-elemental (perfect-dodge→counter, chain-parry stance).
- **Why:** the developer scoped it out — the earlier intent was Mani-flavoured *grades/tiers*, not player attacks. Cutting it also removes the layer's two loose ends at once: the Surge had **no animation source** left once the Magic set went to the Reliquary Husk, and the provisional second meter was already flagged for a merge/keep/cut decision. It restores the fairness firewall in its strictest original form — overlays touch guardian flavour, telegraph VFX and palette, never the player.
- **Specced now:** [[1 Parry Combat - Last Rite]] §"D6 amendment — the player's weapons" (section renamed; the Surge block replaced by a cut note), the elemental rebirth spine, and the open-techniques list.

## 2026-08-07 — Player starts with **one weapon**; attacks unlock by level

- **Was:** implied that both trick weapons and all 16 attacks were available.
- **Now:** the player **starts with the Rite Blade only** (Katana ↔ Halberd) and unlocks its attacks progressively as they level. **The Pyre Censer is deferred** — fully designed and its sets reserved, but acquisition timing is open. **M2's vertical slice therefore needs only the Katana and Halberd sets.**
- **⚠ Tension to resolve:** the design's stated progression is *"mastery IS the progression"* with no skill tree (D6). Level-gated attack unlocks are a different axis. Worth deciding whether "level" is the right gate or whether unlocks should be diegetic **stratum** milestones, which is how the ≤2 techniques already work. *(⚠ 2026-08-11: "rebirth milestones" is stale — there are no rebirths, only strata. The tension itself is still open.)*
- **Specced now:** [[1h Last Rite - Player Moveset & Animation Plan]] §1 + §4 + §6; [[1 Parry Combat - Last Rite]] D6 amendment.

## 2026-08-07 — **CinderScale reclassified as prototype-only**

- **Was:** the locked Agni "First Guardian," the M2 vertical-slice boss and chain teacher, marked `built:true`.
- **Now:** **a test enemy built for combat prototyping, not shipped content.** The model may be reused later; its weapon set is unassigned and **Sword and Shield returns to the pool**. The tutorial-teacher role passes to the Bhu family, whose Garland Husk was already designed as the rhythm baseline.
- **Specced now:** [[1g Last Rite - Enemy Roster]] (card identity), [[1h Last Rite - Player Moveset & Animation Plan]] §5 (row parked).

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
