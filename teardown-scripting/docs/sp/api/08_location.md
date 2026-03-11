# Teardown SP API — Location
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Locations are transforms placed in the editor as markers. Location transforms are always expressed in world space coordinates.

---

### [API] FindLocation

```lua
handle = FindLocation([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
local loc = 0
function init()
	loc = FindLocation("loc1")
end

function tick()
	DebugCross(GetLocationTransform(loc).pos)
end
```

---

### [API] FindLocations

```lua
list = FindLocations([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
local locations
function init()
	locations = FindLocations("loc1")

	for i=1, #locations do
		local loc = locations[i]
		DebugPrint(DebugPrint(loc))
	end
end
```

---

### [API] GetLocationTransform

```lua
transform = GetLocationTransform(handle)
```

**Arguments:**

- `handle` *(number)* — Location handle

**Example:**

```lua
local location = 0
function init()
	location = FindLocation("loc1")
	DebugPrint(VecStr(GetLocationTransform(location).pos))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | **Location** | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)