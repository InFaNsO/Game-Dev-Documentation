# Production Cost Ledger

> Running record of what asset/animation generation costs. **Modal (Kimodo motion) → USD; Meshy (mesh/rig/retopo) → credits.** Figures are **estimates** computed at generation time — the authoritative numbers are your **Modal dashboard (Usage)** for USD and your **Meshy dashboard** for the credit balance. Claude appends a row here every time something is generated.

## Rates & method
- **Modal A10G** = **$0.000306 / sec** (~$1.10/hr), from modal.com/pricing (A10G bills at the A10 tier). Cost = **(active GPU seconds + keep-warm seconds) × rate**.
- **Keep-warm caveat (important):** the Kimodo container stays alive **600 s (10 min)** after each call (`scaledown_window`). That idle time is billed. So a *standalone* generation pays the full 10-min keep-warm, but **batching many generations in one session amortizes it** — each extra clip costs only its inference (~$0.035), not the full ~$0.22.
- **Meshy** = per the backend `cost_estimate` (Remesh 2 credits, Rigging 5 credits, Generation/Texture vary). Credit→USD depends on your Meshy plan/credit-pack price.

---

## Modal — Kimodo motion (USD)

| Date | Game | Asset | Operation | GPU | Active s | Keep-warm s | Est. USD | Notes |
|---|---|---|---|---|---|---|---|---|
| 2026-06-13 | Last Rite | Amphibian Husk | Idle (Kimodo gen) | A10G | 114 | 600 | **$0.22** | cold start; pose poor (Kimodo weak at idles) — will hand-author |
| 2026-06-13 | Last Rite | Amphibian Husk | **Bulk anim ×11** (Kimodo batch) | A10G | 129 | 600 | **$0.22** | 1 cold start (~96s) + ~3s/clip warm → **~$0.02/clip**; batching saved ~$2.20 vs one-at-a-time. Clips: LightClaw, HeavySlam, Lunge, HitReaction, Stagger, FeintPullback, CrystalThrust, ClawCross, AggressiveIdle, GroundSweep, CrystalBolt |

**Modal subtotal (est.): ~$0.44**

> **Key cost insight (measured 2026-06-13):** Kimodo's *inference* is only **~3 s** (motion diffusion = tiny tensors); the ~100 s we kept seeing is the container **cold start** (spin-up + model load). Cold-start + the 10-min keep-warm are paid **once per warm session** — so **a batch of 11 costs about the same as a single clip**. Always batch generations in one sitting.

---

## Meshy — mesh / rig / retopo (credits)

| Date | Game | Asset | Operation | Est. credits | Notes |
|---|---|---|---|---|---|
| 2026-06-13 | Last Rite | Amphibian Husk | Remesh → ~75k LOD0 (retopo, textured) | ~2 | input_task_id path, no re-upload |
| 2026-06-13 | Last Rite | Amphibian Husk | Auto-rig (Mixamo skeleton) | ~5 | bundled walk + run clips included |

**Meshy subtotal (est.): ~7 credits**

---

## Notes
- This ledger starts **2026-06-13**; the original high-poly Meshy *generation* (prior session) is not tracked here.
- Reconcile periodically against the Modal/Meshy dashboards and correct the estimates if they drift.
