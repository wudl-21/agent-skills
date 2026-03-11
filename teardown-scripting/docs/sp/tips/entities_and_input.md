# Entities, Handles, Tags & Input

> Related: [scripting_basics.md](scripting_basics.md) | [physics.md](physics.md) | [scene_queries.md](scene_queries.md)

## Entity Handles

- Handles are plain **Lua numbers** returned by `Find*` functions.
- A handle of **0** means "not found".
- Store handles in global variables for reuse across frames.

```lua
local lightHandle  -- module-level global

function init()
    lightHandle = FindLight("mytag")
    DebugPrint(lightHandle)  -- 0 if not found
end
```

## Local vs Global Entity Search

- By default, `Find*` functions search **only children of the script node** in the scene hierarchy.
- Pass `true` as the global flag to search the **entire scene**:

```lua
local light  = FindLight("mytag")         -- local search (children of script node only)
local lights = FindLights("mytag", true)  -- global search (entire scene)
```

> **[CONSTRAINTS]**
> - For local searches to work, the target entities must be **children** of the script node in the scene explorer.
> - Move entities under the script node in the scene hierarchy to make them discoverable.

### Common Find Functions

| Function              | Returns                              |
|-----------------------|--------------------------------------|
| `FindLight(tag)`      | Single light handle (or 0)           |
| `FindLights(tag)`     | Array of all matching light handles  |
| `FindShape(tag)`      | Single shape handle (or 0)           |
| `FindBody(tag)`       | Single body handle (or 0)            |
| `FindLocation(tag)`   | Single location handle (or 0)        |
| `FindJoint(tag)`      | Single joint handle (or 0)           |

## Tags

- Tags are string labels assigned to entities in the editor.
- A tag can optionally carry a **value** (string), used e.g. for custom interact messages or parameters.

```lua
-- Set a tag with a value
SetTag(handle, "interact", "Turn on")

-- Remove a tag entirely
RemoveTag(handle, "interact")
```

## Working with Light Entities

```lua
SetLightEnabled(lightHandle, true)    -- turn on
SetLightEnabled(lightHandle, false)   -- turn off
SetLightColor(lightHandle, r, g, b)   -- RGB color, values 0.0–1.0
```

## Script Parameters (Editor-Defined)

Query numeric/string parameters configured on the script node in the editor:

```lua
local interval = GetIntParam("interval", 5)    -- int with default value
local speed    = GetFloatParam("speed", 1.0)   -- float with default value
local name     = GetStringParam("label", "")   -- string with default value
```

- Setting parameters per script node instance enables reuse of the same script with different configs.
- Example: duplicate a spinning disc + script, set different `interval` params → each blinks at its own rate.

## Input Handling

> **[CONSTRAINTS]** All input must be checked inside `tick()`.

### Input Key Names: Physical vs Logical

- Prefer **logical** input names for gamepad/custom keybinding compatibility.
- Physical examples: `"lmb"` (left mouse), `"space"`, `"delete"`, `"l"`
- Logical examples: `"jump"`, `"flashlight"`, `"interact"`, `"usetool"`

### Input Functions

```lua
InputDown("jump")     -- true every frame while key is held
InputPressed("jump")  -- true ONLY on the single frame the key was pressed
```

### Toggle Pattern

```lua
local lightOn = false

function tick(dt)
    if InputPressed("l") then
        lightOn = not lightOn
        SetLightEnabled(lightHandle, lightOn)
    end
end
```

## Interact Tag & GetPlayerInteractShape

- Assigning the reserved tag `"interact"` to a shape/body makes the engine show the interact notification UI.
- Optionally give the tag a value for a custom message displayed to the player.

```lua
-- In editor or at runtime:
SetTag(buttonShape, "interact", "Turn on")
```

```lua
function tick(dt)
    local interacting = GetPlayerInteractShape()
    -- Returns 0 if no interactable shape/body is in range

    if interacting == buttonShape and InputPressed("interact") then
        -- Player interacted with our button
        lightOn = not lightOn
        SetLightEnabled(lightHandle, lightOn)

        -- Update the interact message to reflect current state
        SetTag(buttonShape, "interact", lightOn and "Turn off" or "Turn on")
    end
end
```

- To make an object no longer interactable, remove the tag:

```lua
RemoveTag(buttonShape, "interact")
```
