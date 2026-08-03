# Prompting & Motion Quality (Kimodo)

> How to get high-quality motion out of Kimodo, and how to tune the knobs. Sourced from NVIDIA's official Kimodo docs (best-practices, configuration, model zoo) + our own pipeline experience. Companion to [[01 AssetForge]]. Last updated 2026-06-14.

> **⚠ Status note (2026-07-31): Kimodo is deprecated for Last Rite combat animation** (quality — replaced by the purchased Mega Animation Pack v1.8 pipeline; see [[1h Last Rite - Player Moveset & Animation Plan]] + [[1f Last Rite - Combat Iteration Log]]). This doc is **retained for reference/other uses** — the prompting guidance still applies wherever Kimodo is used outside Last Rite combat.

## The one mental model: generation vs retarget

Two *separate* classes of problem — different fixes. Diagnose which one you have before tuning:

| Symptom | Class | Fix lives in |
|---|---|---|
| Wrong/random motion, scarecrow idle, doesn't match the prompt | **Generation** | prompt + settings + constraints (this doc) |
| Motion is right but breaks on our character — hands don't reach the ground, foot-skate, self-intersection | **Retarget** | retarget-time foot/hand IK (see [[01 AssetForge]]), NOT a Kimodo setting |

A pinned hand can't fix a short-limbed rig that physically can't reach — that's retarget. A drifting, intent-less clip can't be fixed by IK — that's generation.

## Settings that matter (baked into `serve_kimodo.py`)

| Setting | Use | Why |
|---|---|---|
| **Model** | `kimodo-soma-rp-v1.1` | 700 h studio mocap (RP), newest point release, SOMA = our skeleton. RP > SEED (288 h public); G1 = robot; SMPL-X breaks our joint map. |
| **`num_denoising_steps`** | **150** (200 for hero clips) | DDIM step count (NOT cfg). Docs: 100–200 = higher quality. Local is free, so favour quality. |
| **`cfg_type` / `cfg_weight`** | `'separated'`, `[text, constraint]`, default `[2.0, 2.0]` | Classifier-free guidance: `final = uncond + w_text·(text−uncond) + w_constraint·(constraint−uncond)`. Raise text if it ignores the prompt; lower if stiff/over-cooked. Raise the constraint weight once authored poses are added. |
| **`post_processing`** | **True** | Foot-skate cleanup + constraint cleanup + floor snap. Keep on, especially for retargeting. |
| **`num_samples`** | **3–4, then curate** | Kimodo is stochastic — best-of-4 is markedly better than one. Local = free, so this is the single biggest quality lever. |
| **`num_frames`** | match the real motion length | Don't pad (esp. idles) → less drift. ~30 fps; **max 10 s per prompt**. |

## Prompt format (the Llama-3 text encoder)

- **Start every prompt with `"A person…"`** — the training format. `"A person performs a heavy overhead slam."`
- **One or two behaviours per prompt**, **medium granularity.** Too vague (`"A person walks."`) and too detailed (per-body-part choreography) both fail.
- **Combat is IN-DOMAIN.** Kimodo is trained on locomotion, gestures, activities, object interactions, **combat**, dancing, and emotional/age styles. (Out-of-domain example to avoid: niche sports like baseball.) Lean into combat verbs.
- **Stylize for feel:** intensity/persona descriptors shape motion quality — `"aggressively"`, `"heavily"`, `"an exhausted person"`, `"an old person"`.
- **Multi-prompt for combos:** split a sequence into self-contained prompts (each must stand alone — vague follow-ups like `"then they stop"` fail). Caveat: transitions snap at the *second* prompt's start, so keep segments distinct.

### Examples (combat, our use case)

- `A person performs a heavy two-handed overhead slam and recovers.`
- `A person lunges forward with a quick claw swipe.`
- `A person staggers backward after taking a hit.`
- `An aggressive creature does a wide low sweep along the ground.`

## Constraints (the "director" lever — planned in our pipeline)

When text alone won't land it, pin keyframes. Confirmed classes in the package:
- **`FullBodyConstraintSet`** — full pose at chosen frames (the apex / contact pose).
- **`EndEffectorConstraintSet`** (+ `LeftHand/RightHand/LeftFoot/RightFoot`) — pin just hands/feet positions+rotations at a frame.
- **`Root2DConstraintSet`** — 2D path / waypoints for root travel.

Rules: **sparse — < 20 keyframes per type**; **never contradict the prompt or each other** (conflicts cause artifacts or get silently ignored); enable `post_processing` when contact accuracy matters.

**Why we want this:** authoring the mid (Commit apex) + contact poses up front (a) fixes weak clips — e.g. one relaxed-stance pose constraint kills the arms-out scarecrow idle — and (b) **bakes the gameplay beat in**: `contact frame_index = round(hit% × num_frames)` ties directly to the 5-phase attack timeline, so runtime time-warp is gentle (less foot-skate / slow-mo mush).

## Known failure modes

- **Subtle / near-static idles** — Kimodo's weak spot (T-pose neutral bleeds through → scarecrow). Fix with a pose constraint, or hand-author idles.
- **Fine hand / finger motion** — not modelled well.
- **Over-long or per-body-part prompts** — blur the intent.
- **Conflicting constraints** — artifacts or ignored.
- **> 10 s per prompt** — not supported; split.

## Per-clip workflow

1. Pick the right `num_frames` for the motion.
2. Write an `"A person…"` prompt (combat vocab + a feel word).
3. Generate **3–4 samples**; curate the cleanest (least foot-skate, best pose).
4. If a key pose is wrong/missing → add a sparse pose/end-effector constraint and regenerate.
5. Retarget; if hands/feet don't honour the ground → that's IK ([[01 AssetForge]]), not the prompt.
6. Judge attacks in motion (play/scrub), never on a paused frame.

See [[01 AssetForge]] · [[00 Tools Hub]] · [[Production Cost Ledger]].
