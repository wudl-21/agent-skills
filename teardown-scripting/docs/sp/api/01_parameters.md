# Teardown SP API — Parameters
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Scripts can have parameters defined in the level XML file. These serve as input to a specific instance of the script and can be used to configure various options and parameters of the script. While these parameters can be read at any time in the script, it's recommended to copy them to a global variable in or outside the init function.

---

### [API] GetIntParam

```lua
value = GetIntParam(name, default)
```

**Arguments:**

- `name` *(string)* — Parameter name

- `default` *(number)* — Default parameter value

**Example:**

```lua
function init()
	--Retrieve blinkcount parameter, or set to 5 if omitted
	local parameterBlinkCount = GetIntParam("blinkcount", 5)
	DebugPrint(parameterBlinkCount)
end
```

---

### [API] GetFloatParam

```lua
value = GetFloatParam(name, default)
```

**Arguments:**

- `name` *(string)* — Parameter name

- `default` *(number)* — Default parameter value

**Example:**

```lua
function init()
	--Retrieve speed parameter, or set to 10.0 if omitted
	local parameterSpeed = GetFloatParam("speed", 10.0)
	DebugPrint(parameterSpeed)
end
```

---

### [API] GetBoolParam

```lua
value = GetBoolParam(name, default)
```

**Arguments:**

- `name` *(string)* — Parameter name

- `default` *(boolean)* — Default parameter value

**Example:**

```lua
function init()
	--Retrieve playsound parameter, or false if omitted
	local parameterPlaySound = GetBoolParam("playsound", false)
	DebugPrint(parameterPlaySound)
end
```

---

### [API] GetStringParam

```lua
value = GetStringParam(name, default)
```

**Arguments:**

- `name` *(string)* — Parameter name

- `default` *(string)* — Default parameter value

**Example:**

```lua
function init()
	--Retrieve mode parameter, or "idle" if omitted
	local parameterMode = GetStringParam("mode", "idle")
	DebugPrint(parameterMode)
end
```

---

### [API] GetColorParam

```lua
value = GetColorParam(name, default)
```

**Arguments:**

- `name` *(string)* — Parameter name

- `default` *(number)* — Default parameter value

**Example:**

```lua
function init()
	--Retrieve color parameter, or set to 0.39, 0.39, 0.39 if omitted
	local color_r, color_g, color_b = GetColorParam("color", 0.39, 0.39, 0.39)
	DebugPrint(color_r .. " " .. color_g .. " " .. color_b)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | **Parameters** | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)