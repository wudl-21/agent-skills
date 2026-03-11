# Registry & Persistent Data

> Related: [scripting_basics.md](scripting_basics.md) | [ui.md](ui.md)

## Overview

The **registry** is a shared key-value store accessible by **all scripts running simultaneously**. It is the only mechanism for cross-script communication.

- Keys are hierarchical (slash-like notation): `"level.score"`, `"game.tool.lasergun.enabled"`.
- All values **default to `0`** if not yet set.

## Registry Functions

```lua
-- Integer
GetInt("level.score")            -- read
SetInt("level.score", value)     -- write

-- Float
GetFloat("level.timer")
SetFloat("level.timer", 2.35)

-- Boolean
GetBool("level.active")
SetBool("level.active", true)

-- String
GetString("level.status")
SetString("level.status", "running")
```

## Reserved Registry Key Prefixes

| Key Prefix       | Purpose                                                          |
|------------------|------------------------------------------------------------------|
| `game.*`         | Engine internal state (e.g., `game.tool.X.enabled`)             |
| `options.*`      | Game settings                                                    |
| `savegame.*`     | Persistent saved data — mostly **read-only** for mods           |
| `savegame.mod.*` | **Writable** persistent storage for mod data                    |
| `level.*`        | Convention for session-scoped level data (not reserved, but recommended) |

> **[CONSTRAINTS]**
> - Scripts are **isolated** — they cannot call each other's functions or access each other's Lua globals.
> - The registry is the **only** cross-script communication channel.
> - `savegame.*` (outside `savegame.mod`) is **read-only** for mods to protect campaign progress.

## Cross-Script Communication Pattern

```lua
-- Script A (target.lua): increments score when target breaks
function tick(dt)
    if ShapeBroken(targetShape) then
        local score = GetInt("level.score")
        SetInt("level.score", score + 1)
    end
end

-- Script B (hud.lua): reads and displays the score
function draw()
    UiFont("bold.ttf", 24)
    UiTranslate(20, 20)
    UiText("Score: " .. GetInt("level.score"))
end
```

## Timer with Delta Time

```lua
local timer = 0

function tick(dt)
    if GetInt("level.score") < 3 then
        timer = timer + dt   -- dt = time since last frame (seconds)
    end
end

function draw()
    -- Display timer truncated to 2 decimal places
    local display = math.floor(timer * 100) / 100
    UiFont("bold.ttf", 20)
    UiText("Time: " .. display)
end
```

## Persistent High Score (savegame.mod)

All entries under `savegame.mod.*` are saved to disk and survive game restarts.

```lua
local function TrySaveHighScore(currentTime)
    local best = GetFloat("savegame.mod.mymod.besttime")
    -- best == 0 means no score saved yet (all registry entries default to 0)
    if best == 0 or currentTime < best then
        SetFloat("savegame.mod.mymod.besttime", currentTime)
    end
end

function draw()
    local best = GetFloat("savegame.mod.mymod.besttime")
    if best > 0 then
        UiText("Best: " .. math.floor(best * 100) / 100 .. "s")
    end
end
```

> **[CONSTRAINTS]**
> - Use `GetFloat` / `SetFloat` for time values (decimals). Use `GetInt` / `SetInt` for whole numbers.
> - Use a mod-specific prefix in `savegame.mod` (e.g., `savegame.mod.mymod.*`) to avoid collisions with other mods.
