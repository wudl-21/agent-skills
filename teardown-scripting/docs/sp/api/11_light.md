# Teardown SP API — Light
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Light sources can be of several differnt types and configured in the editor. If a light source is owned by a shape, the intensity of the light source is scaled by the emissive scale of that shape. If the parent shape breaks, the emissive scale is set to zero and the light source is disabled. A light source without a parent shape will always emit light, unless exlicitly disabled by a script.

---

### [API] FindLight

```lua
handle = FindLight([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	local light = FindLight("main")
	DebugPrint(light)
end
```

---

### [API] FindLights

```lua
list = FindLights([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	--Search for lights tagged "main" in script scope
	local lights = FindLights("main")
	for i=1, #lights do
		local light = lights[i]
		DebugPrint(light)
	end
end
```

---

### [API] SetLightEnabled

```lua
SetLightEnabled(handle, enabled)
```

If light is owned by a shape, the emissive scale of that shape will be set to 0.0 when light is disabled and 1.0 when light is enabled.

**Arguments:**

- `handle` *(number)* — Light handle

- `enabled` *(boolean)* — Set to true if light should be enabled

**Example:**

```lua
function init()
	SetLightEnabled(FindLight("main"), false)
end
```

---

### [API] SetLightColor

```lua
SetLightColor(handle, r, g, b)
```

This will only set the color tint of the light. Use SetLightIntensity for brightness. Setting the light color will not affect the emissive color of a parent shape.

**Arguments:**

- `handle` *(number)* — Light handle

- `r` *(number)* — Red value

- `g` *(number)* — Green value

- `b` *(number)* — Blue value

**Example:**

```lua
function init()
	--Set light color to yellow
	SetLightColor(FindLight("main"), 1, 1, 0)
end
```

---

### [API] SetLightIntensity

```lua
SetLightIntensity(handle, intensity)
```

If the shape is owned by a shape you most likely want to use SetShapeEmissiveScale instead, which will affect both the emissiveness of the shape and the brightness of the light at the same time.

**Arguments:**

- `handle` *(number)* — Light handle

- `intensity` *(number)* — Desired intensity of the light

**Example:**

```lua
function init()
	--Pulsate light
	SetLightIntensity(FindLight("main"), math.sin(GetTime())*0.5 + 1.0)
end
```

---

### [API] GetLightTransform

```lua
transform = GetLightTransform(handle)
```

Lights that are owned by a dynamic shape are automatcially moved with that shape

**Arguments:**

- `handle` *(number)* — Light handle

**Example:**

```lua
local light = 0
function init()
	light = FindLight("main")
	local t = GetLightTransform(light)
	DebugPrint(VecStr(t.pos))
end
```

---

### [API] GetLightShape

```lua
handle = GetLightShape(handle)
```

**Arguments:**

- `handle` *(number)* — Light handle

**Example:**

```lua
local light = 0
function init()
	light = FindLight("main")
	local shape = GetLightShape(light)
	DebugPrint(shape)
end
```

---

### [API] IsLightActive

```lua
active = IsLightActive(handle)
```

**Arguments:**

- `handle` *(number)* — Light handle

**Example:**

```lua
local light = 0
function init()
	light = FindLight("main")
	if IsLightActive(light) then
		DebugPrint("Light is active")
	end
end
```

---

### [API] IsPointAffectedByLight

```lua
affected = IsPointAffectedByLight(handle, point)
```

**Arguments:**

- `handle` *(number)* — Light handle

- `point` *(TVec)* — World space point as vector

**Example:**

```lua
local light = 0
function init()
	light = FindLight("main")
	local point = Vec(0, 10, 0)
	local affected = IsPointAffectedByLight(light, point)
	DebugPrint(affected)
end
```

---

### [API] GetFlashlight

```lua
handle = GetFlashlight()
```

Returns the handle of the player's flashlight. You can work with it as with an entity of the Light type.

**Example:**

```lua
function tick()
	local flashlight = GetFlashlight()
	SetProperty(flashlight, "color", Vec(0.5, 0, 1))
end
```

---

### [API] SetFlashlight

```lua
SetFlashlight(handle)
```

Sets a new entity of the Light type as a flashlight.

**Arguments:**

- `handle` *(number)* — Handle of the light

**Example:**

```lua
local oldLight = 0
function tick()
	-- in order not to lose the original flashlight, it is better to save it's handle
	oldLight = GetFlashlight()
	SetFlashlight(FindEntity("mylight", true))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | **Light** | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)