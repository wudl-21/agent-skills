# Teardown SP API — Trigger
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Triggers can be placed in the scene and queried by scripts to see if something is within a certain part of the scene.

---

### [API] FindTrigger

```lua
handle = FindTrigger([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	local goal = FindTrigger("goal")
end
```

---

### [API] FindTriggers

```lua
list = FindTriggers([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	--Find triggers tagged "toxic" in script scope
	local triggers = FindTriggers("toxic")
	for i=1, #triggers do
		local trigger = triggers[i]
		DebugPrint(trigger)
	end
end
```

---

### [API] GetTriggerTransform

```lua
transform = GetTriggerTransform(handle)
```

**Arguments:**

- `handle` *(number)* — Trigger handle

**Example:**

```lua
function init()
	local trigger = FindTrigger("toxic")
	local t = GetTriggerTransform(trigger)
	DebugPrint(t.pos)
end
```

---

### [API] SetTriggerTransform

```lua
SetTriggerTransform(handle, transform)
```

**Arguments:**

- `handle` *(number)* — Trigger handle

- `transform` *(TTransform)* — Desired trigger transform in world space

**Example:**

```lua
function init()
	local trigger = FindTrigger("toxic")
	local t = Transform(Vec(0, 1, 0), QuatEuler(0, 90, 0))
	SetTriggerTransform(trigger, t)
end
```

---

### [API] GetTriggerBounds

```lua
min, max = GetTriggerBounds(handle)
```

Return the lower and upper points in world space of the trigger axis aligned bounding box

**Arguments:**

- `handle` *(number)* — Trigger handle

**Example:**

```lua
function init()
	local trigger = FindTrigger("toxic")
	local mi, ma = GetTriggerBounds(trigger)
	
	local list = QueryAabbShapes(mi, ma)
	for i = 1, #list do
		DebugPrint(list[i])
	end
end
```

---

### [API] IsBodyInTrigger

```lua
inside = IsBodyInTrigger(trigger, body)
```

This function will only check the center point of the body

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `body` *(number)* — Body handle

**Example:**

```lua
local trigger = 0
local body = 0
function init()
	trigger = FindTrigger("toxic")
	body = FindBody("body")
end

function tick()
	if IsBodyInTrigger(trigger, body) then
		DebugPrint("In trigger!")
	end
end
```

---

### [API] IsVehicleInTrigger

```lua
inside = IsVehicleInTrigger(trigger, vehicle)
```

This function will only check origo of vehicle

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
local trigger = 0
local vehicle = 0
function init()
	trigger = FindTrigger("toxic")
	vehicle = FindVehicle("vehicle")
end

function tick()
	if IsVehicleInTrigger(trigger, vehicle) then
		DebugPrint("In trigger!")
	end
end
```

---

### [API] IsShapeInTrigger

```lua
inside = IsShapeInTrigger(trigger, shape)
```

This function will only check the center point of the shape

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `shape` *(number)* — Shape handle

**Example:**

```lua
local trigger = 0
local shape = 0
function init()
	trigger = FindTrigger("toxic")
	shape = FindShape("shape")
end

function tick()
	if IsShapeInTrigger(trigger, shape) then
		DebugPrint("In trigger!")
	end
end
```

---

### [API] IsPointInTrigger

```lua
inside = IsPointInTrigger(trigger, point)
```

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `point` *(TVec)* — Word space point as vector

**Example:**

```lua
local trigger = 0
local point = {}
function init()
	trigger = FindTrigger("toxic", true)
	point = Vec(0, 0, 0)
end

function tick()
	if IsPointInTrigger(trigger, point) then
		DebugPrint("In trigger!")
	end
end
```

---

### [API] IsPointInBoundaries

```lua
value = IsPointInBoundaries(point)
```

Checks whether the point is within the scene boundaries. If there are no boundaries on the scene, the function returns True.

**Arguments:**

- `point` *(TVec)* — Point

**Example:**

```lua
function tick()
	local p = Vec(1.5, 3, 2.5)
	DebugWatch("In boundaries", IsPointInBoundaries(p))
end
```

---

### [API] IsTriggerEmpty

```lua
empty, maxpoint = IsTriggerEmpty(handle, [demolision])
```

This function will check if trigger is empty. If trigger contains any part of a body it will return false and the highest point as second return value.

**Arguments:**

- `handle` *(number)* — Trigger handle

- `demolision` *(boolean, optional)* — If true, small debris and vehicles are ignored

**Example:**

```lua
local trigger = 0
function init()
	trigger = FindTrigger("toxic")
end

function tick()
	local empty, highPoint = IsTriggerEmpty(trigger)
	if not empty then
		--highPoint[2] is the tallest point in trigger
		DebugPrint("Is not empty")
	end
end
```

---

### [API] GetTriggerDistance

```lua
distance = GetTriggerDistance(trigger, point)
```

Get distance to the surface of trigger volume. Will return negative distance if inside.

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `point` *(TVec)* — Word space point as vector

**Example:**

```lua
local trigger = 0
function init()
	trigger = FindTrigger("toxic")
	local p = Vec(0, 10, 0)
	local dist = GetTriggerDistance(trigger, p)
	DebugPrint(dist)
end
```

---

### [API] GetTriggerClosestPoint

```lua
closest = GetTriggerClosestPoint(trigger, point)
```

Return closest point in trigger volume. Will return the input point itself if inside trigger or closest point on surface of trigger if outside.

**Arguments:**

- `trigger` *(number)* — Trigger handle

- `point` *(TVec)* — Word space point as vector

**Example:**

```lua
local trigger = 0
function init()
	trigger = FindTrigger("toxic")
	local p = Vec(0, 10, 0)
	local closest = GetTriggerClosestPoint(trigger, p)
	DebugPrint(closest)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | **Trigger** | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)