# Game 1 "Last Rite" — Reaction, Defense & Feints Spec (M1 slice)

> **The reaction-layer design that [[1c Last Rite - Combat Core Spec]] reserved in its §9 attachment points.** `1c` remains the authoritative v0 core (two fighters · turn order · one attack · damage · death · HUD). This doc specifies the **M1 slice**: the refined attack timeline, per-attack **defensibility**, the **flag-based reaction windows + resolver**, **feints / attack-strings**, and how **animation is generated and fitted**.
>
> All of `1c`'s invariants apply unchanged — especially **I3** (data is truth; animation is fitted to data), **I2** (all timing in ms of `ISimClock`), **I4** (per-attack timing), **I6** (timestamped intents in). Numbers here are reference placeholders (**I10**) — every window/duration is playtest-open.

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

`AttackDef` timing block (M1 — replaces `1c`'s 3 fields; all ms on the sim clock, I2/I4):

```csharp
[Header("Timing (ms, sim clock) — I3/I4: THIS is the truth; anim is fitted to it")]
public int StartupMs;    // pre-tell wind-in (not yet readable)
public int TelegraphMs;  // readable cue
public int CommitMs;     // committed travel
public int ActiveMs;     // hitbox-live WINDOW (damage resolves inside)
public int RecoverMs;    // exposed tail
```

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

## 3. Defensive windows — flags, authored as % of phase

Each allowed defense is a window expressed as a **phase + percentage range** (resolution-independent — it scales automatically when phase durations are tuned). All resolve against the **Active** window:

| Flag | Opens | Closes | Requires (§2) |
|---|---|---|---|
| `Dodgeable` | Commit start | 11% into Active | `Dodge` allowed |
| `Parriable` | 70% of Commit | Active start (0%) | `Parry` allowed |
| `PerfectParry` | Active start | 11% into Active | `Parry` allowed |
| `Jumpable` *(future)* | Commit start | 11% into Active | `Jump` allowed |

(Percentages are reference, I10.) Authored in a global `DefenseTuningDef` with optional per-attack overrides (`1c` §9.2). During the first 11% of Active, `Dodgeable` + `PerfectParry` overlap — the **button the player pressed** decides which applies.

---

## 4. The `ReactionResolver` — realizes `1c` §9.1

Slots exactly where `1c` specified: **between the Active-window-open edge and `HealthSystem.ApplyDamage`** in the tick. At the defender's timestamped input (I6):

```
outcome =
  if   inputType ∉ attack.AllowedDefenses          → Hit       (defense not allowed at all)
  elif the matching flag-window is closed @ input.ts → Hit      (mistimed)
  elif PerfectParry window open                    → Perfect
  elif Parriable    window open                    → Parried
  elif Dodgeable    window open                    → Dodged
  else                                              → Hit
```

`ReactionOutcome { Perfect, Parried, Dodged, Hit }` (+ `FeintWhiff`, §6). The resolver returns it; `ApplyDamage` plus each host's economy consume it (`1c` §9.1, `1a` seam #1: G1 = purge/counter · LS = AP/crit/Mani-drop).

**Laws:**

- **Instant flag check — no i-frames, no input buffer** (`1c` §9.7: parry is never buffered). The defensive input is graded at its press timestamp against the live flag.
- **Window-width law:** because the check is instant, every authored window MUST be wide enough for human reaction + render/input latency (a few frames minimum). The fairness validator (`1a` §7) audits this.
- **Defensive-recovery (knob, default OFF):** a *whiffed* parry/dodge MAY incur a few frames where the defender can't re-defend. This is the only thing that gives a pure **Cancel feint** (§6) teeth. Turn it up only if playtest shows fakes feel toothless. NOTE: this is the **opposite** of i-frames — *vulnerability*-on-whiff, not invulnerability.

---

## 5. Animation fitting (realizes `1c` §9.6 / I3)

Animation never carries gameplay timing. Kimodo generates **natural** motion that carries **phase-boundary markers**; at runtime Unity **time-warps each phase-segment** to the `AttackDef` ms, **pinning the visual impact onto the Active window**. Re-tune the ms → the visual re-syncs with no regeneration; regenerate the clip → no gameplay drift. (Exactly I3 + `1c` §12 decision D-c1.)

---

## 6. Feints & attack-strings (the "fake attacks")

A multi-attack flow is **data**, not a special case (`1a` §5 reserved `StringDef`):

```csharp
public sealed class AttackStringDef : ScriptableObject
{
    public AttackDef[] Steps;     // an ordered combo the enemy can flow through
    public float ChainFromPct;    // point in a step where the NEXT step may launch,
                                  // CONTINUING FROM THE CURRENT POSE (never an idle reset)
    public bool  CanCancel;       // optional: abort to guard instead of chaining
    public float CancelAtPct;     // where the cancel may trigger (must be before Active)
}
```

- **Chain (combo continuation)** — the enemy flows from mid-attack straight into the next step **from the current pose, with no idle reset** → relentless pressure, less defender breathing room, "alive" aggression. **NOT a read-beating mixup:** defense is telegraphed per step (each step carries its own §2/§3 flags), so the defender simply re-reads each. The value is **flow + sustained reaction demand**, not deception.
- **Cancel (the pure fake)** — play the shared Startup+Telegraph (indistinguishable from the real attack), then abort to guard **before** Active. Baits a panicked defense; its teeth come **only** from the defensive-recovery knob (§4). Optional — keep at zero teeth (pure mind-game / spacing tool) or turn the knob up.

The enemy AI decides at the branch point whether to continue the step, chain the next, or cancel.

---

## 7. Animation development & generation

Two rules drive everything: **(a)** indistinguishable fakes **share the real telegraph**; **(b)** chains **continue from the current pose**, never idle.

1. **Per attack** — Kimodo generates **one natural clip** (Startup → Recover). Auto-detect the impact frame (peak attacking-limb velocity from the NPZ) → place the **Active marker** + phase markers + a **branch marker** (the latest frame still identical to a fake).
2. **Authored combo chains** — generate the *whole string* as **one continuous Kimodo multi-prompt clip**: one prompt per step, with **`num_transition_frames > 0` between them**. Kimodo's transition-frames blend each attack into the next → fluid, no idle reset. Phase markers re-segment it into per-step Active windows + flags. *(Multi-prompt is used to link **whole attacks**, NOT to force tiny per-phase segments — the latter was rejected as stiff.)*
3. **Dynamic chains** (AI picks the next step live) — each step is a standalone clip; Unity **cross-blends from the current pose** into the next step's clip at `ChainFromPct`. Slightly less fluid than authored, fully flexible.
4. **Cancel / fake** — reuse the real clip's shared telegraph, then **blend to a short "pull back to guard" clip** at `CancelAtPct`. Never generate a separate fake telegraph (that would make it readable).
5. **Time-warp** (§5) applies per step regardless; transition/link frames live in the seam and carry **no** gameplay timing.

**Kimodo path change (the one code extension):** `mcp_control.animate` / `KimodoBackend` / `kimodo_app.py` accept a **list of prompts + per-step frame counts + a transition-frames value** (the API already takes lists — small change) and emit step-boundary markers. Everything else is Unity Animator wiring.

**Workload:** signature combo = **1** multi-prompt generation (not N stitched clips); dynamic attack = **1** clip (chaining via runtime blends, no per-combination cost); fake = **+1** short cancel clip (telegraph shared).

---

## 8. Decisions recorded (this pass)

- **D-d1** — **5-phase timeline**; Impact becomes an **Active window** (was instant); Startup split off the front.
- **D-d2** — **Defensibility flags** (`AllowedDefenses` = `Parry|Dodge|Jump`; default `Parry|Dodge`) gate which windows exist; supersede the reserved `ReadType`. **Jump reserved** for heavy sweeps.
- **D-d3** — Defensive windows = **%-of-phase flags**, resolved against Active; **instant flag check, no i-frames / no buffer**; window-width is a fairness law.
- **D-d4** — **Chain = combo continuation from the current pose** (pressure/flow), explicitly **NOT a mixup**; defense stays per-attack telegraphed.
- **D-d5** — **Cancel feint = optional**; teeth come from the **defensive-recovery knob** (default off; the opposite of i-frames).
- **D-d6** — Animation = **natural Kimodo + markers + per-segment time-warp pinned to Active** (I3); authored chains = one multi-prompt clip w/ transition frames; dynamic chains = runtime pose-blend; fakes share the telegraph.

---

## 9. Deferred / playtest-open (I10)

All window percentages + phase ms · the defensive-recovery value · the **Jump** mechanic's implementation · the AI branch policy (when to chain vs cancel) · whether Active is a single window or supports multi-hit · the scrub-to-author tool that writes phase ms into `AttackDef`s from an anim preview (`1c` §9.3).
