# Teardown SP API — Body
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

A body represents a rigid body in the scene. It can be either static or dynamic. Only dynamic bodies are affected by physics.

---

### [API] FindBody

```lua
handle = FindBody([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

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

---

### [API] FindBodies

```lua
list = FindBodies([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

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

---

### [API] GetBodyTransform

```lua
transform = GetBodyTransform(handle)
```

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	local handle = FindBody("target", true)
	local t = GetBodyTransform(handle)
	DebugPrint(TransformStr(t))
end
```

---

### [API] SetBodyTransform

```lua
SetBodyTransform(handle, transform)
```

**Arguments:**

- `handle` *(number)* — Body handle

- `transform` *(TTransform)* — Desired transform

**Example:**

```lua
function init()
	local handle = FindBody("body", true)

	--Move a body 1 meter upwards
	local t = GetBodyTransform(handle)
	t.pos = VecAdd(t.pos, Vec(0, 3, 0))
	SetBodyTransform(handle, t)
end
```

---

### [API] GetBodyMass

```lua
mass = GetBodyMass(handle)
```

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	local handle = FindBody("body", true)

	--Move a body 1 meter upwards
	local mass = GetBodyMass(handle)
	DebugPrint(mass)
end
```

---

### [API] IsBodyDynamic

```lua
dynamic = IsBodyDynamic(handle)
```

Check if body is dynamic. Note that something that was created static may become dynamic due to destruction.

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	DebugPrint(IsBodyDynamic(handle))
end
```

---

### [API] SetBodyDynamic

```lua
SetBodyDynamic(handle, dynamic)
```

Change the dynamic state of a body. There is very limited use for this function. In most situations you should leave it up to the engine to decide. Use with caution.

**Arguments:**

- `handle` *(number)* — Body handle

- `dynamic` *(boolean)* — True for dynamic. False for static.

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	SetBodyDynamic(handle, false)
	DebugPrint(IsBodyDynamic(handle))
end
```

---

### [API] SetBodyVelocity

```lua
SetBodyVelocity(handle, velocity)
```

This can be used for animating bodies with preserved physical interaction, but in most cases you are better off with a motorized joint instead.

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

- `velocity` *(TVec)* — Vector with linear velocity

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end
```

---

### [API] GetBodyVelocity

```lua
velocity = GetBodyVelocity(handle)
```

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

**Example:**

```lua
function init()
	handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end

function tick()
	DebugPrint(VecStr(GetBodyVelocity(handle)))
end
```

---

### [API] GetBodyVelocityAtPos

```lua
velocity = GetBodyVelocityAtPos(handle, pos)
```

Return the velocity on a body taking both linear and angular velocity into account.

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

- `pos` *(TVec)* — World space point as vector

**Example:**

```lua
function init()
	handle = FindBody("body", true)
	local vel = Vec(0,10,0)
	SetBodyVelocity(handle, vel)
end

function tick()
	DebugPrint(VecStr(GetBodyVelocityAtPos(handle, Vec(0, 0, 0))))
end
```

---

### [API] SetBodyAngularVelocity

```lua
SetBodyAngularVelocity(handle, angVel)
```

This can be used for animating bodies with preserved physical interaction, but in most cases you are better off with a motorized joint instead.

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

- `angVel` *(TVec)* — Vector with angular velocity

**Example:**

```lua
function init()
	handle = FindBody("body", true)
	local angVel = Vec(0,100,0)
	SetBodyAngularVelocity(handle, angVel)
end
```

---

### [API] GetBodyAngularVelocity

```lua
angVel = GetBodyAngularVelocity(handle)
```

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

**Example:**

```lua
function init()
	handle = FindBody("body", true)
	local angVel = Vec(0,100,0)
	SetBodyAngularVelocity(handle, angVel)
end

function tick()
	DebugPrint(VecStr(GetBodyAngularVelocity(handle)))
end
```

---

### [API] IsBodyActive

```lua
active = IsBodyActive(handle)
```

Check if body is body is currently simulated. For performance reasons, bodies that don't move are taken out of the simulation. This function can be used to query the active state of a specific body. Only dynamic bodies can be active.

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
-- try to break the body to see the logs
function tick()
	handle = FindBody("body", true)
	if IsBodyActive(handle) then
		DebugPrint("Body is active")
	end
end
```

---

### [API] SetBodyActive

```lua
SetBodyActive(handle, active)
```

This function makes it possible to manually activate and deactivate bodies to include or exclude in simulation. The engine normally handles this automatically, so use with care.

**Arguments:**

- `handle` *(number)* — Body handle

- `active` *(boolean)* — Set to tru if body should be active (simulated)

**Example:**

```lua
function tick()
	handle = FindBody("body", true)

	-- Forces body to "sleep"
	SetBodyActive(handle, false)
	if IsBodyActive(handle) then
		DebugPrint("Body is active")
	end
end
```

---

### [API] ApplyBodyImpulse

```lua
ApplyBodyImpulse(handle, position, impulse)
```

Apply impulse to dynamic body at position (give body a push).

**Arguments:**

- `handle` *(number)* — Body handle (should be a dynamic body)

- `position` *(TVec)* — World space position as vector

- `impulse` *(TVec)* — World space impulse as vector

**Example:**

```lua
function tick()
	handle = FindBody("body", true)

	local pos = Vec(0,1,0)
	local imp = Vec(0,0,10)
	ApplyBodyImpulse(handle, pos, imp)
end
```

---

### [API] GetBodyShapes

```lua
list = GetBodyShapes(handle)
```

Return handles to all shapes owned by a body

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	handle = FindBody("body", true)

	local shapes = GetBodyShapes(handle)
	for i=1,#shapes do
		local shape = shapes[i]
		DebugPrint(shape)
	end
end
```

---

### [API] GetBodyVehicle

```lua
handle = GetBodyVehicle(body)
```

**Arguments:**

- `body` *(number)* — Body handle

**Example:**

```lua
function init()
	handle = FindBody("body", true)

	local vehicle = GetBodyVehicle(handle)
	DebugPrint(vehicle)
end
```

---

### [API] GetBodyBounds

```lua
min, max = GetBodyBounds(handle)
```

Return the world space, axis-aligned bounding box for a body.

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	handle = FindBody("body", true)

	local min, max = GetBodyBounds(handle)
	local boundsSize = VecSub(max, min)
	local center = VecLerp(min, max, 0.5)
	DebugPrint(VecStr(boundsSize) .. " " .. VecStr(center))
end
```

---

### [API] GetBodyCenterOfMass

```lua
point = GetBodyCenterOfMass(handle)
```

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
function init()
	handle = FindBody("body", true)
end

function tick()
	--Visualize center of mass on for body
	local com = GetBodyCenterOfMass(handle)
	local worldPoint = TransformToParentPoint(GetBodyTransform(handle), com)
	DebugCross(worldPoint)
end
```

---

### [API] IsBodyVisible

```lua
visible = IsBodyVisible(handle, maxDist, [rejectTransparent])
```

This function does a very rudimetary check and will only return true if the object is visible within 74 degrees of the camera's forward direction, and only tests line-of-sight visibility for the corners and center of the bounding box.

**Arguments:**

- `handle` *(number)* — Body handle

- `maxDist` *(number)* — Maximum visible distance

- `rejectTransparent` *(boolean, optional)* — See through transparent materials. Default false.

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	if IsBodyVisible(handle, 25) then
		--Body is within 25 meters visible to the camera
		DebugPrint("visible")
	else
		DebugPrint("not visible")
	end
end
```

---

### [API] IsBodyBroken

```lua
broken = IsBodyBroken(handle)
```

Determine if any shape of a body has been broken.

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	DebugPrint(IsBodyBroken(handle))
end
```

---

### [API] IsBodyJointedToStatic

```lua
result = IsBodyJointedToStatic(handle)
```

Determine if a body is in any way connected to a static object, either by being static itself or be being directly or indirectly jointed to something static.

**Arguments:**

- `handle` *(number)* — Body handle

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	DebugPrint(IsBodyJointedToStatic(handle))
end
```

---

### [API] DrawBodyOutline

```lua
DrawBodyOutline(handle, [r], [g], [b], [a])
```

Render next frame with an outline around specified body. If no color is given, a white outline will be drawn.

**Arguments:**

- `handle` *(number)* — Body handle

- `r` *(number, optional)* — Red

- `g` *(number, optional)* — Green

- `b` *(number, optional)* — Blue

- `a` *(number, optional)* — Alpha

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	if InputDown("interact") then
		--Draw white outline at 50% transparency
		DrawBodyOutline(handle, 0.5)
	else
		--Draw green outline, fully opaque
		DrawBodyOutline(handle, 0, 1, 0, 1)
	end
end
```

---

### [API] DrawBodyHighlight

```lua
DrawBodyHighlight(handle, amount)
```

Flash the appearance of a body when rendering this frame. This is used for valuables in the game.

**Arguments:**

- `handle` *(number)* — Body handle

- `amount` *(number)* — Amount

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	if InputDown("interact") then
		DrawBodyHighlight(handle, 0.5)
	end
end
```

---

### [API] GetBodyClosestPoint

```lua
hit, point, normal, shape = GetBodyClosestPoint(body, origin)
```

This will return the closest point of a specific body

**Arguments:**

- `body` *(number)* — Body handle

- `origin` *(TVec)* — World space point

**Example:**

```lua
local handle = 0
function init()
	handle = FindBody("body", true)
end

function tick()
	DebugCross(Vec(1, 0, 0))
	local hit, p, n, s = GetBodyClosestPoint(handle, Vec(1, 0, 0))
	if hit then
		DebugCross(p)
	end
end
```

---

### [API] ConstrainVelocity

```lua
ConstrainVelocity(bodyA, bodyB, point, dir, relVel, [min], [max])
```

This will tell the physics solver to constrain the velocity between two bodies. The physics solver will try to reach the desired goal, while not applying an impulse bigger than the min and max values. This function should only be used from the update callback.

**Arguments:**

- `bodyA` *(number)* — First body handle (zero for static)

- `bodyB` *(number)* — Second body handle (zero for static)

- `point` *(TVec)* — World space point

- `dir` *(TVec)* — World space direction

- `relVel` *(number)* — Desired relative velocity along the provided direction

- `min` *(number, optional)* — Minimum impulse (default: -infinity)

- `max` *(number, optional)* — Maximum impulse (default: infinity)

**Example:**

```lua
local handleA = 0
local handleB = 0
function init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function update()
	--Constrain the velocity between bodies A and B so that the relative velocity 
	--along the X axis at point (0, 5, 0) is always 3 m/s
	ConstrainVelocity(handleA, handleB, Vec(0, 5, 0), Vec(1, 0, 0), 3)
end
```

---

### [API] ConstrainAngularVelocity

```lua
ConstrainAngularVelocity(bodyA, bodyB, dir, relAngVel, [min], [max])
```

This will tell the physics solver to constrain the angular velocity between two bodies. The physics solver will try to reach the desired goal, while not applying an angular impulse bigger than the min and max values. This function should only be used from the update callback.

**Arguments:**

- `bodyA` *(number)* — First body handle (zero for static)

- `bodyB` *(number)* — Second body handle (zero for static)

- `dir` *(TVec)* — World space direction

- `relAngVel` *(number)* — Desired relative angular velocity along the provided direction

- `min` *(number, optional)* — Minimum angular impulse (default: -infinity)

- `max` *(number, optional)* — Maximum angular impulse (default: infinity)

**Example:**

```lua
local handleA = 0
local handleB = 0
function init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function update()
	--Constrain the angular velocity between bodies A and B so that the relative angular velocity
	--along the Y axis is always 3 rad/s
	ConstrainAngularVelocity(handleA, handleB, Vec(1, 0, 0), 3)
end
```

---

### [API] ConstrainPosition

```lua
ConstrainPosition(bodyA, bodyB, pointA, pointB, [maxVel], [maxImpulse])
```

This is a helper function that uses ConstrainVelocity to constrain a point on one body to a point on another body while not affecting the bodies more than the provided maximum relative velocity and maximum impulse. In other words: physically push on the bodies so that pointA and pointB are aligned in world space. This is useful for physically animating objects. This function should only be used from the update callback.

**Arguments:**

- `bodyA` *(number)* — First body handle (zero for static)

- `bodyB` *(number)* — Second body handle (zero for static)

- `pointA` *(TVec)* — World space point for first body

- `pointB` *(TVec)* — World space point for second body

- `maxVel` *(number, optional)* — Maximum relative velocity (default: infinite)

- `maxImpulse` *(number, optional)* — Maximum impulse (default: infinite)

**Example:**

```lua
local handleA = 0
local handleB = 0
function init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function update()
	--Constrain the origo of body a to an animated point in the world
	local worldPos = Vec(0, 3+math.sin(GetTime()), 0)
	ConstrainPosition(handleA, 0, GetBodyTransform(handleA).pos, worldPos)

	--Constrain the origo of body a to the origo of body b (like a ball joint)
	ConstrainPosition(handleA, handleA, GetBodyTransform(handleA).pos, GetBodyTransform(handleB).pos)
end
```

---

### [API] ConstrainOrientation

```lua
ConstrainOrientation(bodyA, bodyB, quatA, quatB, [maxAngVel], [maxAngImpulse])
```

This is the angular counterpart to ConstrainPosition, a helper function that uses ConstrainAngularVelocity to constrain the orientation of one body to the orientation on another body while not affecting the bodies more than the provided maximum relative angular velocity and maximum angular impulse. In other words: physically rotate the bodies so that quatA and quatB are aligned in world space. This is useful for physically animating objects. This function should only be used from the update callback.

**Arguments:**

- `bodyA` *(number)* — First body handle (zero for static)

- `bodyB` *(number)* — Second body handle (zero for static)

- `quatA` *(TQuat)* — World space orientation for first body

- `quatB` *(TQuat)* — World space orientation for second body

- `maxAngVel` *(number, optional)* — Maximum relative angular velocity (default: infinite)

- `maxAngImpulse` *(number, optional)* — Maximum angular impulse (default: infinite)

**Example:**

```lua
local handleA = 0
local handleB = 0
function init()
	handleA = FindBody("body", true)
	handleB = FindBody("target", true)
end

function update()
	--Constrain the orietation of body a to an upright orientation in the world
	ConstrainOrientation(handleA, 0, GetBodyTransform(handleA).rot, Quat())

	--Constrain the orientation of body a to the orientation of body b
	ConstrainOrientation(handleA, handleB, GetBodyTransform(handleA).rot, GetBodyTransform(handleB).rot)
end
```

---

### [API] GetWorldBody

```lua
body = GetWorldBody()
```

Every scene in Teardown has an implicit static world body that contains all shapes that are not explicitly assigned a body in the editor.

**Example:**

```lua
local handle
function init()
	handle = GetWorldBody()
end

function tick()
	DebugCross(GetBodyTransform(handle).pos)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | **Body** | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)