# User Interface (Draw Callback)

> Related: [scripting_basics.md](scripting_basics.md) | [registry.md](registry.md)

## Overview

- All `Ui*` functions are **only valid inside the `draw()` callback**.
- UI is drawn in **screen space** (pixels); origin is top-left corner.
- **State-based**: color, font, position persist until explicitly changed.
- `UiTranslate(x, y)` shifts the current draw origin; effects are cumulative within the same push/pop scope.

## UI State: Push & Pop

Use `UiPush()` / `UiPop()` to save and restore the UI state (color, transform, font, etc.):

```lua
function draw()
    UiPush()
        UiColor(1, 0, 0, 1)   -- set red
        UiTranslate(100, 100)
        UiRect(200, 50)        -- red rectangle at (100, 100)
    UiPop()
    -- color and translation are restored to before the push
    UiRect(80, 40)             -- drawn in previous color at (0, 0)
end
```

## Basic Drawing

```lua
UiRect(width, height)            -- filled rectangle at current origin
UiColor(r, g, b, a)              -- set RGBA color (0.0–1.0)
UiTranslate(x, y)                -- shift draw position by (x, y) pixels
UiAlign("center middle")         -- alignment: "left/center/right" + "top/middle/bottom"
```

## Text

```lua
UiFont("bold.ttf", 24)           -- set font name and size (must call before UiText)
UiText("Hello World")            -- draw text at current position
UiText("Score: " .. score)
```

## Buttons — Immediate Mode

```lua
local clicked = UiTextButton("OK")
if clicked then
    -- handle button click
end
```

> **[CONSTRAINTS]**
> - Buttons and sliders are **immediate mode**: no object creation needed. Just call the function each frame.
> - The function draws the button AND returns its interaction state simultaneously.
> - Mouse input defaults to game control. Must call `UiMakeInteractive()` to redirect input to the UI.

## Interactive UI (Mouse Pointer Redirect)

```lua
local menuVisible = true

function draw()
    if not menuVisible then return end

    UiMakeInteractive()   -- call EVERY FRAME while UI is active; shows mouse cursor

    UiTranslate(UiCenter(), 200)
    UiAlign("center middle")
    UiFont("bold.ttf", 28)
    UiText("Pause Menu")

    UiTranslate(0, 60)
    if UiTextButton("OK") then
        menuVisible = false
        -- Stops calling UiMakeInteractive → mouse/input returns to game automatically
    end
end
```

> **[CONSTRAINTS]**
> - `UiMakeInteractive()` is **continuous/implicit control**: mouse only redirects while it is called every frame.
> - Stopping the calls (e.g., setting `menuVisible = false`) automatically restores game input.
> - This implicit "do it every frame to keep it active" pattern is used throughout the Teardown API beyond UI.

## Screen Dimensions

```lua
UiWidth()     -- screen width in pixels
UiHeight()    -- screen height in pixels
UiCenter()    -- shorthand: UiWidth() / 2
```

## Sliders

```lua
local changed, newValue = UiSlider("label", currentValue, minVal, maxVal)
if changed then
    currentValue = newValue
end
```

## Full Example: Simple HUD with Button

```lua
local score = 0
local showMenu = false

function draw()
    -- Always-visible score HUD
    UiPush()
        UiFont("regular.ttf", 20)
        UiTranslate(20, 20)
        UiText("Score: " .. score)
    UiPop()

    -- Conditional menu overlay
    if showMenu then
        UiMakeInteractive()
        UiPush()
            UiTranslate(UiCenter(), UiHeight() / 2)
            UiAlign("center middle")
            UiFont("bold.ttf", 32)
            UiColor(1, 1, 1, 1)
            UiText("Game Over")
            UiTranslate(0, 50)
            UiFont("regular.ttf", 20)
            if UiTextButton("Restart") then
                showMenu = false
            end
        UiPop()
    end
end
```
