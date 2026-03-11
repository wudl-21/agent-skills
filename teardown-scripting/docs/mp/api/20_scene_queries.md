# Scene queries

Query the level in various ways.

### [API] QueryRequire(layers)
- **Args:**
  - `layers` *(string)* — Space separate list of layers
| Layer | Description |
| --- | --- |
| physical | have a physical representation |
| dynamic | part of a dynamic body |
| static | part of a static body |
| large | above debris threshold |
| small | below debris threshold |
| visible | only hit visible shapes |
| animator | part of an animator hierarchy |
| player | part of an player animator hierarchy |
| tool | part of a tool |
```lua
--Raycast dynamic, physical objects above debris threshold, but not specific vehicle
function client.tick()
	local vehicle = FindVehicle("vehicle")
	QueryRequire("physical dynamic large")
	QueryRejectVehicle(vehicle)
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

### [API] QueryInclude(layers)
- **Args:**
  - `layers` *(string)* — Space separate list of layers
| Layer | Description |
| --- | --- |
| physical | have a physical representation |
| dynamic | part of a dynamic body |
| static | part of a static body |
| large | above debris threshold |
| small | below debris threshold |
| visible | only hit visible shapes |
| animator | part of an animator hierarchy |
| player | part of an player |
| tool | part of a tool |
```lua
--Raycast all the default layers and include the player layer.
function client.tick()
	QueryInclude("player")
	local hit, dist = QueryRaycast(Vec(0, 0, 0), Vec(1, 0, 0), 10)
	if hit then
		DebugPrint(dist)
	end
end
```

### [API] QueryCollisionMask(mask)
- **Args:**
  - `mask` *(number)* — Mask bits (0-255)
```lua
--Find the closest point on any shape (within 2 meters) to the player eye that the player can collide with.
function client.tick()
	QueryRequire("physical")
	QueryCollisionMask(GetPlayerParam("CollisionMask"))
	local hit, hitpos = QueryClosestPoint(GetPlayerEyeTransform().pos, 2)
	if hit then
		DebugCross(hitpos)
	end
end
```

### [API] QueryRejectAnimator(handle)
- **Args:**
  - `handle` *(number)* — Animator handle

### [API] QueryRejectVehicle(vehicle)
- **Args:**
  - `vehicle` *(number)* — Vehicle handle
```lua
function client.tick()
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

### [API] QueryRejectBody(body)
- **Args:**
  - `body` *(number)* — Body handle
```lua
function client.tick()
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

### [API] QueryRejectBodies(bodies)
- **Args:**
  - `bodies` *(table)* — Array with bodies handles
```lua
function client.tick()
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

### [API] QueryRejectShape(shape)
- **Args:**
  - `shape` *(number)* — Shape handle
```lua
function client.tick()
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

### [API] QueryRejectShapes(shapes)
- **Args:**
  - `shapes` *(table)* — Array with shapes handles
```lua
function client.tick()
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

### [API] QueryRejectPlayer([playerId])
- **Args:**
  - `playerId` *(number, optional)* — Player ID. On client, zero means client player. On server, zero means server (host) player.
```lua
--Do not include shape in next raycast
QueryRejectPlayer(1)
QueryRaycast(...)
```

### [API] hit, dist, normal, shape = QueryRaycast(origin, direction, maxDist, [radius], [rejectTransparent])
- **Args:**
  - `origin` *(TVec)* — Raycast origin as world space vector
  - `direction` *(TVec)* — Unit length raycast direction as world space vector
  - `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.
  - `radius` *(number, optional)* — Raycast thickness. Default zero.
  - `rejectTransparent` *(boolean, optional)* — Raycast through transparent materials. Default false.
- **Returns:**
  - `hit` *(boolean)* — True if raycast hit something
  - `dist` *(number)* — Hit distance from origin
  - `normal` *(TVec)* — World space normal at hit point
  - `shape` *(number)* — Handle to hit shape
```lua
function client.init()
	local vehicle = FindVehicle("vehicle")
	QueryRejectVehicle(vehicle)
	--Raycast from a high point straight downwards, excluding a specific vehicle
	local hit, d = QueryRaycast(Vec(0, 100, 0), Vec(0, -1, 0), 100)
	if hit then
		DebugPrint(d)
	end
end
```

### [API] hit, dist, joint = QueryRaycastRope(origin, direction, maxDist, [radius])
- **Args:**
  - `origin` *(TVec)* — Raycast origin as world space vector
  - `direction` *(TVec)* — Unit length raycast direction as world space vector
  - `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.
  - `radius` *(number, optional)* — Raycast thickness. Default zero.
- **Returns:**
  - `hit` *(boolean)* — True if raycast hit something
  - `dist` *(number)* — Hit distance from origin
  - `joint` *(number)* — Handle to hit joint of rope type
```lua
function client.tick()
	local playerCameraTransform = GetPlayerCameraTransform()
	local dir = TransformToParentVec(playerCameraTransform, Vec(0, 0, -1))

	local hit, dist, joint = QueryRaycastRope(playerCameraTransform.pos, dir, 10)
	if hit then
		DebugWatch("distance", dist)
		DebugWatch("joint", joint)
	end
end
```

### [API] hit, dist, hitPos = QueryRaycastWater(origin, direction, maxDist)
- **Args:**
  - `origin` *(TVec)* — Raycast origin as world space vector
  - `direction` *(TVec)* — Unit length raycast direction as world space vector
  - `maxDist` *(number)* — Raycast maximum distance. Keep this as low as possible for good performance.
- **Returns:**
  - `hit` *(boolean)* — True if raycast hit something
  - `dist` *(number)* — Hit distance from origin
  - `hitPos` *(TVec)* — Hit point as world space vector
```lua
function client.init()
	--Raycast from a high point straight downwards, looking for water
	local hit, d = QueryRaycast(Vec(0, 100, 0), Vec(0, -1, 0), 100)
	if hit then
		DebugPrint(d)
	end
end
```

### [API] didHit, dist, shape, playerId, playerDamageFactor, normal = QueryShot(origin, direction, maxDist, [radius], [playerId])
- **Args:**
  - `origin` *(TVec)* — Shot ray origin as world space vector
  - `direction` *(TVec)* — Unit length direction as world space vector
  - `maxDist` *(number)* — Shot maximum distance. Keep this as low as possible for good performance.
  - `radius` *(number, optional)* — Ray thickness. Default zero.
  - `playerId` *(number, optional)* — Instigating player, will be ignored during hit detection.
- **Returns:**
  - `didHit` *(bool)* — If it was a valid hit.
  - `dist` *(number)* — Distance along direction where the hit was registered.
  - `shape` *(number)* — Handle to hit shape, zero if it did not hit a shape
  - `playerId` *(number)* — PlayerId of hit player, zero if it did not hit a player
  - `playerDamageFactor` *(number)* — 1.0 for a hit on the torso, and less for a lower body hit. Applicable only if a player was hit. Use this to scale the damage.
  - `normal` *(Vec)* — Normal vector of the hit
```lua
-- Note: 'shape' and 'player' are IDs/handles (numbers), not object references.
function server.tick()

	for p in Players() do
		if InputPressed("usetool", p) then

			local pos = GetPlayerEyeTransform(p).pos
			local dir = TransformToParentVec(GetPlayerEyeTransform(p), Vec(0, 0, -1))

			local hit, dist, shape, player, hitFactor, normal = QueryShot(pos, dir, 100, 0, p)
			if hit then
				if player then
					DebugPrint("Hit player " .. GetPlayerName(player) .. " with damage factor " .. hitFactor)
					ApplyPlayerDamage(player, 0.2 * hitFactor, "SuperGun", p)
				elseif shape then
					DebugPrint("Hit shape " .. shape .. " at distance " .. dist)
					local body = GetShapeBody(shape)
					local impPos = VecAdd(pos, VecScale(dir, dist))
					local imp = Vec(100, 0, 0)
					ApplyBodyImpulse(body, impPos, imp)
				end
			else
				DebugPrint("No hit")
			end
		end
	end
end
```

### [API] hit, point, normal, shape = QueryClosestPoint(origin, maxDist)
- **Args:**
  - `origin` *(TVec)* — World space point
  - `maxDist` *(number)* — Maximum distance. Keep this as low as possible for good performance.
- **Returns:**
  - `hit` *(boolean)* — True if a point was found
  - `point` *(TVec)* — World space closest point
  - `normal` *(TVec)* — World space normal at closest point
  - `shape` *(number)* — Handle to closest shape
```lua
function client.tick()
	local vehicle = FindVehicle("vehicle")
	--Find closest point within 10 meters of {0, 5, 0}, excluding any point on myVehicle
	QueryRejectVehicle(vehicle)
	local hit, p, n, s = QueryClosestPoint(Vec(0, 5, 0), 10)
	if hit then
		DebugPrint(p)
	end
end
```

### [API] list = QueryAabbShapes(min, max)
- **Args:**
  - `min` *(TVec)* — Aabb minimum point
  - `max` *(TVec)* — Aabb maximum point
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all shapes in the aabb
```lua
function client.tick()
	local list = QueryAabbShapes(Vec(0, 0, 0), Vec(10, 10, 10))
	for i=1, #list do
		local shape = list[i]
		DebugPrint(shape)
	end
end
```

### [API] list = QueryAabbBodies(min, max)
- **Args:**
  - `min` *(TVec)* — Aabb minimum point
  - `max` *(TVec)* — Aabb maximum point
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all bodies in the aabb
```lua
function client.tick()
	local list = QueryAabbBodies(Vec(0, 0, 0), Vec(10, 10, 10))
	for i=1, #list do
		local body = list[i]
		DebugPrint(body)
	end
end
```

### [API] QueryPath(start, end, [maxDist], [targetRadius], [type])
- **Args:**
  - `start` *(TVec)* — World space start point
  - `end` *(TVec)* — World space target point
  - `maxDist` *(number, optional)* — Maximum path length before giving up. Default is infinite.
  - `targetRadius` *(number, optional)* — Maximum allowed distance to target in meters. Default is 2.0
  - `type` *(string, optional)* — Type of path. Can be "low", "standart", "water", "flying". Default is "standart"
```lua
function client.init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end
```

### [API] id = CreatePathPlanner()
- **Returns:**
  - `id` *(number)* — Path planner id
```lua
local paths = {}

function server.init()
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

### [API] DeletePathPlanner(id)
- **Args:**
  - `id` *(number)* — Path planner id
```lua
local paths = {}

function server.init()
	local id = CreatePathPlanner()
	DeletePathPlanner(id)
	-- now calling PathPlannerQuery for 'id' will result in an error
end
```

### [API] PathPlannerQuery(id, start, end, [maxDist], [targetRadius], [type])
- **Args:**
  - `id` *(number)* — Path planner id
  - `start` *(TVec)* — World space start point
  - `end` *(TVec)* — World space target point
  - `maxDist` *(number, optional)* — Maximum path length before giving up. Default is infinite.
  - `targetRadius` *(number, optional)* — Maximum allowed distance to target in meters. Default is 2.0
  - `type` *(string, optional)* — Type of path. Can be "low", "standart", "water", "flying". Default is "standart"
```lua
local paths = {}

function server.init()
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

### [API] AbortPath([id])
- **Args:**
  - `id` *(number, optional)* — Path planner id. Default value is 0.
```lua
function server.init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
	AbortPath()
end
```

### [API] state = GetPathState([id])
- **Args:**
  - `id` *(number, optional)* — Path planner id. Default value is 0.
- **Returns:**
  - `state` *(string)* — Current path planning state
| State | Description |
| --- | --- |
| idle | No recent query |
| busy | Busy computing. No path found yet. |
| fail | Failed to find path. You can still get the resulting path (even though it won't reach the target). |
| done | Path planning completed and a path was found. Get it with GetPathLength and GetPathPoint) |
```lua
function server.init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function server.tick()
	local s = GetPathState()
	if s == "done" then
		DebugPrint("done")
	end
end
```

### [API] length = GetPathLength([id])
- **Args:**
  - `id` *(number, optional)* — Path planner id. Default value is 0.
- **Returns:**
  - `length` *(number)* — Length of last path planning result (in meters)
```lua
function server.init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function server.tick()
	local s = GetPathState()
	if s == "done" then
		DebugPrint("done " .. GetPathLength())
	end
end
```

### [API] point = GetPathPoint(dist, [id])
- **Args:**
  - `dist` *(number)* — The distance along path. Should be between zero and result from GetPathLength()
  - `id` *(number, optional)* — Path planner id. Default value is 0.
- **Returns:**
  - `point` *(TVec)* — The path point dist meters along the path
```lua
function client.init()
	QueryPath(Vec(-10, 0, 0), Vec(10, 0, 0))
end

function client.tick()
	local d = 0
	local l = GetPathLength()
	while d < l do
		DebugCross(GetPathPoint(d))
		d = d + 0.5
	end
end
```

### [API] volume, position = GetLastSound()
- **Returns:**
  - `volume` *(number)* — Volume of loudest sound played last frame
  - `position` *(TVec)* — World position of loudest sound played last frame
```lua
function client.tick()
	local vol, pos = GetLastSound()
	if vol > 0 then
		DebugPrint(vol .. " " .. VecStr(pos))
	end
end
```

### [API] inWater, depth = IsPointInWater(point)
- **Args:**
  - `point` *(TVec)* — World point as vector
- **Returns:**
  - `inWater` *(boolean)* — True if point is in water
  - `depth` *(number)* — Depth of point into water, or zero if not in water
```lua
function client.tick()
	local wet, d = IsPointInWater(Vec(10, 0, 0))
	if wet then
		DebugPrint("point" .. d .. " meters into water")
	end
end
```

### [API] vel = GetWindVelocity(point)
- **Args:**
  - `point` *(TVec)* — World point as vector
- **Returns:**
  - `vel` *(TVec)* — Wind at provided position
```lua
function client.tick()
	local v = GetWindVelocity(Vec(0, 10, 0))
	DebugPrint(VecStr(v))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)