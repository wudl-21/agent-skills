# Script Control

## Input Keys

| Physical input | Description |
| --- | --- |
| esc | Escape key |
| tab | Tab key |
| lmb | Left mouse button |
| rmb | Right mouse button |
| mmb | Middle mouse button |
| uparrow | Up arrow key |
| downarrow | Down arrow key |
| leftarrow | Left arrow key |
| rightarrow | Right arrow key |
| f1-f12 | Function keys |
| backspace | Backspace key |
| alt | Alt key |
| delete | Delete key |
| home | Home key |
| end | End key |
| pgup | Pgup key |
| pgdown | Pgdown key |
| insert | Insert key |
| space | Space bar |
| shift | Shift key |
| ctrl | Ctrl key |
| return | Return key |
| any | Any key or button |
| a,b,c,... | Latin, alphabetical keys a through z |
| 0-9 | Digits, zero to nine |
| mousedx | Mouse horizontal diff. Only valid in InputValue. |
| mousedy | Mouse vertical diff. Only valid in InputValue. |
| mousewheel | Mouse wheel. Only valid in InputValue. |

| Logical input | Description |
| --- | --- |
| up | Move forward / Accelerate |
| down | Move backward / Brake |
| left | Move left |
| right | Move right |
| interact | Interact |
| flashlight | Flashlight |
| jump | Jump |
| crouch | Crouch |
| usetool | Use tool |
| grab | Grab |
| handbrake | Handbrake |
| map | Map |
| pause | Pause game (escape) |
| vehicleraise | Raise vehicle parts |
| vehiclelower | Lower vehicle parts |
| vehicleaction | Vehicle action |
| camerax | Camera x movement, scaled by sensitivity. Only valid in InputValue. |
| cameray | Camera y movement, scaled by sensitivity. Only valid in InputValue. |
| tool_group_prev | Switch to previous tool group |
| tool_group_next | Switch to next tool group |
| extra0 | Extra action 0 |
| extra1 | Extra action 1 |
| extra2 | Extra action 2 |
| extra3 | Extra action 3 |
| extra4 | Extra action 4 |
| extra5 | Extra action 5 |
| extra6 | Extra action 6 |
| photomode | Photomode |
| zoom | Zoom |
| menu_left | Menu left |
| menu_right | Menu right |
| menu_up | Menu up |
| menu_down | Menu down |
| menu_next | Menu next |
| menu_prev | Menu prev |
| menu_accept | Menu accept |
| menu_cancel | Menu cancel |

## Functions

### [API] version = GetVersion()
- **Returns:**
  - `version` *(string)* — Dot separated string of current version of the game
```lua
function init()
	local v = GetVersion()
	--v is "0.5.0"
	DebugPrint(v)
end
```

### [API] match = HasVersion(version)
- **Args:**
  - `version` *(string)* — Reference version
- **Returns:**
  - `match` *(boolean)* — True if current version is at least provided one
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

### [API] time = GetTime()
- **Returns:**
  - `time` *(number)* — The time in seconds since level was started
```lua
function client.update()
	local t = GetTime()
	DebugPrint(t)
end
```

### [API] dt = GetTimeStep()
- **Returns:**
  - `dt` *(number)* — The timestep in seconds
```lua
function client.tick()
	local dt = GetTimeStep()
	DebugPrint("tick dt: " .. dt)
end

function client.update()
	local dt = GetTimeStep()
	DebugPrint("update dt: " .. dt)
end
```

### [API] name = InputLastPressedKey([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `name` *(string)* — Name of last pressed key, empty if no key is pressed
```lua
function client.tick()
	local name = InputLastPressedKey()
	if string.len(name) > 0 then
		DebugPrint(name)
	end
end
```

### [API] pressed = InputPressed(input, [playerId])
- **Args:**
  - `input` *(string)* — The input identifier
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `pressed` *(boolean)* — True if input was pressed during last frame
```lua
function client.tick()
	if InputPressed("interact") then
		DebugPrint("interact")
	end
end
```

### [API] pressed = InputReleased(input, [playerId])
- **Args:**
  - `input` *(string)* — The input identifier
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `pressed` *(boolean)* — True if input was released during last frame
```lua
function client.tick()
	if InputReleased("interact") then
		DebugPrint("interact")
	end
end
```

### [API] pressed = InputDown(input, [playerId])
- **Args:**
  - `input` *(string)* — The input identifier
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `pressed` *(boolean)* — True if input is currently held down
```lua
function client.tick()
	if InputDown("interact") then
		DebugPrint("interact")
	end
end
```

### [API] value = InputValue(input, [playerId])
- **Args:**
  - `input` *(string)* — The input identifier
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `value` *(number)* — Depends on input type
```lua
local scrollPos = 0
function client.tick()
	scrollPos = scrollPos + InputValue("mousewheel")
	DebugPrint(scrollPos)
end
```

### [API] InputClear()
```lua
function client.update()
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

### [API] InputResetOnTransition()
```lua
function update()
	if InputDown("interact") then
        -- In this form, you won't be able to notice the result of the function; you need a specific context
		InputResetOnTransition()
	end
end
```

### [API] value = LastInputDevice()
- **Returns:**
  - `value` *(number)* — Last device id
```lua
#include "ui/ui_helpers.lua"

function client.update()
	if LastInputDevice() == UI_DEVICE_GAMEPAD then
		DebugPrint("Last input was from gamepad")
	elseif LastInputDevice() == UI_DEVICE_MOUSE then
		DebugPrint("Last input was mouse & keyboard")
	elseif LastInputDevice() == UI_DEVICE_TOUCHSCREEN then
		DebugPrint("Last input was touchscreen")
	end
end
```

### [API] SetValue(variable, value, [transition], [time])
- **Args:**
  - `variable` *(string)* — Name of number variable in the global context
  - `value` *(number)* — The new value
  - `transition` *(string, optional)* — Transition type. See description.
  - `time` *(number, optional)* — Transition time (seconds)
| Transition | Description |
| --- | --- |
| linear | Linear transition |
| cosine | Slow at beginning and end |
| easein | Slow at beginning |
| easeout | Slow at end |
| bounce | Bounce and overshoot new value |
```lua
myValue = 0
function client.tick()
	--This will change the value of myValue from 0 to 1 in a linear fasion over 0.5 seconds
	SetValue("myValue", 1, "linear", 0.5)
	DebugPrint(myValue)
end
```

### [API] SetValueInTable(tableId, memberName, newValue, type, length)
- **Args:**
  - `tableId` *(table)* — Id of the table
  - `memberName` *(string)* — Name of the member
  - `newValue` *(number)* — New value
  - `type` *(string)* — Transition type
  - `length` *(number)* — Transition length
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

### [API] clicked = PauseMenuButton(title, [location], [disabled])
- **Args:**
  - `title` *(string)* — Text on button
  - `location` *(string, optional)* — Button location. If "bottom_bar" - bottom bar, if "main_bottom" - below "Main menu" button, if "main_top" - above "Main menu" button. Default "bottom_bar".
  - `disabled` *(bool, optional)* — Disable button. Button will be rendered as grayed out. Default is false. Only available when used with "bottom_bar".
- **Returns:**
  - `clicked` *(boolean)* — True if clicked, false otherwise
```lua
function server.startLevel(mission, path)
	StartLevel(mission, path)
end

function server.respawnPlayer(player)
	-- Respawn player
end

function client.tick()


	for p in Players() do
		if IsPlayerHost(p) then
			-- Primary button which will be placed in the main pause menu below "Main menu" button
			if PauseMenuButton("Back to Hub", "main_bottom") then
				ServerCall("server.startLevel", "hub", "level/hub.xml")	
			end

			-- Primary button which will be placed in the main pause menu above "Main menu" button
			if PauseMenuButton("Back to Hub", "main_top") then
				ServerCall("server.startLevel", "hub", "level/hub.xml")
			end

			-- Button will be placed in the bottom bar of the pause menu
			if PauseMenuButton("MyMod Settings") then
				visible = true
			end
		else
			if PauseMenuButton("Respawn (wait 8s)", "bottom_bar", true) then
				ServerCall("server.respawnPlayer", p)
			end
		end
	end
end

function draw()
	if visible then
		UiMakeInteractive()
	end
end
```

### [API] exists = HasFile(path)
- **Args:**
  - `path` *(string)* — Path to file
- **Returns:**
  - `exists` *(boolean)* — True if file exists
```lua
local file = "gfx/circle.png"

function draw()
	if HasFile(image) then
		DebugPrint("file " .. file .. " exists")
	end
end
```

### [API] StartLevel(mission, path, [layers], [passThrough])
- **Args:**
  - `mission` *(string)* — An identifier of your choice
  - `path` *(string)* — Path to level XML file
  - `layers` *(string, optional)* — Active layers. Default is no layers.
  - `passThrough` *(boolean, optional)* — If set, loading screen will have no text and music will keep playing
```lua
function server.init()
	--Start level with no active layers
	StartLevel("level1", "MOD/level1.xml")

	--Start level with two layers
	StartLevel("level1", "MOD/level1.xml", "vehicles targets")
end
```

### [API] SetPaused(paused)
- **Args:**
  - `paused` *(boolean)* — True if game should be paused
```lua
function client.tick()
	if InputPressed("interact") then
		--Pause game and bring up pause menu on HUD
		SetPaused(true)
	end
end
```

### [API] Restart()
```lua
function server.tick()
	if InputPressed("interact") then
		Restart()
	end
end
```

### [API] Menu()
```lua
function client.tick()
	if InputPressed("interact") then
		Menu()
	end
end
```

### [API] ClientCall(playerId, function, [param1, param2, .., paramN])
- **Args:**
  - `playerId` *(number)* — Player ID of the recipient. Use 0 to broadcast to every player.
  - `function` *(string)* — Name of the function to be invoked. This function must exist within issuing script.
  - `param1, param2, .., paramN` *(any, optional)* — Optional parameters to send to the recipent(s). Arguments should match the signature of the specified function.
```lua
function server.tick()
	for p in Players() do
		if GetPlayerHealth(p) == 0) then
			ClientCall(p, "client.showRespawnBtn")
		end
	end
	
	if matchEnded then
		ClientCall(0, "client.displayParticles", "confetti", 200, 0.3, Vec(0, 30, 0))
	end
end

function client.showRespawnBtn()
	-- show respawn ui..
end

function client.displayParticles(particleName, amount, life, pos)
	-- spawn particles..
end
```

### [API] ServerCall(function, [param1, param2, .., paramN])
- **Args:**
  - `function` *(string)* — Name of the function to be invoked. This function must exist within issuing script.
  - `param1, param2, .., paramN` *(any, optional)* — Optional parameters to send to the server. Arguments should match the signature of the specified function.
```lua
function client.tick()
	if UiTextButton("I am Ready") then
		ServerCall("server.setPlayerReady", GetLocalPlayer()) 
	end
end

function server.setPlayerReady(playerId)
	shared.playersReady[playerId] = true
end
```

---
**Navigation:** [_INDEX](_INDEX.md)