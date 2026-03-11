# Screen

Screens display the content of UI scripts and can be made interactive.

### [API] handle = FindScreen([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first screen with specified tag or zero if not found
```lua
function client.init()
	local screen = FindScreen("tv")
	DebugPrint(screen)
end
```

### [API] list = FindScreens([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all screens with specified tag
```lua
function client.init()
	--Find screens tagged "tv" in script scope
	local screens = FindScreens("tv")
	for i=1, #screens do
		local screen = screens[i]
		DebugPrint(screen)
	end
end
```

### [API] SetScreenEnabled(screen, enabled)
- **Args:**
  - `screen` *(number)* — Screen handle
  - `enabled` *(boolean)* — True if screen should be enabled
```lua
function server.init()
	SetScreenEnabled(FindScreen("tv"), true)
end
```

### [API] enabled = IsScreenEnabled(screen)
- **Args:**
  - `screen` *(number)* — Screen handle
- **Returns:**
  - `enabled` *(boolean)* — True if screen is enabled
```lua
function client.init()
	local b = IsScreenEnabled(FindScreen("tv"))
	DebugPrint(b)
end
```

### [API] shape = GetScreenShape(screen)
- **Args:**
  - `screen` *(number)* — Screen handle
- **Returns:**
  - `shape` *(number)* — Shape handle or zero if none
```lua
local screen = 0
function client.init()
	screen = FindScreen("tv")
	local shape = GetScreenShape(screen)
	DebugPrint(shape)
end
```

### [API] GetScreenPlayer(screen, [playerId])
- **Args:**
  - `screen` *(number)* — Screen handle
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
local player = GetScreenPlayer(screen)
```

---
**Navigation:** [_INDEX](_INDEX.md)