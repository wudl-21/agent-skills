# Teardown Scripting Basics

> Related: [modding.md](modding.md) | [entities_and_input.md](entities_and_input.md) | [ui.md](ui.md) | [registry.md](registry.md)

## Language & Runtime

- Teardown uses Lua **5.1** (older version; some online tutorials may reference unsupported features).
- Enabled Lua standard libraries: **`math`**, **`string`**, **`table`** only.
- No file I/O, no `os.execute()`, no starting executables (disabled for security).
- No bitwise operators or `goto` (Lua 5.2+ features).

## Script Setup

- Scripts are `.lua` files placed in the mod folder.
- Two ways to attach a script to a level:
  1. `main.lua` in the mod root folder (runs for the entire mod).
  2. **Script node** in the editor — recommended for Content mods (more flexible; can be placed anywhere in scene hierarchy).
- Script node must be the **parent** of any entities it searches locally (non-global searches).

## Game Callback Functions (Singleplayer)

```lua
function init()
    -- Called ONCE at level load. Use for initialization, handle lookup, state setup.
    DebugPrint("Hello World")
end

function tick(dt)
    -- Called ONCE per frame. dt = delta time in seconds (max 0.0333333 s = 1/30 s cap).
    -- Handle ALL input here.
end

function update(dt)
    -- Fixed timestep: always 0.0166667 s (1/60 s), called 0-2x per frame.
    -- Do NOT handle input here — button presses can be missed.
end

function draw(dt)
    -- Called ONCE at the end of each frame.
    -- ONLY place where Ui* functions are valid.
end
```

### Callback Order Per Frame

`tick()` → `update()` (0–2×) → `draw()`

> **[CONSTRAINTS]**
> - User input **must** be handled in `tick()`, never in `update()`.
> - `Ui*` functions are **only valid inside `draw()`**.
> - `dt` passed to tick/update/draw = time elapsed since last rendered frame.
> - `update()` timestep is internally capped at 1/30 s → called at most 2× per frame.
> - If frame rate drops below 30 fps, the game moves in slow motion rather than calling `update()` more than twice.

## `update()` vs `tick()` — Fixed vs Variable Timestep

|                  | `tick()`                  | `update()`                    |
|------------------|---------------------------|-------------------------------|
| Calls per frame  | Exactly 1                 | 0, 1, or 2                    |
| Timestep         | Variable (fps-dependent)  | Fixed: 1/60 s                 |
| Input safe       | ✅ Yes                    | ❌ No (can miss button presses)|
| Use for          | Gameplay logic, input     | Frame-rate-independent physics |

- Recommendation: **use only `tick()`** for most logic. Move to `update()` only if behavior is visibly frame-rate-dependent.

## DebugPrint

```lua
DebugPrint("text")       -- prints to debug console overlay (visible in-game)
DebugPrint(someNumber)
```

## Asset Path Resolution

All asset-loading functions (`LoadSprite`, `LoadSound`, etc.) use this search order:

1. **Same folder as the script** — provide just the filename.
2. **`MOD/` prefix** — expands to the mod root folder (works for local and Workshop mods).
   - ⚠️ Must use **capital letters**: `MOD/` not `mod/`.
3. **Game data folder fallback** — predefined paths per asset type (e.g., `data/snd/` for sounds).

```lua
LoadSound("explosion.ogg")            -- searches script's folder
LoadSound("MOD/sound/explosion.ogg")  -- explicit mod-relative path
LoadSound("explosion")                -- fallback: also tries data/snd/explosion.ogg
```

### Encrypted Game Assets (`.tde`)

- Many built-in game assets end in `.tde` (licensed, encrypted for legal reasons).
- Load them by omitting the `.tde` extension — the engine auto-appends it as a fallback:

```lua
LoadSound("bang")  -- tries "bang.ogg" first, then "bang.tde"
```

## API Reference

- Full API reference: `api.html` (accessible via link on the Mods menu in-game).
- Also available as `api.xml` for download from the modding home page.
