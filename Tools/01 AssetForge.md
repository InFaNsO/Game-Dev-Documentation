# AssetForge

> Our in-house asset pipeline: it turns a concept into a rigged, animated, Unity-ready character by orchestrating Meshy (geometry) + Kimodo (motion) inside Blender, and exposes every stage as MCP tools so Claude can drive it. Part of [[00 Tools Hub]]. Last updated 2026-06-14.

> **⚠ Status note (2026-07-31): Kimodo is deprecated for Last Rite combat animation** — output quality judged unacceptable by the developer; combat clips now come from the purchased **Mega Animation Pack v1.8** (Unity Asset Store, Humanoid retarget → Blender fix/re-time pass) instead — see [[1h Last Rite - Player Moveset & Animation Plan]] + [[1f Last Rite - Combat Iteration Log]]. The Meshy stages (mesh/retopo/rig/export) are unaffected. The Kimodo sections below are **retained for reference/other uses** (non-combat motion, experiments).

## Repo & layout

- **Repo:** `InFaNsO/AssetForge` (renamed 2026-06-14 from `Auto-Asset-Development`).
- **Mounted as a submodule** of the superproject at **`tools/assetforge`**.
- Key dirs:
  - `core/` — backend-agnostic pipeline logic (stages, adapters, backends for Meshy/Kimodo/etc.).
  - `blender_addon/` — the Blender-side addon: `mcp_control.py` (verbs Claude calls), `handlers.py` (slotted-action auto-bind), geometry/anim utilities.
  - `mcp_server/` — the thin MCP server (`server.py`) exposing `af_*` tools; talks to the Blender addon socket (port 9876). Launched via `uv run`.
  - `local/serve_kimodo.py` — **on-prem Kimodo server** (see below).
  - `modal/kimodo_app.py` — the Modal cloud Kimodo deployment (fallback).

## The pipeline (13 stages)

`concept · blockout · generate · retopo · uv · bake · texture · rig · animate · lod · collision · export · validate`

Driven via MCP `af_*` tools: `af_setup`, `af_status`, `af_run_stage`, `af_generate`, `af_generate_lods`, `af_animate`, `af_import_mesh`, `af_export_unity`, the async `af_start`/`af_poll`, and the bulk-animation `af_animate_batch_start`/`_poll`/`_apply`. (New MCP tools require a Claude restart to surface; the underlying `mcp_control` verbs can be driven immediately via `execute_blender_code`.)

## Backends

- **Meshy** — generation, remesh→LOD, auto-rig. Produces a consistent **24-bone Mixamo skeleton** (Hips, Spine/Spine01/Spine02, shoulders/arms/forearms/hands, legs/feet/toes, neck/head). Cost in credits.
- **Kimodo** (NVIDIA text→motion) — novel motion from a text prompt. Outputs an **NPZ** on the **SOMA skeleton** (77 joints; SMPL-X-compatible for j<24). See [[02 Prompting & Motion Quality]] for how to get good results.

## Kimodo: local (default) + Modal (fallback)

Both expose the **same HTTP contract**, so the backend is a one-variable swap:

```
POST /generate  {"prompt": "...", "num_frames": 196}  -> NPZ bytes
GET  /health                                            -> {"ok": true, ...}
```

The client (`core/backends/kimodo/kimodo.py`) defaults to `http://localhost:9551`; `ASSETFORGE_KIMODO_URL` overrides it. On the local path it reads the NPZ straight from the POST; on Modal it follows the 303-poll pattern.

### Local server — `local/serve_kimodo.py` (the default)

- **Hardware:** RTX 4080 (16 GB) + 88 GB RAM. **Motion model runs on the GPU; the Llama-3 text encoder runs on CPU** (`TEXT_ENCODER_DEVICE=cpu`) so the full-precision pipeline fits 16 GB (full-GPU would need ~17 GB). This is higher fidelity than the community "6 GB" quantized-encoder hack, which we don't need.
- **Checkpoint:** **`kimodo-soma-rp-v1.1`** — the 700 h "Rigplay" studio-mocap SOMA model (the official recommended default; SOMA matches our retarget). RP > SEED (288 h public, research-comparison only); G1 = robot skeleton; SMPL-X would break our SOMA joint map.
- **Quality defaults baked in:** 150 denoising steps, `cfg_type='separated'` `cfg_weight=[2.0,2.0]`, `post_processing=True`.
- **Cost:** free + instant after warmup. First generation downloads ~16 GB (Llama-3 + Kimodo weights) to the HF cache.

#### Setup / run

```
conda create -n kimodo python=3.10           # one-time
conda run -n kimodo pip install torch --index-url https://download.pytorch.org/whl/cu124
conda run -n kimodo pip install git+https://github.com/nv-tlabs/kimodo.git   # needs CMake + MSVC (MotionCorrection C++ ext)
conda run -n kimodo pip install fastapi "uvicorn[standard]" pydantic
hf auth login                                 # one-time; gated Llama-3 access (same token as the Modal secret)
conda run -n kimodo python tools/assetforge/assetforge/local/serve_kimodo.py
```

Then point AssetForge at it: leave `ASSETFORGE_KIMODO_URL` unset (defaults to `localhost:9551`) or set it explicitly.

### Modal fallback

Cloud A10G deployment (`modal/kimodo_app.py`). URL: `https://bhavilg101--last-rite-kimodo-kimodo-api.modal.run`. ~$0.02/clip when batched (cold start dominates; batch everything in one session). Use only if the local GPU is unavailable. Deploy: `modal deploy modal/kimodo_app.py`.

## Retarget (Kimodo NPZ → Blender rig)

Kimodo outputs SOMA local rotations; copying them raw onto a Meshy/Mixamo rig distorts the pose (different rest orientations). AssetForge re-expresses each rotation in the target bone's rest frame:

```
basis[b] = A_b⁻¹ · (W · R_soma_local[b] · W⁻¹) · A_b      (W = Rx(−90°), empirically validated)
```

`npz_to_blender_action()` in `core/backends/kimodo/kimodo.py` does this for the 22 mapped body joints. Root translation is off by default (in-place combat clips).

**Roadmap (separate from generation quality):** the realistic→chibi proportional gap (hands not reaching ground, foot-skate) is a **retarget-time IK** problem — pin feet using the NPZ's `foot_contacts`, pin hands for ground-contact clips. See [[02 Prompting & Motion Quality]] for the generation-vs-retarget split.

## Bulk generation

`af_animate_batch_*` (and the `mcp_control` `start_batch`/`poll_batch`/`apply_batch` verbs) generate many clips in one warm session. Each clip: `{name, motion_prompt, num_frames, playback}`. With Modal this amortized cold-start (batch ≈ cost of one); with the local server it's simply free.

**Pose-authoring (planned):** add `constraints` per clip (pose keyframes / end-effector pins) so Kimodo hits authored mid/contact poses — bakes the gameplay-beat timing in and fixes weak clips (e.g. idles). Constraint classes confirmed in the package (`FullBodyConstraintSet`, `EndEffectorConstraintSet`, `Root2DConstraintSet`); keep < 20 keyframes/type, never contradict the prompt.

## Gotchas (hard-won)

- **Blender 4.4+ slotted actions:** switching an action in the Action Editor leaves the *old* slot bound → rig appears frozen. `handlers.py::af_autobind_slots` rebinds whenever the bound slot doesn't belong to the active action.
- **glTF bone display:** the glTF importer assigns absurd bone tail lengths (spiky-star skeleton). `fix_armature_bone_display()` shortens each bone along its direction to its child joint (display only; skinning unchanged). Runs after every rigged-GLB import.
- **Meshy rig 0.01 scale:** don't hand-pose (explodes) or rescale the armature alone (desyncs from mesh); validate with real clips at native scale — export applies scale correctly.
- **Attacks look broken when paused:** a one-shot attack clip frozen on one frame looks contorted — press play/scrub to judge it.
- **MotionCorrection build:** the kimodo pip install builds a C++ extension — needs CMake (+ MSVC, already present).

See [[02 Prompting & Motion Quality]] · [[Production Cost Ledger]] · [[00 Tools Hub]].
