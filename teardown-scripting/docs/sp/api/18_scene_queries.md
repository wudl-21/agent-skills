# Teardown SP API — Scene queries
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Query the level in various ways.

---

### [API] QueryRequire

```lua
QueryRequire(layers)
```

Set required layers for next query. Available layers are: Layer Description physical have a physical representationdynamic part of a dynamic bodystatic part of a static bodylarge above debris thresholdsmall below debris thresholdvisible only hit visible shapesanimator part of an animator hierachyplayer part of an player animator hierachytool part of a tool

**Arguments:**

- `layers` *(string)* — Space separate list of layers

**Example:**

```lua
--Raycast dynamic, physical objects above debris threshold, but not specific vehicle
function tick()
	local vehicle = FindVehicle("vehicle")
	QueryRequire("physical dynamic large")
	QueryRejectVehicle(vehicle)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryInclude

```lua
QueryInclude(layers)
```

Set included layers for next query. Queries include all layers except tool and player per default. Available layers are: Layer Description physical have a physical representationdynamic part of a dynamic bodystatic part of a static bodylarge above debris thresholdsmall below debris thresholdvisible only hit visible shapesanimator part of an animator hierachyplayer part of an playertool part of a tool

**Arguments:**

- `layers` *(string)* — Space separate list of layers

**Example:**

```lua
--Raycast all the default layers and include the player layer.
function tick()
	QueryInclude("player")
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRejectAnimator

```lua
QueryRejectAnimator(handle)
```

Exclude animator from the next query

**Arguments:**

- `handle` *(number)* — Animator handle

---

### [API] QueryRejectVehicle

```lua
QueryRejectVehicle(vehicle)
```

Exclude vehicle from the next query

**Arguments:**

- `vehicle` *(number)* — Vehicle handle

**Example:**

```lua
function tick()
	local vehicle = FindVehicle("vehicle")
	QueryRequire("physical dynamic large")
	--Do not include vehicle in next raycast
	QueryRejectVehicle(vehicle)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRejectBody

```lua
QueryRejectBody(body)
```

Exclude body from the next query

**Arguments:**

- `body` *(number)* — Body handle

**Example:**

```lua
function tick()
	local body = FindBody("body")
	QueryRequire("physical dynamic large")
	--Do not include body in next raycast
	QueryRejectBody(body)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRejectBodies

```lua
QueryRejectBodies(bodies)
```

Exclude bodies from the next query

**Arguments:**

- `bodies` *(table)* — Array with bodies handles

**Example:**

```lua
function tick()
	local body = FindBody("body")
	QueryRequire("physical dynamic large")
	local bodies = {body}
	--Do not include body in next raycast
	QueryRejectBodies(bodies)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRejectShape

```lua
QueryRejectShape(shape)
```

Exclude shape from the next query

**Arguments:**

- `shape` *(number)* — Shape handle

**Example:**

```lua
function tick()
	local shape = FindShape("shape")
	QueryRequire("physical dynamic large")
	--Do not include shape in next raycast
	QueryRejectShape(shape)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRejectShapes

```lua
QueryRejectShapes(shapes)
```

Exclude shapes from the next query

**Arguments:**

- `shapes` *(table)* — Array with shapes handles

**Example:**

```lua
function tick()
	local shape = FindShape("shape")
	QueryRequire("physical dynamic large")
	local shapes = {shape}
	--Do not include shape in next raycast
	QueryRejectShapes(shapes)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

---

### [API] QueryRaycast

```lua
hit, dist, normal, shape = QueryRaycast(origin, direction, maxDist, [radius], [rejectTransparent])
```

This will perform a raycast or spherecast (if radius is more than zero) query. If you want to set up a filter for the query you need to do so before every call to this function.

**Arguments:**

- `origin` *(TVec)* — Raycast origin as world space vector

- `direction` *(TVec)* — Unit length raycast direction as world space vector

- `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.

- `radius` *(number, optional)* — Raycast thickness. Default zero.

- `rejectTransparent` *(boolean, optional)* — Raycast through transparent materials. Default false.

**Example:**

```lua
function init()
	local vehicle = FindVehicle("vehicle")
	QueryRejectVehicle(vehicle)
	--Raycast from a high point straight downwards, excluding a specific vehicle
	local hit, d = QueryRaycast(Vec(0, 100, 0), Vec(0, -1, 0), 100)
	if hit then
		DebugPrint(d)
	end
end
```

---

### [API] QueryRaycastRope

```lua
hit, dist, joint = QueryRaycastRope(origin, direction, maxDist, [radius])
```

This will perform a raycast query that returns the handle of the joint of rope type when if collides with it. There are no filters for this type of raycast.

**Arguments:**

- `origin` *(TVec)* — Raycast origin as world space vector

- `direction` *(TVec)* — Unit length raycast direction as world space vector

- `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.

- `radius` *(number, optional)* — Raycast thickness. Default zero.

**Example:**

```lua
function tick()
	local playerCameraTransform = GetPlayerCameraTransform()
	local dir = TransformToParentVec(playerCameraTransform, Vec(0, 0, -1))

	local hit, dist, joint = QueryRaycastRope(playerCameraTransform.pos, dir, 10)
	if hit then
		DebugWatch("distance", dist)
		DebugWatch("joint", joint)
	end
end
```

---

### [API] QueryClosestPoint

```lua
hit, point, normal, shape = QueryClosestPoint(origin, maxDist)
```

This will query the closest point to all shapes in the world. If you want to set up a filter for the query you need to do so before every call to this function.

**Arguments:**

- `origin` *(TVec)* — World space point

- `maxDist` *(number)* — Maximum distance. Keep this as low as possible for good performance.

**Example:**

```lua
function tick()
	local vehicle = FindVehicle("vehicle")
	--Find closest point within 10 meters of {0, 5, 0}, excluding any point on myVehicle
	QueryRejectVehicle(vehicle)
	local hit, p, n, s = QueryClosestPoint(Vec(0, 5, 0), 10)
	if hit then
		DebugPrint(p)
	end
end
```

---

### [API] QueryAabbShapes

```lua
list = QueryAabbShapes(min, max)
```

Return all shapes within the provided world space, axis-aligned bounding box

**Arguments:**

- `min` *(TVec)* — Aabb minimum point

- `max` *(TVec)* — Aabb maximum point

**Example:**

```lua
function tick()
	local list = QueryAabbShapes(Vec(0, 0, 0), Vec(10, 10, 10))
	for i=1, #list do
		local shape = list[i]
		DebugPrint(shape)
	end
end
```

---

### [API] QueryAabbBodies

```lua
list = QueryAabbBodies(min, max)
```

Return all bodies within the provided world space, axis-aligned bounding box

**Arguments:**

- `min` *(TVec)* — Aabb minimum point

- `max` *(TVec)* — Aabb maximum point

**Example:**

```lua
function tick()
	local list = QueryAabbBodies(Vec(0, 0, 0), Vec(10, 10, 10))
	for i=1, #list do
		local body = list[i]
		DebugPrint(body)
	end
end
```

---

### [API] QueryPath

```lua
QueryPath(start, end, [maxDist], [targetRadius], [type])
```

Initiate path planning query. The result will run asynchronously as long as GetPathState returns "busy". An ongoing path query can be aborted with AbortPath. The path planning query will use the currently set up query filter, just like the other query functions. Using the 'water' type allows you to build a path within the water. The 'flying' type builds a path in the entire three-dimensional space.

**Arguments:**

- `start` *(TVec)* — World space start point

- `end` *(TVec)* — World space target point

- `maxDist` *(number, optional)* — Maximum path length before giving up. Default is infinite.

- `targetRadius` *(number, optional)* — Maximum allowed distance to target in meters. Default is 2.0

- `type` *(string, optional)* — Type of path. Can be "low", "standart", "water", "flying". Default is "standart"

**Example:**

```lua
function init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end
```

---

### [API] CreatePathPlanner

```lua
id = CreatePathPlanner()
```

Creates a new path planner that can be used to calculate multiple paths in parallel. It is supposed to be used together with PathPlannerQuery. Returns created path planner id/handler. It is recommended to reuse previously created path planners, because they exist throughout the lifetime of the script.

**Example:**

```lua
local paths = {}

function init()
	paths[1] = {
		id = CreatePathPlanner(),
		location = GetProperty(FindEntity("loc1", true), "transform").pos,
	}

	paths[2] = {
		id = CreatePathPlanner(),
		location = GetProperty(FindEntity("loc2", true), "transform").pos,
	}

	for i = 1, #paths do
		PathPlannerQuery(paths[i].id, GetPlayerTransform().pos, paths[i].location)
	end
end
```

---

### [API] DeletePathPlanner

```lua
DeletePathPlanner(id)
```

Deletes the path planner with the specified id which can be used to save some memory. Calling CreatePathPlanner again can initialize a new path planner with the id previously deleted.

**Arguments:**

- `id` *(number)* — Path planner id

**Example:**

```lua
local paths = {}

function init()
	local id = CreatePathPlanner()
	DeletePathPlanner(id)
	-- now calling PathPlannerQuery for 'id' will result in an error
end
```

---

### [API] PathPlannerQuery

```lua
PathPlannerQuery(id, start, end, [maxDist], [targetRadius], [type])
```

It works similarly to QueryPath but several paths can be built simultaneously within the same script. The QueryPath automatically creates a path planner with an index of 0 and only works with it.

**Arguments:**

- `id` *(number)* — Path planner id

- `start` *(TVec)* — World space start point

- `end` *(TVec)* — World space target point

- `maxDist` *(number, optional)* — Maximum path length before giving up. Default is infinite.

- `targetRadius` *(number, optional)* — Maximum allowed distance to target in meters. Default is 2.0

- `type` *(string, optional)* — Type of path. Can be "low", "standart", "water", "flying". Default is "standart"

**Example:**

```lua
local paths = {}

function init()
	paths[1] = {
		id = CreatePathPlanner(),
		location = GetProperty(FindEntity("loc1", true), "transform").pos,
	}

	paths[2] = {
		id = CreatePathPlanner(),
		location = GetProperty(FindEntity("loc2", true), "transform").pos,
	}

	for i = 1, #paths do
		PathPlannerQuery(paths[i].id, GetPlayerTransform().pos, paths[i].location)
	end
end
```

---

### [API] AbortPath

```lua
AbortPath([id])
```

Abort current path query, regardless of what state it is currently in. This is a way to save computing resources if the result of the current query is no longer of interest.

**Arguments:**

- `id` *(number, optional)* — Path planner id. Default value is 0.

**Example:**

```lua
function init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
	AbortPath()
end
```

---

### [API] GetPathState

```lua
state = GetPathState([id])
```

Return the current state of the last path planning query. State Description idle No recent querybusy Busy computing. No path found yet.fail Failed to find path. You can still get the resulting path (even though it won't reach the target).done Path planning completed and a path was found. Get it with GetPathLength and GetPathPoint)

**Arguments:**

- `id` *(number, optional)* — Path planner id. Default value is 0.

**Example:**

```lua
function init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function tick()
	local s = GetPathState()
	if s == "done" then
		DebugPrint("done")
	end
end
```

---

### [API] GetPathLength

```lua
length = GetPathLength([id])
```

Return the path length of the most recently computed path query. Note that the result can often be retrieved even if the path query failed. If the target point couldn't be reached, the path endpoint will be the point closest to the target.

**Arguments:**

- `id` *(number, optional)* — Path planner id. Default value is 0.

**Example:**

```lua
function init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function tick()
	local s = GetPathState()
	if s == "done" then
		DebugPrint("done " .. GetPathLength())
	end
end
```

---

### [API] GetPathPoint

```lua
point = GetPathPoint(dist, [id])
```

Return a point along the path for the most recently computed path query. Note that the result can often be retrieved even if the path query failed. If the target point couldn't be reached, the path endpoint will be the point closest to the target.

**Arguments:**

- `dist` *(number)* — The distance along path. Should be between zero and result from GetPathLength()

- `id` *(number, optional)* — Path planner id. Default value is 0.

**Example:**

```lua
function init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function tick()
	local d = 0
	local l = GetPathLength()
	while d < l do
		DebugCross(GetPathPoint(d))
		d = d + 0.5
	end
end
```

---

### [API] GetLastSound

```lua
volume, position = GetLastSound()
```

**Example:**

```lua
function tick()
	local vol, pos = GetLastSound()
	if vol > 0 then
		DebugPrint(vol .. " " .. VecStr(pos)) 
	end
end
```

---

### [API] IsPointInWater

```lua
inWater, depth = IsPointInWater(point)
```

**Arguments:**

- `point` *(TVec)* — World point as vector

**Example:**

```lua
function tick()
	local wet, d = IsPointInWater(Vec(10, 0, 0))
	if wet then
		DebugPrint("point" .. d .. " meters into water")
	end
end
```

---

### [API] GetWindVelocity

```lua
vel = GetWindVelocity(point)
```

Get the wind velocity at provided point. The wind will be determined by wind property of the environment, but it varies with position procedurally.

**Arguments:**

- `point` *(TVec)* — World point as vector

**Example:**

```lua
function tick()
	local v = GetWindVelocity(Vec(0, 10, 0))
	DebugPrint(VecStr(v))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | **Scene queries** | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)