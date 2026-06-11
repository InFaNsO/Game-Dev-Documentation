# Game 1 "Last Rite" — Art Bible & Production Pipeline

> **The art + production blueprint for Game 1 — and the PORTFOLIO art lock (2026-06-10).** Companion to [[1 Parry Combat - Last Rite]] (the design; D1–D8) and [[1a Last Rite - Code Architecture]] (the engineering seams). **Style ratified, not invented here** — this doc executes the Topic 2 art direction ([[11 Factions and Species]] §1) and finalizes it for the actual toolchain (Meshy AI for meshes/textures/LOD/rigs · NVIDIA Kimodo for animation). Because the one-rig + corruption-shader pipeline flows through all 10 games ([[00 Game Concepts Hub]]), **this is the art bible the whole portfolio inherits** — Game 1 is just where it's proven first.

**Tooling:** Meshy AI (image→3D meshes, PBR textures, auto-LOD, auto-rig) · NVIDIA Kimodo (text+constraint humanoid motion diffusion) · Unity 6 URP + Humanoid avatar retargeting · one shared toon/ramp shader + one parameterized corruption shader + one grading LUT-per-rebirth.

---

## 0. The lock (read this first)

**Style = "Charm-forward dead world": Duckov/Universim stylized simplicity, rendered inside a desaturated, graded, cosmic-horror palette.** Cute-proportioned beings, corrupted and glowing, laid to rest one by one in a dead ruin. The contrast — *corrupted charm* — IS the identity (Hollow Knight's lesson: charm + dread unsettles harder than gore, and sells at $5–10 where cohesion beats ambition).

This is the certain answer, not one option of several, because **every reason points the same way**:

1. **The style is a combat-readability feature.** Last Rite lives or dies on reads — parryable vs perilous telegraphs, colour-coded tells, a ~130ms Perfect window inside a ~330ms Block window ([[1 Parry Combat - Last Rite]] D8). Stylized simplicity is the highest signal-to-noise style there is: no surface detail competes with the telegraph language. Realism would be visual noise fighting the one thing the game is about.
2. **It matches the grain of the AI tools.** Meshy is strongest on chunky, rounded, stylized forms and weakest on realistic anatomy, hands, and faces. **The style has no fingers, no facial rig, no anatomy — it designs away exactly what AI 3D generation fails at.** Stylization hides AI artifacts; realism amplifies them. Simple forms also decimate cleanly, so Meshy's auto-LOD output stays usable.
3. **The portfolio depends on it.** One rig + three meshes + corruption shader is the cost model that makes "art is near-free" true ([[1 Parry Combat - Last Rite]] Scope) and lets Game 1's guardians become the Looter Shooter's named-Husk mini-bosses with no re-art. Lock the style once, now, at portfolio altitude.
4. **It fits the price point.** "$5–10, thin story, charm-forward read" ([[00 Game Concepts Hub]]) is exactly what this style serves. Players forgive simple art that's cohesive; they punish ambitious art that's inconsistent.

---

## 1. The style, specified (the referee — every Meshy prompt and shader call answers to this)

| Rule | Spec | The test |
|---|---|---|
| **Proportions** | Oversized head (~1 : 3.5 head-to-body), small rounded body, mitten hands, stub limbs. | Does it read as a charming chibi silhouette? |
| **Forms** | Rounded, chunky, silhouette-first. No surface greebling. | If it reads as a flat black shape, **approve**. If it needs texture detail to be identified, **reject**. |
| **Faces/hands** | Eyes = texture. Mouth = one blendshape max. No fingers. No facial micro-rig. | (Designed away — see §2.) |
| **Surfaces** | Flat, hand-painted-look, low detail density. **One shared toon/ramp shader on every asset in the game.** | This shader is the great unifier — it's what makes assets from different AI generations read as one game. |
| **Light & value** | Dark, desaturated environments; characters held brighter with rim light. | The ruin is the shadow; everything alive (or pretending to be) glows against it. |
| **Colour** | Governed, not decorative. Saturation is a reserved gameplay channel (§3). | Does this colour carry meaning, or is it noise in the signal? |

**Build rule that makes the whole thing work:** no baked lighting colour in textures. All mood comes from real-time lights + a grading LUT. This is non-negotiable — §4's free re-skins depend on it.

---

## 2. The one amendment to the locked design — rig spec

⚠ **AMENDMENT to [[11 Factions and Species]] §1 Principle 1 (rig only — everything else stands).** Topic 2 locked "~8–10 bones" because the cost being minimized was **hand-animating per bone**. The toolchain inverts that economics:

- **Kimodo outputs full standard-humanoid skeleton motion.** A 9-bone custom rig means writing a custom retargeter and discarding most of the generated data.
- **Meshy auto-rig outputs a standard humanoid skeleton.**
- **Unity Humanoid retargets any humanoid clip onto any humanoid rig for free** — including across the three species meshes.

**→ The rig becomes one standard Unity-Humanoid-compatible skeleton (~20–25 bones), with finger and facial bones present-but-unused.** The discipline is untouched: still exactly **one rig**, three meshes, no facial rigging in practice, archetypes via accessories, tail/frill stay **procedural physics chains** (Kimodo won't drive them and shouldn't). Only the bone *count* changes — the cost it was guarding against no longer exists. This is the single edit the toolchain forces; the asset-budget summary updates to match.

---

## 3. Colour governance — the core leverage ("in a dead world, saturation = information")

The desaturated palette isn't just mood; it's a free information channel. Because the world is grey, **any saturated colour reads instantly as meaning.** Reserve the channel:

| Channel | Colour (reference, tune in gray-box) | Meaning — never overloaded |
|---|---|---|
| Environment | Desaturated earth / stone neutrals | No gameplay meaning, ever. The canvas. |
| **Jal corruption** | Saturated blue glow | Amphibian-Husk identity + their attack VFX ([[11 Factions and Species]] §2) |
| **Agni corruption** | Saturated orange-red glow | Reptile-Husk identity + their attack VFX |
| **Perilous telegraph** | One reserved hot colour (e.g. magenta/red flash) + icon | "**DODGE** — unparryable" (Sekiro's perilous tell, [[1 Parry Combat - Last Rite]] Combat model) |
| **Parryable telegraph** | White / gold flash | "Parry window incoming" |
| **Ranged telegraph** | A distinct accent | "Perfect-parry to deflect it back" |
| **Purge / purification** | One reserved cool colour (e.g. pale teal / white-violet) | The mercy mechanic — purge meter, finisher, player VFX. "Take my pain." |

The rule that protects it: **a gameplay colour never appears as decoration.** If teal means purification, nothing in the set dressing is teal.

---

## 4. How the style powers the locked design (style → system, not just look)

1. **Palette shift = the sanity/difficulty HUD ([[1 Parry Combat - Last Rite]] D7, the rebirth loop).** Flat-shaded, ungraded-in-texture surfaces re-grade beautifully — **one LUT swap re-skins the entire ruin per rebirth, for free.** The world's hue tells the player how deep in the curse (and how hard) they are at a glance. This is *only* possible because of the §1 "no baked colour" build rule.
2. **Corruption shader = difficulty signalling.** The shader's intensity parameter scales with rebirth depth — guardians visibly more crystallized/cracked each cycle ([[1 Parry Combat - Last Rite]]: "guardians grow more corrupted"). That escalation is **one float**, not new art.
3. **Silhouette-first = telegraph fairness.** Chibi proportions make exaggerated anticipation (huge wind-ups, squash-and-stretch) read as natural, not goofy. **Telegraphs read from posture before VFX even fire** — and the smart action-cam's push-to-OTS ([[1 Parry Combat - Last Rite]] D4) stays legible at close range because there's no detail clutter to parse.
4. **Rim-lit characters vs dark ruin** = the duel is always readable AND the dead-world tone comes free from the same lighting decision. One choice, two payoffs.

---

## 5. Production pipeline (concrete)

### 5.1 The asset list is small (this is why the plan works)
3 base meshes (Human / Amphibian / Reptile) + ~3 bolt-on accessories + ~10–15 props + a **modular ruin kit** (~25–30 pieces across the 3 strata: upper ruin → flooded depths → sealed core) + palettes/LUTs. A duel game shows **one guardian at a time** — so LOD effort goes to the environment kit, not characters.

### 5.2 Characters: 2D style sheet FIRST, then Meshy **image→3D** (never text→3D)
1. Lock a single **2D concept sheet** — the three species + the purifier, same angle, same style — and approve it.
2. Feed those approved images to **Meshy image→3D**. Image-to-3D is how cross-asset consistency is locked; text-to-3D drifts per generation.
3. Keep **one fixed style-descriptor block** reused verbatim in every prompt (proportions, flat-shaded, rounded, no-detail). The style sheet + the descriptor block are the two consistency anchors.

### 5.3 The unifying post-pass (mandatory — treat AI output as INPUT)
Every asset passes through: **shared toon shader → corruption overlay (where applicable) → grading LUT.** **Never ship a raw Meshy texture** — desaturate and regrade it into the governed §3 palette first. This pass, not the generator, is what makes the game look like one game.

### 5.4 Animation: Kimodo generates motion, **you author timing**
Workflow: **Kimodo (text prompt + keyframe/path constraints) → standard-skeleton FBX → Unity Humanoid retarget → hand-edit the telegraph timing.**

> **The load-bearing rule: the D8 frame windows are gameplay DATA, not animation data.** Drive the parry/block/dodge windows from ScriptableObjects ([[1a Last Rite - Code Architecture]]: attack-as-data, sim = source of truth) with animation events aligned to them; then stretch/hold Kimodo's anticipation poses to land on those exact frame counts. AI buys motion *quality* (the real budget per [[1 Parry Combat - Last Rite]]'s cost-truth: "moveset animation + telegraph design"); the *design* is the timing you impose. The animation mirrors the sim — it never feeds back into it.

### 5.5 What AI does NOT touch (the budget the tools just freed flows HERE)
Telegraph VFX · the corruption shader · purge/purification VFX · UI · grading LUTs. These are the gameplay *language* and the bespoke identity — hand-authored, always.

---

## 6. Validation order — the first art slice (before mass-generating anything)

Build **one corner** end-to-end and judge it inside the gray-box that [[1 Parry Combat - Last Rite]] already ranks #1 (the smart-action-cam reactive loop):

> **1 guardian** (Meshy mesh + auto-rig) **+ 1 Kimodo attack** with hand-tuned telegraph timing **+ the corruption shader + one LUT swap**, played inside the smart-action-cam loop.

- If that slice **feels right**, the style is proven end-to-end → scale to the full roster.
- If the **chibi retarget fights you** (see §7), you've spent days, not months, finding out.

This is the art-equivalent of the design's "gray-box the riskiest thing first" — never mass-produce on an unproven pipeline.

---

## 7. Risks & deferred

- **⚠ #1 risk — Kimodo retarget onto chibi proportions.** Kimodo trains on realistic human proportions; retargeting onto big-head / stub-limb characters causes **foot-skating and self-intersection.** Mitigation is standard (Unity Humanoid foot-IK correction; a slight leg-length compromise on the mesh) — but it belongs in the §6 first art slice, **not month six.** This is the one thing to validate before committing to the pipeline.
- **Deferred to the gray-box (per portfolio discipline):** exact reference colours/hex per §3 channel; the precise LUT-per-rebirth ramp; final proportion ratio (feel-dependent — tune against telegraph readability + retarget quality together); the ruin-kit piece count.
- **Tooling caveat:** Kimodo is open-source and runs locally (GPU-bound). Confirm the local install + Unity import round-trip works in the first slice before scaling — a broken FBX round-trip silently breaks the §5.4 rule.

---

## 8. Canon ripple / portfolio note

This style is the **portfolio art lock**, proven here first. It propagates as-is to every later game ([[00 Game Concepts Hub]]): the Looter Shooter inherits Game 1's guardians as named-Husk mini-bosses with zero re-art; the corruption shader, Mani VFX colour language, one-rig pipeline, and AI-asset post-pass carry forward additively. **⚠ Note the 4th species ripple already logged** (Avian/Vayu, added in the City-Builder pass) — when the shared mesh set grows to 4, this bible's one-rig discipline absorbs it the same way (shared skeleton, new mesh, accessories — no new rig).
