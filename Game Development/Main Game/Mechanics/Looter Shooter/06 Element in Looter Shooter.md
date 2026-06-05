
### Mani Brief

**Mani** (Sanskrit मणि, "jewel") is the rare cosmic substance at the heart of the game's lore and combat. Found as raw ore — unpredictable and dangerous — it can be refined by [[14 Naming Glossary#The Substance|imposing structure on its chaotic potential]] to produce **five elemental forms**: Bhu (Earth), Jal (Water), Agni (Fire), Vayu (Air), and Akash (Ether / Space).

The formal name for the substance is **Panchamani** — "the Five-Jewel," the substance that contains all five elements. *Mani* is the common name in everyday speech and all gameplay UI. *Panchamani* appears in audio logs, Accord-era texts, and the optional lore codex.

> Full naming reference: [[14 Naming Glossary]]. Cosmic-horror lore underlying Mani: [[12 The Akashic and The Bleed]].

### History (the surface story)

Long ago, the planet hosted the **Mani Accord** — a pragmatic federation of three species (Humans, Amphibians, Reptiles) who built the three [[05 Mega Structures - Game Worlds|Mega Structures]] to refine Mani into its elemental forms. They were destroyed in **The Breach** — what the world now believes was a catastrophic accident during the final refinement of Akash-Mani. The substance has been lost knowledge ever since.

### History (the truth, revealed mid-game)

The Accord secretly used refined Akash-Mani as a **doorway to Akash** (the source dimension). The doorway opened. Something on the other side pushed back. The doorway collapsed inward, the Akash-Mani crystal destroyed in the same instant. Interdimensional radiation — **The Akashic** (formal) / **The Bleed** (common) — leaked through for the few seconds the wound was open. Then The Breach sealed itself.

**The original Akash-Mani is destroyed. No Akash-Mani exists in the present world. The conspiracy died with the inner circle — until the first named-NPC purification.**

> See [[12 The Akashic and The Bleed]] for full detail.

### Mani's Role in the Present World

The current Grinder civilization remembers Mani as folk-memory — *"the ancients made things from it"* — but no one alive can refine it. Raw Mani ore is sometimes scavenged from old Accord sites and used as crude grenades with **unpredictable, often self-harming effects** (the chaotic potential rolled live, unfiltered). Grinders frequently injure themselves with their own Mani throws — the player can bait this in combat.

**The player is the last carrier of refinement knowledge.** Teaching the Grinders to make the first Bhu-Mani is the watershed moment of the trust arc ([[15 Grinder Trust Arc]]).

### Raw vs Refined — the central mechanic

| Form | Behavior | Risk |
|---|---|---|
| **Raw Mani** | Unstructured. Random rolls from the **Mani Effect Table** on detonation. Live-timer grenade mechanic (auto-detonates after ~5–8 seconds). | High. Can self-harm. Grinders use this exclusively because they cannot refine. |
| **Refined elemental Mani** (Bhu / Jal / Agni / Vayu) | Structured. Predictable single-element effects. **Power scales with crystal size** — small for safe use, large for ult-tier effects but more dangerous to handle. | Controlled per use; rises with size. |
| **Refined Akash-Mani** | Theoretically the fifth refined form. The Accord made one. It destroyed them. **Never refined in this campaign.** | Catastrophic. The Accord proved it. |

### Mani Effect Table (raw rolls)

- **Common (60%):** Burn / Freeze / Push / Stagger
- **Uncommon (30%):** Honey (root) / Vamp / Ink / Suppressed
- **Rare (10%):** Phase (teleport caster) / Summon Mani-construct (temporary hostile) / Anti-grav lift / Create cover

### Refined Mani Mapping to Mega Structures

| Refined form | Element | Refining structure | Knowledge source |
|---|---|---|---|
| **Bhu-Mani** | Earth | Lithic Mow | Player wakes knowing it (cryo-memory). Teaches Grinders. |
| **Jal-Mani** | Water | Genesis Vats | Vats Named Husk 1 (mandatory mini-boss) |
| **Vayu-Mani** | Air | Genesis Vats | Vats Named Husk 2 (mandatory mini-boss) |
| **Agni-Mani** | Fire | Prism Forge | Forge Named Husk 1 (mandatory mini-boss) |
| **Akash-Mani** | Ether / Space | Player base (produced at endgame, multi-species path only) | Head of Accord Research (campaign final boss, beneath the Vats — mandatory) |

### Combat — The Three Weapon Lanes (Phase 1 lock, 2026-06-02)

**There are NO ballistic firearms anywhere in the world.** A civilization whose entire technology base was elemental Mani never developed gunpowder; combat is Mani-driven end to end. This keeps Mani load-bearing in combat (not a decorative spice on top of bullets) and fits the Grinders as primitive scavengers. All ranged attacks are **travel-based projectiles, never hitscan** — which gives the grid's spacing/positioning real teeth.

The player's kit is three lanes, ordered as an **inverse cost ↔ range ↔ risk curve** (cheap = close & risky; expensive = far & safe). Exact numbers are deferred (tuned in Phase 3/4); the *hierarchy* is locked:

| Lane | Cost | Role | Range (nature) | Notes |
|---|---|---|---|---|
| **Enchanted melee** (knives) | **Free / cheap** | Every-turn default | Adjacent (1 tile) | The workhorse. You *want* to close to it — but it puts you in danger. |
| **Channeled refined-Mani magic** | **Mid** | Mid-layer reach | Short / medium | Innate abilities (player woke knowing Bhu). The positioning layer. |
| **Mani projectile launchers** | **High / limited-use** | Ultimate-tier burst | Long / AoE | Looted devices firing Mani-charged projectiles. The rare, safe big hit / board-clear. The "loot" lane (mod slots: focus crystal / chamber / charge coil). |

**Player posture = close-range-primary** (lean on cheap melee, reach with spells, save launchers for big moments). NOTE: this *inverts* the legacy "gunslinger manages the gap" framing — the **player** is now the melee-forward actor and **enemies apply ranged pressure**. Legacy docs pending Phase 5 reconciliation.

**Enemies differ from the player** (they do not mirror the kit): Grinders = crude Mani-shard launchers + melee Driller + chaotic raw-Mani Shaman; Husks (main game) = melee / elemental.

> Cost/economy exact values, action economy, and per-lane range numbers are **deferred** — locked here is only the lane structure, no-guns rule, and the cost/range/risk ordering.

### Combat — Arena & Camera (Phase 2, 2026-06-02)

> Spatial frame for the Encounter Arena. **Rules are locked; the numbers are reference targets, finalized in gray-box/playtest.** (To be extracted into a dedicated combat-spec doc in the Phase 5 doc-sync.)

**Arena rules:**
- **Compact single-screen grid, NO camera panning** — the whole arena must be readable in one fixed shot.
- Odd-sized grid (true center tile for symmetric layouts + central hazard/objective).
- *Reference target:* **7×7** (49 positions), tile ≈ 2m. Chosen compact because position-count complexity scales worse than N² (AI, authoring, readability). Bump toward 8×8 only if playtest shows the three range bands (melee/spell/launcher) feel mushy — size and range-band distinction get validated together.
- Consequence at 7×7: the launcher's **long-range** band is compressed (max straight distance ~6–7 tiles), so its identity rests on **AoE + high cost**, not sniper reach. Intended.

**Camera rules (the dual-camera is the #1 identity bet → must-playtest):**
- **One unified PERSPECTIVE rig** — NOT orthographic-isometric, NOT 90° top-down. (Keep Into the Breach's *readable-tile aesthetic*, but achieve it via a steep perspective cam so the tactical ↔ OTS blend stays smooth. Orthographic would fight the perspective OTS cam.)
- **Tactical angle = OTS reaction-cam angle + 15–25°.** The blend swing is the design constraint — a small, natural movement by construction, so the eye barely readjusts on snap-back.
- **Derived floor:** tactical view stays plannable on the grid (**≥ ~45°**), which sets the OTS cam to a **slightly elevated** over-the-shoulder (**≥ ~25–30°**) — not a ground-level cinematic parry cam. It still shows the attacker's wind-up but keeps the world coherent with the planning view.
- *Reference targets:* OTS ~25–35°, tactical ~45–55°. Direction reference = South Park: The Fractured But Whole (perspective grid tactics).

### Combat — Turn Structure, Action Economy & Resources (Phase 3, 2026-06-02)

> Rules locked; numbers are reference targets, finalized in gray-box/playtest.

**Turn structure (3.0):**
- **Side-based phases:** full **Player Phase** → full **Enemy Phase**, repeat. NOT individually-interleaved initiative. Enemies ordered by agility within the Enemy Phase.
- Rationale: concentrates all enemy attacks into one window = exactly when the OTS reaction-cam fires. Cleanly separates *planning* (tactical cam) from *reacting* (reaction cam), so the camera structure falls out of the turn structure.
- **Core loop:** Player Phase (plan / move / attack in tactical cam) → Enemy Phase (each incoming attack = OTS reaction-cam + parry window) → repeat.

**Action economy (3.3) — AP pool (Option B):**
- One **AP pool** with a cap (ref ~6). **NO carryover** — AP refreshes to a small base each Player Phase. (Carryover dropped: the launcher is fed by Veins, not AP-banking, so carryover had no job left and only weakened parry motivation.)
- **AP sources:** small **base regen** (ref +2 — the floor, so you can always at least melee, no death-spiral) + **parry generation** (perfect parry = +big AP, block = +small, miss = 0). **Parry is the only way to exceed base each turn → the skill engine.**
- **Costs (ref):** melee 1 AP · spell 2–3 AP · launcher = Charge + a chunk of AP (so even when stocked it competes within the turn, never spam).
- **Movement is a SEPARATE budget** (not AP) — protects the close-range-primary identity.

**Parry vs Dodge — the greed split (makes the reaction-cam a decision):**
- **Dodge** = wider/safer window, avoids damage, **generates NO AP.**
- **Parry** = tight window, **risk of the full hit on a miss**, but success = avoid damage **+ AP + counter.**
- Every incoming attack: play safe (dodge) or risk the parry to fuel your next turn.

**Three-resource model (each a distinct job + timescope):**
| Resource | Job | Timescope | Source |
|---|---|---|---|
| **AP** | per-turn action currency (melee + base spells) | this turn only (no carryover) | base regen + **parry** |
| **Mani Shard** | upgrade a spell / cast an off-element "advanced" spell | **PLAYTEST — see below** | found scattered in the arena |
| **Mani Vein → Launcher Charges** | fuel a launcher burst (ult-tier) | **persistent**, carried between battles (1 vein ≈ 3–4 rounds) | harvested from veins |

**OPEN — Mani Shard scope (prototype BOTH, decide by playtest):**
- (a) **Battle-scoped, use-it-or-lose-it** — abundant, a shard drops *every battle*; scarcity is positional (reach it under fire) + temporal (this fight only). Preserves the clean three-timescope split (spatial axis).
- (b) **Persistent, limited** (like Charges) — carried between battles, **rarer drop similar to veins.** Collapses into the "carried consumable" axis with Charges.
- **Dev note:** drop-rate MUST differ by mode — (a) drops every battle, (b) is vein-rare. Build both, A/B in playtest.

**Ranges & movement (3.1 / 3.2) — relationships locked, numbers reference:**
- **Melee = 1** (adjacent). **Spell ≈ movement budget** — the spacing engine: closing from spell-range to melee eats your whole move, forcing the "poke safe vs. commit and get exposed" choice every turn. **Launcher = board-spanning + AoE** (rare; at 7×7 its identity is AoE + cost, not reach).
- **Movement = separate budget, splittable** around the action (move → attack → move) for kiting / flanking reposition — **EXCEPT the launcher is heavy and roots you** (no move after firing; revives the good part of the old "commit-to-the-shot" rule for one lane only).
- *Reference (7×7):* movement ~3–4 · spell ~3–4 · launcher ~6–7 + AoE radius ~1.

**Naming reconciliation (for Phase 5 doc-sync):** locked docs currently use "shard" for the raw-Mani grenade and say "veins drop shards." New clean split → **Raw Mani** = chaotic live-timer grenade (Effect Table, Grinders' tool, unchanged) · **Mani Shard** = refined spell fuel (new role) · **Mani Vein** = deposit → Launcher Charges (new role).

### Combat — Damage Model (Phase 4.1, 2026-06-02)

> Rules locked; numbers are reference targets. **NO armor in the prototype** (revisit after a playable build).

**Deterministic philosophy:**
- **Flat base damage, NO ±variance** — exact-lethality planning (a 30-dmg hit on a 28-HP enemy kills, *guaranteed*). Variance would quietly steal that back.
- **Every multiplier is earned by a decision** (position / timing / elemental setup), never rolled.
- **RNG is confined to exactly two places:** the raw-Mani Effect Table, and the enchant status-proc chance. Nothing else rolls.

**Facing & flanking (the sole positional damage lever — with no armor, this carries the entire "positioning = damage" identity):**
- Every unit has a **facing direction**; it faces the way it **last moved/attacked**.
- **Re-facing in place** (turning without moving) **costs movement** (ref: 1 tile of the move budget). *PLAYTEST-review.* This is what keeps flanking meaningful — neither side can spin to face a threat for free.
- **Arc** = attacker's position vs. target's facing → **Front / Side / Rear**.
- Multipliers (ref): **Front ×1.0 · Side ×1.25 · Rear → triggers Crit.**

**Crit — earned, not rolled (ref ×2):** one crit channel, three deterministic triggers:
- **Rear-flank** (positioned behind the target),
- **Parry-counter** (won the reaction-cam parry — ties the Phase 3 parry economy straight into damage),
- **Status-combo** (see below).

**Status — apply, then cash in (this is the status→crit wiring):**
- **Apply (step 1):** either **GUARANTEED** (spells/abilities designed to apply a status — plannable) or a **PROC** (enchanted weapons have a per-hit *chance* to also apply their element's status — the sanctioned RNG, a bonus you don't rely on).
- **Combo crit (step 2):** a **status-afflicted enemy + the right follow-up hit = guaranteed crit.** Flagship example: **Frozen → heavy hit → Shatter (×2)**.
- Effect: the status deck is wired **into** the crit channel, not left floating — Burn/Freeze/Push are crit *setups*. Works in the Bhu-only prototype too (Freeze via the raw-Mani Effect Table → Shatter via a Bhu melee hit).

**Body zones: CUT for the prototype.** Flank arcs carry all positional damage depth; a deterministic head-shot multiplier would just mean "always aim head." "Cripple a limb"-type effects, if ever wanted, arrive later as a specific status/ability (Phase 4.4), not a per-attack aim-menu.

**No armor (prototype decision):** removed for now. Rationale — Expedition 33 precedent (our reaction-defense reference puts mitigation in *active* parry/dodge, with armor as only a thin stat), solo-dev scope, and avoiding a *passive* defense layer redundant with active parry (the same reason cover was cut). Revisit once there's a playable prototype. Knock-on: Bhu's enchant drops "armor break."

**Reference damage pipeline:** `Final = Base × FlankMult × CritMult` (no armor term).

### Refined Mani — Combat Role (enchanted weapons)

Each refined elemental Mani can be consumed to **enchant weapons** with elemental attacks. Enchantments apply to:

- **Enchanted projectiles** (Mani-charged launcher ammo) — per-shot proc chance of the corresponding status effect on hit
- **Enchanted knives / melee weapons** — per-hit proc chance, plus passive elemental aura when wielded

| Mani | Combat flavor | Status proc |
|---|---|---|
| **Bhu-Mani enchantment** | Earth / physical | Stagger · Knockback (was "armor break" — dropped, no armor in prototype) |
| **Jal-Mani enchantment** | Water | Freeze · Slow |
| **Vayu-Mani enchantment** | Air | Push · Knockdown · Displacement |
| **Agni-Mani enchantment** | Fire | Burn · Melt armor |

Players progressively unlock more elemental options as they reclaim Mega Structures and meet named Husks. The four elemental enchantments cover the entire status-deck combat layer.

**Akash-Mani is not used for weapon enchantment** — its only manifestation in the campaign is the endgame production sequence (multi-species path).
