# Game 1 "Last Rite" — The Shroud, Mani Defense & Sanity

> **The player's health/armor system, the defensive mani economy, the two-pool sanity model, the death economy + kalpa loop, and the promoted endless mode — designed and confirmed in the 2026-08-09 design session** (ablation concept study → systems pass). Companion to [[1 Parry Combat - Last Rite]] (D-locks; **D1/D7/D8 amended today**), [[1f Last Rite - Combat Iteration Log]] (the four 2026-08-09 entries), [[1b Last Rite - Art Bible]] §9 (the Shroud's visual law), [[1a Last Rite - Code Architecture]] (§2.6 / §4.5 / §6 notes), [[1i Last Rite - Bhu Mani Husks]] (what drops mani).
>
> **Two developer decisions anchor this doc (2026-08-09):** (1) the 08-07 elemental cut is **narrowed, not reversed** — mani returns player-side as **defense only**: no attacks, no offensive meter; **Purge stays the only offensive meter**, AP the only offense input. (2) Sanity keeps the fairness firewall in combat — **combat information never lies by default**; true cue-corruption lives only behind the opt-in **"embrace the hallucination"** toggle, tuned/enforced after playtesting.
>
> Numbers are reference values, **ALL playtest-open**, per portfolio discipline.

---

## 0. Fiction & naming (canon)

> **⚠ 2026-08-11 — this section is now downstream of [[1k Last Rite - Lore Bible]], which is authoritative for all fiction.** Four things below were rewritten by the story lock: the rite-giver is named, the "first beast's heart" is cut, the ending set forks three ways, and the kalpa is redefined now that the re-descent loop is gone (D2).

- **The Kavach Rite** (*kavach* = armor; the protective invocation) — the ritual performed on the purifier **before the game**: dying, she was sheathed in consecrated mani as a stopgap. The rite is literally what keeps her alive — and **it leaks**, needing a constant mani feed to stay lit. **She is a prisoner of her own cure**, and that is her motive for the dive: steal permanent immortality so she can finally stop feeding it. **⚠ Her mentor performed the Rite** and runs the tutorial (§8.1 — the ★ is answered).
- **The Shroud** — the garment the rite left behind: a second skin woven of mani, worn as armor. **English in all UI** ("Shroud depleted"), *kavach* in lore register. *(Rejected name candidates logged: Wick, Sheath, Kosha.)*
- **Mani is Husk-substance.** Every repair patches her in more of what she hunts — **the healing IS the disease.** Husks are previews of her ending: what remains of those who wore too much. **⚠ 2026-08-11 — the delivery changed, not the thesis:** the story is **tragedy, not mystery**, so this is *stated early* and the tension is dramatic irony, not a reveal (§8.2). The mechanic still carries it; the writing no longer has to hide it.
- **Immortality framing (D1 amendment, ⚠ re-amended 2026-08-11):** she is NOT immortal — she is *deathless*, unstably, by the rite; **true immortality is what she hunts**, and this facility reportedly cracked it. ~~The first beast's heart (D1) still locks her into the rebirth cycle.~~ **CUT** — there is no inciting heart-pickup and no cycle to be locked into; she is deathless from minute one because the Rite already happened.
- **The three terminal states (⚠ REPLACES "both endings are the Husk-making"):**
  - **Husk out** (sanity zero, §3.3) — the deathless state completing itself *blind*. Failure, mid-dive, but authored.
  - **Take it** (at the heart) — the same becoming, *knowing*. The tragic true ending; hands the baton to the LS.
  - **Renounce it** (at the heart) — she drops the gem-crutch and walks out with the leaking mortal rite she came in wanting to escape. **Neither failure nor the Husk-making** — this is the ending the whole restraint theme exists to make possible. Full text: [[1k Last Rite - Lore Bible]] §8.
- **The kalpa loop (⚠ REDEFINED 2026-08-11):** a **kalpa = one full campaign attempt** — *previously "its ~5 rebirth descents included"*, which is void now that the campaign is a single one-way dive (D2). A kalpa is now **one dive: tutorial → the four wings in any order → the seal-gated sanctum → the heart.** *(⚠ 2026-08-12: read "tutorial → Bhu → Jal → Vayu → Agni → the heart" until the strata order was freed — [[1k Last Rite - Lore Bible]] §6.)* Husking out ends the kalpa → **the next kalpa begins** (title card *"Kalpa II"* — diegetic per the locked canon plant: her attempt is one small turning of the world's own). Kalpa count remains a visible pride stat ("finished in Kalpa 1").

---

## 1. The Shroud — two-layer health

### 1.1 The layers

| Layer | What it is | Damaged by | Healed by |
|---|---|---|---|
| **Shroud** (primary) | mani-woven ablative armor — *it* takes the hits, not her | all enemy damage while any Shroud remains | mani — field patch or shrine re-vest (§2) |
| **Body** (short bar) | the woman underneath | damage in **Ashform** only | **shrine rest only** — mani repairs the Shroud, never her |

### 1.2 The four ablation states (the health bar you wear)

Shroud integrity IS the health readout, rendered on her model (mirrored in HUD):

| State | Integrity (ref.) | Read |
|---|---|---|
| **Vested** | 100–75% | full suit, marks hidden — dark, sleek, gold-lined |
| **Kindled** | 75–50% | extremities ablated, marks faintly lit |
| **Unbound** | 50–25% | torso panels gone, marks fully lit, casting light |
| **Ashform** | 0% | minimal bindings; marks BLAZE; ash streams; **body exposed** |

- **Ablation grammar — carbonize, never tear** (char → ember rim → flake to ash; no open flame). Full visual law: [[1b Last Rite - Art Bible]] §9.
- **Value carries the read, not silhouette** (a second skin can't change shape): dark suit → skin + blazing gold. Each state must read in **~200ms at duel camera distance**.
- **The Shroud is a pacing gauge:** tune per-room damage budgets so a mid player reaches a guardian door around **Kindled/Unbound** — arriving Vested = undertuned, arriving Ashform = overtuned. An encounter-balance instrument you can *see* in playtests.

### 1.3 Ashform — risk/reward, not a fear state

- **Execution-gated boost:** in Ashform her marks blaze — **Perfect-parry counters and the Purge ultimate deal heavily amplified damage** (implement as an `IDamageStep`, e.g. `AshformMult`); normal attacks unchanged. Skilled players get a spectacular payoff for clean play; panic play still dies.
- **Considered and CUT: passive sanity drain for *being* in Ashform.** The cost is *taking body damage* there (§3.1) — presence is free, mistakes compound. A no-hit **Ashform run** is a legitimate hardcore style, self-limited by the fact that one mistake starts the fraying spiral.
- Body damage taken in Ashform drains **fraying** proportionally (§3.1).

### 1.4 Reference numbers (D8 retarget — playtest-open)

- Shroud absorbs ≈ the old "~4 hits to death" (roughly one state per guardian hit); **body ≈ 2 hits**.
- Ashform amp reference: counter/ultimate **×1.5–2.0**.
- All D8 window/timing numbers untouched.

---

## 2. The mani defense economy (the 08-07 cut, narrowed)

**Scope guard first — what this is NOT (the cut's spirit stands):** no Mani-based attacks, no elemental player offense, no absorption, no Surge, no elemental trails, **no offensive Mani meter — Purge remains the player's only offensive meter**, AP the only offense input. Elements never touch the player's *reaction windows* (fairness firewall untouched). The mani reserve is **one scalar + one element tag** — a resource, not an inventory (the [[1a Last Rite - Code Architecture]] §6 guardrail holds).

- **⚠ Source — the gem IS the kill (CANONIZED 2026-08-11).** Mani is not loot that falls out of a corpse: **the mani-gem lodged in each Husk is the core of its deathlessness, and tearing it out is the killing blow.** The body crumbles instantly to dust, centuries overdue — a mercy, a last rite. Mechanically this is the **purification finisher** (the D5-amendment killing blow), so the harvest costs no new verb, no new input, and no new animation slot. Fiction: [[1k Last Rite - Lore Bible]] §5.
- **Drops:** each Husk yields mani of **its element**; some yield **raw** (element-neutral) mani. (Diegetic anchor: the gem is visible on every enemy and doubles as its telegraph — [[1i Last Rite - Bhu Mani Husks]] §1.1.)
- **Raw mani heals MORE** (pure, unspecialized). **Elemental mani heals less** but re-weaves the Shroud **in that element's color**: an elemental **resist** + a paired **weakness** (reference pairs, playtest-open: **Agni ↔ Jal**, **Bhu ↔ Vayu**). Choosing your color is a read-the-descent gamble, never a strict upgrade — this inversion is what keeps raw mani from becoming vendor trash.
- **Field patch vs re-vest:** in the field, spending reserve **patches** the Shroud (partial, costs **fraying** §3.1 — the soft cap on mid-fight chugging). At **shrines / room checkpoints**, **re-vesting** fully restores the Shroud and is the **only place to switch element** — the re-consecration beat.
- **Element = defense loadout.** Swapping vestments IS swapping element. The element axis returns to Game 1 *defensively* — visible on her body, never in her attacks.

---

## 3. Sanity — one bar, two kinds of loss

Mirrors the two-layer health: **fraying** (recoverable, in-the-moment pressure) beneath **scarring** (permanent-per-kalpa attrition). One HUD bar: scars permanently blacken the end; fraying moves under the remaining ceiling.

### 3.1 Fraying (recoverable) — **⚠ now a trade, not a pure cost (2026-08-11)**

- **Drained by:** body damage taken in Ashform (proportional) + mani spent in the field.
- **⚠ What it BUYS (NEW — the give-and-take):** as sanity frays she gets **damage up and elemental resistance up.** Coming apart makes her more dangerous — which is the whole reason a player would ever choose to spend rather than hoard. *(Previously fraying was pure downside, which made every optimal line "never fray"; that made the bar decorative.)* **Spend it, claw it back.**
- **The exchange the player is actually operating:** fray → hit harder and eat elements better, but read the world through dampened cues; rest → clear your head and give the power back. **The buff must never touch defense windows or telegraph legibility** — it is damage/resistance only, so the fairness firewall (§3.4) is untouched.
- **Restored:** fully, at shrine rest.
- **⚠ Tuning risk to watch at gray-box:** if the fraying buff is strong enough that staying frayed is strictly optimal, the sensory-narrowing cost stops reading as a cost and the system inverts into a "stay mad" meta. Scarring (§3.2) is the intended brake — verify it actually bites.
- **Expressed as (CONFIRMED 2026-08-09 — the fair channels):**
  - **Sensory narrowing:** music/ambience duck toward silence, desaturation, vignette, arena edges breathe — **the enemy and its telegraph VFX stay full-contrast.** The world dissolves; the threat remains. Fairness-*positive*: cue salience rises as everything else rots.
  - **HUD unreliability:** *abstract* UI flickers/ghosts (meters, prompts) — **diegetic reads are always true** (Shroud state on her body, marks, enemy animation). Weans players onto the reads the game teaches anyway. **Shrines are fully honest** — consecration clears her head.
- Onset must be *felt* by **the second stratum — whichever one it is** — a slow creep would never be seen inside a 3–5hr core. *(Was "Descent 2" under the cut re-descent loop; then "the **Jal** stratum (the second)" until **⚠ 2026-08-12**, when strata order went free and no wing is reliably second any more — [[1k Last Rite - Lore Bible]] §6.)* **Tune the onset curve against `strataCleared`, not against a named wing** — the same axis the difficulty ascension should use ([[1l Last Rite - World & Environment Bible]] §6 item 8).

### 3.2 Scarring (permanent within a kalpa) — the greed ratchet + the death countdown

> **⚠ AMENDED 2026-08-11 — scarring now has TWO sources, not one.** It was death-count only; the story lock adds **greedy play** as a first-class trigger, because "greed is a loan with permanent interest" is the game's thesis and a pure death-counter never charged for greed that *worked*. **Developer decision: keep both.** The knot countdown stays fully load-bearing.

- **⚠ Greed triggers (NEW — the ratchet):** three acts carve scars —
  1. **taking body damage** (she is out of Shroud and eating hits with the woman underneath),
  2. **over-leaning on gem buffs** (riding the harvest instead of the read),
  3. **pushing sanity past threshold** (spending fraying below a floor to keep the §3.1 buff up).
  These are the *chosen* costs — they fire on greedy play that succeeded, which is exactly what a death counter can't see. **Thresholds, weights, and whether all three ship are gray-box work** (§8.3); the design requirement is that a player who never gets greedy can finish **with zero scars**, and that a player who leans on all three brushes the brink without ever dying.
- **Every Nth death permanently blackens one notch** (reference: **N = 3**, pool ≈ **10** — sized so a mid player *brushes* the brink late in Kalpa 1; playtest-open). **This stands** — dying is a mistake, greed is a decision, and the design charges for both.
- **⚠ Combined-budget warning:** with two sources feeding one pool of ~10, the pool is now easier to drain than it was priced for. **Re-tune N and the pool size together with the greed weights** — do not carry the old N=3 / pool-10 reference forward unexamined.
- **Death-screen countdown, diegetically framed** — the rite's seals/knots fraying: *"the rite holds. two knots remain."* Predictable dread, legible, marketable.
- The counter **persists across rests** (a reset would mean it never fires). **No regen, no farmable recovery — the next kalpa is the recovery path.** *(A finite boss-relic scar-cure was considered and CUT for the short campaign; relics stay a candidate as true-ending material only — §8.)*
- Replaces both the earlier "per-death sanity loss" and "max-Shroud attrition" drafts — **one permanent currency.**

### 3.3 Zero scars = ARMED → the Husk ending

- At zero remaining, the state **arms**: the **next death plays the Husk ending** — authored, credits, canon (she turns *blind*, mid-hunt). Not a game-over screen: the failure is content, and it hands its own baton to the LS exactly as the *Take it* ending does.
- **⚠ 2026-08-11 — this is one of THREE terminal states, not one of two** (§0). Husking out is the *blind* becoming; **Take it** at the heart is the *knowing* becoming; **Renounce it** is neither. **Scar count is the price tag on that final fork** — the more scarred she arrives, the more the choice costs her ([[1k Last Rite - Lore Bible]] §8). **Open: what "costs" means mechanically** — a narrowed fork (heavily scarred runs lose access to Renounce), a harsher epilogue, or a scaled final encounter. Gray-box + writing pass (§8.3).
- → **the next kalpa begins** (§4). *Optional epilogue hook (proposed, review): the Husk ending flows directly into the endless mode — "what's left of her keeps fighting" (§5).*

### 3.4 The firewall, amended (D7)

- **"Fairness sacred" now means: combat information never lies by default.** Telegraphs, cue timing, windows — honest at every sanity level. Sanity's teeth are **stakes** (scarring → the Husk ending) and **the wrapper** (narrowing, HUD, explore-canvas hallucination, narration decay — the last two already specced in [[1a Last Rite - Code Architecture]] §4.5).
- **Why the firewall held (logged so it's never relitigated):** at a ~130ms Perfect window, players respond to cues *pre-cognitively* — a false spark is a reflex hijack, not unreliable information. Same DNA as the 2026-07-18 difficulty decision: earned muscle memory stays valid.
- **True cue-corruption (false sparks, phantom tells) exists ONLY behind the "embrace the hallucination" toggle** — the D7 stretch, now its designated home. Opt-in, candidate score/mani reward, **enforced/tuned after playtesting**.
- **⚠ 2026-08-09 (b) — the toggle is a BUILD ITEM, not a post-launch stretch.** It ships playable **ON/OFF so the fake-cue experience can be playtested both ways** before its final status is decided (stays opt-in · becomes default · earns a reward · gets cut). Until that call, it carries **no score/mani reward**, and its state is **recorded with any endless run** so leaderboards can segregate on it (§5.1).
  - **⚠ The craft rule (binding whenever it is ON):** corruption may **ADD false-positive cues** — phantom sparks, ghost/early tells, audio that isn't there — but may **NEVER remove, dim below the floor, or delay a real cue, and may NEVER touch window timing.** Corrupt *information*, never the contract. **An absent cue reads as a bug; a false cue reads as her mind** — that asymmetry is the entire reason the toggle is shippable at a ~130ms Perfect window.
  - Still subscriber-only: the toggle adds *presentation* events, it does not gain a sim API.
- **Accessibility:** narrowing preserves cue audio *by construction*; a **"steady HUD"** toggle pins the abstract UI; a floor below which nothing degrades.
- Architecture unchanged: `SanityDirector` stays a pure subscriber with no sim API — [[1a Last Rite - Code Architecture]] §4.5 note.

---

## 4. Death economy & the kalpa loop

- **Any death:** respawn at duel start (diegetic, unchanged) · the scar countdown ticks · **unconverted mani reserve drops where you fell — one retrieval**, Souls-style (worn Shroud is never lost).
- The earlier "Shroud-death vs Ashform-death" two-penalty draft is **replaced** by this unified rule + the countdown.
- **Husk-out → Kalpa N+1:** campaign resets to the top of the dive. **Carry-forward = weapon/attack unlocks, codex/lore, shortcut knowledge** — light meta, not a roguelite grind. Scars reset with the kalpa.
- **Why a full reset is right at this scope:** the locked core is ~3–5hr; a Sifu-shaped reset at that length is mastery pressure, not content-deletion. Kalpa 2+ replays compress toward score-attack length (~1.5–2hr) — the campaign is designed for the clean run; deaths inflate a first run on their own (~4–5hr).
- **⚠ 2026-08-11 — the kalpa reset is now the campaign's ONLY repeat structure.** With the re-descent loop cut (D2), husking out is the one thing that sends the player back to the top. That raises its weight: it is no longer one reset among five authored cycles, it is *the* reset. Two consequences to watch at gray-box — (1) **scar tuning is now the sole pacing control on run length**, and (2) the shortcut-knowledge carry-forward matters more, since a Kalpa 2 player re-walks strata they have already cleared rather than a remixed variant.

---

## 5. Replay structure — the promoted endless pillar

- **⚠ 2026-08-09 — the Chaos Descent is PROMOTED from v1-stretch to CORE v1**, as the vehicle for the endless mode replayability now centers on. Still **zero procgen** (random *selection* of authored content) — but element assignment extends from one-theme-per-stratum to **mixed per-encounter assignment** ("any combination of enemies, not just one element").
- **⚠ 2026-08-12 — it now has a physical address: the pit BENEATH the sanctum** ([[1l Last Rite - World & Environment Bible]] §2.2). This is the **only deep-underground part of the game** — the campaign spreads horizontally at ground level along the processional spine, so going *down* is reserved for the one mode that never ends. Two things this buys for free: the mixed-element roster is diegetically justified (everything the facility made, fallen into one hole), and "the only place the descent repeats" gets a location to be true of. The pit interior is **not yet designed** ([[1l Last Rite - World & Environment Bible]] §6 item 9).
- **Unlock: the first ending reached — Take, Renounce OR Husk** *(⚠ 2026-08-11: three endings now, §0; and there is no NG+ left to gate it behind anyway — the replay mode must not hide behind ~6+ hours).*
- **Sanity is the run timer:** in endless, sanity only falls; mani use accelerates it; **the run ends when she turns.** You can survive forever; you can't stay yourself forever.
- **Composition discipline — curated, never pure-random:** role tags (pressure / controller / support / elite — e.g. the Mourner's force-multiplier identity, [[1i Last Rite - Bhu Mani Husks]]) constrain valid waves; the element mix is the random axis. Pure-random pairing ships trivial *and* unwinnable waves.
- **Score = depth × sanity kept** (working name *Remnant* or *Purity* — open): a high score means you didn't lean on the thing that keeps you alive — the fiction as a scoring rule.
- **Campaign scoring:** the ending screen reports **kalpa count · deaths · scars carried · score**. **⚠ 2026-08-11 — scars are the headline stat**, because the campaign's replay pillar is now *the clean run*: "reaching the finale with sanity intact is the true victory, and a no-scar clean run is a genuine flex" ([[1k Last Rite - Lore Bible]] §7). A **zero-scar clear wants a named badge on that screen** and should be the thing players screenshot. Per-duel ranking (D7 layer 2) stands unchanged; the full combat-scoring pass (combo scoring · defense-weighted grading · rank→mani hooks · heal-penalty) is **a deferred design session** (§8).

### 5.1 Endless leaderboards (DECIDED 2026-08-09 — Steam)

**Why endless can carry a board when the campaign can't:** layout and difficulty are held constant; only enemy selection and element mixing vary. Runs are comparable by construction.

- **Platform: Steamworks leaderboards.** No backend to run or keep alive — Steam owns storage, friends filtering and pagination. Friends boards are the feature players actually use; global is the aspiration.
- **⚠ HARD REQUIREMENT — segregate the boards.** One board per **menu difficulty tier** (D9 — the setting changes defense-window delays, so cross-tier scores are not comparable), and **record the hallucination-toggle state** with every run, segregating on it too until that toggle's scoring policy is settled (§3.4). A single undifferentiated board is dominated by the easiest configuration within a day.
- **⚠ Verified boards are architecturally foreclosed — design accordingly.** Replay-validated scoring needs a **deterministic sim** (fixed timestep, seeded RNG throughout, no float/animation-timing variance). The as-built architecture is **MonoBehaviour-driven with the Animator as the clock** ([[1a Last Rite - Code Architecture]] §AS-BUILT) — non-deterministic across machines by construction, and *not* retrofittable without rewriting the combat layer. **This is a live constraint, recorded so it is never rediscovered as a surprise:** if bit-exact verification ever becomes a requirement, it is a combat-layer rewrite, not a feature.
- **Therefore: heuristic validation, and be honest about it.** Submit run metadata (waves cleared · sanity remaining · elapsed · difficulty · toggle state · seed) and reject implausible submissions server-side against bounds the design already provides — sanity can only fall, so **a run has a computable maximum depth**; minimum time-per-wave and maximum score-per-wave are similarly bounded. This catches egregious cheating cheaply and never blocks a legitimate run.
- **Accept that the global top will eventually be cheated**, and make that survivable: lead the UI with **friends boards + personal best**, keep global secondary. Do not build systems whose value collapses if the #1 slot is fraudulent.
- **Not planned:** async social (death markers, player messages, ghosts) — a separate backend and a real scope decision if ever revived.

---

## 6. Pacing & tuning targets (playtest-open — 2026-08-09 session)

- **Model: duel-spined (Punch-Out / Sekiro), NOT zone-curriculum.** Fixed kit — **defense verbs all-in from minute one** (parry/block/dodge are the game's alphabet); **offense staggered** (attack unlocks + open Censer timing, per the 08-07 entry — a mid-game weapon is a renewed-interest beat; don't spend it in hour zero). All depth is enemy-side. Three rules keep "enemy variety carries it" true:
  1. **First contact stays readable** — never introduce an unknown timing table inside a mixed pack; unknowns debut solo (duels do this automatically) or beside *known* partners only.
  2. **New enemy = new read** — tempo, delay, feint rate, string length, punish window are the variety axes; a new skin with familiar timings is inventory, not variety.
  3. **Parseable in 2–3 attempts, then layered** — base patterns readable within a couple of retries; depth arrives via phases, so retries feel like learning, not grinding.
- **Duration targets:** duel 30–90s (D8, stands) · boss attempts **3–8** (under 3 = pushover; 10+ = wall, for Kalpa 1) · death→retry runback **≤60–90s** · something *new* (enemy, pairing, overlay, tool) every **≤4 min**.
- **The Shroud-as-gauge** (§1.2) is the primary encounter-tuning instrument.
- **Length truth:** first run ~4–5hr with deaths · clean informed run ~2–2.5hr · Kalpa 2+ ~1.5–2hr — consistent with the locked "3–5hr core."

---

## 7. Build notes (implementation hooks)

- **One mesh, one rig:** only Vested is modeled; states 2–4 = a **dissolve mask driven by Shroud integrity** + an **emissive mark layer** (marks = their own texture set: albedo + emissive mask; element hue + blaze intensity = material params — one shader property drives both element color and health intensity). The body beneath is modeled/textured (final-state bindings). Full art law: [[1b Last Rite - Art Bible]] §9.
- **No cloth sim** — form-fitting by design (the ablation grammar was chosen partly to dodge the cloth/hair tearing tax).
- **Damage steps:** `AshformMult` (counter/ultimate amp) + element resist/weakness both ride the existing `IDamageStep` chain — no new architecture.
- **Saves:** `RunSave` += mani reserve · Shroud element + integrity · fraying; `MetaSave` += scars · kalpa index · endings seen ([[1a Last Rite - Code Architecture]] §2.6 note).
- **SanityDirector** extends with narrowing (mixer/volume + post weights ∝ fraying, enemy/telegraph channels exempt) + a HUD-unreliability driver targeting `LastRite.UI` *abstract* widgets only — still subscriber-only ([[1a Last Rite - Code Architecture]] §4.5 note).
- **Enemy-side ablation (stretch, proposed):** guardians char/flake as they take damage — same shader family at smaller scale; free readability + world consistency.
- **Concept sheets:** the 2026-08-09 ablation studies (4-panel Vested→Ashform + element recolors) currently live only in the design-session chat — **TODO (dev): file them into the concept-art store and mark the chosen sheet.** Production translation is **onto the chibi rig** — the sheets define material/state grammar, never proportions ([[1b Last Rite - Art Bible]] §9).

---

## 8. Open questions

### 8.1 ✅ ANSWERED by the story lock (2026-08-11) — [[1k Last Rite - Lore Bible]]

*These were deferred to the tutorial writing pass on 2026-08-09. The Lore Bible closed them; kept here with their answers so the reasoning isn't rediscovered.*

- **★ Who performed the Kavach Rite → HER MENTOR**, who also runs the tutorial. **⚠ The "rite-giver is the antagonist" working hypothesis is DEAD** — the antagonist is a different figure entirely: the **lead researcher who first tested the immortality-medicine on themselves**, waiting at the heart. The mentor and the antagonist are not the same person, and folding rite-giver into kalpa-namer is no longer on the table. *(Still open: the mentor's fate, and whether they appear beyond the tutorial — [[1k Last Rite - Lore Bible]] §11.)*
- **★ Her motivation beyond survival → SHE COMES TO STEAL.** The Rite leaks and needs a constant mani feed; she heard this facility cracked permanent immortality and dives to take it so she can stop feeding the rite. She does not know the "cure" is what hollowed everyone inside. **Her solution and her doom are the same substance.** *(This is the answer to the question the 08-09 session flagged as too thin to leave open — "a hunter curious about Husks" is retired.)*
- **★ Knowledge staging → RESTRUCTURED as dramatic irony**, not staged reveals. See §8.2, rewritten.
- **What the tutorial actually teaches** — defense verbs are all-in from minute one (§6), so its real job is the *fiction* (the mentor, the Rite, the leak, the rumor) plus Shroud/mani literacy, not verb drip-feed. **Still open, and now the tutorial's whole design brief.**

### 8.2 Knowledge staging — **⚠ REWRITTEN 2026-08-11: dramatic irony, not staged reveals**

> **The structural decision that forced this: the story is TRAGEDY, not mystery** ([[1k Last Rite - Lore Bible]] §9). *"The player will see the doom coming; the dread is watching whether she (and they) can stop in time."* The previous three-layer staging — withholding "mani is Husk-substance" to mid-game and "every enemy is a previous her" to late — **is cut.** It was built to serve D7's "the mystery unfolds across rebirths," and both its delivery vehicle (the re-descent loop) and its structural purpose (mystery) are gone.

**What the player knows, and when:**

1. **Known from hour one (unchanged):** the Shroud keeps her alive · mani repairs it · repairing costs sanity. This is the health system; obscuring it would just be bad UX.
2. **⚠ Known EARLY (moved up from "mid-game reveal"): mani is Husk-substance.** The material she patches herself with is *them*. Told plainly, near the top of the dive — the facility is full of evidence and the mentor has no reason to hide it. **The power was never in the surprise**; it is in the player continuing to heal anyway, one informed choice at a time. That is the trade §3.1 asks them to make all game.
3. **⚠ Known EARLY (moved up from "late reveal"): the Husks are what became of people who did exactly this.** The facility's own staff, and the **fallen purifiers who came before her** littering the route. **Every enemy is a preview of her ending, and the game says so.**

**What IS withheld — the only thing that needs to be:** *whether she can stop.* The tension is the fork at the heart (§0), and the player's own scar count walking into it.

- **The asymmetry that makes this work:** the **player** understands early; **she** rationalizes. She keeps insisting the immortality she's stealing is the *other* kind — the permanent one, the clean one — right up to the moment she meets the researcher who already took it. The player watches her build that excuse in real time.
- **Consequence for D1:** ~~the first beast's heart should grant the *illusion* of arrival~~ — **void, the heart is cut** (§0). The illusion-of-arrival beat has no pickup to hang on; if the writing pass still wants it, it belongs at a **stratum boundary** (a false summit — she believes the next floor down is the last one).
- **Consequence for the tutorial:** it must plant the doom, not hide it. The mentor is the natural mouth for it — someone who performed the Rite on her and knows exactly what it is made of.

### 8.3 Systems still open

- **⚠ Difficulty-axis composition (D9) — SIMPLIFIED 2026-08-11.** This asked how the menu difficulty setting and **the rebirth ascension** combine into one `delay[tier]`. **The rebirth ascension no longer exists** (D2 — the re-descent loop is cut), so there is only one axis left: the menu setting, plus whatever per-stratum baseline the strata are authored with. The composition problem is largely dissolved; what remains is verifying the §3 zero-width guardrail at both extreme corners.
- **Combat scoring session** (combo scoring · defense-weighted grading · rank→mani · heal-penalty) — next design session; converges with the existing per-duel ranking and now also feeds the endless leaderboard score **and the clean-run/scar readout on the campaign ending screen** (§5).
- **⚠ NEW (2026-08-11) — the greed-scar triggers:** thresholds and weights for body damage · gem-buff leaning · sanity-floor pushes (§3.2), **re-tuned jointly with death-N and the scar pool** now that two sources feed one pool.
- **⚠ NEW (2026-08-11) — the fraying buff magnitudes:** how much damage and elemental resistance fraying buys (§3.1), and the guardrail that keeps "stay frayed" from becoming strictly optimal.
- **⚠ NEW (2026-08-11) — what scars cost at the fork:** how scar count prices the Take/Renounce choice (§3.3) — narrowed options, harsher epilogue, or a scaled final encounter.
- Ashform multiplier · resist/weakness magnitudes · the element opposition pairs.
- Boss relics as *Take-it*-ending material — and whether the Husk-ending→endless epilogue ships.
- **Hallucination toggle:** final status after the ON/OFF playtest (stays opt-in · becomes default · earns a reward · cut). Its craft rule is already binding (§3.4).
- Score name (*Remnant* / *Purity*) · endless wave-table authoring format.
- Whether "Shroud" survives contact with marketing (*Wick*, *Sheath* logged as alternates).
