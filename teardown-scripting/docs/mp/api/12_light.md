# Light

Light sources can be of several differnt types and configured in the editor. If a light source is owned
by a shape, the intensity of the light source is scaled by the emissive scale of that shape. If the
parent shape breaks, the emissive scale is set to zero and the light source is disabled. A light source
without a parent shape will always emit light, unless exlicitly disabled by a script.

### [API] handle = FindLight([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first light with specified tag or zero if not found
```lua
function client.init()
	local light = FindLight("main")
	DebugPrint(light)
end
```

### [API] list = FindLights([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all lights with specified tag
```lua
function client.init()
	--Search for lights tagged "main" in script scope
	local lights = FindLights("main")
	for i=1, #lights do
		local light = lights[i]
		DebugPrint(light)
	end
end
```

### [API] SetLightEnabled(handle, enabled)
- **Args:**
  - `handle` *(number)* — Light handle
  - `enabled` *(boolean)* — Set to true if light should be enabled
```lua
function server.init()
	SetLightEnabled(FindLight("main"), false)
end
```

### [API] SetLightColor(handle, r, g, b)
- **Args:**
  - `handle` *(number)* — Light handle
  - `r` *(number)* — Red value
  - `g` *(number)* — Green value
  - `b` *(number)* — Blue value
```lua
function server.init()
	--Set light color to yellow
	SetLightColor(FindLight("main"), 1, 1, 0)
end
```

### [API] SetLightIntensity(handle, intensity)
- **Args:**
  - `handle` *(number)* — Light handle
  - `intensity` *(number)* — Desired intensity of the light
```lua
function server.init()
	--Pulsate light
	SetLightIntensity(FindLight("main"), math.sin(GetTime())*0.5 + 1.0)
end
```

### [API] transform = GetLightTransform(handle)
- **Args:**
  - `handle` *(number)* — Light handle
- **Returns:**
  - `transform` *(TTransform)* — World space light transform
```lua
local light = 0
function client.init()
	light = FindLight("main")
	local t = GetLightTransform(light)
	DebugPrint(VecStr(t.pos))
end
```

### [API] handle = GetLightShape(handle)
- **Args:**
  - `handle` *(number)* — Light handle
- **Returns:**
  - `handle` *(number)* — Shape handle or zero if not attached to shape
```lua
local light = 0
function client.init()
	light = FindLight("main")
	local shape = GetLightShape(light)
	DebugPrint(shape)
end
```

### [API] active = IsLightActive(handle)
- **Args:**
  - `handle` *(number)* — Light handle
- **Returns:**
  - `active` *(boolean)* — True if light is currently emitting light
```lua
local light = 0
function client.init()
	light = FindLight("main")
	if IsLightActive(light) then
		DebugPrint("Light is active")
	end
end
```

### [API] affected = IsPointAffectedByLight(handle, point)
- **Args:**
  - `handle` *(number)* — Light handle
  - `point` *(TVec)* — World space point as vector
- **Returns:**
  - `affected` *(boolean)* — Return true if point is in light cone and range
```lua
local light = 0
function client.init()
	light = FindLight("main")
	local point = Vec(0, 10, 0)
	local affected = IsPointAffectedByLight(light, point)
	DebugPrint(affected)
end
```

### [API] handle = GetFlashlight([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
- **Returns:**
  - `handle` *(number)* — Handle of the player's flashlight
```lua
function setFlashlightColor(playerId)
	local flashlight = GetFlashlight(playerId)
	SetProperty(flashlight, "color", Vec(0.5, 0, 1))
end
```

### [API] SetFlashlight(handle, [playerId])
- **Args:**
  - `handle` *(number)* — Handle of the light
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
local oldLight = 0
function server.tick()
	... -- some code
	-- in order not to lose the original flashlight, it is better to save it's handle
	oldLight = GetFlashlight(playerId)
	SetFlashlight(FindEntity("mylight", true), playerId)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)