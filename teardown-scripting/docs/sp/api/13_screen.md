# Teardown SP API — Screen
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Screens display the content of UI scripts and can be made interactive.

---

### [API] FindScreen

```lua
handle = FindScreen([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	local screen = FindScreen("tv")
	DebugPrint(screen)
end
```

---

### [API] FindScreens

```lua
list = FindScreens([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	--Find screens tagged "tv" in script scope
	local screens = FindScreens("tv")
	for i=1, #screens do
		local screen = screens[i]
		DebugPrint(screen)
	end
end
```

---

### [API] SetScreenEnabled

```lua
SetScreenEnabled(screen, enabled)
```

Enable or disable screen

**Arguments:**

- `screen` *(number)* — Screen handle

- `enabled` *(boolean)* — True if screen should be enabled

**Example:**

```lua
function init()
	SetScreenEnabled(FindScreen("tv"), true)
end
```

---

### [API] IsScreenEnabled

```lua
enabled = IsScreenEnabled(screen)
```

**Arguments:**

- `screen` *(number)* — Screen handle

**Example:**

```lua
function init()
	local b = IsScreenEnabled(FindScreen("tv"))
	DebugPrint(b)
end
```

---

### [API] GetScreenShape

```lua
shape = GetScreenShape(screen)
```

Return handle to the parent shape of a screen

**Arguments:**

- `screen` *(number)* — Screen handle

**Example:**

```lua
local screen = 0
function init()
	screen = FindScreen("tv")
	local shape = GetScreenShape(screen)
	DebugPrint(shape)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | **Screen** | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)