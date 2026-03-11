# Joint

Joints are used to physically connect two shapes. There are several types of joints and they are typically
placed in the editor. When destruction occurs, joints may be transferred to new shapes, detached or
completely disabled.

### [API] handle = FindJoint([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first joint with specified tag or zero if not found
```lua
function client.init()
	local joint = FindJoint("doorhinge")
	DebugPrint(joint)
end
```

### [API] list = FindJoints([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all joints with specified tag
```lua
--Search for locations tagged "doorhinge" in script scope
function client.init()
	local hinges = FindJoints("doorhinge")
	for i=1, #hinges do
		local joint = hinges[i]
		DebugPrint(joint)
	end
end
```

### [API] broken = IsJointBroken(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `broken` *(boolean)* — True if joint is broken
```lua
function client.init()
	local broken = IsJointBroken(FindJoint("joint"))
	DebugPrint(broken)
end
```

### [API] type = GetJointType(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `type` *(string)* — Joint type
```lua
function client.init()
	local joint = FindJoint("joint")
	if GetJointType(joint) == "rope" then
		DebugPrint("Joint is rope")
	end
end
```

### [API] other = GetJointOtherShape(joint, shape)
- **Args:**
  - `joint` *(number)* — Joint handle
  - `shape` *(number)* — Shape handle
- **Returns:**
  - `other` *(number)* — Other shape handle
```lua
function client.init()
	local joint = FindJoint("joint")
	--joint is connected to A and B

	otherShape = GetJointOtherShape(joint, FindShape("shapeA"))
	--otherShape is now B

	otherShape = GetJointOtherShape(joint, FindShape("shapeB"))
	--otherShape is now A
end
```

### [API] shapes = GetJointShapes(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `shapes` *(number)* — Shape handles
```lua
local mainBody
local shapes
local joint
function server.init()
	joint = FindJoint("joint")
	mainBody = GetVehicleBody(FindVehicle("vehicle"))
	shapes = GetJointShapes(joint)
end

function server.tick()
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

### [API] SetJointMotor(joint, velocity, [strength])
- **Args:**
  - `joint` *(number)* — Joint handle
  - `velocity` *(number)* — Desired velocity
  - `strength` *(number, optional)* — Desired strength. Default is infinite. Zero to disable.
```lua
function server.init()
	--Set motor speed to 0.5 radians per second
	SetJointMotor(FindJoint("hinge"), 0.5)
end
```

### [API] SetJointMotorTarget(joint, target, [maxVel], [strength])
- **Args:**
  - `joint` *(number)* — Joint handle
  - `target` *(number)* — Desired movement target
  - `maxVel` *(number, optional)* — Maximum velocity to reach target. Default is infinite.
  - `strength` *(number, optional)* — Desired strength. Default is infinite. Zero to disable.
```lua
function server.init()
	--Make joint reach a 45 degree angle, going at a maximum of 3 radians per second
	SetJointMotorTarget(FindJoint("hinge"), 45, 3)
end
```

### [API] min, max = GetJointLimits(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `min` *(number)* — Minimum joint limit (angle or distance)
  - `max` *(number)* — Maximum joint limit (angle or distance)
```lua
function client.init()
	local min, max = GetJointLimits(FindJoint("hinge"))
	DebugPrint(min .. "-" .. max)
end
```

### [API] movement = GetJointMovement(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `movement` *(number)* — Current joint position or angle
```lua
function client.init()
	local current = GetJointMovement(FindJoint("hinge"))
	DebugPrint(current)
end
```

### [API] bodies = GetJointedBodies(body)
- **Args:**
  - `body` *(number)* — Body handle (must be dynamic)
- **Returns:**
  - `bodies` *(table)* — Handles to all dynamic bodies in the jointed structure. The input handle will also be included.
```lua
local body = 0
function client.init()
	body = FindBody("body")
end

function client.tick()
	--Draw outline for all bodies in jointed structure
	local all = GetJointedBodies(body)
	for i=1,#all do
		DrawBodyOutline(all[i], 0.5)
	end
end
```

### [API] DetachJointFromShape(joint, shape)
- **Args:**
  - `joint` *(number)* — Joint handle
  - `shape` *(number)* — Shape handle
```lua
function server.init()
	DetachJointFromShape(FindJoint("joint"), FindShape("door"))
end
```

### [API] amount = GetRopeNumberOfPoints(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `amount` *(number)* — Number of points in a rope or zero if invalid
```lua
function client.init()
	local joint = FindJoint("joint")
	local numberPoints = GetRopeNumberOfPoints(joint)
end
```

### [API] pos = GetRopePointPosition(joint, index)
- **Args:**
  - `joint` *(number)* — Joint handle
  - `index` *(number)* — The point index, starting at 1
- **Returns:**
  - `pos` *(TVec)* — World position of the point, or nil, if invalid
```lua
function client.init()
	local joint = FindJoint("joint")
	numberPoints = GetRopeNumberOfPoints(joint)

	for pointIndex = 1, numberPoints do
		DebugCross(GetRopePointPosition(joint, pointIndex))
	end
end
```

### [API] min, max = GetRopeBounds(joint)
- **Args:**
  - `joint` *(number)* — Joint handle
- **Returns:**
  - `min` *(TVec)* — Lower point of rope bounds in world space
  - `max` *(TVec)* — Upper point of rope bounds in world space
```lua
function client.init()
	local joint = FindJoint("joint")
	local mi, ma = GetRopeBounds(joint)

	DebugCross(mi)
	DebugCross(ma)
end
```

### [API] BreakRope(joint, point)
- **Args:**
  - `joint` *(number)* — Rope type joint handle
  - `point` *(TVec)* — Point of break as world space vector
```lua
function doPlayerAction(playerId)
	local playerCameraTransform = GetPlayerCameraTransform(playerId)
	local dir = TransformToParentVec(playerCameraTransform, Vec(0, 0, -1))

	local hit, dist, joint = QueryRaycastRope(playerCameraTransform.pos, dir, 5)
	if hit then
		local breakPoint = VecAdd(playerCameraTransform.pos, VecScale(dir, dist))
		BreakRope(joint, breakPoint)
	end
end
```

---
**Navigation:** [_INDEX](_INDEX.md)