
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

### Refined Mani — Combat Role (enchanted weapons)

Each refined elemental Mani can be consumed to **enchant weapons** with elemental attacks. Enchantments apply to:

- **Enchanted projectiles** (Mani-charged launcher ammo) — per-shot proc chance of the corresponding status effect on hit
- **Enchanted knives / melee weapons** — per-hit proc chance, plus passive elemental aura when wielded

| Mani | Combat flavor | Status proc |
|---|---|---|
| **Bhu-Mani enchantment** | Earth / physical | Stagger · Armor break · Knockback |
| **Jal-Mani enchantment** | Water | Freeze · Slow |
| **Vayu-Mani enchantment** | Air | Push · Knockdown · Displacement |
| **Agni-Mani enchantment** | Fire | Burn · Melt armor |

Players progressively unlock more elemental options as they reclaim Mega Structures and meet named Husks. The four elemental enchantments cover the entire status-deck combat layer.

**Akash-Mani is not used for weapon enchantment** — its only manifestation in the campaign is the endgame production sequence (multi-species path).
