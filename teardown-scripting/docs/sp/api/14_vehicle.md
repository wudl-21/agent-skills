# Teardown SP API — Vehicle
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Vehicles are set up in the editor and consists of multiple parts owned by a vehicle entity.

---

### [API] FindVehicle

```lua
handle = FindVehicle([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	local vehicle = FindVehicle("mycar")
end
```

---

### [API] FindVehicles

```lua
list = FindVehicles([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	--Find all vehicles in level tagged "boat"
	local boats = FindVehicles("boat")
	for i=1, #boats do
		local boat = boats[i]
		DebugPrint(boat)
	end
end
```

---

### [API] GetVehicleTransform

```lua
transform = GetVehicleTransform(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function init()
	local vehicle = FindVehicle("vehicle")
	local t = GetVehicleTransform(vehicle)
end
```

---

### [API] GetVehicleExhaustTransforms

```lua
transforms = GetVehicleExhaustTransforms(vehicle)
```

Returns the exhausts transforms in local space of the vehicle.

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleExhaustTransforms(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

---

### [API] GetVehicleVitalTransforms

```lua
transforms = GetVehicleVitalTransforms(vehicle)
```

Returns the vitals transforms in local space of the vehicle.

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleVitalTransforms(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

---

### [API] GetVehicleBodies

```lua
transforms = GetVehicleBodies(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function tick()
	local vehicle = FindVehicle("car", true)
	local t = GetVehicleBodies(vehicle)
	for i = 1, #t do
		DebugWatch(tostring(i), t[i])
	end
end
```

---

### [API] GetVehicleBody

```lua
body = GetVehicleBody(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function init()
	local vehicle = FindVehicle("vehicle")
	local body = GetVehicleBody(vehicle)
	if IsBodyBroken(body) then
		DebugPrint("Is broken")
	end
end
```

---

### [API] GetVehicleHealth

```lua
health = GetVehicleHealth(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function init()
	local vehicle = FindVehicle("vehicle")
	local health = GetVehicleHealth(vehicle)
	DebugPrint(health)
end
```

---

### [API] GetVehicleParams

```lua
params = GetVehicleParams(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function tick()
	local params = GetVehicleParams(FindVehicle("car", true))
	for key, value in pairs(params) do
		DebugWatch(key, value)
	end
end
```

---

### [API] SetVehicleParam

```lua
SetVehicleParam(handle, param, value)
```

Available parameters: spring, damping, topspeed, acceleration, strength, antispin, antiroll, difflock, steerassist, friction

**Arguments:**

- `handle` *(number)* — Vehicle handler

- `param` *(string)* — Param name

- `value` *(number)* — Param value

**Example:**

```lua
function init()
	SetVehicleParam(FindVehicle("car", true), "topspeed", 200)
end
```

---

### [API] GetVehicleDriverPos

```lua
pos = GetVehicleDriverPos(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function init()
	local vehicle = FindVehicle("vehicle")
	local driverPos = GetVehicleDriverPos(vehicle)
	local t = GetVehicleTransform(vehicle)
	local worldPos = TransformToParentPoint(t, driverPos)
	DebugPrint(worldPos)
end
```

---

### [API] GetVehicleSteering

```lua
steering = GetVehicleSteering(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
local steering = GetVehicleSteering(vehicle)
```

---

### [API] GetVehicleDrive

```lua
drive = GetVehicleDrive(vehicle)
```

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
local drive = GetVehicleDrive(vehicle)
```

---

### [API] DriveVehicle

```lua
DriveVehicle(vehicle, drive, steering, handbrake)
```

This function applies input to vehicles, allowing for autonomous driving. The vehicle will be turned on automatically and turned off when no longer called. Call this from the tick function, not update.

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

- `drive` *(number)* — Reverse/forward control -1 to 1

- `steering` *(number)* — Left/right control -1 to 1

- `handbrake` *(boolean)* — Handbrake control

**Example:**

```lua
function tick()
	--Drive mycar forwards
	local v = FindVehicle("mycar")
	DriveVehicle(v, 1, 0, false)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | **Vehicle** | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)