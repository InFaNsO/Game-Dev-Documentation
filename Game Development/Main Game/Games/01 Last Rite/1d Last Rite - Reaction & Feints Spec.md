# Game 1 "Last Rite" — Reaction, Defense & Feints Spec (M1 slice)

> **The reaction-layer design that [[1c Last Rite - Combat Core Spec]] reserved in its §9 attachment points.** `1c` remains the authoritative v0 core (two fighters · turn order · one attack · damage · death · HUD). This doc specifies the **M1 slice**: the refined attack timeline, per-attack **defensibility**, the **flag-based reaction windows + resolver**, **feints / attack-strings**, and how **animation is generated and fitted**.
>
> **⚠ Timing model revised — see [[1f Last Rite - Combat Iteration Log]] (2026-07-18).** `1c`'s **I2** (ms of `ISimClock`) and **I3** (data-is-truth / anim-events-cosmetic-only) are **superseded**: timing is **animation-frame** based, and for phases + defense windows the **clip's animation events ARE the truth** (§3–§5, §7), consumed via a `CombatAnimEventListener`. Still live from `1c`: **I4** (per-attack timing), the reaction outcomes, and **I10** (every window/duration is a playtest-open reference number). Where the sections below still read in the old "SO-frames-are-truth / ms" voice, the events model in §3–§5/§7 wins.

---

## 1. The attack timeline — refined to 5 phases

`1c` models an attack as Telegraph → Commit → **instant** Impact → Recovery. M1 refines this two ways: a **Startup** phase splits off the front, and the instant Impact becomes a windowed **Active** phase.

| # | Phase | What it is | Defender can act? |
|---|---|---|---|
| 1 | **Startup** | attack begins; not yet readable | no — there's no tell yet |
| 2 | **Telegraph** | the readable cue | yes — reading starts here |
| 3 | **Commit** | locked in; the swing travels | yes |
| 4 | **Active** | hitbox live — a **window, not an instant**; damage resolves here | this window is the resolution anchor |
| 5 | **Recover** | exposed; returns to neutral | — |

**v0 → M1 mapping** (v0 is a strict subset; the v0 build keeps 3 phases until this slice lands):

- v0 `Telegraph` ⊃ M1 `Startup` + `Telegraph`
- v0 `Commit` = M1 `Commit`
- v0 instant Impact (commit-end) → M1 `Active` window (open … close)
- v0 `Recovery` = M1 `Recover`

```
t0 ─► Startup ─► Telegraph ─► Commit ─► ACTIVE[ open .. close ] ─► Recover ─► done
                 (cue up)     (locked)   ▲ damage resolves in this window
```

**Phase delivery (revised 2026-07-18 — supersedes the ms block below):** the five phases are **not** ms/frame fields on the SO. They are **animation events on the clip** — `AnimStartup / AnimTelegraph / AnimCommit / AnimHit / AnimRecover` — fired frame-accurately during the animation update and forwarded to `CombatEntity` by the **`CombatAnimEventListener`** (each event carries the `SOAttackDef` + the entity). "**Active**" in the table above is the **`Hit`** phase in code. The `SOAttackDef` now carries only *non-frame* data (`Damage`, the difficulty start-delay, `AllowedDefences`, combo metadata). See [[1f Last Rite - Combat Iteration Log]] and §7.

> **Legacy (pre-2026-07-18, retired):** the phases were ms/frame fields on the SO (`StartupMs … RecoverMs`, or `TimeWindow{StartFrame,EndFrame}` in the as-built), polled each frame from the Animator playback position. The M1 event-port replaced this — the polled-frame path is being removed.

---

## 2. Defensibility — which reactions an attack even allows

Independent of *timing*, each attack declares **which defenses can succeed against it at all.** This realizes (and supersedes) the `ReadType` field `1c` §3.1 reserved.

```csharp
[System.Flags]
public enum DefenseOption { None = 0, Parry = 1, Dodge = 2, Jump = 4 }

[Header("Defensibility — which reactions can succeed (gates the windows in §3)")]
public DefenseOption AllowedDefenses = DefenseOption.Parry | DefenseOption.Dodge; // default
```

- **Default** = `Parry | Dodge` (most attacks).
- **Dodge-only** = `Dodge` — unparryable / "perilous"; forces movement (the anti-turtle guard `1c` referenced).
- **Jump-only** *(future)* = `Jump` — heavy low **sweeping** attacks cleared only by jumping; neither parry nor dodge succeeds.

`AllowedDefenses` **gates which flag-windows exist** (§3): a dodge-only attack has no parriable/perfect window, so a parry press fails by definition (full hit) regardless of timing.

---

## 3. Defensive windows — delivered by clip events, tightened by a start-delay

**Revised 2026-07-18.** Each allowed defense window is **opened and closed by animation events on the clip** — a start event (`OnDefenseParryStart` / `OnDefensePerfectParryStart` / `OnDefenseDodgeStart`) and a close event (`OnDefenseWindowClose`), forwarded to `CombatEntity` by the `CombatAnimEventListener`. Between open and close the window is *live*; the resolver (§4) grades the player's press against that live state. The author places each start/close event by scrubbing the clip in Unity's FBX Events UI — **that placement, per clip, is where the window's feel is set** (not a global percentage formula).

The relative ordering the old %-of-phase model encoded still holds as an **authoring guideline** — parry opens late in Commit and closes at impact; perfect-parry is the tight window *at* impact; dodge spans Commit into the start of Hit — but it's now expressed by *where you drop the events* on each clip.

**Difficulty = a start-delay, not a width multiplier.** Higher tiers **delay the window's OPEN** by a per-tier amount held in the SO (defaults in a global `DefenseTuningDef`, per-attack override allowed); the CLOSE — anchored to the visual impact — does not move. Recommendation (playtest-open): **leave Perfect-parry un-delayed** (delay Block + Dodge only) so earned muscle memory stays valid across tiers and the tightening widens the skill gap. Grading is query-at-press, so the delay is simply `pressTime ≥ openTime + delay[tier]` — no timers. **Guardrail:** the delay must never push a window's effective open past its close (zero-width = undefendable = a fairness violation — assert it). Width-vs-delay rationale + history: [[1f Last Rite - Combat Iteration Log]].

During any overlap of a dodge and a parry window, the **button the player pressed** decides which applies (unchanged).

> **⚠ 2026-08-09 — a MENU DIFFICULTY setting now writes this same delay.** Difficulty is no longer diegetic-only: the player picks a difficulty in the menu and it drives the defense-window **start-delay**. **⚠ 2026-08-11 — this simplified: there is no longer a second axis.** The clause "exactly as the rebirth tier does … the menu setting **scales the rebirth ascension curve** … so the diegetic ladder survives" is **void** — the rebirth ladder is cut with the re-descent loop ([[1 Parry Combat - Last Rite]] D2, [[1k Last Rite - Lore Bible]] §6). The delay is now driven by **the menu setting over a per-stratum authored baseline**; the composition question is largely dissolved. **⚠ 2026-08-12 — the baseline axis just changed.** Strata order is now **free** ([[1k Last Rite - Lore Bible]] §6, [[1l Last Rite - World & Environment Bible]] §1.7), so a *per-stratum* baseline would hand a first-time player whichever wing happens to be hardest — Agni is explicitly the most intense, and nothing stops it being wing #1. **Recommended: key the baseline to `strataCleared` (how many wings you have finished), not to which wing you are in.** The ramp then survives free order intact, and each wing is free to author a *character* — tempo, telegraph language, roster — instead of a rung. ⚠ **Open, confirm at gray-box** ([[1l Last Rite - World & Environment Bible]] §6 item 8); the same axis should drive the fraying onset curve ([[1j Last Rite - Shroud, Mani & Sanity]] §3.1). Every law above holds unchanged, and two get *more* load-bearing: **Perfect stays un-delayed by default** (muscle memory must survive both axes), and the **zero-width assert** must now be verified at the extreme corners of the combined space, not just the deepest tier. See [[1j Last Rite - Shroud, Mani & Sanity]] §5.1 — the difficulty setting also **segregates the endless leaderboards**.

---

## 4. The resolver — `CombatEntity.GetDefenseResult` (as-built)

The reaction resolver is **`CombatEntity.GetDefenseResult`**, called from `CombatSystem.OnEntityDefense` when the defender presses. It grades the press against the **live window state the clip events set** (§3), gated by `AllowedDefences`; damage is dealt at the `Hit`/`Recover` edge unless a valid parry/dodge negated it. At the press:

```
outcome =
  if   inputType ∉ attack.AllowedDefences           → Hit       (defense not allowed at all)
  elif the matching window is not open @ press       → Hit       (mistimed)
  elif PerfectParry window open                      → Perfect
  elif Parry        window open                      → Parried
  elif Dodge        window open                      → Dodged
  else                                               → Hit
```

`ReactionOutcome { Perfect, Parried, Dodged, Hit }` (+ `FeintWhiff`, §6). The turn's `TurnData` records `Parried`/`Dodged`; the payout (purge / counter / damage-negate) + the host economy consume it (`1a` seam #1: G1 = purge/counter · LS = AP/crit/Mani-drop).

**Laws (survive the model change):**

- **Instant check — no i-frames, no input buffer** (`1c` §9.7: parry is never buffered). The press is graded at its timestamp against the live window.
- **Window-width law:** every window MUST be wide enough for human reaction + render/input latency (a few frames minimum) — *including at the tightest tier after the start-delay* (§3). With windows now on the clip, this is verified by the human authoring each clip by eye, and optionally by a validator that reads the clip events (`AnimationUtility.GetAnimationEvents`) and audits widths against the reaction floor.
- **Defensive-recovery (knob, default OFF):** a *whiffed* parry/dodge MAY incur a few frames where the defender can't re-defend — the only thing that gives a pure **Cancel feint** (§6.3) teeth. The **opposite** of i-frames (vulnerability-on-whiff). Turn up only if playtest shows fakes feel toothless.

---

## 5. Animation = pre-made clips carrying the timing (revised 2026-07-18)

Supersedes the "animation never carries gameplay timing / Kimodo time-warp" model (I3, `1c` §9.6 / D-c1). Each attack is **one pre-made single clip** (Mixamo / marketplace / one Kimodo gen), grounded + in-place, wired as an Animator state. The clip **carries the gameplay timing as animation events** — phases + defense-window open/close — so there is no time-warp and no SO-frame fitting step. Re-tune the feel → move the event on the clip; the runtime reads it directly via the `CombatAnimEventListener`. Full pipeline in §7; history in [[1f Last Rite - Combat Iteration Log]]. *(Clip sourcing decided 2026-07-31: the primary source is the purchased **Mega Animation Pack v1.8** — Kimodo dropped for combat clips; see [[1f Last Rite - Combat Iteration Log]] + [[1h Last Rite - Player Moveset & Animation Plan]].)*

---

## 6. Attack-strings, chains & feints (combo attacks)

**Combos are built by *linking pre-made single-attack clips*, not by generating bespoke multi-attack clips.** A string is **data** — an ordered list of `SOAttackDef`s the attacker flows through — and the flow between steps is a **normal Animator transition** driven by the as-built `AnimController.Play(nextStep)` param mechanism (`AnimTransitionData`). This is the **cost decision (D-d7):** single attacks are cheap and abundant (Mixamo / marketplaces / one Kimodo clip each), so a new combo costs ~**one data asset, not one generation** — and "authored" vs "dynamic" chaining become the *same* animation pipeline, differing only in **who picks the order**. Frames, not ms, per the as-built `SOAttackDef` (`TimeWindow` StartFrame/EndFrame).

```csharp
public sealed class SOAttackStringDef : ScriptableObject
{
    public SOAttackDef[] Steps;      // ordered combo; each step carries its OWN §2/§3 flags + windows
    public int   ChainFromFrame;     // frame in a step where the next step's transition fires
                                     //   (continue FROM THE CURRENT POSE — never an idle reset)
    public bool  CanCancel;          // may abort to guard instead of chaining
    public int   CancelAtFrame;      // where the cancel may trigger (must be before the step's Hit window)
    public SOAttackDef[] NextAllowed;// DYNAMIC chaining: which attacks may legally follow (the fairness set)
}
```

- **Chain (combo continuation)** — at `ChainFromFrame` the attacker flows straight into the next step **from the current pose, no idle reset** → relentless pressure, "alive" aggression. **Each step is independently telegraphed** (its own `SOAttackDef` windows), so defense stays a per-step *read*, and **each step's Hit/Recover edge deals its own damage** — the as-built `CombatSystem.OnAttackPhaseChange` already resolves damage per `ActiveAttack` at `Recover`, so chaining just advances `ActiveAttack` and **keeps the turn alive** instead of ending it via `AttackStateBehaviour`.
- **Each successful parry feeds the purge meter** → a **fully-parried string is a fat meter payday** (the D5-amendment synergy). Miss one read mid-string and that hit lands; the rest of the string keeps coming.

### 6.1 Enemy chains — authored spine + dynamic recombination

Two modes on the **same** linked-clip tech:

- **Authored** — `Steps` is a fixed, designer-picked order. A learnable *signature* (a boss's recognizable opener / phase-transition combo) → rank-able, no-hit-able identity.
- **Dynamic** — the planner picks each next step at runtime from `NextAllowed`; the player can't memorize the order and must **read live**. This is the anti-staleness / replay tool and the cheap **rebirth-remix multiplier** (M3/M4): the same steps reshuffled make cycle 2+ feel *re-learned* with zero new authoring.

Default per guardian = **hybrid** — an authored signature spine wrapped in dynamic connective chaining. The value of dynamic order is *preventing rote memorization* (keeping the fight a reading test), **not** surprise: in a no-buffer reactive game foreknowledge buys nothing, so the player reacts to the telegraph, not to memory.

### 6.2 Difficulty scaling (two clean axes)

Difficulty rises by **longer chains + tighter windows**, tuned per tier / rebirth overlay:
- **Chain length** — more steps before the string resolves (the *carrier*).
- **Window tightening** — a per-tier multiplier shrinks each step's parry/dodge windows (the *load*).
- Amplifiers that do the real work at high tiers: **mixed defense types** across a string (a dodge-only step next to a parry-only step forces a response-switch mid-chain) and **inserted cancel-feints**.

### 6.3 Feints / cancels (the fake)

**Cancel (the pure fake)** — play the shared Startup+Telegraph (indistinguishable from a real step), then abort to guard **before** the Hit window at the cancel branch. Baits a panicked parry; its teeth come **only** from the defensive-recovery knob (§4, default off). Keep at zero teeth (pure spacing / mind-game) or turn the knob up if fakes feel toothless in playtest.

**Switch (the attack-swap fake)** — the branch point sits at the **end of Telegraph, before Commit**. `Commit` is the point of no return, so an attack can only change *before* it — which is exactly why the phase order is `Startup → Telegraph → [branch: continue / switch / cancel] → Commit → Hit → Recover`. A guardian may show attack A's telegraph, then **switch to attack B**, punishing a defender who committed early to A. **Re-telegraph law (fairness sacred):** the switched-to B must play **its own** Telegraph → Commit → Hit before it can connect — *read → switch → re-read* — so a swap is never an unreactable sucker punch. In event terms this is free: the switch crossfades into B's clip, which fires its own phase events from frame zero. (Higher tiers may *shorten* B's post-switch telegraph, never below the reaction floor.)

The planner decides at each branch point: continue the step · chain the next · switch · cancel.

---

## 6b. Player combos — QTE-chained, AP-bounded (the offense side)

The offense mirror of enemy chaining: the player links their own single attacks into a combo on **their** turn. Expedition 33-shaped — a **planning** layer, not a twitch-execution test.

```csharp
// on the player CombatEntity — offense economy, DISTINCT from the parry-fed Purge meter
public int MaxAP;        // combo budget for the turn
public int CurrentAP;    // spent per chained hit; refill rule playtest-open
```

- **The loop** — on the player's attack turn, the player chains attacks by **matching a forgiving QTE timed-press** at each step (E33-style: press *near* the hit frame to extend/empower — **not** frame-perfect). Each landed step **spends AP**. The combo ends when **AP is exhausted** or a **QTE is missed** → no infinite combos.
- **The decision (why it fits turn-based)** — the player chooses **which moves to chain and in what order** from their moveset, each move an AP cost. A light tactical puzzle (which moves · what order · spend now or bank AP) — a *different cognitive mode* from the parry read, so it **adds** rather than competing with defense.
- **Keep the QTE forgiving** — a demanding QTE would secretly become a *second reaction layer* competing with the parry. It stays a planning flourish.

**Meter reconciliation (no third-economy creep):** **AP = offense input** (spent on the player's turn to chain) · **Purge = defense payoff** (parry-fed, arms the ultimate — D5 amendment). Distinct roles, no overlap. The parry read (enemy turn) and the AP-combo (player turn) are **temporally separated by the turn boundary** — they never demand attention at once, which is exactly why carrying both stays fair (E33 gets away with both for the same reason).

---

## 7. Animation: linked pre-made clips via the Animator graph

Supersedes the earlier Kimodo-multi-prompt authoring plan. Two rules still hold: **(a)** fakes **share the real telegraph**; **(b)** chains **continue from the current pose**, never idle. The mechanism is now the **Animator's own transition graph**, not generated multi-clips:

1. **Per attack** — one pre-made single clip (Mixamo / marketplace / one Kimodo gen), grounded + in-place, wired as an Animator state. Its **phase/window boundaries are authored as animation events on the clip** (Unity's built-in FBX Events UI); a `CombatAnimEventListener` on the Animator's GameObject forwards each event — with the `attackDef` + entity — into the combat runtime. **No scrub-to-author tool** (dropped 2026-07-18): the built-in Events UI *is* the scrub surface, and each clip needs a human's eye to place events on the right frame regardless, so a tool only re-wraps the required step. *(Sourcing note 2026-07-31: "marketplace" is now the primary path — the **Mega Animation Pack v1.8** per-weapon folders; Kimodo dropped for combat clips. Pack → retarget → Blender fix/re-time pipeline + clip mapping: [[1h Last Rite - Player Moveset & Animation Plan]].)*
2. **Chains (authored or dynamic)** — `AnimController.Play(nextStep)` sets the `AnimTransitionData` params that transition state A → state B; the graph edge's blend duration carries the pose-to-pose flow. **No per-combo generation, no blind runtime cross-blend** — the transitions live in the graph and are reused across every string. *(Validated empirically in the test combo system: seams read fine and no per-pair curation or seam-gap enforcer was needed — Unity's transition blending handles it.)*
3. **Cancel / fake** — reuse the real step's shared telegraph state, then transition to a short "pull back to guard" state at `CancelAtFrame`. Never a separate fake-telegraph clip (that would make it readable).
4. **Timing truth lives on the clip** (revised 2026-07-18): the phase/window **events** are the source of timing, fired frame-accurately from the clip and consumed via the listener. The `SOAttackDef` now holds the **non-frame** data only — `Damage`, the per-tier **defense-window start-delay** (difficulty), AP cost, `AllowedDefences` / `NextAllowed`. *(This inverts §3–§5's original "SO frames are truth, clip fitted to them" framing — superseded for windows/phases; the fuller §3–§5 rewrite is pending.)*

**Workload:** a signature combo = **1 data asset** (an ordered `SOAttackDef` list) + reused graph transitions; a dynamic chain = **0 extra** (the planner reshuffles existing steps); a fake = **+1 short guard-return clip** (telegraph shared). The moveset's single attacks are the only real animation cost — cheap and reusable.

---

## 8. Decisions recorded (this pass)

- **D-d1** — **5-phase timeline**; Impact becomes an **Active window** (was instant); Startup split off the front.
- **D-d2** — **Defensibility flags** (`AllowedDefenses` = `Parry|Dodge|Jump`; default `Parry|Dodge`) gate which windows exist; supersede the reserved `ReadType`. **Jump reserved** for heavy sweeps.
- **D-d3** — Defensive windows = **%-of-phase flags**, resolved against Active; **instant flag check, no i-frames / no buffer**; window-width is a fairness law.
- **D-d4** — **Chain = combo continuation from the current pose** (pressure/flow), explicitly **NOT a mixup**; defense stays per-attack telegraphed.
- **D-d5** — **Cancel feint = optional**; teeth come from the **defensive-recovery knob** (default off; the opposite of i-frames).
- **D-d6** — Animation = pre-made single clip per attack + **markers + per-segment time-warp pinned to the Hit window** (I3); fakes share the telegraph. *(Supersedes the Kimodo-multi-prompt authoring of the original pass — see D-d7.)*
- **D-d7 (2026-07-15)** — **Combos = linked pre-made single-attack clips via the Animator transition graph**, not bespoke multi-attack generations. A string is a data list of `SOAttackDef`s (`SOAttackStringDef`); the seam is a normal graph transition driven by `AnimController.Play`/`AnimTransitionData`. Cost of a new combo ≈ one data asset. **Empirically validated** (test combo system): no per-pair transition curation and no seam-gap enforcer needed — Unity's transition blending handles the seams. **Supersedes §7's earlier multi-prompt-Kimodo plan and the Kimodo path change.**
- **D-d8 (2026-07-15)** — **Enemy chaining = hybrid**: an authored signature spine + **dynamic** runtime recombination from a per-attack `NextAllowed` fairness set. Dynamic order is the anti-memorization / **rebirth-remix multiplier** (its value is a live reading test, not surprise). Difficulty scales on **chain length + window-tightening**, amplified by mixed defense types + feints.
- **D-d9 (2026-07-15)** — **Player combos = QTE-chained, AP-bounded offense** (§6b). Forgiving E33-style timed-press per step; each step spends AP; combo ends on AP-exhaust or QTE-miss. It's a *planning* layer (which moves, what order), temporally separated from the parry read by the turn boundary. **Economy split: AP = offense input · Purge = parry-fed defense payoff** (no third meter). This is a conscious scope expansion (reactive-half → two-sided combat) the user opted into.

---

## 9. Deferred / playtest-open (I10)

All window frames + phase durations · the defensive-recovery value · the **Jump** mechanic's implementation · the planner branch policy (when to chain vs cancel, which `NextAllowed` step to pick) · whether the Hit window is single or supports multi-hit · *(the scrub-to-author tool is **dropped** — 2026-07-18 — windows/phases/branch points are authored as clip animation events via Unity's built-in Events UI; the M1 work is the code port, not a tool)* · **the AP refill rule** (per-turn reset vs earned-by-attacking vs earned-by-parrying — decides whether the game rewards aggression or patience) · **the QTE window width** · the chain-length caps + window-tightening curve per elemental tier · whether the counter (perfect-parry window) also carries a timed-press flourish.
