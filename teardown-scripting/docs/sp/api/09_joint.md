# Teardown SP API — Joint
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Joints are used to physically connect two shapes. There are several types of joints and they are typically placed in the editor. When destruction occurs, joints may be transferred to new shapes, detached or completely disabled.

---

### [API] FindJoint

```lua
handle = FindJoint([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
function init()
	local joint = FindJoint("doorhinge")
	DebugPrint(joint)
end
```

---

### [API] FindJoints

```lua
list = FindJoints([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
--Search for locations tagged "doorhinge" in script scope
function init()
	local hinges = FindJoints("doorhinge")
	for i=1, #hinges do
		local joint = hinges[i]
		DebugPrint(joint)
	end
end
```

---

### [API] IsJointBroken

```lua
broken = IsJointBroken(joint)
```

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local broken = IsJointBroken(FindJoint("joint"))
	DebugPrint(broken)
end
```

---

### [API] GetJointType

```lua
type = GetJointType(joint)
```

Joint type is one of the following: "ball", "hinge", "prismatic" or "rope". An empty string is returned if joint handle is invalid.

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local joint = FindJoint("joint")
	if GetJointType(joint) == "rope" then
		DebugPrint("Joint is rope")
	end
end
```

---

### [API] GetJointOtherShape

```lua
other = GetJointOtherShape(joint, shape)
```

A joint is always connected to two shapes. Use this function if you know one shape and want to find the other one.

**Arguments:**

- `joint` *(number)* — Joint handle

- `shape` *(number)* — Shape handle

**Example:**

```lua
function init()
	local joint = FindJoint("joint")
	--joint is connected to A and B

	otherShape = GetJointOtherShape(joint, FindShape("shapeA"))
	--otherShape is now B

	otherShape = GetJointOtherShape(joint, FindShape("shapeB"))
	--otherShape is now A
end
```

---

### [API] GetJointShapes

```lua
shapes = GetJointShapes(joint)
```

Get shapes connected to the joint.

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
local mainBody
local shapes
local joint
function init()
	joint = FindJoint("joint")
	mainBody = GetVehicleBody(FindVehicle("vehicle"))
	shapes = GetJointShapes(joint)
end

function tick()
	-- Check to see if joint chain is still connected to vehicle main body
	-- If not then disable motors

	local connected = false
	for i=1,#shapes do
	
		local body = GetShapeBody(shapes[i])
	
		if body == mainBody then
			connected = true
		end
	
	end
	
	if connected then
		SetJointMotor(joint, 0.5)
	else
		SetJointMotor(joint, 0.0)
	end
end
```

---

### [API] SetJointMotor

```lua
SetJointMotor(joint, velocity, [strength])
```

Set joint motor target velocity. If joint is of type hinge, velocity is given in radians per second angular velocity. If joint type is prismatic joint velocity is given in meters per second. Calling this function will override and void any previous call to SetJointMotorTarget.

**Arguments:**

- `joint` *(number)* — Joint handle

- `velocity` *(number)* — Desired velocity

- `strength` *(number, optional)* — Desired strength. Default is infinite. Zero to disable.

**Example:**

```lua
function init()
	--Set motor speed to 0.5 radians per second
	SetJointMotor(FindJoint("hinge"), 0.5)
end
```

---

### [API] SetJointMotorTarget

```lua
SetJointMotorTarget(joint, target, [maxVel], [strength])
```

If a joint has a motor target, it will try to maintain its relative movement. This is very useful for elevators or other animated, jointed mechanisms. If joint is of type hinge, target is an angle in degrees (-180 to 180) and velocity is given in radians per second. If joint type is prismatic, target is given in meters and velocity is given in meters per second. Setting a motor target will override any previous call to SetJointMotor.

**Arguments:**

- `joint` *(number)* — Joint handle

- `target` *(number)* — Desired movement target

- `maxVel` *(number, optional)* — Maximum velocity to reach target. Default is infinite.

- `strength` *(number, optional)* — Desired strength. Default is infinite. Zero to disable.

**Example:**

```lua
function init()
	--Make joint reach a 45 degree angle, going at a maximum of 3 radians per second
	SetJointMotorTarget(FindJoint("hinge"), 45, 3)
end
```

---

### [API] GetJointLimits

```lua
min, max = GetJointLimits(joint)
```

Return joint limits for hinge or prismatic joint. Returns angle or distance depending on joint type.

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local min, max = GetJointLimits(FindJoint("hinge"))
	DebugPrint(min .. "-" .. max)
end
```

---

### [API] GetJointMovement

```lua
movement = GetJointMovement(joint)
```

Return the current position or angle or the joint, measured in same way as joint limits.

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local current = GetJointMovement(FindJoint("hinge"))
	DebugPrint(current)
end
```

---

### [API] GetJointedBodies

```lua
bodies = GetJointedBodies(body)
```

**Arguments:**

- `body` *(number)* — Body handle (must be dynamic)

**Example:**

```lua
local body = 0
function init()
	body = FindBody("body")
end

function tick()
	--Draw outline for all bodies in jointed structure
	local all = GetJointedBodies(body)
	for i=1,#all do
		DrawBodyOutline(all[i], 0.5)
	end
end
```

---

### [API] DetachJointFromShape

```lua
DetachJointFromShape(joint, shape)
```

Detach joint from shape. If joint is not connected to shape, nothing happens.

**Arguments:**

- `joint` *(number)* — Joint handle

- `shape` *(number)* — Shape handle

**Example:**

```lua
function init()
	DetachJointFromShape(FindJoint("joint"), FindShape("door"))
end
```

---

### [API] GetRopeNumberOfPoints

```lua
amount = GetRopeNumberOfPoints(joint)
```

Returns the number of points in the rope given its handle. Will return zero if the handle is not a rope

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local joint = FindJoint("joint")
	local numberPoints = GetRopeNumberOfPoints(joint)
end
```

---

### [API] GetRopePointPosition

```lua
pos = GetRopePointPosition(joint, index)
```

Returns the world position of the rope's point. Will return nil if the handle is not a rope or the index is not valid

**Arguments:**

- `joint` *(number)* — Joint handle

- `index` *(number)* — The point index, starting at 1

**Example:**

```lua
function init()
	local joint = FindJoint("joint")
	numberPoints = GetRopeNumberOfPoints(joint)

	for pointIndex = 1, numberPoints do
		DebugCross(GetRopePointPosition(joint, pointIndex))
	end
end
```

---

### [API] GetRopeBounds

```lua
min, max = GetRopeBounds(joint)
```

Returns the bounds of the rope. Will return nil if the handle is not a rope

**Arguments:**

- `joint` *(number)* — Joint handle

**Example:**

```lua
function init()
	local joint = FindJoint("joint")
	local mi, ma = GetRopeBounds(joint)

	DebugCross(mi)
	DebugCross(ma)
end
```

---

### [API] BreakRope

```lua
BreakRope(joint, point)
```

Breaks the rope at the specified point.

**Arguments:**

- `joint` *(number)* — Rope type joint handle

- `point` *(TVec)* — Point of break as world space vector

**Example:**

```lua
function tick()
	local playerCameraTransform = GetPlayerCameraTransform()
	local dir = TransformToParentVec(playerCameraTransform, Vec(0, 0, -1))

	local hit, dist, joint = QueryRaycastRope(playerCameraTransform.pos, dir, 5)
	if hit then
		local breakPoint = VecAdd(playerCameraTransform.pos, VecScale(dir, dist))
		BreakRope(joint, breakPoint)
	end
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | **Joint** | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)