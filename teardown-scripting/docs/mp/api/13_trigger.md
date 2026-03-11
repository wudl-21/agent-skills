# Trigger

Triggers can be placed in the scene and queried by scripts to see if something is within a certain part
of the scene.

### [API] handle = FindTrigger([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first trigger with specified tag or zero if not found
```lua
function server.init()
	local goal = FindTrigger("goal")
end
```

### [API] list = FindTriggers([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all triggers with specified tag
```lua
function client.init()
	--Find triggers tagged "toxic" in script scope
	local triggers = FindTriggers("toxic")
	for i=1, #triggers do
		local trigger = triggers[i]
		DebugPrint(trigger)
	end
end
```

### [API] transform = GetTriggerTransform(handle)
- **Args:**
  - `handle` *(number)* — Trigger handle
- **Returns:**
  - `transform` *(TTransform)* — Current trigger transform in world space
```lua
function client.init()
	local trigger = FindTrigger("toxic")
	local t = GetTriggerTransform(trigger)
	DebugPrint(t.pos)
end
```

### [API] SetTriggerTransform(handle, transform)
- **Args:**
  - `handle` *(number)* — Trigger handle
  - `transform` *(TTransform)* — Desired trigger transform in world space
```lua
function server.init()
	local trigger = FindTrigger("toxic")
	local t = Transform(Vec(0, 1, 0), QuatEuler(0, 90, 0))
	SetTriggerTransform(trigger, t)
end
```

### [API] min, max = GetTriggerBounds(handle)
- **Args:**
  - `handle` *(number)* — Trigger handle
- **Returns:**
  - `min` *(TVec)* — Lower point of trigger bounds in world space
  - `max` *(TVec)* — Upper point of trigger bounds in world space
```lua
function client.init()
	local trigger = FindTrigger("toxic")
	local mi, ma = GetTriggerBounds(trigger)

	local list = QueryAabbShapes(mi, ma)
	for i = 1, #list do
		DebugPrint(list[i])
	end
end
```

### [API] inside = IsBodyInTrigger(trigger, body)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `body` *(number)* — Body handle
- **Returns:**
  - `inside` *(boolean)* — True if body is in trigger volume
```lua
local trigger = 0
local body = 0
function client.init()
	trigger = FindTrigger("toxic")
	body = FindBody("body")
end

function client.tick()
	if IsBodyInTrigger(trigger, body) then
		DebugPrint("In trigger!")
	end
end
```

### [API] inside = IsVehicleInTrigger(trigger, vehicle)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `vehicle` *(number)* — Vehicle handle
- **Returns:**
  - `inside` *(boolean)* — True if vehicle is in trigger volume
```lua
local trigger = 0
local vehicle = 0
function client.init()
	trigger = FindTrigger("toxic")
	vehicle = FindVehicle("vehicle")
end

function client.tick()
	if IsVehicleInTrigger(trigger, vehicle) then
		DebugPrint("In trigger!")
	end
end
```

### [API] inside = IsShapeInTrigger(trigger, shape)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `shape` *(number)* — Shape handle
- **Returns:**
  - `inside` *(boolean)* — True if shape is in trigger volume
```lua
local trigger = 0
local shape = 0
function client.init()
	trigger = FindTrigger("toxic")
	shape = FindShape("shape")
end

function client.tick()
	if IsShapeInTrigger(trigger, shape) then
		DebugPrint("In trigger!")
	end
end
```

### [API] inside = IsPointInTrigger(trigger, point)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `point` *(TVec)* — Word space point as vector
- **Returns:**
  - `inside` *(boolean)* — True if point is in trigger volume
```lua
local trigger = 0
local point = {}
function client.init()
	trigger = FindTrigger("toxic", true)
	point = Vec(0, 0, 0)
end

function client.tick()
	if IsPointInTrigger(trigger, point) then
		DebugPrint("In trigger!")
	end
end
```

### [API] value, dist = IsPointInBoundaries(point)
- **Args:**
  - `point` *(TVec)* — Point
- **Returns:**
  - `value` *(boolean)* — True if point is inside scene boundaries or if there are no boundaries
  - `dist` *(number)* — Distance to the scene boundaries. Zero if there are no boundaries or if point is outside.
```lua
function client.tick()
	local p = Vec(1.5, 3, 2.5)
	DebugWatch("In boundaries", IsPointInBoundaries(p))
end
```

### [API] empty, maxpoint = IsTriggerEmpty(handle, [demolision])
- **Args:**
  - `handle` *(number)* — Trigger handle
  - `demolision` *(boolean, optional)* — If true, small debris and vehicles are ignored
- **Returns:**
  - `empty` *(boolean)* — True if trigger is empty
  - `maxpoint` *(TVec)* — World space point of highest point (largest Y coordinate) if not empty
```lua
local trigger = 0
function client.init()
	trigger = FindTrigger("toxic")
end

function client.tick()
	local empty, highPoint = IsTriggerEmpty(trigger)
	if not empty then
		--highPoint[2] is the tallest point in trigger
		DebugPrint("Is not empty")
	end
end
```

### [API] distance = GetTriggerDistance(trigger, point)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `point` *(TVec)* — Word space point as vector
- **Returns:**
  - `distance` *(number)* — Positive if point is outside, negative if inside
```lua
local trigger = 0
function client.init()
	trigger = FindTrigger("toxic")
	local p = Vec(0, 10, 0)
	local dist = GetTriggerDistance(trigger, p)
	DebugPrint(dist)
end
```

### [API] closest = GetTriggerClosestPoint(trigger, point)
- **Args:**
  - `trigger` *(number)* — Trigger handle
  - `point` *(TVec)* — Word space point as vector
- **Returns:**
  - `closest` *(TVec)* — Closest point in trigger as vector
```lua
local trigger = 0
function client.init()
	trigger = FindTrigger("toxic")
	local p = Vec(0, 10, 0)
	local closest = GetTriggerClosestPoint(trigger, p)
	DebugPrint(closest)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)