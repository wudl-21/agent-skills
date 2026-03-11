# Teardown SP Tips — Knowledge Base Index

> Scope: **Singleplayer (SP)** only. For Multiplayer, see `docs/mp/`.

## File Navigation

| File | Contents |
|------|----------|
| [modding.md](modding.md) | Mod types, folder structure, info.txt, Workshop, content replacement, custom tools, spawnable assets, multi-scene |
| [scripting_basics.md](scripting_basics.md) | Callback functions (init/tick/update/draw), Lua 5.1 constraints, DebugPrint, asset path resolution |
| [entities_and_input.md](entities_and_input.md) | Entity handles, Find* functions, tags, SetTag/RemoveTag, input handling (InputDown/InputPressed), interact system |
| [vector_math.md](vector_math.md) | Vec/Quat/Transform types, VecCopy, space conversions (TransformToLocalPoint / TransformToParentPoint), common math functions |
| [physics.md](physics.md) | Body/shape hierarchy, GetShapeBody, SetBodyAngularVelocity, SetJointMotor, ShapeBroken, shape destruction behavior |
| [ui.md](ui.md) | draw() callback, UiRect/UiColor/UiTranslate, UiPush/UiPop, UiFont/UiText, UiTextButton, UiMakeInteractive |
| [registry.md](registry.md) | Cross-script data sharing (GetInt/SetInt etc.), reserved keys, persistent savegame.mod |
| [scene_queries.md](scene_queries.md) | Triggers, QueryRaycast, QueryRejectBody, ray reflection pattern, volume queries |
| [sprites_sound_particles.md](sprites_sound_particles.md) | LoadSprite/DrawSprite, LoadSound/PlaySound, looping sounds, ParticleReset/SpawnParticle, particle burst pattern |

## Quick Reference: Callback Order

```lua
function init()   end   -- once at level load
function tick(dt) end   -- every frame; handle input here
function update(dt) end -- fixed 1/60 s; 0–2× per frame; no input
function draw(dt) end   -- end of frame; Ui* functions only here
```

## Quick Reference: Key Patterns

| Task | Solution |
|------|----------|
| Cross-script data sharing | `GetInt` / `SetInt` / `GetFloat` etc. via registry |
| Persistent mod data | Read/write `savegame.mod.*` registry keys |
| Attach world position to moving body | `TransformToLocalPoint` in `init()`, `TransformToParentPoint` in `tick()` |
| Interactive UI with mouse | Call `UiMakeInteractive()` every frame while menu is visible |
| Avoid self-hit in raycast | `QueryRejectBody(selfHandle)` before each `QueryRaycast()` |
| Prevent state leaking in UI | Wrap changes in `UiPush()` / `UiPop()` |
| Sound variation | Name files `sound0.ogg`, `sound1.ogg`, … — engine picks randomly |
| Fixed-timestep simulation | Move logic from `tick()` to `update()` |
| Mod-local asset loading | Use `MOD/` prefix (capital letters required) |

## Source Documents

- `modding/index.html` — Official Teardown modding documentation page
- `scripting/` — Transcribed subtitles from Teardown Scripting Tutorial series (Parts 1–10) by Tuxedo Labs
