# Game 1 "Last Rite" — Code Architecture & Abstraction Map

> **The engineering blueprint for Game 1 — written BEFORE the scaffold (2026-06-10).** Companion to [[1 Parry Combat - Last Rite]] (the design; all D1–D8 locks). Pattern source: the Tier-0 *patterns* (assemblies · service locator · typed event bus · SO data layer · top-level state machine · build order) — its LS-specific machinery (grid/cover/Mani/faction/inventory) is NOT built here. *(The older top-level "08 Unity Code Architecture" doc that first captured these patterns was removed 2026-06-13, pending main-game work; this doc is now the authoritative Tier-0 source.)* **This doc is the concrete v1 of the Tier-0 foundation** — the first real instance of the shared architecture every later game inherits.

**Target:** Unity 6 (6000.3.6f1) · C# · URP · Input System · Cinemachine 3. Single-player, single project.

---

## ⚑ AS-BUILT — Combat Prototype (updated 2026-07-04)

> Everything from §0 down is the **north-star blueprint** (pure-C# deterministic sim, asmdef extraction story, camera director, etc.), written before any code. The **combat prototype actually shipped a different, MonoBehaviour-driven, animation-integrated architecture** — a deliberate solo-dev pivot toward "Unity-only, no pure-C# sim." This section is the source of truth for *what exists today*; the blueprint below stays the aspiration for later hardening/extraction.

**Key divergences from the blueprint**
- **No pure-C# sim, no `ISimClock`/`IRng`/`IEventBus`, no asmdefs, no `Assets/Code/`, no EditMode tests.** All game code is Unity types (`MonoBehaviour` / `ScriptableObject`) in a single `Assembly-CSharp`, under `Assets/Scripts/`.
- **The animation IS the clock.** Attack windows are authored in **frames** on `SOAttackDef` (`TimeWindow{StartFrame,EndFrame}`), and the live attack phase + defense-validity are read from the **Animator's current playback frame** (`AnimController.GetCurrentAnimationFrame()` off `AnimatorStateInfo.normalizedTime`) — not a separate ms clock. Data-is-truth survives (timing lives in the SO), but sim and presentation share one timeline.
- **Turn flow is event + `StateMachineBehaviour` driven**, not a scheduler over a sim loop.

**As-built module map** (`Assets/Scripts/`)

| Namespace / file | Role |
|---|---|
| `BGamer.Core/GameManager` | singleton service locator (`Register<T>` / `Get<T>`, `[DefaultExecutionOrder(-200)]`) |
| `BGamer.Core/ISystem`, `IGameEntity` | interfaces with **default methods** for entity↔system registration; `IGameEntity` also carries `Team Side`. `enum Team { None, Player, FriendlyNPC, AggressiveNPC }` |
| `BGamer.Core/AnimController` | data-driven animator wrapper: string-key → `List<AnimTransitionData>` (param/trigger sets); `Play(name)`, `GetCurrentAnimationFrame()`, `GetActiveAnimName()` |
| `BGamer.Combat/CombatSystem` | `ISystem`; turn loop (`StartCombat` picks the `AggressiveNPC`, `NextTurn` alternates aggressor by side); self-registers with `GameManager` in `Awake` (`[DefaultExecutionOrder(-100)]`); events `OnEntityRegistered/OnResolved/OnTurnComplete/OnAttackPhaseChanged`; routes defense via `CombatEntity.GetDefenseResult` |
| `BGamer.Combat/CombatEntity` | `IGameEntity`; owns per-swing state (`ActiveAttack/ActiveTarget/ActiveAttackPhase`), picks its own move + target, drives `AnimController.Play`, single-writer `ApplyDamage`; events `OnDefensePerformed/OnHealthChanged/OnDied/OnAttackEnd`; player input LMB=dodge / RMB=parry |
| `BGamer.Combat/SOAttackDef` | attack data: frame windows (Startup/Telegraph/Commit/Hit/Recover + Parry/PerfectParry/Dodge), Damage, `AllowedDefences`, `AttackPhase PhaseAt(frame)`, per-attack `AnimTransitionData` |
| `BGamer.Combat/SOFighterDef` | Name, MaxHP, `Moveset` (`List<SOAttackDef>`), `AnimData` (non-attack anim map: idle/hit/parried/death/dodge) |
| `BGamer.Combat/DebugCombatHUD` | IMGUI HUD — HP bars (event-driven) + attack-window box (polled from the live attacker) |
| `LastRite/AttackStateBehaviour` | `StateMachineBehaviour` on each attack state → on state-exit fires `CombatEntity.AttackAnimEnded → OnAttackEnd → CombatSystem.NextTurn` |

**Prototype behaviour (done):** a real-character 1v1 duel (extensible to #-vs-#): fighters alternate turns, each auto-selects a random moveset attack + an enemy target, plays the attack anim, and applies damage at the **Hit** frame; the player defends by timing **RMB parry / LMB dodge** against the boss's live animation-frame windows; `DebugCombatHUD` shows both HP bars and the colour-coded current attack window. Characters (Purifier, CinderScale) are rigged Humanoid with grounded Mixamo anims; consolidated single-file FBX (`Purifier_Full.fbx`, `CinderScale_Full.fbx`) exist as portable character assets.

**Still to build (blueprint items not yet real):** the reaction→economy resolver (parry currently only negates the swing; purge/counter unbuilt), camera director / smart action-cam, purge meter + purification finisher, run/rebirth spine, sanity/narration, ranking/gauntlet, the guardian roster, the asmdef split + `BGamer.*` extraction, and any automated tests.

---

## 0. The abstraction rule (read this first)

**A seam goes exactly where a future game swaps something out — nowhere else.**

For every system we ask: *what stays identical in Game 2 (Tactical Combat), the Looter Shooter, and the dream game?* That part lives **below the seam** in a `BGamer.*` assembly (future UPM package). *What is Last-Rite-specific policy?* That part lives **above the seam** in `LastRite.*` and ships with this game only.

Three corollaries:

1. **Mechanics below, economies above.** The reactive layer (timelines, windows, outcomes) is portfolio tech. What an outcome *pays out* (purge here; AP + crit + Mani-drop in the LS) is host-game policy. `BGamer.CombatReactive` emits **outcomes**; each game maps outcomes → its own economy.
2. **Data is the truth; presentation is a skin.** Combat runs as a pure-C# sim on a millisecond clock. Animation, VFX, camera, UI, audio *mirror* the sim via events and never feed back into it. (Same philosophy as the automation pillar's "sim = source of truth, VFX = cosmetic skin.")
3. **No speculative seams.** No grid substrate (Game 2 adds it), no inventory/factions/Mani economy (Game 3+/LS), no DI container, no ECS, no Addressables in v1 (a catalog seam keeps the door open), no localization framework (string-ID-keyed text keeps the door open). If no future game swaps it, it's concrete.

---

## 1. Assembly & folder map (the extraction story)

```
LastRite.UI ──► LastRite.Game ──► BGamer.CombatReactive ──► BGamer.Core
                      │                                          ▲
                      └──────────────────────────────────────────┘

(+ mirrored *.Editor assemblies; + LastRite.Tests / BGamer.Tests editmode asms)
```

| Assembly                  | Becomes                                                      | Contents                                                                                                                                                                                             | May reference          |
| ------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| **BGamer.Core**           | `com.bgamer.core` (extracted at Game 2)                      | Bootstrap + service locator · typed event bus · FSM framework · sim clock + feel (hitstop) · input · save · data conventions · camera director · UI framework · audio · RNG · pooling · scene loader | Unity only             |
| **BGamer.CombatReactive** | `com.bgamer.combat-reactive` (extracted at Game 2 or the LS) | Attack-as-data model · timeline runner · reaction resolver · fighter combat state · projectile sim · damage-step chain · schedulers · combat events                                                  | Core                   |
| **LastRite.Game**         | ships in this repo forever                                   | Game states · player kit FSM · purge/finisher · run/rebirth director · explore mode · sanity · narration · ranking/gauntlet · guardian content bindings                                              | CombatReactive, Core   |
| **LastRite.UI**           | ships in this repo                                           | HUD + screens (MVP-lite), event-fed                                                                                                                                                                  | Game (read-only), Core |

**Extraction = `git mv` + a UPM manifest, zero code change.** The asmdefs enforce from day one that `BGamer.*` never references `LastRite.*` — extraction-readiness is structural, not aspirational. Game 1 ships with the code in-repo (its repo pins it forever — ship-everything principle satisfied); packages are born when Game 2 first consumes them.

```
Assets/
  _Project/            Art/  Audio/  Prefabs/  Scenes/  VFX/
  Code/
    BGamer.Core/           BGamer.Core.Editor/
    BGamer.CombatReactive/ BGamer.CombatReactive.Editor/
    LastRite.Game/         LastRite.UI/   LastRite.Editor/
    Tests/
  Data/                (SO assets: Attacks/ Movesets/ Guardians/ Descent/ RebirthTiers/ Narration/ Ranking/ Shots/ Audio/)
  Settings/            (URP, Input Actions, mixers)
```

---

## 2. BGamer.Core — the Tier-0 services

Template per system: **Job · Seam (the abstraction + where it sits) · Game 1 concrete · Absorbs later.**

### 2.1 Bootstrap & service locator
- **Job:** one composition root; everything reachable, nothing static-coupled.
- **Seam:** `Services.Register<T>/Get<T>` over interfaces (`IEventBus`, `ISaveService`, …). The *interface set* is the seam — each game's `GameBootstrapper` (in the game assembly) decides the concrete registrations. Core ships the locator + interfaces; games ship the composition root.
- **Game 1:** a `Boot` scene with `LastRiteBootstrapper` registering ~12 services, then `ChangeTo<TitleState>`.
- **Later:** Game 2 registers grid services alongside; nothing in Core changes. (No DI framework — the locator is the 20-line tool that's enough.)

### 2.2 Typed event bus
- **Job:** the decoupling spine. Systems publish immutable structs; presentation subscribes.
- **Seam:** `IEventBus` (Publish/Subscribe/Unsubscribe). Event *types* live beside their owning system (combat events in CombatReactive, run events in LastRite.Game) — the bus is generic plumbing.
- **Game 1:** sync dispatch, no queuing (single-threaded sim; revisit never, probably).
- **Later:** identical everywhere; the LS adds its own event types, zero bus changes.

### 2.3 FSM framework + top-level game states
- **Job:** one state-machine utility reused at three scales: game flow, player combat FSM, enemy agent FSM.
- **Seam:** Core ships `StateMachine<TContext>` + `IState` (Enter/Exit/Tick). The *states themselves* are game content, always in the game assembly.
- **Game 1 flow:** `Boot → Title → Descent(Explore ⇄ Duel) → RebirthTransition → … → TrueEnding → Credits`. **Explore ⇄ Duel is a state + camera/UI swap, NOT a scene load** — duels happen in-place in the room (stationary duels need no arena transition; the LS's Encounter-Arena transition is ITS policy, not ours).
- **Later:** the LS plugs its Explore/Combat/Arena states into the same framework.

### 2.4 Sim clock + feel service (THE feel-critical service)
- **Job:** combat truth runs on a **millisecond sim clock**, not frames. Hitstop, slow-mo, and pause are *clock operations*, so reaction windows can never desync from presentation.
- **Seam:** `ISimClock` (Now-ms, scaled dt, pause) + `IFeelService` (`Hitstop(ms, curve)`, `RequestTimescale(…)`, shake → camera director). All combat timing reads the sim clock; input timestamps are captured in sim-time. Hitstop freezes timelines AND input-time together — windows stay fair by construction.
- **Game 1:** hitstop on every parry (small for Block, fat for Perfect), slow-mo pulse on purge-break and finisher.
- **Later:** identical in the LS reaction-cam windows; D8-style windows stay ms-specified (frame counts were ~60fps references only).

### 2.5 Input
- **Job:** wrap the Input System; deliver *timestamped intents*, not raw polling.
- **Seam:** `IInputService` exposing semantic actions (`Strike`, `Parry`, `Dodge`, `Lock`, `Interact`, …) with sim-time stamps + a buffering utility. **Policy lives with the consumer:** Strike/Dodge are buffered (~150 ms); **Parry is NEVER buffered** — press-time graded (the whole skill expression).
- **Game 1:** keyboard+mouse and pad, rebinding deferred (Input System makes it cheap later).
- **Later:** Game 2 adds grid-cursor actions; same service.

### 2.6 Save system
- **Job:** save-safe by construction.
- **Seam:** `ISaveService` (slots, JSON, version field + migration hook) + `ISaveable` registry (CaptureState/RestoreState, stable string keys). Content referenced **by string ID only** — never asset refs in saves.
- **Game 1:** two files per slot — `RunSave` (tier index, room index, cleared encounters, RNG seed) + `MetaSave` (best ranks, lore unlocked, techniques, gauntlet scores, true-ending flags) + settings. Checkpoint at room boundaries; death respawns at duel start regardless (diegetic).
- **Later:** the LS registers stash/reputation/etc. as more `ISaveable`s; the mechanism never changes.

### 2.7 Data conventions (Def/Instance + catalogs)
- **Job:** every tunable is a ScriptableObject `…Def` with a stable `Id`; runtime uses lightweight instances (flyweight).
- **Seam:** `IContentCatalog` — `Get<TDef>(id)` over registered catalog SOs. **This is the Addressables seam:** v1 backs it with direct-reference catalog assets; if a later game needs streaming, the catalog implementation swaps and no call-site changes.
- **Game 1:** one catalog per Def family (attacks, movesets, guardians, tiers, narration, ranking, shots).

### 2.8 Camera director (the smart action-cam lives here as framework + data)
- **Job:** one camera authority. Gameplay never touches cameras; it **requests shots**.
- **Seam:** `ICameraDirector.Request(ShotRequest) / Release(handle)` where `ShotRequest = {shotId, priority, anchors, holdPolicy}`; `ShotDef` SOs bind Cinemachine vcams + blend params. **Cinemachine types stay internal to this module** — the vocabulary is ours, the vendor is swappable.
- **Game 1 policy (the locked D4 smart action-cam), expressed purely as data + requests:**
  - `Shot.Walk` (explore follow) → `Shot.DuelNeutral` (auto-frames both fighters, baseline priority)
  - `Shot.TelegraphPush` — requested by the attack timeline at telegraph start (each `AttackDef` carries a shot hint), released at resolution → the push-to-OTS-and-back IS just priority arbitration
  - `Shot.Finisher` (top priority), `Shot.Heart`, sanity adds low-amplitude noise modifiers only.
- **Later — the proof this seam is right:** Game 2's two-mode rig (tactical ↔ OTS) is a different *request policy* over the same director; the LS reuses Game 2's. No reactive-layer code changes when the camera model changes — that's the D4 roadmap correction expressed structurally.

### 2.9 UI framework
- **Job:** MVP-lite. Views subscribe to events/read-models; no view ever mutates game state directly (input goes through intents).
- **Seam:** `IUiService` (screen stack push/pop, HUD show/hide) in Core; all concrete screens in `LastRite.UI`.
- **Game 1 screens:** Title/slots · HUD (player HP, enemy HP + **purge**, lock marker, technique prompt) · Pause · Duel Result (rank) · Lore Log · Gauntlet score · Settings.

### 2.10 Audio, RNG, pooling, scenes (brief)
- **Audio:** `IAudioService` — one-shots by ID (SO table), music layers, **mixer snapshots** (the sanity director's main lever). Unity mixer; no FMOD at this scope.
- **RNG:** `IRngService`, named seeded streams (`run`, `remix`, `presentation`). Remix selection is seeded per run → deterministic, replay-fair, and procgen-thread-friendly later.
- **Pooling:** generic pool for VFX/projectiles/floating text.
- **Scenes:** `ISceneLoader` (async + fade). Scene list: `Boot` (persistent), `Title`, `Stratum_01/02/03`. Rebirth = reload Stratum_01 with the next `RebirthTierDef` applied.

---

## 3. BGamer.CombatReactive — the crown jewel

The portfolio's #1 carry-forward artifact. Everything here is **host-game-agnostic**: it knows attacks, timelines, windows, and outcomes. It does NOT know purge, AP, Mani, grids, or cameras.

### 3.1 Attack-as-data (the content seam)
```
AttackDef (SO): id · ReadType (Parryable | Perilous | Ranged) · timing-ms
  {telegraphDur, impactOffset, recoveryDur} · damage packet · behaviorId
  · presentation refs (anim, telegraph cue: color/SFX) · camera shot hint
  · per-attack window overrides (else global ReactionTuningDef)
AttackStringDef: sequence of attacks + gaps + FEINT BRANCHES
  (cancel-points that re-route mid-string — the mix-up engine)
MovesetDef: weighted strings + policy params (aggression, punish-after-player-strike)
PhaseDef: HP/purge thresholds → moveset swap + transition beat
GuardianDef: visual loadout (rig accessories · palette · corruption-shader params)
  + phases + finisher ref + lore refs
RemixOverlayDef: per-rebirth deltas — add/remove strings, reweight, timing
  multipliers (CLAMPED by fairness floors), new feint branches
```
- **Seam:** content is 100% data; *new behaviors* (a new attack verb — e.g. a grab, a shockwave ring) plug in via a **behavior registry** (`behaviorId → IAttackBehavior` strategy). Data covers the expected space; the registry absorbs the unexpected.
- **The acceptance test:** *a new guardian ships with zero new C#* unless it introduces a genuinely new verb. The ~6 guardians + ~5 lessers are pure data + animation work.
- **Later:** these same `AttackDef`s/`GuardianDef`s become the LS named-Husk mini-bosses — re-bound into LS encounters, not re-authored. Remix overlays are exactly the "remixed movesets per rebirth" from D2.

### 3.2 Timeline runner + reaction resolver (the truth)
- **Job:** run `AttackTimeline` on the sim clock: Telegraph → Commit → Impact(t_I) → Recovery. Grade reactions **pure-functionally**: timeline × timestamped input attempts → outcome.
- **Window model (reference defaults in one global `ReactionTuningDef`, all playtest-open):** Block = press in [t_I − ~330ms, t_I] · Perfect = the last ~130ms of that · Dodge = i-frames [press+30, press+230] overlapping t_I · **parry whiff = ~250ms lockout** (the feint punisher — pressing at a fake tell leaves you open for the real impact; mashing dies here) · Perilous = parry attempts auto-fail, dodge only · Ranged = projectile sim with travel time; Block negates, Perfect flags `DeflectedBack`.
- **Seam (the deepest in the codebase):** the resolver is pure C#, unit-testable, MonoBehaviour-free, and **emits `ReactionOutcome` events only** (`{attackId, grade: Perfect|Block|Dodged|FeintBaited|Hit, deflected}`).
  **Host games map outcomes → economies:** Game 1 → purge + counter-window; LS → flat AP + crit-counter + Mani-shard drop. The reactive *feel* transfers; the *reward* is per-game policy. This is the single most load-bearing line in the architecture.

### 3.3 Scheduling vs resolution (the turn-based seam)
- **Job:** WHO attacks WHEN is separate from HOW attacks resolve.
- **Seam:** `IAttackScheduler` feeds `ScheduledAttack`s to the runner.
  - Game 1: `RealtimeDuelScheduler` — **token-based**: exactly 1 aggression token; in mixed fights only the token-holder attacks (attacks resolve one at a time — the locked readability rule), others posture at lock positions. The duel = an encounter with one agent.
  - The LS later: a `TurnBasedScheduler` — the enemy phase iterates units, each building the SAME timeline, resolved by the SAME resolver inside the reaction-cam window. **Turn-based vs real-time differs only in who schedules; resolution is identical. That's the carry-forward guarantee, provable by a unit test that drives the resolver from a fake turn-based scheduler.**
- **Game 1 "AI" honesty:** the duel director is an attack-string scheduler with policy knobs (aggression curve, punish window after player strikes, feint frequency by tier) — deliberately not a planner. CA0 on the AI thread ladder.

### 3.4 Damage chain + fighter state + projectiles
- **Damage:** the 08 chain-of-responsibility pattern, miniature: hosts register `IDamageStep`s (Game 1: OpeningMult → CounterCrit; LS later: Flank → Crit). Pipeline pattern carries; steps are per-game.
- **Fighter combat state:** Attacking / Recovering / **Staggered(duration)** / Dead lives HERE (schedulers must respect stagger); *what causes* stagger is host policy (Game 1: purge-break calls `Stagger()`).
- **Projectiles:** minimal travel-time sim + deflect state, pooled. (LS lock: never hitscan — same tech.)

### 3.5 Animation rule (production-critical)
**Data is truth: animation playback rate is fitted to the timeline, never vice-versa.** Remix timing-multipliers tighten a telegraph by scaling the anim — no re-authoring per rebirth tier. Telegraph cues (color flash + SFX at telegraph-start) come from the timeline, so fairness floors are enforceable in data.

---

## 4. LastRite.Game — the game above the seams

### 4.1 Player kit (FSM over CombatReactive)
- States: Locomotion(explore) · DuelStance · Strike · ParryAttempt · Dodge · CounterWindow · Staggered · FinisherExecution. Strike/Dodge buffered, Parry press-graded (§2.5).
- **Counter:** Perfect opens a counter window; pressing Strike inside = the crit counter (input, not auto — mastery game).
- **Techniques (≤2, D6):** `TechniqueDef {id, rebirth-milestone unlock, input, behaviorId}` — data + registry, no tree. Candidates (gray-box-open): perfect-dodge→counter, chain-parry stance.

### 4.2 Purge + finisher (host economy — deliberately NOT in CombatReactive)
- `PurgeSystem` subscribes outcomes: Perfect +big, Block +small, decay when idle (D5). Purge-break → `fighter.Stagger()` → `FinisherSystem` runs the purification micro-sequence (`FinisherDef`: shot + paired anim + corruption-shader purge-to-clean + VO hook). "Take my pain" — seeds the LS forced-purification.
- The LS will NOT take purge — it maps the same outcomes to AP/crit/Mani. Purge staying host-side is what makes that a re-skin instead of a rewrite.

### 4.3 Run / rebirth director (the roguelike spine)
- **Data:** `DescentDef` = ordered `RoomDef`s (encounters, warp-flag groups, lore spots, heart chamber). `RebirthTierDef` = **one SO per loop** holding: URP volume profile + global shader params (the palette-shift-as-HUD), remix overlay set, narration variant set, ruin warp flags, audio snapshot, escalation knobs, **`maniTheme` (None / Bhu / Jal / Vayu / Agni — the cycle's element, LOCKED 2026-06-11)**, `isTerminal` (the final tier routes to the true-ending sequence).
- **Seam:** the whole D2 loop ("full re-descent, remixed") is **data overlays applied at rebirth** — adding tier 6 or rebalancing tier 3 touches zero code. RunDirector publishes `RebirthStarted/RoomEntered/EncounterCleared`; save = its memento.
- **D1:** heart pickup #1 = the inciting rebirth — a scripted beat in the heart chamber's `RoomDef`.
- **Elemental spine (LOCKED 2026-06-11):** tiers = `None → Bhu → Jal → Vayu → Agni` (Agni = `isTerminal`). The element is an **overlay** — the tier's `RemixOverlayDef`s carry the element's attack flavor onto the base guardians (corruption flavor, never a re-skin). **Fairness law (enforced by the §5 validator):** element overlays may alter only *guardian* attack data + presentation — they never emit player-side status (no slow/freeze/push on the player; keeps D7 fairness-sacred inside a stationary duel).
- **Chaos Descent (v1-stretch / first content update, NOT core v1):** post-true-ending unlockable — RunDirector takes a runtime-assembled `DescentDef` + a randomly-selected element theme + seeded remix overlays (the `remix` RNG stream). Randomized *selection* of authored content, zero procgen, no new system — the bridge to the eventual full-procgen Endless mode.

### 4.4 Explore mode (deliberately thin)
Third-person walk (CharacterController), interactables (lore, doors, the heart), `Shot.Walk`. **No stealth, no loot, no sprint-jump platforming** — it's the breath between duels and the narration/sanity canvas. (LS explore is a different, richer system; nothing here pretends to be it.)

### 4.5 Sanity & narration (presentation-only, structurally)
- `SanityDirector`: maps rebirth tier + scripted beats → post-volume weights, audio snapshots, hallucination prefabs (pooled, cosmetic). `NarrationService`: line tables keyed by trigger ID with **per-tier degradation variants** (the D2 "narration degrades" lever).
- **The fairness guarantee is architectural:** both are pure event *subscribers*. They cannot write to the sim — there is no API surface for it. Telegraphs stay readable at every sanity level by construction, honoring "fairness is sacred" (D7).

### 4.6 Ranking & gauntlet
- `MetricsRecorder` (per-encounter event subscriber: hits taken, time, style events) → `RankResolver(RankingDef)` → grade → UI + MetaSave. Style weights are data.
- **The Rite gauntlet** = a `DescentDef` variant (all guardians, one healthbar, score) — reuses the entire run pipeline; near-free by construction, as the concept promised.

---

## 5. Editor & tests (fairness as tooling)
- **`AttackDef` fairness validator** (editor): telegraph ≥ floor per read type, windows within bounds, remix multipliers clamped — "fairness sacred" enforced at import time, not by discipline.
- Guardian completeness validator (phases wired, finisher present, string graphs acyclic).
- **EditMode unit tests:** reaction resolver grading (the ms-math), purge math, rank resolver, remix-clamp, and the fake-turn-based-scheduler test (§3.3).
- CI-checkable extraction rule: `BGamer.*` asmdefs have no `LastRite.*` references.

---

## 6. What we deliberately do NOT build (the guardrail list)
No grid substrate (Game 2) · no inventory/factions/vendor/Mani economy (Game 3+/LS) · no Encounter-Arena scene transition (duels in-place) · no Command-pattern combat actions (returns in Game 2's tactical layer) · no DI container/ECS/Addressables/localization-framework/netcode · no behavior-tree AI (string-scheduler + policy knobs is the honest scope) · no stamina, no skill tree (locked design).

---

## 7. Gray-box slice — Milestone 0 (the build order inside the build)
1. **Core minimal:** bootstrap + locator + bus + sim clock + feel(hitstop) + input + camera director with `DuelNeutral`/`TelegraphPush`.
2. **CombatReactive minimal:** AttackDef + timeline runner + resolver; one capsule guardian with 3 attacks (parryable / perilous / ranged) + 1 feint branch.
3. **Player FSM** (strike/parry/dodge/counter) + debug HUD + purge bar.
4. **Tune until the answer is yes:** telegraph → push-in → parry → hitstop → counter. **Exit criterion: "you immediately want to do it again."** This validates the portfolio's combat identity (the reframed Game-1 milestone per D4).

Then M1: real guardian #1 + finisher + ranking → M2: descent spine + rebirth tiers → M3: content fill (6+5) → M4: gauntlet/lore/polish. (Real task breakdown happens at scaffold.)

---

## 8. Carry-forward checklist (architecture edition)
| Artifact | Game 2 (Tactical) | LS | Change type |
|---|---|---|---|
| BGamer.Core services | all | all | additive (grid services join in G2) |
| Attack data model + resolver | reaction layer on enemy turns | the defensive half | none — host maps outcomes |
| Scheduler seam | TurnBasedScheduler | same | new impl, same interface |
| Camera director + ShotDefs | two-mode rig = new request policy | reuses G2's | new policy, same vocabulary |
| Guardian defs/movesets | — | named-Husk mini-bosses | re-bound, not re-authored |
| Damage-step chain | flank/crit steps | full pipeline | new steps, same chain |
| Purge/finisher | — (LS maps to AP/crit/Mani) | purification *presentation* reused | host-policy swap |
| Run-director pattern | run structure | extraction loop kinship | pattern reuse |

*If a sprint produces something that can't go on this table or §6, stop and rethink (08's rule, inherited).*
