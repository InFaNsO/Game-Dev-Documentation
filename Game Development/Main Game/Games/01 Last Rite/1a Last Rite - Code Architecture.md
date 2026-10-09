# Game 1 "Last Rite" — Code Architecture & Abstraction Map

> **The engineering blueprint for Game 1 — written BEFORE the scaffold (2026-06-10).** Companion to [[1 Parry Combat - Last Rite]] (the design; all D1–D8 locks). Pattern source: the Tier-0 *patterns* (assemblies · service locator · typed event bus · SO data layer · top-level state machine · build order) — its LS-specific machinery (grid/cover/Mani/faction/inventory) is NOT built here. *(The older top-level "08 Unity Code Architecture" doc that first captured these patterns was removed 2026-06-13, pending main-game work; this doc is now the authoritative Tier-0 source.)* **This doc is the concrete v1 of the Tier-0 foundation** — the first real instance of the shared architecture every later game inherits.

**Target:** Unity 6 (6000.3.6f1) · C# · URP · Input System · Cinemachine 3. Single-player, single project.

---

## ⚑ TARGET ARCHITECTURE: events, presentation and assemblies (LOCKED 2026-10-09)

> Decided with the user on 2026-10-09, after the October code audit. **This section is the rewrite target.** It supersedes the as-built notes below where they conflict, and it supersedes the 2026-08-29 ruling that `BGamer.Juice` is a "sub-core" any namespace may call. Visual explainer (artifact): [Last Rite Event Flow](https://claude.ai/artifact/KmdHGfApGUqpuse9wg1ntq). Nothing in this section is built yet; §T9 gives the order of work.

### T0. Why the rewrite
The 2026-10-09 audit of all 251 scripts (36k lines) found:
- **No assembly definitions.** Everything compiles as one Assembly-CSharp, so no namespace compiles alone and the compiler enforces none of the layering rules.
- **A two-way dependency between Combat and Juice.** `JuiceReactionBloodSplat` casts to a type nested in Combat's `CombatJuiceListener`, while `CombatSystem` and `CombatFeelEvents` reach into Juice's `FeelLab`.
- **Visuals connected in code.** `PlayerFaceDriver` subscribes to 13 Combat events, and `HuskParrySurge` and `ReliquaryBossDirector` do the same, each with rules of its own.
- **Duplication.** There is one script per effect, three hair solvers, three weapon-grip solutions, and the same patch-noise function in five places.
- **Hard-coded lookups.** Animator states, sounds, weapons, lamps and Feel Lab switches are all found by strings.

### T1. Layers and assemblies
**The rule: namespaces never reference each other.** Every assembly references **`BGamer.Core` only**, and namespaces are linked by `UnityEvent<>`s wired in the Inspector. **`LastRite` is the one exception.** It owns the prefabs and scenes where those wires are stored, plus code glue only where the Inspector can't express the link (the Traversal ↔ Combat encounter flow).

| Tier | Assembly | Holds | References in code |
|---|---|---|---|
| Foundation | `BGamer.Core` | entities, modes, drivers, systems, camera ownership · the Cue types · the shared base classes (§T6) | nothing in the project |
| Gameplay | `BGamer.Combat` · `BGamer.Traversal` · `BGamer.AI` | rules and state, plus one Cue emitter per library | Core |
| Presentation engine | `BGamer.Juice.Base` | `Moment` (sync), `TimeSystem`, `RumbleSystem`, Feel Lab; **the only assembly that uses Feel (MoreMountains)** | Core, Feel |
| Presentation channels | `BGamer.Juice.Visuals` · `BGamer.Juice.Audio` · `BGamer.Juice.Cinematic` | maintainers (§T4) and the owner system of each channel's resource (§T5) | Core |
| Presentation | `BGamer.UI` | HUD, menus, prompts | Core |
| Game | `LastRite` (+ `LastRite.Dev`, `LastRite.Editor`) | prefabs, scenes, content assets, glue between gameplay libraries | anything |
| Tools | `BGamer.LevelDesign` | level builders; takes no part in events | Core |

- Every runtime assembly gets a matching `.Editor` assembly.
- `BGamer.Rendering` and `BGamer.Dynamics` fold into `BGamer.Juice.Visuals`.
- The study folders (FaceLab, ParrySurgeStudy, VfxShowcase) will reference production assemblies instead of copying production code. They copied it only because an assembly cannot reference Assembly-CSharp.

### T2. Events: four kinds, all `UnityEvent<>`
**User rule: no C# `event` or `Action` fields anywhere; every event is a `UnityEvent<>`.**

| Kind | Direction | Raised by | Heard by | Payload | Crosses namespaces |
|---|---|---|---|---|---|
| Driver input | engine → gameplay | Core drivers (`AnimationDriver` relays clip markers via `AnimMarkerRelay`) | the source the mode machine bound | Core types | no (Core → gameplay) |
| State event | gameplay → the same library | the system or entity that owns the state | its emitter, same-library code, LastRite glue | the library's own types (`HitInfo`, `DefenseResult`) | no |
| Cue | gameplay → presentation | the library's emitter; a `CueChannel` asset for screen-wide moments | Moments, live effects, presenters, UI | `Cue` (Core) | **yes, wired in the Inspector only** |
| Command | anything → gameplay | UI buttons, Timeline signals, level triggers | public gameplay methods | none, primitives, or asset references | **yes, wired in the Inspector only** |

- **Scope.**
  - A moment about one fighter stays inside its prefab: the emitter's `UnityEvent<Cue>` is wired to receivers on the same prefab's Visuals child.
  - Screen-wide moments use a `CueChannel` asset, with a `CueChannelListener` in each scene. An Inspector wire cannot leave its prefab or scene, which is why HUD, music and arena props listen this way.
- **Animation timing.**
  - Clip markers are driver input.
  - Defence-window times are baked into `SOAttackDef` by the Attack Events tool, so emitters never read clips by name. This retires `CombatFeelEvents.TryFindSwing` and the copied event-name strings.
- **Example chain:**
  1. A hit marker reaches `AnimationDriver`, which passes it to `CombatAnimSource`, then to `CombatSystem`, then to `CombatEntity.GotHit`.
  2. The entity raises the state event `UnityEvent<HitInfo>`, wired in the Inspector to `CombatEntityCues.Hurt`.
  3. The emitter raises `UnityEvent<Cue>`, wired in the Inspector to `Moment.Play`.
  4. The Moment fires timed beats to `VfxOneShot` / `SoundOneShot` / `CameraOneShot`, and its hitstop beat asks `TimeSystem`.

### T3. The Cue (Core)
```csharp
public sealed class Cue
{
    public GameObject Subject, Other;              // who it is about; the other party
    public Vector3? Position, Direction;           // where; which way
    public float? Intensity01, Value01, Display;   // how hard; a gauge after it; a number to show
    public int? Step;                              // phase, combo step, QTE step
    public float? LeadSeconds, Duration;           // time to the peak; how long it stays open
    public CueProfile Profile;                     // designer look data from the gameplay asset
}
```
- **Facts only, never looks.** The moment's identity is the event slot it is raised on, not a field.
- **Never subclass the Cue per effect, and never cast it.** The old `JuiceReactionBloodSplat` downcast is the cautionary example.
- **Look data:**
  - A `CueProfile` hangs on gameplay assets (`SOAttackDef`, weapon, fighter). It holds `CueSettings` entries keyed by `EffectSlot` assets.
  - Each channel derives its own look types (`VfxLook`, `SoundLook`, `ShotLook`).
  - Gameplay forwards the profile and never opens it.
  - Per-enemy looks sit on the prefab's effect; per-attack overrides sit in the profile.
- **Migration.** `JuiceReactionData` becomes `Cue` and `IJuiceReaction` becomes `ICueReceiver`, with `[MovedFrom]` so serialized UnityEvents keep working.

### T4. Presentation: maintainers
**A new effect is a new prefab, not a new script.** A maintainer is one class that runs every effect of its kind; the prefab chooses the asset it plays (graph, sound, shot).

| Kind | Behaviour | Visuals | Audio | Cinematic | UI |
|---|---|---|---|---|---|
| **One-shot** | fire and forget; plays, finishes, returns to the pool | `VfxOneShot` · `MaterialOneShot` · `PoseOneShot` · `GhostOneShot` · `FaceOneShot` | `SoundOneShot` | `CameraOneShot` | `HudBanner` · `DamageNumber` · `ScreenFade` |
| **Live** | has a lifetime; moves or follows; changes state when gameplay answers | `VfxLive` | `SoundLive` | `CameraLive` | `TimingRing` |
| **Presenter** | mirrors a value for as long as its owner lives | `MaterialPresenter` · `CountPresenter` · `SelectionOutline` · `FaceRig` | `MusicPresenter` | `ScreenPresenter` | `HudBar` · `PipRow` |
| **System** | the one owner of a shared resource (§T5) | `MaterialBlock` | `AudioMixSystem` | `PostFxSystem` | — |

- **Visuals one-shots are split by mechanism, not by effect.**
  - `VfxOneShot` plays a VFX graph: every splat, spark, burst and dust puff.
  - `MaterialOneShot` pulses a shader property: hit flash, glow flare.
  - `PoseOneShot` squashes or recoils the model.
  - `GhostOneShot` draws an afterimage.
- **Live verbs** are `Begin`, `Open`, `Impact`, `Deflect`, `Miss` and `End`.
  - `FX_MagicBolt` starts on the telegraph Cue with `LeadSeconds`, so it lands on the hit frame. It then follows the outcome: Impact on a hit, Deflect on a parry, Miss on a dodge.
  - `FX_WeaponSurge` is the same class with different data. It builds over `LeadSeconds`, peaks for `Duration`, bursts on Deflect, and fades on Miss or Impact. This retires `HuskParrySurge`.
- **Sync.**
  - A `Moment` (Juice.Base) wraps a Feel stack. `MMF_CueBeat` feedbacks fire `UnityEvent<Cue>` at set times to maintainers in any channel. The channels never reference each other, and they stay in sync because one clock fires them all.
  - Live effects and presenters take Cues directly.
  - The Feel Lab gate is checked once, in the Moment, using `SOFeelFeature` assets, never strings.

### T5. Shared resources: owner systems
Every shared resource has **one owner system, built like the existing `CameraSystem`**:
- A request carries a priority and a duration, and returns a handle.
- The system resolves the live requests.
- When the last handle ends, the resource returns to its resting value.
- Nothing else writes the resource.
- A maintainer only asks a system in its own library or in Core, so the Core-only rule holds.

| Shared resource | Owner | Lives in | Asked by |
|---|---|---|---|
| `Time.timeScale` | `TimeSystem` | Juice.Base | Moment beats (hitstop, slow-mo); the pause button through an Inspector command |
| The camera | `CameraSystem` | Core (already built) | `CameraOneShot`, `CameraLive`, gameplay camera sources; rigs behind a Core `IShotRig`, while `ShotRig` itself moves to Juice.Cinematic, taking Cinemachine out of Core |
| Post-processing | `PostFxSystem` | Juice.Cinematic | `ScreenPresenter`, `CameraOneShot` |
| Audio mix and music | `AudioMixSystem` | Juice.Audio | `SoundOneShot`, `SoundLive`, `MusicPresenter` |
| Gamepad motors | `RumbleSystem` | Juice.Base | Moment beats |
| One renderer's property block | `MaterialBlock` (one per renderer) | Juice.Visuals | `MaterialOneShot`, `MaterialPresenter`, `FaceRig`. Merging every writer fixes the property-block clobber found in the audit |

### T6. Shared base classes: no duplicated functionality
**User rule (2026-10-09): when two classes do similar work, they share one base class.** Bases used by more than one library must live in Core, the only assembly they all reference. Bases used inside one library stay in that library. All of them are MonoBehaviours, ScriptableObjects or serializable data, per the Unity-only rule. A generic MonoBehaviour base is never added to a GameObject itself, only its concrete subclasses, the same as `CharacterModeMachine`.

**In Core (shared across libraries)**

| Base | The shared work | Derived classes |
|---|---|---|
| `CueEmitter` | building Cues (subject, hit-point and direction helpers), raising events and channels, asserting its wiring | `CombatEntityCues`, `CombatSystemCues`, `BossCues`, `TraversalCues`, `AICues` |
| `CueReceiver` (implements `ICueReceiver`) | look resolution: the profile's entry for its `EffectSlot`, else the prefab's default | every maintainer |
| `OneShot<TLook>` : `CueReceiver` | `Play(Cue)` → resolve the look → `Fire`; pooled spawn and return | `VfxOneShot`, `MaterialOneShot`, `PoseOneShot`, `GhostOneShot`, `FaceOneShot`, `SoundOneShot`, `CameraOneShot`, `HudBanner`, `DamageNumber`, `ScreenFade` |
| `LiveEffect<TLook, TInstance>` : `CueReceiver` | the six verbs, state, lifetime, timing from `LeadSeconds`/`Duration`, matching an outcome to the oldest open instance | `VfxLive`, `SoundLive`, `CameraLive`, `TimingRing` |
| `Presenter<TLook>` : `CueReceiver` | `Set(Cue)` → `Value01`/`Step` → smoothing → `ValueMap` → `Apply` | `MaterialPresenter`, `CountPresenter`, `SelectionOutline`, `FaceRig`, `MusicPresenter`, `ScreenPresenter`, `HudBar`, `PipRow` |
| `ValueMap` + `SOStepTable` | value → output by curve or thresholds, with hysteresis | every presenter. One Kavach-forms table is shared by the outfit and the face, which removes the copied `.34`/`.67` |
| `OwnerSystem<TRequest>` + `OwnedRequest` | requests, handles, priorities, durations on real or game time, restore when empty; a subclass only implements `Resolve` | `TimeSystem`, `PostFxSystem`, `AudioMixSystem`, `RumbleSystem`, `MaterialBlock`. `CameraSystem`'s shot stack moves onto it last, once the others are proven |
| `EffectPool<T>` | pooling | VFX pool, voice pool, damage numbers, banners, afterimages |

**Inside one library**

| One class | Replaces |
|---|---|
| `WeaponGrip` (Visuals) | `TwoHandWeaponSocket`, `HandGripIK`, `HuskWeaponPlacement` (three grip solvers) |
| `SpringChain` (Visuals) | `HairSpringChain`, `PonytailDynamics`, `PlayerPonytailPhysics`, `MireCrownTabardFollower` |
| `MaterialTarget` (Visuals, serializable data) | the renderer + property + `MaterialBlock` link duplicated in `MaterialOneShot` and `MaterialPresenter` |
| `PatchField.hlsl` + one C# port | the five copies of the patch-noise function |
| `AnimeFlash.hlsl` | the hit-flash code pasted into five shaders; also brings the flash to the three player shaders that lack it |

**Gameplay and tooling candidates.** Verify these while rewriting, and merge only where the shared part is real:

| Candidate base | Shared work | Derived |
|---|---|---|
| `AnimationSourceBase` (Core) | transition table and handle plumbing | `CombatAnimSource`, `TraversalAnimationSource`, `NPCLocomotionAnimSource` |
| `MotionSourceBase` (Core) | intent and handle plumbing | `TraversalMotionSource`, `NPCMotionSource` |
| `AttackPlannerBase` (Combat) | turn start/end bookkeeping | `NPCAttackPlanner`, `PlayerAttackPlanner` |
| `LastRiteModeMachine` (LastRite) | the duplicated `StartMode` enum and start logic | `PlayerModeMachine`, `EnemyModeMachine` |
| `ClipRecorder`, `DemoPilot` (LastRite.Dev) | frame capture; scripted turn driving | `FeelClipRecorder` and the prototype capture; `AutoDefender` and `BossDemoPilot` |
| ProBuilder kit (editor helper) | block and material helpers | `ArenaPillarCreator`, `ArenaWallCreator`, `ManiCrystalLampCreator` |

### T7. Class list
New classes, plus existing ones that change role. Unlisted gameplay classes stay as they are.

**BGamer.Core**
| Class | Kind | Replaces |
|---|---|---|
| `Cue` · `ICueReceiver` · `CueProfile` · `CueSettings` · `EffectSlot` · `CueChannel` · `CueChannelListener` | shared types | `JuiceReactionData` and its 8 subclasses · `IJuiceReaction` · `IJuiceListener` · `IJuiceSystem` · name lookups |
| `CueEmitter` · `CueReceiver` · `OneShot<TLook>` · `LiveEffect<TLook, TInstance>` · `Presenter<TLook>` · `ValueMap` · `SOStepTable` · `OwnerSystem<TRequest>` · `OwnedRequest` · `EffectPool<T>` | base classes | §T6 |
| `AnimMarkerRelay` · `AnimMarker` | driver input | `CombatAnimEventListener` added at runtime; event names as strings |
| `IShotRig` | contract | `ShotRig` living in Core |
| `WireCheck` (Core.Editor) | tool | — (§T8) |

**BGamer.Combat**
| Class | Kind | Replaces |
|---|---|---|
| `CombatSystem` · `CombatTurnSequencer` · `CombatEntity` family | reworked | state events become `UnityEvent<T>` |
| `HitInfo` · `DefenseResult` · `TurnInfo` | state-event payloads; never leave Combat | loose parameter lists |
| `CombatEntityCues` · `CombatSystemCues` · `BossCues` | emitters | `CombatJuiceListener` · `CombatFeelEvents` · the presentation half of `ReliquaryBossDirector` |
| `ArmorGauge` | gameplay | `PlayerShroud` (LastRite) |
| `SOAttackDef` | reworked | gains its `CueProfile` and baked defence-window times |
| `SOCombatRules` | data | the `"mec-purge"` Feel Lab string (open, §T10) |

UI leaves Combat: `CombatUIJuiceSystem`, `ComboBuilderUI`, `QTEOverlayUI` and `JuiceReactionFloatingHealthBar` move to `BGamer.UI` (below).

**BGamer.Traversal / BGamer.AI:** existing classes stay, plus the emitters `TraversalCues` (landing, jump, footfall) and `AICues` (spotted, alerted, lost).

**BGamer.Juice.Base**
| Class | Replaces |
|---|---|
| `Moment` · `MMF_CueBeat` | `JuiceReactionFeel` · the eleven house `MMF_*` feedbacks (their work moves into maintainers) |
| `TimeSystem` · `TimeRequest` | `JuiceTimeSystem`, `TimeScaleJuiceSystem`, `TimeScaleJuiceCollector`, `JuiceReactionTimeScale` |
| `RumbleSystem` · `SOHapticPattern` | `MMF_RumblePattern` |
| `FeelLab` · `SOFeelFeature` · `SOFeelProfile` · `FeelLabPanel` | `FeelStackGate` |

**BGamer.Juice.Visuals**
| Class | Replaces |
|---|---|
| `VfxOneShot` · `VfxLook` | `JuiceReactionBloodSplat`, `BloodSplatOnHit`, `HitImpactVFX`, `MMF_VfxAtContact`, `MMF_ParticlesAtContact`, `BossFx` |
| `MaterialOneShot` · `PoseOneShot` · `GhostOneShot` | `FighterFeel`, `MMF_FighterFlash`, `MMF_CorruptionGlow`, `MMF_FighterSquash`, `MMF_Afterimage`, `AfterimageFade` |
| `VfxLive` · `LiveMotion` · `LiveVerbs` | `WeaponSurge`, `SOWeaponSurgeSettings`, `HuskParrySurge`, `SOHuskSurgePalette`, `GlyphRing` |
| `MaterialPresenter` · `CountPresenter` · `SelectionOutline` | `ShroudStateBlend`, `PlayerShroudDriver`, `CorruptionGlowReaction`, `BossPhaseLook`, `RiteLamps`, `SelectionHighlightJuiceSystem`, `JuiceReactionSelectionHighlight`, `MMF_SelectionOutline` |
| `FaceRig` · `FaceOneShot` · `SOFaceSheet` · `SOFaceExpression` | `FaceAnimator`, `PlayerFaceDriver` and its `"Moment_"` names |
| `SpringChain` · `WeaponGrip` · `FingerRig` · `SOWeaponMotionSet` | §T6 (hair, cloth, grips), `HuskFingerRig`, `HuskWeaponMotionSet`, `FingerAnimationToggle`, `JalOffHandRelease` |
| `MaterialBlock` · renderer features (outline, AA) | four components writing the same property block · `BGamer.Rendering` |

**BGamer.Juice.Audio**
| Class | Replaces |
|---|---|
| `SoundOneShot` · `SoundLook` | sound feedbacks inside Feel stacks |
| `SoundLive` | `MMF_PeakCue` (the riser peaks at `LeadSeconds` and stops when the player defends) |
| `MusicPresenter` | `BossMusic`, the music start and stop stacks |
| `AudioMixSystem` · `DuckRequest` | direct sound-manager calls |

**BGamer.Juice.Cinematic**
| Class | Replaces |
|---|---|
| `CameraOneShot` | `MMF_DirectionalKick`, `CameraShakeJuiceSystem`, `JuiceReactionCameraShake` |
| `CameraLive` | `ShotPose`, the prototype's shots |
| `ShotRig` | `ShotRig` (Core), `ImpulseListenerBinder` |
| `ScreenPresenter` | `ScreenEdgeOverlay`, `BossHUD`'s letterbox |
| `PostFxSystem` · `PostFxRequest` | `PostFxShakerBinder`, `PlayerGlowPresentation` |

**BGamer.UI**
| Class | Replaces |
|---|---|
| `HudBar` · `PipRow` | `CombatUIJuiceSystem`'s bars, `JuiceReactionFloatingHealthBar`, `BossHUD`'s bar and lamp dots |
| `HudBanner` · `DamageNumber` · `ScreenFade` | turn banners and boss cards · `MMF_LastRiteDamageNumber` · `ScreenFade` (LastRite) |
| `TimingRing` · `OptionMenu` | `QTEOverlayUI` · `ComboBuilderUI` (`OptionMenu` waits on the list-data decision, §T10) |

**LastRite**
- **Namespaced glue:** `EncounterDirector`, `EncounterSite`, `EncounterGate`, `CombatEncounter`, `Arena`, `ArenaSceneLoader`, `SOEncounterDef`, `SOArenaId`.
- **Mode machines:** `LastRiteModeMachine` → `PlayerModeMachine`, `EnemyModeMachine`.
- **Game-specific setup:** input sources, camera policy and contexts.
- **`LastRite.Dev`:** `FeelClipRecorder`, `AutoDefender`, `BossDemoPilot`, `PauseOnAttackHit`.

**Removed, with no class of their own any more:** `PlayerFaceDriver`, `PlayerShroudDriver`, `HuskParrySurge`, `SOHuskSurgePalette`, `HitImpactVFX`, `BloodSplatOnHit`, `ReliquaryBossDirector`, `BossHUD`, `BossMusic`, `BossPhaseLook`, `RiteLamps`, `GlyphRing`, `BossFx`, `ShotPose`, `BossCue` and `BossCueReceiver` (replaced by Timeline signals), `CombatFeelEvents`, `CombatJuiceListener`, `IJuiceListener`, `IJuiceSystem`.

### T8. Wiring safety and the hard-coding rule
- **`WireCheck` (Core.Editor):** scans prefabs and scenes for UnityEvent calls whose target method no longer exists. It runs on demand and before every build. This covers the main cost of editor wiring, which is that the compiler can't see a renamed method.
  - Built 2026-10-09 (Tools/BGamer/Wire Check). It also flags a missing target, a type that no longer exists, and an object-argument type that no longer resolves (the silent break found in §T9 step 0). A moved type with `[MovedFrom]` counts as a warning, because its wire still fires.
  - Its first run found **24 wires that were broken before the rewrite**: dead `JuiceReactionSelectionHighlight` wires on the three Purifier prefabs, wires whose target was deleted on the Garland, Reliquary and HandRigTests prefabs, and the QTE wires in `Arenas.unity` and `Combat_TestScene`. The prefab rewiring in steps 3–4 clears them.
- **`MovedTypeStrings` (Core.Editor):** rewrites the stored type strings for every `[MovedFrom]` type, with a preview first. Scenes that are open in the editor are skipped.
- **Receiver method names are fixed:** `Play`; `Begin`/`Open`/`Impact`/`Deflect`/`Miss`/`End`; `Set`; `React`.
- **No lookups by name in runtime code.** Use asset and serialized references, `LayerMask` fields instead of layer-name strings, and `SOFeelFeature` references instead of Feel Lab key strings. No `GameObject.Find` or `Shader.Find`.
- **Grep checks:**
  - `using MoreMountains` appears only in `BGamer.Juice.Base`;
  - no C# `event` declarations;
  - no project assembly references anything but Core, except LastRite.

### T9. Order of work (playable after every step)
0. **Prepare.**
   - Commit the October work.
   - Prove that one type can move namespace and assembly without breaking prefab and UnityEvent links (`[MovedFrom]`).
   - ✅ 2026-10-09, proved in a throwaway batch-mode project. When a type moves, its components and assets survive (they are found by script GUID), and its void and dynamic wires still fire. **A wire that passes an Object argument of a moved type is dropped silently**, because Unity resolves the argument type from the string stored in the scene or prefab. Neither `[MovedFrom]` nor re-saving rewrites that string; rewriting the text does. **Every move is therefore:**
     1. Move the file with its `.meta`.
     2. Add `[MovedFrom]`.
     3. Compile.
     4. Run **Tools/BGamer/Moved Types/Rewrite Type Strings**.
     5. Run **Tools/BGamer/Wire Check**.
   - Still to do: commit the October work.
1. **Core.**
   - Add the Cue types, the §T6 bases and `AnimMarkerRelay`.
   - Create the empty assemblies with their reference rules.
   - ✅ 2026-10-09: the Cue types, the §T6 Core bases and `WireCheck` are in `Assets/Scripts/BGamer.Core/` (`Cues/`, `Effects/`, `Systems/`, `Editor/`). They compile clean in Unity and also offline as one standalone Core assembly. Still open: `AnimMarkerRelay` moves to step 3 (it changes how clips call in); `IShotRig` moves to step 2 with `ShotRig`; the asmdefs wait for the step 0 `[MovedFrom]` proof. Before Core gets its asmdef, delete the stray `using`s for NUnit, VisualScripting and `System.Reflection.Metadata`. Core's real package references are Input System, Cinemachine (until `ShotRig` moves) and UniTask.
2. **Break the two-way dependency and fix namespaces.**
   - Fix the blood-splat pair.
   - Give the global-namespace files a namespace.
   - Move `HandRigTests` into Juice.Visuals.
   - Move `ShotRig` out of Core.
3. **Emitters.**
   - Convert state events to `UnityEvent<T>`.
   - Add the Combat emitters, running alongside the old path.
4. **Juice.Base and the first maintainers.**
   - Add `Moment`, `TimeSystem` and `RumbleSystem`, then the one-shots.
   - Migrate one effect at a time, with before and after recordings, then delete the old script.
5. **Owner systems.** `MaterialBlock` first (it fixes the clobber), then `PostFxSystem` and `AudioMixSystem`.
6. **The rest of the presentation.** Live effects (surge, bolts), presenters (Kavach, face), UI, boss presentation, then the Traversal and AI cues.
7. **Finish.**
   - Run the dedupe pass (§T6 candidates).
   - Move `CameraSystem` onto `OwnerSystem`.
   - Turn on `WireCheck` in builds.
   - Complete the hard-code sweep.

### T9b. As built (2026-10-09/10: wave 1 done, wave 2 open)
**Done so far:**
- **Assemblies:** `BGamer.Core` (+ `.Editor`), `BGamer.Juice.Base`, `BGamer.Juice.Visuals` and `BGamer.Juice.Cinematic` exist. Core no longer references Cinemachine: `ShotRig` moved to Cinematic behind `IShotRig`.
- **The CombatJuice hub** (Bhu_V6_Layout, Boss_Reliquary_Lab):
  - `CombatFeelEvents` became `CombatSystemCues`, raising `UnityEvent<Cue>`; its 22 wires were kept.
  - The 19 Feel stacks are `Moment`s.
  - 27 house feedbacks became `MMF_CueBeat` → `MaterialOneShot` / `PoseOneShot` / `GhostOneShot` / `VfxOneShot` / `CameraOneShot`, with their tuning in look assets under `Assets/Data/Juice/Looks`.
  - 6 Feel time feedbacks became `MMF_TimeBeat` → `TimeSystem`, which is now the sole writer of `Time.timeScale`.
  - The low-health edges are a `ScreenPresenter`.
- **Fighters:** every fighter prefab carries `CombatEntityCues`, wired in the Inspector to its own `CombatEntity` events. `CombatJuiceListener` and the blood-splat downcast are gone, which removes the Combat ↔ Juice cycle; the splat is a `Moment` playing a `VfxOneShot`. Undriven systems were deleted (camera-shake system, time-scale collector).

**Deviations from §T6/§T7, by decision:**
- **`LiveEffect<TLook, TInstance>`:** it takes the instance type too.
- **`CameraKickLook`** instead of `ShotLook`, to avoid confusion with `SOShotDef`.
- **`SubjectLooks`** (Core): per-subject look overrides resolved after the Cue's profile, so shared hubs can play per-enemy looks.
- **`Cue.Strength(range)`:** maps Intensity01 the way the old Feel stacks did.
- **`SwingClock`** (Combat): clip timing, formerly `CombatFeelEvents.TryFindSwing`.
- **`MaterialBlock`:** its own layered owner (Override/Add/Max per property) and not an `OwnerSystem`, because its writers are continuous layers, not timed requests.
- **`TimeRequest.EaseRate`:** each request carries its own exponential ease rate, matching Feel's behaviour.

**Migration tooling:**
- `Tools/BGamer/Wire Check` and `Tools/BGamer/Moved Types/Rewrite Type Strings`.
- An offline build harness that compiles every assembly before any change reaches the shared editor.
- One-off migration tools, run once and then deleted.

**Wave 1 complete (2026-10-10, game repo commit `3b80a29c`):**
- **Assemblies:** `BGamer.Juice.Audio` and `BGamer.UI` added; all seven presentation and Core assemblies reference Core only (Juice.Base also MoreMountains; Visuals VFX Graph/URP; Cinematic Cinemachine/URP).
- **Packages migrated:** materials (MaterialBlock, corruption glow, Kavach shroud blend, selection outline, renderer features), face (FaceRig/FaceOneShot), surge (`VfxLive` + `SurgeLook`), audio (SoundOneShot/Live, AudioMixSystem), post (PostFxSystem, lens punch), UI (ScreenFade, DamageNumber), rigs (SpringChain, WeaponGrip, FingerRig, SOWeaponMotionSet).
- The commit was scoped by a GUID-reference closure (nothing committed points at an uncommitted asset) and the exact snapshot was compiled offline against HEAD before committing.

**Port and clean-up (2026-10-10, game repo commit `6f104e6e`):**
- **Playtest** in an isolated project copy (its own editor, so the shared one was never touched): Bhu fights E04 and E06 (victories), E17 and E23 (defeats), and the Reliquary boss fight through its cinematics to the end card. Hits, defences, ghosts, surge, damage numbers, purge, death, post-FX pulses and the boss cinematics all showed; the new code logged no error. Pre-existing items it surfaced: a teardown-only turn-order error in `CombatTurnSequencer`, missing `Idle`/`GotHit`/`Die` transitions on the Deadfall and Mourner families, Animator states the Reliquary director asks for that its controller lacks, and the HUD's PARRY / PERFECT PARRY / DODGE banners, which contradict the F2 "no outcome words" rule.
- **Feel Lab gating is per feedback.** `MMF_GatedFeedback` is the shared base of `MMF_CueBeat`, `MMF_TimeBeat` and `MMF_RumblePattern` (one `Feature`, one gate). `FeelStackGate` is gone: its 7 label bindings moved onto the rumble feedbacks' own `Feature`; the other 12 gates were empty.
- **Removed:** `JuiceTimeSystem` (TimeSystem is the only time owner), `JuiceReactionFeel` + `IJuiceReaction`, the legacy `MMF_CorruptionGlow` / `MMF_LastRiteDamageNumber` / `MMF_SelectionOutline`, `HandGripIK`, `WeaponGripDebugView`, `HuskFingerRig`, `HuskWeaponPlacement`, `HitImpactVFX`, the throwaway `CombatCamProto`, the old Feel channel assets (`Data/JuiceChannels`), the surge palette and both migration tools. `Arenas.unity` is retired (no build entry, no references; git keeps it).
- **Bridges closed:** Moment's legacy play-context keys, and the old serialized fields on `CorruptionGlowReaction` and `ShroudStateBlend` (every instance was already migrated).
- **Scenes:** the SurgeLineup study scene runs on VfxLive and the new rigs; Post Processing is ticked on the Main Camera of Combat_TestScene, Gameloop_TestScene, Bhu Level and Traversal_Test (the force that PlayerGlowPresentation applied is gone).
- **`FeelCatalogueBuilder`** moved to `LastRite/Editor` (it names game asset paths).
- **T9 step 7, WireCheck in builds:** `WireCheckBuildGate` stops a player build when a build scene, or an asset it uses, has a broken wire; files the build never loads only warn. Project-wide: 570 wires, 0 errors, 0 warnings.

**Still to do (wave 2):**
- **Combat, Traversal, AI and LastRite assemblies and namespaces**, Core-only. This collides with the Reliquary boss work in progress (`LastRite/Bosses`, `BGamer.Combat/Boss`, global-namespace game scripts), so it waits for that work to land.
- **`SOCombatRules`:** `CombatSystem` still asks Juice's FeelLab for `"mec-purge"` and adds a fixed 10% purge. Recommended: a Combat-owned rules asset; done with the Combat assembly.
- **Boss presentation split** (ReliquaryBossDirector → BossCues + maintainers; BossHUD, BossMusic, BossPhaseLook, RiteLamps, GlyphRing) and the `CorruptionGlowReaction` namespace move, both in the boss session's files.
- `AnimMarkerRelay`, Traversal and AI cues, `CameraSystem` onto `OwnerSystem`, the hard-code sweep and the §T6 gameplay/tooling dedupe candidates.

### T10. Locked decisions and open items
**Locked (2026-10-09):**
- the hierarchy and assemblies (§T1);
- Core-only references with LastRite as the one exception;
- `UnityEvent<>` only;
- Cue types in Core;
- the Juice split into Base, Visuals, Audio and Cinematic;
- maintainers in three kinds plus systems, with presenters as the third kind;
- owner systems for shared resources;
- the `WireCheck` tool;
- shared base classes for similar functionality.

**Decided 2026-10-10 (user: "go with what you recommend"):**
- **List-shaped UI data stays in the game layer.** `CombatUIJuiceSystem`, `ComboBuilderUI` and `QTEOverlayUI` live in `LastRite/UI/`, and the Cue gets no text field for now. Add `Cue.Label` only when a reusable `BGamer.UI` HUD is actually built.
- **Music** is decided in the boss wave: a looped `SoundOneShot` plus `AudioMixSystem`, and a `MusicPresenter` only if phases need intensity steps.
- **`Arenas.unity` is retired.** It is a legacy scene with its own GameManager and Feel stacks, and not in the build. Remove it in the final clean-up after confirming nothing loads it (git keeps it); this frees the last legacy juice classes it keeps alive.
- **The QTE stays as it is today** (not wired). Reviving it is a design decision, not migration work.
- **Visual checks in the playtest:** the damage-number font material, and DeathDesat (Feel's default colour-adjustment values, copied exactly).

**Open:**
1. ~~**List-shaped UI data** (turn order, combo options): a small Core payload, or game-layer code.~~ Decided above: game layer.
2. **`SOCombatRules`** replacing the `"mec-purge"` Feel Lab switch: confirm.
3. **The §T6 gameplay and tooling candidate bases:** confirm each during the rewrite.

---

## ⚑ AS-BUILT — Combat Prototype (updated 2026-07-04)

> Everything from §0 down is the **north-star blueprint** (pure-C# deterministic sim, asmdef extraction story, camera director, etc.), written before any code. The **combat prototype actually shipped a different, MonoBehaviour-driven, animation-integrated architecture** — a deliberate solo-dev pivot toward "Unity-only, no pure-C# sim." This section is the source of truth for *what exists today*; the blueprint below stays the aspiration for later hardening/extraction.

**Key divergences from the blueprint**
- **No pure-C# sim, no `ISimClock`/`IRng`/`IEventBus`, no asmdefs, no `Assets/Code/`, no EditMode tests.** All game code is Unity types (`MonoBehaviour` / `ScriptableObject`) in a single `Assembly-CSharp`, under `Assets/Scripts/`. ⚠ 2026-10-09: one assembly per namespace is now the target (§T1); the single `Assembly-CSharp` is what the rewrite removes.
- **The animation IS the clock — and (2026-07-18) the animation *events* are the truth.** Originally attack windows were authored in **frames** on `SOAttackDef` (`TimeWindow{StartFrame,EndFrame}`) and polled from the **Animator's playback frame** (`AnimController.GetCurrentAnimationFrame()` off `AnimatorStateInfo.normalizedTime`). The build is now moving phases + defense windows **onto the clip as animation events**, forwarded by a **`CombatAnimEventListener`** (nested `MonoBehaviour`, runtime-`AddComponent`-ed onto the Animator's GameObject, holds a back-ref to its `CombatEntity`) into `CombatEntity`'s events; the SO keeps only *non-frame* data (`Damage`, difficulty start-delay, `AllowedDefences`, combo metadata). Either way sim + presentation share one timeline. **This inverts blueprint I3** (which forbade gameplay timing in anim events) — full trail in [[1f Last Rite - Combat Iteration Log]].
- **Turn flow is event + `StateMachineBehaviour` driven**, not a scheduler over a sim loop: `AttackStateBehaviour` (state-exit) → `CombatSystem.NextTurn`; `CombatTurnSequencer` orders turns and handles the **parry → counter** turn; move/target selection lives in **`IAttackPlanner`** (`NPCAttackPlanner` random-from-moveset · `PlayerAttackPlanner` player-chosen). `CombatEntity` is a `partial` class (the anim-event region + nested listener live in a partial file).

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
| `BGamer.Combat/IAttackPlanner`, `NPCAttackPlanner`, `PlayerAttackPlanner` | who-attacks-what: NPC picks a random moveset attack + cycles target; player chooses via `PlayerChoseAttack`. Carried on each fighter GameObject |
| `BGamer.Combat/CombatTurnSequencer` | builds turn order (player first, then enemies) and injects the **parry → counter** turn (`TurnData.IsParriedTurn`) |
| `BGamer.Combat/CombatEntity.CombatAnimEventListener` *(nested, partial)* | runtime-added MonoBehaviour on the Animator's GameObject; receives the clip's animation events (`AnimStartup … AnimRecover`, defense-window open/close, `ChainBranch`/`CancelBranch`) and raises `CombatEntity`'s events (passing `SOAttackDef` + entity). See [[1d Last Rite - Reaction & Feints Spec]] §7 |
| `BGamer.Juice/*` | decoupled reaction layer (camera shake, blood splat, selection highlight) driven by scene wiring, never referenced from `BGamer.Combat`. ⚠ 2026-10-09: splits into `BGamer.Juice.Base` / `.Visuals` / `.Audio` / `.Cinematic`, fed by Cues (§T1, §T4) |

**Prototype behaviour (done):** a real-character 1v1 duel (extensible to #-vs-#): fighters alternate turns, each auto-selects a random moveset attack + an enemy target, plays the attack anim, and applies damage at the **Hit** frame; the player defends by timing **RMB parry / LMB dodge** against the boss's live animation-frame windows; `DebugCombatHUD` shows both HP bars and the colour-coded current attack window. Characters (Purifier, CinderScale) are rigged Humanoid with grounded Mixamo anims; consolidated single-file FBX (`Purifier_Full.fbx`, `CinderScale_Full.fbx`) exist as portable character assets.

**Still to build (blueprint items not yet real):** the reaction→economy resolver (parry currently only negates the swing; purge/counter unbuilt), camera director / smart action-cam, purge meter + purification finisher, run/descent spine, sanity/narration, ranking/gauntlet, the guardian roster, the asmdef split + `BGamer.*` extraction, and any automated tests.

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

> ⚠ 2026-10-09: superseded by §T1. Every assembly references `BGamer.Core` only, and `LastRite` is the one exception.

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
  Data/                (SO assets: Attacks/ Movesets/ Guardians/ Descent/ Strata/ Narration/ Ranking/ Shots/ Audio/)   ⚠ was RebirthTiers/ — 2026-08-11
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
> ⚠ 2026-10-09: superseded by §T2. There is no bus: every event is a `UnityEvent<>`, and namespaces are linked in the Inspector (Cues for presentation, `CueChannel` assets for screen-wide moments).
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
> ⚠ 2026-10-09: superseded by §T5. Hitstop and slow-mo are requests to `TimeSystem` (an `OwnerSystem` in `BGamer.Juice.Base`), made by `Moment` beats. Nothing else writes `Time.timeScale`.
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
- **Game 1:** two files per slot — `RunSave` (tier index, room index, cleared encounters, RNG seed) + `MetaSave` (best ranks, lore unlocked, techniques, gauntlet scores, true-ending flags) + settings. Checkpoint at room boundaries; death respawns at duel start regardless (diegetic). **⚠ 2026-08-09:** `RunSave` += mani reserve · Shroud element + integrity · fraying; `MetaSave` += scars · kalpa index · endings seen ([[1j Last Rite - Shroud, Mani & Sanity]]).
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
- **Scenes:** `ISceneLoader` (async + fade). Scene list: `Boot` (persistent), `Title`, **`Stratum_00` (tutorial) + `Stratum_01/02/03/04`**. **⚠ 2026-08-11: "Rebirth = reload Stratum_01 with the next `RebirthTierDef` applied" is CUT** — the re-descent loop is gone (D2). A stratum transition **loads the next stratum**; the only full reload left is husking out → Kalpa N+1.

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
- **The acceptance test:** *a new guardian ships with zero new C#* unless it introduces a genuinely new verb. The **13 bespoke enemies (4 bosses + 9 guardians) + 6 regulars** are pure data + animation work. *(⚠ Two known new-verb exceptions, 2026-08-07: the Reliquary Husk's **Rite of Return** needs scheduler work to re-insert a dead fighter into the rotation, and the player's **transform-attack** needs a moveset-swap step.)*
- **Later:** these same `AttackDef`s/`GuardianDef`s become the LS named-Husk mini-bosses — re-bound into LS encounters, not re-authored. Remix overlays were exactly the "remixed movesets per rebirth" from D2. **⚠ 2026-08-11: D2's re-descent loop is cut** ([[1k Last Rite - Lore Bible]] §6) — the overlay *machinery* survives and is still worth having, but its consumers are now **per-stratum authoring** and **the Chaos Descent**, not a rebirth ladder.

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
**Data is truth: animation playback rate is fitted to the timeline, never vice-versa.** Remix timing-multipliers tighten a telegraph by scaling the anim — no re-authoring per stratum *(or per Chaos-Descent roll; "per rebirth tier" is stale — 2026-08-11)*. Telegraph cues (color flash + SFX at telegraph-start) come from the timeline, so fairness floors are enforceable in data.

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
- **Data:** `DescentDef` = ordered `RoomDef`s (encounters, lore spots, fallen-purifier beats, heart chamber). **`StratumDef`** *(⚠ renamed from `RebirthTierDef` — 2026-08-11)* = **one SO per stratum** holding: URP volume profile + global shader params (the palette-shift-as-HUD), its **enemy set**, narration variant set, audio snapshot, escalation knobs, **`maniTheme` (None / Bhu / Jal / Vayu / Agni — the stratum's element, LOCKED 2026-06-11)**, ~~`isTerminal`~~ **(⚠ VOID 2026-08-12 — see below)**. *(Was "one SO per loop … the cycle's element … routes to the true-ending sequence"; the loop is cut, D2. The remix-overlay field survives for the Chaos Descent, not for a rebirth ladder.)*

> **⚠ 2026-08-12 — FREE STRATUM ORDER kills `isTerminal`.** The four strata are now playable in **any order** ([[1k Last Rite - Lore Bible]] §6, [[1l Last Rite - World & Environment Bible]] §1.7/§2.2). No stratum is last, so no `StratumDef` can carry terminality. **Terminality moves to the sanctum, gated by seals:**
> - **`StratumDef` gains `sealGranted`** (the key/seal this wing yields on clear). **`isTerminal` is removed.**
> - **RunDirector holds the seal set**, not an index. The **sanctum `RoomDef` is gated on all four seals held**; clearing it routes to the heart + ending fork (Take / Renounce). Save = the seal set, not a stratum ordinal.
> - **`DescentDef` stops being a single ordered `RoomDef` list.** It becomes **spine + four independently-enterable wing branches + the gated sanctum** — closer to a hub than a corridor. This is the one real structural change; everything else is a field edit.
> - **⚠ Difficulty must key off `strataCleared`, not `stratumIndex`** — any wing can be first, so a per-stratum authored ladder would hand a first-time player the hardest wing. Recommended and **open pending gray-box**: [[1d Last Rite - Reaction & Feints Spec]] §3, [[1l Last Rite - World & Environment Bible]] §6 item 8.
> - **Unchanged:** `maniTheme`, the enemy set, LUT/volume profile, narration set, audio snapshot, the remix-overlay field, and the whole fairness law below. The element is still an overlay; the wings are still authored places.
- **Seam:** the whole D2 loop ("full re-descent, remixed") is **data overlays applied at rebirth** — adding tier 6 or rebalancing tier 3 touches zero code. RunDirector publishes `RebirthStarted/RoomEntered/EncounterCleared`; save = its memento.
- **D1: ⚠ CUT 2026-08-11.** Was *"heart pickup #1 = the inciting rebirth — a scripted beat in the heart chamber's `RoomDef`."* **There is no heart pickup and no inciting rebirth** — the Kavach Rite already made her deathless before the game starts, so death-retry needs no in-world trigger and the heart chamber hosts the **final encounter + ending fork** instead ([[1k Last Rite - Lore Bible]] §4/§8).
- **Elemental spine (LOCKED 2026-06-11; ⚠ ORDER FREED 2026-08-12):** the set is `None` (tutorial) + `{Bhu, Jal, Vayu, Agni}` — **an unordered set, not a ladder.** *(Was `None → Bhu → Jal → Vayu → Agni` with `Agni = isTerminal`.)* The tutorial is still first; the four wings are entered in any order and each grants a `sealGranted`; the sanctum is gated on all four. The element is an **overlay** — the tier's `RemixOverlayDef`s carry the element's attack flavor onto the base guardians (corruption flavor, never a re-skin). **Fairness law (enforced by the §5 validator):** element overlays may alter only *guardian* attack data + presentation — they never emit player-side status (no slow/freeze/push on the player; keeps D7 fairness-sacred inside a stationary duel).
- **Chaos Descent — ⚠ PROMOTED to CORE v1 (2026-08-09; this line said "v1-stretch / first content update, NOT core v1"):** unlocked at **the first ending reached** — Take it, Renounce it or Husk out *(⚠ 2026-08-11: three endings, not "post-true-ending")*. RunDirector takes a runtime-assembled `DescentDef` + **mixed per-encounter element assignment** + seeded remix overlays (the `remix` RNG stream). It is also **the only place the descent repeats**, now that the re-descent loop is cut. Randomized *selection* of authored content, zero procgen, no new system — the bridge to the eventual full-procgen Endless mode.

### 4.4 Explore mode (deliberately thin)
Third-person walk (CharacterController), interactables (lore, doors, the heart), `Shot.Walk`. **No stealth, no loot, no sprint-jump platforming** — it's the breath between duels and the narration/sanity canvas. (LS explore is a different, richer system; nothing here pretends to be it.)

### 4.5 Sanity & narration (presentation-only, structurally)
- `SanityDirector`: maps **stratum + fraying/scar state** + scripted beats → post-volume weights, audio snapshots, hallucination prefabs (pooled, cosmetic). `NarrationService`: line tables keyed by trigger ID with **per-stratum degradation variants** (the "narration degrades" lever). *(Was "rebirth tier" / "per-tier" — 2026-08-11.)*
- **The fairness guarantee is architectural:** both are pure event *subscribers*. They cannot write to the sim — there is no API surface for it. Telegraphs stay readable at every sanity level by construction, honoring "fairness is sacred" (D7).
- **⚠ 2026-08-09 — contract extended, guarantee unchanged.** Sanity gains two additional *fair* output lanes: **sensory narrowing** (mixer/volume + post weights scaling with **fraying**, with the enemy/telegraph channels exempt by construction) and a **HUD-unreliability driver** (targets `LastRite.UI` *abstract* widgets only — diegetic reads and shrine screens exempt; a "steady HUD" accessibility toggle pins it). Still subscriber-only, still no sim API. **Scarring, the death countdown and the kalpa reset live in the run director + saves — not here.** True cue-corruption exists only inside the opt-in "embrace the hallucination" toggle (post-playtest). Spec: [[1j Last Rite - Shroud, Mani & Sanity]] §3.

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
No grid substrate (Game 2) · no inventory/factions/vendor/Mani economy (Game 3+/LS) · no Encounter-Arena scene transition (duels in-place) · no Command-pattern combat actions (returns in Game 2's tactical layer) · no DI container/ECS/Addressables/localization-framework/netcode · no behavior-tree AI (string-scheduler + policy knobs is the honest scope) · no stamina, no skill tree (locked design). *(⚠ 2026-08-09: the Shroud's defensive mani reserve — one scalar + an element tag — is a meter, not the LS Mani economy; the guardrail stands. [[1j Last Rite - Shroud, Mani & Sanity]] §2.)*

---

## 7. Gray-box slice — Milestone 0 (the build order inside the build)
1. **Core minimal:** bootstrap + locator + bus + sim clock + feel(hitstop) + input + camera director with `DuelNeutral`/`TelegraphPush`.
2. **CombatReactive minimal:** AttackDef + timeline runner + resolver; one capsule guardian with 3 attacks (parryable / perilous / ranged) + 1 feint branch.
3. **Player FSM** (strike/parry/dodge/counter) + debug HUD + purge bar.
4. **Tune until the answer is yes:** telegraph → push-in → parry → hitstop → counter. **Exit criterion: "you immediately want to do it again."** This validates the portfolio's combat identity (the reframed Game-1 milestone per D4).

Then M1: real guardian #1 + finisher + ranking → M2: descent spine + strata → M3: content fill → M4: gauntlet/lore/polish. (Real task breakdown happens at scaffold.)

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
