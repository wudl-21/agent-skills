# Body

A body represents a rigid body in the scene. It can be either static or dynamic. Only dynamic bodies are
affected by physics.

### [API] handle = FindBody([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first body with specified tag or zero if not found
```lua
function init()
	--Search for a body tagged "target" in script scope
	local target = FindBody("body")
	DebugPrint(target)

	--Search for a body tagged "escape" in entire scene
	local escape = FindBody("body", true)
	DebugPrint(escape)
end
```

### [API] list = FindBodies([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all bodies with specified tag
```lua
function init()
	--Search for bodies tagged "target" in script scope
	local targets = FindBodies("target", true)
	for i=1, #targets do
		local target = targets[i]
		DebugPrint(target)
	end
end
```

### [API] transform = GetBodyTransform(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `transform` *(TTransform)* — Transform of the body
```lua
function init()
	local handle = FindBody("target", true)
	local t = GetBodyTransform(handle)
	DebugPrint(TransformStr(t))
end
```

### [API] SetBodyTransform(handle, transform)
- **Args:**
  - `handle` *(number)* — Body handle
  - `transform` *(TTransform)* — Desired transform
```lua
function init()
	local handle = FindBody("body", true)

	--Move a body 1 meter upwards
	local t = GetBodyTransform(handle)
	t.pos = VecAdd(t.pos, Vec(0, 3, 0))
	SetBodyTransform(handle, t)
end
```

### [API] mass = GetBodyMass(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `mass` *(number)* — Body mass. Static bodies always return zero mass.
```lua
function init()
	local handle = FindBody("body", true)

	--Move a body 1 meter upwards
	local mass = GetBodyMass(handle)
	DebugPrint(mass)
end
```

### [API] dynamic = IsBodyDynamic(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `dynamic` *(boolean)* — Return true if body is dynamic
```lua
function init()
	local handle = FindBody("body", true)
	DebugPrint(IsBodyDynamic(handle))
end
```

### [API] SetBodyDynamic(handle, dynamic)
- **Args:**
  - `handle` *(number)* — Body handle
  - `dynamic` *(boolean)* — True for dynamic. False for static.
```lua
function init()
	local handle = FindBody("body", true)
	SetBodyDynamic(handle, false)
	DebugPrint(IsBodyDynamic(handle))
end
```

### [API] SetBodyVelocity(handle, velocity)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
  - `velocity` *(TVec)* — Vector with linear velocity
```lua
function init()
	local handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end
```

### [API] velocity = GetBodyVelocity(handle)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
- **Returns:**
  - `velocity` *(TVec)* — Linear velocity as vector
```lua
handle = 0
function server.init()
	handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end

function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugPrint(VecStr(GetBodyVelocity(handle)))
end
```

### [API] velocity = GetBodyVelocityAtPos(handle, pos)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
  - `pos` *(TVec)* — World space point as vector
- **Returns:**
  - `velocity` *(TVec)* — Linear velocity on body at pos as vector
```lua
handle = 0
function server.init()
	handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end

function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugPrint(VecStr(GetBodyVelocityAtPos(handle, Vec(0, 0, 0))))
end
```

### [API] SetBodyAngularVelocity(handle, angVel)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
  - `angVel` *(TVec)* — Vector with angular velocity
```lua
function server.init()
	handle = FindBody("body", true)
	local angVel = Vec(0,100,0)
	SetBodyAngularVelocity(handle, angVel)
end
```

### [API] angVel = GetBodyAngularVelocity(handle)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
- **Returns:**
  - `angVel` *(TVec)* — Angular velocity as vector
```lua
handle = 0
function server.init()
	handle = FindBody("body", true)
	local angVel = Vec(0,100,0)
	SetBodyAngularVelocity(handle, angVel)
end

function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugPrint(VecStr(GetBodyAngularVelocity(handle)))
end
```

### [API] active = IsBodyActive(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `active` *(boolean)* — Return true if body is active
```lua
-- try to break the body to see the logs
function client.tick()
	handle = FindBody("body", true)
	if IsBodyActive(handle) then
		DebugPrint("Body is active")
	end
end
```

### [API] SetBodyActive(handle, active)
- **Args:**
  - `handle` *(number)* — Body handle
  - `active` *(boolean)* — Set to tru if body should be active (simulated)
```lua
handle = 0
function server.tick()
	handle = FindBody("body", true)

	-- Forces body to "sleep"
	SetBodyActive(handle, false)
end

function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	handle = FindBody("body", true)

	if IsBodyActive(handle) then
		DebugPrint("Body is active")
	end
end
```

### [API] ApplyBodyImpulse(handle, position, impulse)
- **Args:**
  - `handle` *(number)* — Body handle (should be a dynamic body)
  - `position` *(TVec)* — World space position as vector
  - `impulse` *(TVec)* — World space impulse as vector
```lua
function applyImpulse()
	handle = FindBody("body", true)

	local pos = Vec(0,1,0)
	local imp = Vec(0,0,10)
	ApplyBodyImpulse(handle, pos, imp)
end
```

### [API] list = GetBodyShapes(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `list` *(table)* — Indexed table of shape handles
```lua
function client.init()
	handle = FindBody("body", true)

	local shapes = GetBodyShapes(handle)
	for i=1,#shapes do
		local shape = shapes[i]
		DebugPrint(shape)
	end
end
```

### [API] handle = GetBodyVehicle(body)
- **Args:**
  - `body` *(number)* — Body handle
- **Returns:**
  - `handle` *(number)* — Get parent vehicle for body, or zero if not part of vehicle
```lua
function client.init()
	handle = FindBody("body", true)

	local vehicle = GetBodyVehicle(handle)
	DebugPrint(vehicle)
end
```

### [API] handle = GetBodyAnimator(body)
- **Args:**
  - `body` *(number)* — Body handle
- **Returns:**
  - `handle` *(number)* — Get parent animator for body, or zero if not part of an animator hierarchy
```lua
local animator = GetBodyAnimator(body)
```

### [API] playerId = GetBodyPlayer(body)
- **Args:**
  - `body` *(number)* — Body handle
- **Returns:**
  - `playerId` *(number)* — Get parent player for body, or zero if not part of a player animator hierarchy
```lua
local player = GetBodyPlayer(body)
```

### [API] min, max = GetBodyBounds(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `min` *(TVec)* — Vector representing the AABB lower bound
  - `max` *(TVec)* — Vector representing the AABB upper bound
```lua
function client.init()
	handle = FindBody("body", true)

	local min, max = GetBodyBounds(handle)
	local boundsSize = VecSub(max, min)
	local center = VecLerp(min, max, 0.5)
	DebugPrint(VecStr(boundsSize) .. " " .. VecStr(center))
end
```

### [API] point = GetBodyCenterOfMass(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `point` *(TVec)* — Vector representing local center of mass in body space
```lua
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	--Visualize center of mass on for body
	local com = GetBodyCenterOfMass(handle)
	local worldPoint = TransformToParentPoint(GetBodyTransform(handle), com)
	DebugCross(worldPoint)
end
```

### [API] visible = IsBodyVisible(handle, maxDist, [rejectTransparent], [playerId])
- **Args:**
  - `handle` *(number)* — Body handle
  - `maxDist` *(number)* — Maximum visible distance
  - `rejectTransparent` *(boolean, optional)* — See through transparent materials. Default false.
  - `playerId` *(number, optional)* — Player ID. On player, zero means local player.
- **Returns:**
  - `visible` *(boolean)* — Return true if body is visible
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	if IsBodyVisible(handle, 25) then
		--Body is within 25 meters visible to the camera
		DebugPrint("visible")
	else
		DebugPrint("not visible")
	end
end
```

### [API] broken = IsBodyBroken(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `broken` *(boolean)* — Return true if body is broken
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugPrint(IsBodyBroken(handle))
end
```

### [API] result = IsBodyJointedToStatic(handle)
- **Args:**
  - `handle` *(number)* — Body handle
- **Returns:**
  - `result` *(boolean)* — Return true if body is in any way connected to a static body
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugPrint(IsBodyJointedToStatic(handle))
end
```

### [API] DrawBodyOutline(handle, [r], [g], [b], [a])
- **Args:**
  - `handle` *(number)* — Body handle
  - `r` *(number, optional)* — Red
  - `g` *(number, optional)* — Green
  - `b` *(number, optional)* — Blue
  - `a` *(number, optional)* — Alpha
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	if InputDown("interact") then
		--Draw white outline at 50% transparency
		DrawBodyOutline(handle, 0.5)
	else
		--Draw green outline, fully opaque
		DrawBodyOutline(handle, 0, 1, 0, 1)
	end
end
```

### [API] DrawBodyHighlight(handle, amount)
- **Args:**
  - `handle` *(number)* — Body handle
  - `amount` *(number)* — Amount
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	if InputDown("interact") then
		DrawBodyHighlight(handle, 0.5)
	end
end
```

### [API] hit, point, normal, shape = GetBodyClosestPoint(body, origin)
- **Args:**
  - `body` *(number)* — Body handle
  - `origin` *(TVec)* — World space point
- **Returns:**
  - `hit` *(boolean)* — True if a point was found
  - `point` *(TVec)* — World space closest point
  - `normal` *(TVec)* — World space normal at closest point
  - `shape` *(number)* — Handle to closest shape
```lua
local handle = 0
function client.init()
	handle = FindBody("body", true)
end

function client.tick()
	DebugCross(Vec(1, 0, 0))
	local hit, p, n, s = GetBodyClosestPoint(handle, Vec(1, 0, 0))
	if hit then
		DebugCross(p)
	end
end
```

### [API] ConstrainVelocity(bodyA, bodyB, point, dir, relVel, [min], [max])
- **Args:**
  - `bodyA` *(number)* — First body handle (zero for static)
  - `bodyB` *(number)* — Second body handle (zero for static)
  - `point` *(TVec)* — World space point
  - `dir` *(TVec)* — World space direction
  - `relVel` *(number)* — Desired relative velocity along the provided direction
  - `min` *(number, optional)* — Minimum impulse (default: -infinity)
  - `max` *(number, optional)* — Maximum impulse (default: infinity)
```lua
local handleA = 0
local handleB = 0
function server.init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function server.update()
	--Constrain the velocity between bodies A and B so that the relative velocity
	--along the X axis at point (0, 5, 0) is always 3 m/s
	ConstrainVelocity(handleA, handleB, Vec(0, 5, 0), Vec(1, 0, 0), 3)
end
```

### [API] ConstrainAngularVelocity(bodyA, bodyB, dir, relAngVel, [min], [max])
- **Args:**
  - `bodyA` *(number)* — First body handle (zero for static)
  - `bodyB` *(number)* — Second body handle (zero for static)
  - `dir` *(TVec)* — World space direction
  - `relAngVel` *(number)* — Desired relative angular velocity along the provided direction
  - `min` *(number, optional)* — Minimum angular impulse (default: -infinity)
  - `max` *(number, optional)* — Maximum angular impulse (default: infinity)
```lua
local handleA = 0
local handleB = 0
function server.init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function server.update()
	--Constrain the angular velocity between bodies A and B so that the relative angular velocity
	--along the Y axis is always 3 rad/s
	ConstrainAngularVelocity(handleA, handleB, Vec(1, 0, 0), 3)
end
```

### [API] ConstrainPosition(bodyA, bodyB, pointA, pointB, [maxVel], [maxImpulse])
- **Args:**
  - `bodyA` *(number)* — First body handle (zero for static)
  - `bodyB` *(number)* — Second body handle (zero for static)
  - `pointA` *(TVec)* — World space point for first body
  - `pointB` *(TVec)* — World space point for second body
  - `maxVel` *(number, optional)* — Maximum relative velocity (default: infinite)
  - `maxImpulse` *(number, optional)* — Maximum impulse (default: infinite)
```lua
local handleA = 0
local handleB = 0
function server.init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function server.update()
	--Constrain the origo of body a to an animated point in the world
	local worldPos = Vec(0, 3+math.sin(GetTime()), 0)
	ConstrainPosition(handleA, 0, GetBodyTransform(handleA).pos, worldPos)

	--Constrain the origo of body a to the origo of body b (like a ball joint)
	ConstrainPosition(handleA, handleA, GetBodyTransform(handleA).pos, GetBodyTransform(handleB).pos)
end
```

### [API] ConstrainOrientation(bodyA, bodyB, quatA, quatB, [maxAngVel], [maxAngImpulse])
- **Args:**
  - `bodyA` *(number)* — First body handle (zero for static)
  - `bodyB` *(number)* — Second body handle (zero for static)
  - `quatA` *(TQuat)* — World space orientation for first body
  - `quatB` *(TQuat)* — World space orientation for second body
  - `maxAngVel` *(number, optional)* — Maximum relative angular velocity (default: infinite)
  - `maxAngImpulse` *(number, optional)* — Maximum angular impulse (default: infinite)
```lua
local handleA = 0
local handleB = 0
function server.init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function server.update()
	--Constrain the orietation of body a to an upright orientation in the world
	ConstrainOrientation(handleA, 0, GetBodyTransform(handleA).rot, Quat())

	--Constrain the orientation of body a to the orientation of body b
	ConstrainOrientation(handleA, handleB, GetBodyTransform(handleA).rot, GetBodyTransform(handleB).rot)
end
```

### [API] body = GetWorldBody()
- **Returns:**
  - `body` *(number)* — Handle to the static world body
```lua
local handle
function client.init()
	handle = GetWorldBody()
end

function client.tick()
	DebugCross(GetBodyTransform(handle).pos)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)