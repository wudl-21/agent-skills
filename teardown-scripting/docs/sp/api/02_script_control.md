# Teardown SP API — Script control
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

General functions that control the operation and flow of the script.

**Physical**

| Key | Description |
|-----|-------------|
| `esc` | Escape key |
| `tab` | Tab key |
| `lmb` | Left mouse button |
| `rmb` | Right mouse button |
| `mmb` | Middle mouse button |
| `uparrow` | Up arrow key |
| `downarrow` | Down arrow key |
| `leftarrow` | Left arrow key |
| `rightarrow` | Right arrow key |
| `f1-f12` | Function keys |
| `backspace` | Backspace key |
| `alt` | Alt key |
| `delete` | Delete key |
| `home` | Home key |
| `end` | End key |
| `pgup` | Pgup key |
| `pgdown` | Pgdown key |
| `insert` | Insert key |
| `space` | Space bar |
| `shift` | Shift key |
| `ctrl` | Ctrl key |
| `return` | Return key |
| `any` | Any key or button |
| `a,b,c,...` | Latin, alphabetical keys a through z |
| `0-9` | Digits, zero to nine |
| `mousedx` | Mouse horizontal diff. Only valid in InputValue. |
| `mousedy` | Mouse vertical diff. Only valid in InputValue. |
| `mousewheel` | Mouse wheel. Only valid in InputValue. |

**Logical**

| Key | Description |
|-----|-------------|
| `up` | Move forward / Accelerate |
| `down` | Move backward / Brake |
| `left` | Move left |
| `right` | Move right |
| `interact` | Interact |
| `flashlight` | Flashlight |
| `jump` | Jump |
| `crouch` | Crouch |
| `usetool` | Use tool |
| `grab` | Grab |
| `handbrake` | Handbrake |
| `map` | Map |
| `pause` | Pause game (escape) |
| `vehicleraise` | Raise vehicle parts |
| `vehiclelower` | Lower vehicle parts |
| `vehicleaction` | Vehicle action |
| `camerax` | Camera x movement, scaled by sensitivity. Only valid in InputValue. |
| `cameray` | Camera y movement, scaled by sensitivity. Only valid in InputValue. |
| `tool_group_prev` | Switch to previous tool group |
| `tool_group_next` | Switch to next tool group |
| `extra0` | Extra action 0 |
| `extra1` | Extra action 1 |
| `extra2` | Extra action 2 |
| `extra3` | Extra action 3 |
| `extra4` | Extra action 4 |
| `extra5` | Extra action 5 |
| `extra6` | Extra action 6 |
| `photomode` | Photomode |
| `zoom` | Zoom |
| `menu_left` | Menu left |
| `menu_right` | Menu right |
| `menu_up` | Menu up |
| `menu_down` | Menu down |
| `menu_next` | Menu next |
| `menu_prev` | Menu prev |
| `menu_accept` | Menu accept |
| `menu_cancel` | Menu cancel |

---

### [API] GetVersion

```lua
version = GetVersion()
```

**Example:**

```lua
function init()
	local v = GetVersion()
	--v is "0.5.0"
	DebugPrint(v)
end
```

---

### [API] HasVersion

```lua
match = HasVersion(version)
```

**Arguments:**

- `version` *(string)* — Reference version

**Example:**

```lua
function init()
	if HasVersion("1.5.0") then
		--conditional code that only works on 0.6.0 or above
		DebugPrint("New version")
	else
		--legacy code that works on earlier versions
		DebugPrint("Earlier version")
	end
end
```

---

### [API] GetTime

```lua
time = GetTime()
```

Returns running time of this script. If called from update, this returns the simulated time, otherwise it returns wall time.

**Example:**

```lua
function update()
	local t = GetTime()
	DebugPrint(t)
end
```

---

### [API] GetTimeStep

```lua
dt = GetTimeStep()
```

Returns timestep of the last frame. If called from update, this returns the simulation time step, which is always one 60th of a second (0.0166667). If called from tick or draw it returns the actual time since last frame.

**Example:**

```lua
function tick()
	local dt = GetTimeStep()
	DebugPrint("tick dt: " .. dt)
end

function update()
	local dt = GetTimeStep()
	DebugPrint("update dt: " .. dt)
end
```

---

### [API] InputLastPressedKey

```lua
name = InputLastPressedKey()
```

**Example:**

```lua
function tick()
	local name = InputLastPressedKey()
	if string.len(name) > 0 then
		DebugPrint(name) 
	end
end
```

---

### [API] InputPressed

```lua
pressed = InputPressed(input)
```

**Arguments:**

- `input` *(string)* — The input identifier

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		DebugPrint("interact")
	end
end
```

---

### [API] InputReleased

```lua
pressed = InputReleased(input)
```

**Arguments:**

- `input` *(string)* — The input identifier

**Example:**

```lua
function tick()
	if InputReleased("interact") then
		DebugPrint("interact")
	end
end
```

---

### [API] InputDown

```lua
pressed = InputDown(input)
```

**Arguments:**

- `input` *(string)* — The input identifier

**Example:**

```lua
function tick()
	if InputDown("interact") then
		DebugPrint("interact")
	end
end
```

---

### [API] InputValue

```lua
value = InputValue(input)
```

**Arguments:**

- `input` *(string)* — The input identifier

**Example:**

```lua
local scrollPos = 0
function tick()
	scrollPos = scrollPos + InputValue("mousewheel")
	DebugPrint(scrollPos)
end
```

---

### [API] InputClear

```lua
InputClear()
```

All player input is "forgotten" by the game after calling this function

**Example:**

```lua
function update()
    -- Prints '2' because InputClear() allows the game to "forget" the player's input
	if InputDown("interact") then
        InputClear()
		if InputDown("interact") then
			DebugPrint(1)
		else
			DebugPrint(2)
		end
	end
end
```

---

### [API] InputResetOnTransition

```lua
InputResetOnTransition()
```

This function will reset everything we need to reset during state transition

**Example:**

```lua
function update()
	if InputDown("interact") then
        -- In this form, you won't be able to notice the result of the function; you need a specific context
		InputResetOnTransition()
	end
end
```

---

### [API] LastInputDevice

```lua
value = LastInputDevice()
```

Returns the last input device id. 0 - none, 1 - mouse, 2 - gamepad

**Example:**

```lua
#include "ui/ui_helpers.lua"

function update()
	if LastInputDevice() == UI_DEVICE_GAMEPAD then
		DebugPrint("Last input was from gamepad")
	elseif LastInputDevice() == UI_DEVICE_MOUSE then
		DebugPrint("Last input was mouse & keyboard")
	elseif LastInputDevice() == UI_DEVICE_TOUCHSCREEN then
		DebugPrint("Last input was touchscreen")
	end
end
```

---

### [API] SetValue

```lua
SetValue(variable, value, [transition], [time])
```

Set value of a number variable in the global context with an optional transition. If a transition is provided the value will animate from current value to the new value during the transition time. Transition can be one of the following: Transition Description linear Linear transitioncosine Slow at beginning and endeasein Slow at beginningeaseout Slow at endbounce Bounce and overshoot new value

**Arguments:**

- `variable` *(string)* — Name of number variable in the global context

- `value` *(number)* — The new value

- `transition` *(string, optional)* — Transition type. See description.

- `time` *(number, optional)* — Transition time (seconds)

**Example:**

```lua
myValue = 0
function tick()
	--This will change the value of myValue from 0 to 1 in a linear fasion over 0.5 seconds
	SetValue("myValue", 1, "linear", 0.5)
	DebugPrint(myValue)
end
```

---

### [API] SetValueInTable

```lua
SetValueInTable(tableId, memberName, newValue, type, length)
```

Chages the value of a table member in time according to specified args. Works similar to SetValue but for global variables of trivial types

**Arguments:**

- `tableId` *(table)* — Id of the table

- `memberName` *(string)* — Name of the member

- `newValue` *(number)* — New value

- `type` *(string)* — Transition type

- `length` *(number)* — Transition length

**Example:**

```lua
local t = {}
function init()
	SetValueInTable(t, "score", 1, "number", 1)
end
function update()
	if InputPressed("interact") then
		SetValueInTable(t, "score", t.score + 1, "number", 1)
        DebugPrint(t.score)
	end
end
```

---

### [API] PauseMenuButton

```lua
clicked = PauseMenuButton(title, [location])
```

Calling this function will add a button on the bottom bar or in the main pause menu (center of the screen) when the game is paused. Identified by 'location' parameter, it can be below "Main menu" button (by passing "main_bottom" value)or above (by passing "main_top"). A primary button will be placed in the main pause menu if this function is called from a playable mod. There can be only one primary button. Use this as a way to bring up mod settings or other user interfaces while the game is running. Call this function every frame from the tick function for as long as the pause menu button should still be visible. Only one button per script is allowed. Consecutive calls replace button added in previous calls.

**Arguments:**

- `title` *(string)* — Text on button

- `location` *(string, optional)* — Button location. If "bottom_bar" - bottom bar, if "main_bottom" - below "Main menu" button, if "main_top" - above "Main menu" button. Default "bottom_bar".

**Example:**

```lua
function tick()

    -- Primary button which will be placed in the main pause menu below "Main menu" button
	if PauseMenuButton("Back to Hub", "main_bottom") then
		StartLevel("hub", "level/hub.xml")
	end

	-- Primary button which will be placed in the main pause menu above "Main menu" button
	if PauseMenuButton("Back to Hub", "main_top") then
		StartLevel("hub", "level/hub.xml")
	end
	
	-- Button will be placed in the bottom bar of the pause menu
	if PauseMenuButton("MyMod Settings") then
		visible = true
	end
end

function draw()
	if visible then
		UiMakeInteractive()
	end
end
```

---

### [API] HasFile

```lua
exists = HasFile(path)
```

Checks that file exists on the specified path. It is preferable to use UiHasImage whenever possible - it has better performance

**Arguments:**

- `path` *(string)* — Path to file

**Example:**

```lua
local file = "gfx/circle.png"

function draw()
	if HasFile(image) then
		DebugPrint("file " .. file .. " exists")
	end
end
```

---

### [API] StartLevel

```lua
StartLevel(mission, path, [layers], [passThrough])
```

Start a level

**Arguments:**

- `mission` *(string)* — An identifier of your choice

- `path` *(string)* — Path to level XML file

- `layers` *(string, optional)* — Active layers. Default is no layers.

- `passThrough` *(boolean, optional)* — If set, loading screen will have no text and music will keep playing

**Example:**

```lua
function init()
	--Start level with no active layers
	StartLevel("level1", "MOD/level1.xml")

	--Start level with two layers
	StartLevel("level1", "MOD/level1.xml", "vehicles targets")
end
```

---

### [API] SetPaused

```lua
SetPaused(paused)
```

Set paused state of the game

**Arguments:**

- `paused` *(boolean)* — True if game should be paused

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		--Pause game and bring up pause menu on HUD
		SetPaused(true)
	end
end
```

---

### [API] Restart

```lua
Restart()
```

Restart level

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		Restart()
	end
end
```

---

### [API] Menu

```lua
Menu()
```

Go to main menu

**Example:**

```lua
function tick()
	if InputPressed("interact") then
		Menu()
	end
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | **Script control** | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)