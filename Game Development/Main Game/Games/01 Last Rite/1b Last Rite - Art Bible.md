# Game 1 "Last Rite" — Art Bible & Production Pipeline

> **The art + production blueprint for Game 1 — and the PORTFOLIO art lock (2026-06-10).** Companion to [[1 Parry Combat - Last Rite]] (the design; D1–D8) and [[1a Last Rite - Code Architecture]] (the engineering seams). **Style ratified, not invented here** — this doc executes the Topic 2 art direction ([[11 Factions and Species]] §1) and finalizes it for the actual toolchain (Meshy AI for meshes/textures/LOD/rigs · NVIDIA Kimodo for animation). Because the one-rig + corruption-shader pipeline flows through all 10 games ([[00 Game Concepts Hub]]), **this is the art bible the whole portfolio inherits** — Game 1 is just where it's proven first.

**Tooling:** Meshy AI (image→3D meshes, PBR textures, auto-LOD, auto-rig) · NVIDIA Kimodo (text+constraint humanoid motion diffusion) · Unity 6 URP + Humanoid avatar retargeting · one shared toon/ramp shader + one parameterized corruption shader + one grading LUT-per-stratum.

> **⚠ Environment amendment (2026-08-12):** the environment direction is now locked in its own doc — **[[1l Last Rite - World & Environment Bible]]** (the complex, the four wings, the threshold grammar, the per-wing palettes). It amends **three** things here: **§3** (the world is colourful, not grey — value replaces saturation as the separator, and two new décor channels are added), **§5.1** (the kit is eight spaces, not five — and "Agni sealed core" was wrong), and a new **§10** (the environment must never deform its mesh). The §1 style lock, the §2 rig amendment and the §9 ablation law are **untouched**.

> **⚠ Animation-sourcing amendment (2026-07-31):** **Kimodo is dropped for Last Rite combat animation** — output quality judged unacceptable by the developer. Combat clips now come from a purchased pack (**Mega Animation Pack v1.8**, Unity Asset Store) retargeted via Unity Humanoid — see the §5.4 status note + [[1h Last Rite - Player Moveset & Animation Plan]] (the full clip plan) + [[1f Last Rite - Combat Iteration Log]]. Everything else in this bible — Meshy meshes/rigs, the style lock, colour governance, the §2 rig amendment — stands unchanged.

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
| **Light & value** | Dark environments; characters held brighter with rim light. **Environment is LIT, characters are UNLIT** — see the shading split below. | The ruin is the shadow; everything alive (or pretending to be) glows against it. |
| **Colour** | Governed, not decorative. Saturation is a reserved gameplay channel (§3). | Does this colour carry meaning, or is it noise in the signal? |

**Build rule that makes the whole thing work:** no baked lighting colour in textures. All mood comes from real-time lights + a grading LUT. This is non-negotiable — §4's free re-skins depend on it.

### 1.1 The shading split — CHARACTERS UNLIT, ENVIRONMENT LIT *(added 2026-08-12)*

> **⚠ AMENDS the 2026-08-08 direction "ALL `BGamer/AnimeToonLit` materials run UNLIT."** That call was made when the project contained only characters and props — **it was a character decision worded as a global one**, and applying it to architecture is wrong. Corrected:

| Population | `_Unlit` | Why |
|---|---|---|
| **Characters, weapons, character props** | **1** (keyword `_UNLIT` on) | The locked player look — flat drawn fills + smooth black hull outline, texture carries all form, zero grading. Reads as *drawn*, and stays identical wherever the character stands. |
| **Environment / architecture / dressing** | **0** (lit path) | Large architecture rendered unlit is flat cardboard — a 130 m shikhara in flat colour has no readable form. Lighting is what makes carving read as carved. |

**No new shader is needed.** `AnimeToonLit.shader` already declares `[Toggle(_UNLIT)] _Unlit` as a `shader_feature_local` **defaulting to 0**, with the lit path fully implemented (`_MAIN_LIGHT_SHADOWS`, cascades, soft shadows, fog). The split is a per-material checkbox on one shader — so §1's *"one shared toon shader on every asset"* unifier rule is untouched.

**Three reasons this is the right way round, not a compromise:**

1. **It restores this bible to internal consistency.** The build rule directly above — *"no baked lighting colour in textures; all mood comes from real-time lights + a grading LUT"* — is **meaningless on unlit geometry.** The per-stratum LUT re-skin in §4 depends on the environment being lit. Unlit architecture would have quietly voided both.
2. **It strengthens the colour law (§3).** Décor value can now be controlled with *lighting* rather than baked into albedo, which makes the décor-dark / gameplay-bright separation far easier to hold — and that separation is what carries combat readability now that the world is colourful (§3 amendment).
3. **Characters pop.** Unlit characters against lit architecture is exactly *"characters held brighter with rim light"* — the split delivers §1's own requirement for free, and is the standard treatment in the stylized-fighter lineage this style descends from.

**Consequence for vertex AO (environment only):** with real lights doing directional form, baked **vertex occlusion drops from load-bearing to supplementary** — still worth baking, because directional light cannot produce contact darkening in creases and inside corners. The **grime and wear** vertex channels are unaffected; they were never about lighting. Full pipeline: [[1l Last Rite - World & Environment Bible]] §4.5.

---

## 2. The one amendment to the locked design — rig spec

⚠ **AMENDMENT to [[11 Factions and Species]] §1 Principle 1 (rig only — everything else stands).** Topic 2 locked "~8–10 bones" because the cost being minimized was **hand-animating per bone**. The toolchain inverts that economics:

- **Kimodo outputs full standard-humanoid skeleton motion.** A 9-bone custom rig means writing a custom retargeter and discarding most of the generated data.
- **Meshy auto-rig outputs a standard humanoid skeleton.**
- **Unity Humanoid retargets any humanoid clip onto any humanoid rig for free** — including across the three species meshes.

**→ The rig becomes one standard Unity-Humanoid-compatible skeleton (~20–25 bones), with finger and facial bones present-but-unused.** The discipline is untouched: still exactly **one rig**, three meshes, no facial rigging in practice, archetypes via accessories, tail/frill stay **procedural physics chains** (Kimodo won't drive them and shouldn't). Only the bone *count* changes — the cost it was guarding against no longer exists. This is the single edit the toolchain forces; the asset-budget summary updates to match.

---

## 3. Colour governance — the core leverage ("in a dead world, **value** = information")

> **⚠ AMENDED 2026-08-12 — the environment is no longer grey.** This section read *"in a dead world, saturation = information"* and *"because the world is grey, any saturated colour reads instantly as meaning,"* with Environment specified as *"desaturated earth / stone neutrals."* The environment direction lock ([[1l Last Rite - World & Environment Bible]] §2.5) replaces that with **"the dead world is colourful"** — a warm faded mineral palette (sandstone gold, dusty rose, ochre, verdigris copper-green, basalt, terracotta), where colour comes from **material and light** rather than from saturation.
>
> **The information channel survives, because it was never really carried by saturation alone.** It is carried by **value + motion**:
>
> | | Décor | Gameplay |
> |---|---|---|
> | **Value** | dark | bright |
> | **Saturation** | low, matte | high |
> | **Behaviour** | static | moving, momentary, on an enemy |
>
> Everything below still holds — a gameplay colour still never appears as decoration. What changed is that the *canvas* is now colourful, so **value separation does the work saturation used to do alone.** Full per-wing palettes, vein colours and collision guardrails: [[1l Last Rite - World & Environment Bible]] §2.5.

The palette isn't just mood; it's a free information channel. Reserve the channel:

| Channel | Colour (reference, tune in gray-box) | Meaning — never overloaded |
|---|---|---|
| Environment | **Warm faded mineral palette — colourful but dead.** Dark, matte, static. *(⚠ was "desaturated earth / stone neutrals")* | No gameplay meaning, ever. The canvas. |
| **Raw-mani veins** *(NEW 2026-08-12)* | The wing's own element colour — **dark, low-saturation, matte** (Bhu emerald · Jal indigo · Vayu **pale mint-green** · Agni antique bronze) | "Unrefined element-mani runs through this structure." Décor, not gameplay. |
| **Threshold crystal** *(NEW 2026-08-12)* | The wing's element colour at **maximum brightness and saturation** — the brightest thing in the wing | "You can enter here." One per wing. The threshold grammar, [[1l Last Rite - World & Environment Bible]] §2.4. |
| **Jal corruption** | Saturated blue glow | Amphibian-Husk identity + their attack VFX ([[11 Factions and Species]] §2) |
| **Agni corruption** | Saturated orange-red glow | Reptile-Husk identity + their attack VFX |
| **Perilous telegraph** | One reserved hot colour (e.g. magenta/red flash) + icon | "**DODGE** — unparryable" (Sekiro's perilous tell, [[1 Parry Combat - Last Rite]] Combat model) |
| **Parryable telegraph** | White / gold flash | "Parry window incoming" |
| **Ranged telegraph** | A distinct accent | "Perfect-parry to deflect it back" |
| **Purge / purification** | One reserved cool colour (e.g. pale teal / white-violet) | The mercy mechanic — purge meter, finisher, player VFX. "Take my pain." |

The rule that protects it: **a gameplay colour never appears as decoration.** If teal means purification, nothing in the set dressing is teal.

> **⚠ THE MANI-AURA EXCEPTION — added 2026-08-12, developer decision. Canonical text lives in [[1l Last Rite - World & Environment Bible]] §2.5; this is the pointer so it is not re-litigated from here.**
>
> The rule above governs **décor — i.e. surfaces.** It does **not** govern **mani itself.** Wherever a colour is the aura of a mani gem, or of something directly powered by one, it takes that element's true hue — **including hues in the reserved table.** Mani is what the world runs on; denying it its own colour denies the world its physics.
>
> **Legalises:** Agni's live amber gems / lamp-cups / braziers and its amber threshold crystal; saturated blue on live Jal mani; the equivalent per wing.
> **Does not legalise:** orange-painted stone, orange banners, orange grime, warm mood lighting, or flame. The exception attaches to **mani**, never to **surfaces**.
> **Readability is preserved by the value + motion table above, not by hue:** décor-mani is **dim, small, static and rare**; gameplay hues are **bright, high-value, momentary, moving, on an enemy.**
> **Standing test:** *is this colour coming from mani, or from a surface?*

> **✅ 2026-08-12 — this rule just did its job.** The environment direction originally gave **every wing a teal-violet threshold crystal** as a visual through-line to the sanctum. That is décor in a reserved gameplay colour, in every single wing — the exact thing this rule forbids. **It was rejected and the crystals are now element-coloured** ([[1l Last Rite - World & Environment Bible]] §2.4). Teal / white-violet remains **100% purge-reserved and appears nowhere in the environment.** The one candidate exception under consideration — teal-violet as the *sanctum's* exclusive colour, since the sanctum is literally where purification was manufactured — is **open, not approved** ([[1l Last Rite - World & Environment Bible]] §6 item 3).
>
> ⚠ **Second live collision, still unresolved:** the Shroud's neutral amber-gold vs the parryable white/gold flash (§9).
>
> ⚠ **Third live collision, opened 2026-08-12:** Agni's threshold crystal is now **amber / fire-gold** ([[1l Last Rite - World & Environment Bible]] §2.4), which puts **three amber-golds in one room** — Shroud, parry flash, and crystal. The separation should hold on behaviour (the crystal is huge, static and architectural; the parry flash is small, momentary and on an enemy; the Shroud is on the player), but this is now the **highest-priority colour check in the gray-box pass**, and Agni is the wing to test it in. Three colour collisions on the list — resolve them together.

---

## 4. How the style powers the locked design (style → system, not just look)

1. **Palette shift = the depth/sanity HUD ([[1 Parry Combat - Last Rite]] D7 + the elemental strata spine).** Flat-shaded, ungraded-in-texture surfaces re-grade beautifully — **one LUT per stratum re-skins the whole environment, for free.** The world's hue tells the player how deep they are at a glance. This is *only* possible because of the §1 "no baked colour" build rule. **⚠ 2026-08-11: the LUT is now keyed to the stratum you're in, not to a rebirth counter** — the re-descent loop is cut (D2), so there is no cycle index to grade against. The technique and its cost are unchanged; only what drives the float changed.
2. **Corruption shader = depth signalling.** The shader's intensity parameter scales with **stratum depth** — enemies visibly more crystallized/cracked the further down you go. That escalation is **one float**, not new art. *(Was "scales with rebirth depth ... more corrupted each cycle.")*
3. **Silhouette-first = telegraph fairness.** Chibi proportions make exaggerated anticipation (huge wind-ups, squash-and-stretch) read as natural, not goofy. **Telegraphs read from posture before VFX even fire** — and the smart action-cam's push-to-OTS ([[1 Parry Combat - Last Rite]] D4) stays legible at close range because there's no detail clutter to parse.
4. **Rim-lit characters vs dark ruin** = the duel is always readable AND the dead-world tone comes free from the same lighting decision. One choice, two payoffs.

---

## 5. Production pipeline (concrete)

### 5.1 The asset list is small (this is why the plan works)
3 base meshes (Human / Amphibian / Reptile) + ~3 bolt-on accessories + ~10–15 props + a **modular facility kit** + palettes/LUTs. A duel game shows **one guardian at a time** — so LOD effort goes to the environment kit, not characters.

> **⚠ 2026-08-11 — the kit spec changed with the story lock ([[1k Last Rite - Lore Bible]] §3/§6).** It was *"~25–30 pieces across the 3 strata: upper ruin → flooded depths → sealed core."* It is now **four strata** — Bhu burial terraces → Jal flooded wards → Vayu wind-scoured heights → Agni sealed core — plus a tutorial space. **Re-price the piece count against four**, and note the architectural register: this is the Accord's **mani-medicine division**, a place that spent centuries trying to cure death — so it is a **hospital built like a temple** (consecration halls, reliquaries, burial terraces), not a laboratory. That register is what makes the funerary enemy family ([[1i Last Rite - Bhu Mani Husks]]) look native to the building instead of imported into it. **Reuse across strata is now more load-bearing, not less** — the old plan amortised one kit over ~5 re-descents; it must now amortise over four distinct-looking places on a single pass, so lean on shared structural pieces + per-stratum dressing and LUT.

> **⚠ 2026-08-12 — RE-PRICE AGAIN. The environment lock breaks the "four strata + tutorial" count** ([[1l Last Rite - World & Environment Bible]] §2.2/§3). Two errors in the line above: **Agni is not the sealed core** (it's a ground-level temple courtyard — the sealed sanctum is a *separate* space), and the four wing forms are nothing like the placeholder names. Corrected:
>
> | Was | Is |
> |---|---|
> | Bhu burial terraces | **Bhu** — rocky overgrown mountain-shrine, specimen vats (rises) |
> | Jal flooded wards | **Jal** — monumental sunken stepwell, cascade filtration (sinks) |
> | Vayu wind-scoured heights | **Vayu** — tall hollow lattice tower, breath-anatomy nadis |
> | Agni sealed core | **Agni** — carved Hindu temple courtyard, dead fire-altar + lamp-towers |
>
> **The real space count is eight, not five:** entrance · processional spine · 4 wings · **sealed sanctum** · **Chaos Descent pit** · tutorial space. The reuse discipline is therefore *more* load-bearing again — the shared structural set is **arched doorways, banded mouldings, pillar/bracket sets, stepped plinths, carved niches**, which the temple-grammar register (below) makes natural across all eight.
>
> **The architectural register also sharpens: this is a *temple with no god*** — a refinery wearing a devotional skin, where a deity's murti would sit there is a crystal instead ([[1l Last Rite - World & Environment Bible]] §2.1). That stacks with the "hospital built like a temple" register rather than replacing it: **temple is the architecture, medicine is the function, funeral is the residue.** All three feed the kit.
>
> **⚠ New art law — the environment must never deform its mesh.** See §10.

### 5.2 Characters: 2D style sheet FIRST, then Meshy **image→3D** (never text→3D)
1. Lock a single **2D concept sheet** — the three species + the purifier, same angle, same style — and approve it.
2. Feed those approved images to **Meshy image→3D**. Image-to-3D is how cross-asset consistency is locked; text-to-3D drifts per generation.
3. Keep **one fixed style-descriptor block** reused verbatim in every prompt (proportions, flat-shaded, rounded, no-detail). The style sheet + the descriptor block are the two consistency anchors.

### 5.3 The unifying post-pass (mandatory — treat AI output as INPUT)
Every asset passes through: **shared toon shader → corruption overlay (where applicable) → grading LUT.** **Never ship a raw Meshy texture** — desaturate and regrade it into the governed §3 palette first. This pass, not the generator, is what makes the game look like one game.

### 5.4 Animation: Kimodo generates motion, **you author timing**

> **⚠ SUPERSEDED for combat animation — 2026-07-31.** Kimodo is **dropped for Last Rite combat clips** (output quality judged unacceptable). The replacement pipeline:
> **Mega Animation Pack v1.8** (Unity Asset Store, publisher Alcaboce, asset id 170897, ~$70 — Humanoid rig; per-weapon folders of ~70 clips each; ships an Animator controller + avatar mask; standard Asset Store EULA — one purchase covers player + all enemies + future games)
> → **Unity Humanoid retarget** onto the chibi rigs
> → **per-clip Blender fixes** (shoulder rotation to avoid head clipping · foot-curve pinning · ~10–15% amplitude reduction)
> → **re-time in Blender NLA/Graph Editor** to the AttackDef frame windows (130ms perfect / 330ms block; keep speed change ≤15%)
> → **Mecanim Animator** with shared link poses, cross-fades (0.1–0.15s chains · 0.05–0.08s parry deflect · 0.15–0.2s heavy wind-ups) and **Animation Events on impact frames** (the clip-events timing model of [[1d Last Rite - Reaction & Feints Spec]] §7 — unchanged).
> Full tiered animation list + pack clip mapping + validation checklist: [[1h Last Rite - Player Moveset & Animation Plan]]. The workflow below is retained as the original Kimodo plan — Kimodo remains available for non-combat / other uses (Tools docs carry matching status notes).

Workflow: **Kimodo (text prompt + keyframe/path constraints) → standard-skeleton FBX → Unity Humanoid retarget → hand-edit the telegraph timing.**

> **The load-bearing rule: the D8 frame windows are gameplay DATA, not animation data.** Drive the parry/block/dodge windows from ScriptableObjects ([[1a Last Rite - Code Architecture]]: attack-as-data, sim = source of truth) with animation events aligned to them; then stretch/hold Kimodo's anticipation poses to land on those exact frame counts. AI buys motion *quality* (the real budget per [[1 Parry Combat - Last Rite]]'s cost-truth: "moveset animation + telegraph design"); the *design* is the timing you impose. The animation mirrors the sim — it never feeds back into it.

### 5.5 What AI does NOT touch (the budget the tools just freed flows HERE)
Telegraph VFX · the corruption shader · purge/purification VFX · UI · grading LUTs. These are the gameplay *language* and the bespoke identity — hand-authored, always.

---

## 6. Validation order — the first art slice (before mass-generating anything)

Build **one corner** end-to-end and judge it inside the gray-box that [[1 Parry Combat - Last Rite]] already ranks #1 (the smart-action-cam reactive loop):

> **1 guardian** (Meshy mesh + auto-rig) **+ 1 Kimodo attack** with hand-tuned telegraph timing **+ the corruption shader + one LUT swap**, played inside the smart-action-cam loop. *(2026-07-31: read "1 Kimodo attack" as "one pack attack clip" — see the §5.4 status note.)*

- If that slice **feels right**, the style is proven end-to-end → scale to the full roster.
- If the **chibi retarget fights you** (see §7), you've spent days, not months, finding out.

This is the art-equivalent of the design's "gray-box the riskiest thing first" — never mass-produce on an unproven pipeline.

---

## 7. Risks & deferred

- **⚠ #1 risk — Kimodo retarget onto chibi proportions.** Kimodo trains on realistic human proportions; retargeting onto big-head / stub-limb characters causes **foot-skating and self-intersection.** Mitigation is standard (Unity Humanoid foot-IK correction; a slight leg-length compromise on the mesh) — but it belongs in the §6 first art slice, **not month six.** This is the one thing to validate before committing to the pipeline. **⚠ 2026-07-31: moot for combat clips — Kimodo dropped (§5.4 status note). The chibi-retarget risk transfers to the pack clips in reduced form: first session, retarget one weapon folder onto the chibi purifier rig and check arm/head clipping + root-motion vs in-place before building the Animator — checklist in [[1h Last Rite - Player Moveset & Animation Plan]].**
- **Deferred to the gray-box (per portfolio discipline):** exact reference colours/hex per §3 channel; the precise LUT-per-stratum ramp; final proportion ratio (feel-dependent — tune against telegraph readability + retarget quality together); the ruin-kit piece count.
- **Tooling caveat:** Kimodo is open-source and runs locally (GPU-bound). Confirm the local install + Unity import round-trip works in the first slice before scaling — a broken FBX round-trip silently breaks the §5.4 rule. *(2026-07-31: combat clips no longer depend on this — see the §5.4 status note.)*

---

## 8. Canon ripple / portfolio note

This style is the **portfolio art lock**, proven here first. It propagates as-is to every later game ([[00 Game Concepts Hub]]): the Looter Shooter inherits Game 1's guardians as named-Husk mini-bosses with zero re-art; the corruption shader, Mani VFX colour language, one-rig pipeline, and AI-asset post-pass carry forward additively. **⚠ Note the 4th species ripple already logged** (Avian/Vayu, added in the City-Builder pass) — when the shared mesh set grows to 4, this bible's one-rig discipline absorbs it the same way (shared skeleton, new mesh, accessories — no new rig).

---

## 9. The purifier's Shroud — ablation visual law (added 2026-08-09)

> The player's health system is **worn**: the **Shroud** ([[1j Last Rite - Shroud, Mani & Sanity]] §1) ablates through four states rendered on one mesh. This section is the art law for it. **The §1 style lock is untouched** — the 2026-08-09 concept studies (semi-stylized anime sheets) define **material/state grammar only, never proportions**; production translates the grammar onto the chibi purifier rig.

- **The four states:** **Vested** (full suit — dark, sleek, gold filigree; marks hidden) → **Kindled** (extremities ablated; marks faintly lit) → **Unbound** (torso panels gone; marks fully lit, casting light onto remaining suit) → **Ashform** (minimal bindings; marks BLAZE, light bleeding through skin; ash streams; she stands *taller* — dangerous, never defeated).
- **Ablation grammar — carbonize, never tear:** char → ember rim → curl → flake to rising ash motes. **No open flame** (fire reads as VFX soup at duel distance and is a different, expensive tech object — think cooling charcoal, not bonfire). **No fabric tearing** — tearing reads as damage-to-*her*; ash reads as *the rite spending itself*.
- **Value carries the read, not silhouette** (a second skin can't change shape): dark suit → skin + blazing gold. Each state must read in **~200ms at duel camera distance**; verify in a §6-style slice before mass production.
- **One pattern, two materials:** the suit's gold filigree traces **exactly** the sacred-mark geometry engraved in her skin — ablation *hands the pattern off* from inlay to glyph, continuous (it implies the suit was cast from her marks). One authored pattern texture; suit and skin both sample it.
- **Material language: lacquer, never latex** — deep-gloss *urushi* ceremonial-armor register in every prompt and every public description. Same asset, entirely different read.
- **Colour governance (§3 addition):** the mark/inlay glow = **the current Shroud element** (it joins the element channels — it means "her mani element," nothing else); raw/neutral = warm **amber**, matching the Bhu Mani shard identity in [[1i Last Rite - Bhu Mani Husks]]. **⚠ Collision risk (resolve at gray-box):** neutral amber-gold vs the **parryable-telegraph white/gold flash** — the telegraph channel wins; push the Shroud's neutral glow warm/ember until the two can never be confused.
- **Tech:** one mesh, one rig; states = a **dissolve mask** driven by Shroud integrity + an **emissive mark layer** (own albedo+emissive texture set; element hue + blaze intensity = material params). The body beneath is modeled/textured (final-state bindings). **No cloth sim** — form-fitting by design. Enemy-side ablation (guardians charring as they take damage) is a same-shader stretch — [[1j Last Rite - Shroud, Mani & Sanity]] §7.
- **Concept sheets (2026-08-09):** 4-panel ablation studies (Vested→Ashform; element recolors = the same sheet with the glow hue swapped). **TODO (dev): file the generation set into the concept-art store and mark the chosen sheet** — they currently exist only in the design-session chat. Meshy input follows the all-sides-sheet rule when the Vested suit goes to production.

---

## 10. Environment motion — "living structure via SHADER, never MESH" (added 2026-08-12)

> The environment direction lock ([[1l Last Rite - World & Environment Bible]] §2.6) establishes an art law with the same force as §1's style lock and §9's ablation law. It belongs in this bible because it is a **budget guardrail**, not a wing-specific note.

**The rule: never deform, rig, or blend-shape an environment mesh to make a building move.** It breaks silhouette, breaks collision, fights the silhouette-first flat-toon style (§1), and is the single most over-budget thing a small team can chase. **Life comes from shader, light and small-prop animation instead.**

Three tiers, cheapest first — stop at any point:

1. **MUST — pulse the emissive veins.** Animate glow *intensity* on a slow curve. Nearly free. In a dead, still world the eye latches onto rhythmic light; this alone reads as alive.
2. **SHOULD — vertex-displacement surface swell.** Millimetre in-out wobble on **organic bits only** (bulging masses, vat membranes). Silhouette unchanged → collision and style stay safe. No rig, no new geometry.
3. **PAYOFF — real animation on small props.** Specimens drifting in vats, fluid caustics, flexing membranes, drifting ash. Cheap to animate, and it lands at inspection distance where the player leans in.

**Build it once for Bhu, reskin per wing — the rhythm is characterization, and it costs one curve:**

| Wing | Motion | Rhythm | Reads as |
|---|---|---|---|
| Bhu | swells outward | slow regular heartbeat | Growth. Alive. |
| Jal | flows downward | continuous trickle | Circulation. Still running. |
| Agni | rises upward | irregular guttering | Dying. Cannot finish dying. |
| Vayu | nothing physical moves | pulse with holds | Holding its breath. |

**How it composes with the rest of this bible:** the veins are exactly the emissive channel §3 governs, so the motion tier-1 pulse is *also* what enforces the new value/motion separation — **décor is dark, desaturated and static-or-slow; gameplay hues are bright, saturated and sharply momentary.** A slow vein pulse can never be mistaken for a telegraph. Per-wing detail (Agni's endless convection, Vayu's suspended dust and the one working bellows): [[1l Last Rite - World & Environment Bible]] §2.6.
