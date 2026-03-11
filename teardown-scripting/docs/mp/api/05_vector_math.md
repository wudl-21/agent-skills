# Vector math

Vector math is used in Teardown scripts to represent 3D positions, directions,
rotations and transforms. The base types are vectors, quaternions and transforms.
Vectors and quaternions are indexed tables with three and four components. Transforms
are tables consisting of one vector (pos) and one quaternion (rot)

### [API] vec = Vec([x], [y], [z])
- **Args:**
  - `x` *(number, optional)* — X value
  - `y` *(number, optional)* — Y value
  - `z` *(number, optional)* — Z value
- **Returns:**
  - `vec` *(TVec)* — New vector
```lua
function init()
	--These are equivalent
	local a1 = Vec()
	local a2 = {0, 0, 0}
	DebugPrint("a1 == a2: " .. tostring(VecStr(a1) == VecStr(a2)))

	--These are equivalent
	local b1 = Vec(0, 1, 0)
	local b2 = {0, 1, 0}
	DebugPrint("b1 == b2: " .. tostring(VecStr(b1) == VecStr(b2)))
end
```

### [API] new = VecCopy(org)
- **Args:**
  - `org` *(TVec)* — A vector
- **Returns:**
  - `new` *(TVec)* — Copy of org vector
```lua
function init()
	--Do this to assign a vector
	local right1 = Vec(1, 2, 3)
	local right2 = VecCopy(right1)

	--Never do this unless you REALLY know what you're doing
	local wrong1 = Vec(1, 2, 3)
	local wrong2 = wrong1
end
```

### [API] str = VecStr(vector)
- **Args:**
  - `vector` *(TVec)* — Vector
- **Returns:**
  - `str` *(string)* — String representation
```lua
function init()
	local v = Vec(0, 10, 0)
	DebugPrint(VecStr(v))
end
```

### [API] length = VecLength(vec)
- **Args:**
  - `vec` *(TVec)* — A vector
- **Returns:**
  - `length` *(number)* — Length (magnitude) of the vector
```lua
function init()
	local v = Vec(1,1,0)
	local l = VecLength(v)
	--l now equals 1.4142
	DebugPrint(l)
end
```

### [API] norm = VecNormalize(vec)
- **Args:**
  - `vec` *(TVec)* — A vector
- **Returns:**
  - `norm` *(TVec)* — A vector of length 1.0
```lua
function init()
	local v = Vec(0,3,0)
	local n = VecNormalize(v)
	--n now equals {0,1,0}
	DebugPrint(VecStr(n))
end
```

### [API] norm = VecScale(vec, scale)
- **Args:**
  - `vec` *(TVec)* — A vector
  - `scale` *(number)* — A scale factor
- **Returns:**
  - `norm` *(TVec)* — A scaled version of input vector
```lua
function init()
	local v = Vec(1,2,3)
	local n = VecScale(v, 2)
	--n now equals {2,4,6}
	DebugPrint(VecStr(n))
end
```

### [API] c = VecAdd(a, b)
- **Args:**
  - `a` *(TVec)* — Vector
  - `b` *(TVec)* — Vector
- **Returns:**
  - `c` *(TVec)* — New vector with sum of a and b
```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,0,0)
	local c = VecAdd(a, b)
	--c now equals {4,2,3}
	DebugPrint(VecStr(c))
end
```

### [API] c = VecSub(a, b)
- **Args:**
  - `a` *(TVec)* — Vector
  - `b` *(TVec)* — Vector
- **Returns:**
  - `c` *(TVec)* — New vector representing a-b
```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,0,0)
	local c = VecSub(a, b)
	--c now equals {-2,2,3}
	DebugPrint(VecStr(c))
end
```

### [API] c = VecDot(a, b)
- **Args:**
  - `a` *(TVec)* — Vector
  - `b` *(TVec)* — Vector
- **Returns:**
  - `c` *(number)* — Dot product of a and b
```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,1,0)
	local c = VecDot(a, b)
	--c now equals 5
	DebugPrint(c)
end
```

### [API] c = VecCross(a, b)
- **Args:**
  - `a` *(TVec)* — Vector
  - `b` *(TVec)* — Vector
- **Returns:**
  - `c` *(TVec)* — Cross product of a and b (also called vector product)
```lua
function init()
	local a = Vec(1,0,0)
	local b = Vec(0,1,0)
	local c = VecCross(a, b)
	--c now equals {0,0,1}
	DebugPrint(VecStr(c))
end
```

### [API] c = VecLerp(a, b, t)
- **Args:**
  - `a` *(TVec)* — Vector
  - `b` *(TVec)* — Vector
  - `t` *(number)* — fraction (usually between 0.0 and 1.0)
- **Returns:**
  - `c` *(TVec)* — Linearly interpolated vector between a and b, using t
```lua
function init()
	local a = Vec(2,0,0)
	local b = Vec(0,4,2)
	local t = 0.5
	
	--These two are equivalent
	local c1 = VecLerp(a, b, t)
	local c2 = VecAdd(VecScale(a, 1-t), VecScale(b, t))
	
	--c1 and c2 now equals {1, 2, 1}
	DebugPrint("c1" .. VecStr(c1) .. " == c2" .. VecStr(c2))
end
```

### [API] quat = Quat([x], [y], [z], [w])
- **Args:**
  - `x` *(number, optional)* — X value
  - `y` *(number, optional)* — Y value
  - `z` *(number, optional)* — Z value
  - `w` *(number, optional)* — W value
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	--These are equivalent
	local a1 = Quat()
	local a2 = {0, 0, 0, 1}

	DebugPrint(QuatStr(a1) == QuatStr(a2))
end
```

### [API] new = QuatCopy(org)
- **Args:**
  - `org` *(TQuat)* — Quaternion
- **Returns:**
  - `new` *(TQuat)* — Copy of org quaternion
```lua
function init()
	--Do this to assign a quaternion
	local right1 = QuatEuler(0, 90, 0)
	local right2 = QuatCopy(right1)

	--Never do this unless you REALLY know what you're doing
	local wrong1 = QuatEuler(0, 90, 0)
	local wrong2 = wrong1
end
```

### [API] quat = QuatAxisAngle(axis, angle)
- **Args:**
  - `axis` *(TVec)* — Rotation axis, unit vector
  - `angle` *(number)* — Rotation angle in degrees
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	--Create quaternion representing rotation 30 degrees around Y axis
	local q = QuatAxisAngle(Vec(0,1,0), 30)
	DebugPrint(QuatStr(q))
end
```

### [API] quat = QuatDeltaNormals(normal0, normal1)
- **Args:**
  - `normal0` *(TVec)* — Unit vector
  - `normal1` *(TVec)* — Unit vector
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	--Create quaternion representing a rotation between x-axis and y-axis
	local q = QuatDeltaNormals(Vec(1,0,0), Vec(0,1,0))
end
```

### [API] quat = QuatDeltaVectors(vector0, vector1)
- **Args:**
  - `vector0` *(TVec)* — Vector
  - `vector1` *(TVec)* — Vector
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	--Create quaternion representing a rotation between two non-unit vectors aligned along x-axis and y-axis
	local q = QuatDeltaVectors(Vec(10,0,0), Vec(0,5,0))
end
```

### [API] quat = QuatEuler(x, y, z)
- **Args:**
  - `x` *(number)* — Angle around X axis in degrees, sometimes also called roll or bank
  - `y` *(number)* — Angle around Y axis in degrees, sometimes also called yaw or heading
  - `z` *(number)* — Angle around Z axis in degrees, sometimes also called pitch or attitude
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	--Create quaternion representing rotation 30 degrees around Y axis and 25 degrees around Z axis
	local q = QuatEuler(0, 30, 25)
end
```

### [API] quat = QuatAlignXZ(xAxis, zAxis)
- **Args:**
  - `xAxis` *(TVec)* — X axis
  - `zAxis` *(TVec)* — Z axis
- **Returns:**
  - `quat` *(TQuat)* — Quaternion
```lua
function update()
	local laserSprite = LoadSprite("gfx/laser.png")
	local origin = Vec(0, 0, 0)
	local dir = Vec(1, 0, 0)
	local length = 10
	local hitPoint = VecAdd(origin, VecScale(dir, length))
	local t = Transform(VecLerp(origin, hitPoint, 0.5))
	local xAxis = VecNormalize(VecSub(hitPoint, origin))
	local zAxis = VecNormalize(VecSub(origin, GetCameraTransform().pos))
	t.rot = QuatAlignXZ(xAxis, zAxis)
	DrawSprite(laserSprite, t, length, 0.05+math.random()*0.01, 8, 4, 4, 1, true, true)
	DrawSprite(laserSprite, t, length, 0.5, 1.0, 0.3, 0.3, 1, true, true)
end
```

### [API] x, y, z = GetQuatEuler(quat)
- **Args:**
  - `quat` *(TQuat)* — Quaternion
- **Returns:**
  - `x` *(number)* — Angle around X axis in degrees, sometimes also called roll or bank
  - `y` *(number)* — Angle around Y axis in degrees, sometimes also called yaw or heading
  - `z` *(number)* — Angle around Z axis in degrees, sometimes also called pitch or attitude
```lua
function init()
	--Return euler angles from quaternion q
	q = QuatEuler(30, 45, 0)
	rx, ry, rz = GetQuatEuler(q)
	DebugPrint(rx .. " " .. ry .. " " .. rz)
end
```

### [API] quat = QuatLookAt(eye, target)
- **Args:**
  - `eye` *(TVec)* — Vector representing the camera location
  - `target` *(TVec)* — Vector representing the point to look at
- **Returns:**
  - `quat` *(TQuat)* — New quaternion
```lua
function init()
	local eye = Vec(0, 10, 0)
	local target = Vec(0, 1, 5)
	local rot = QuatLookAt(eye, target)
	SetCameraTransform(Transform(eye, rot))
end
```

### [API] c = QuatSlerp(a, b, t)
- **Args:**
  - `a` *(TQuat)* — Quaternion
  - `b` *(TQuat)* — Quaternion
  - `t` *(number)* — fraction (usually between 0.0 and 1.0)
- **Returns:**
  - `c` *(TQuat)* — New quaternion
```lua
function init()
	local a = QuatEuler(0, 10, 0)
	local b = QuatEuler(0, 0, 45)

	--Create quaternion half way between a and b
	local q = QuatSlerp(a, b, 0.5)
	DebugPrint(QuatStr(q))
end
```

### [API] str = QuatStr(quat)
- **Args:**
  - `quat` *(TQuat)* — Quaternion
- **Returns:**
  - `str` *(string)* — String representation
```lua
function init()
	local q = QuatEuler(0, 10, 0)
	DebugPrint(QuatStr(q))
end
```

### [API] c = QuatRotateQuat(a, b)
- **Args:**
  - `a` *(TQuat)* — Quaternion
  - `b` *(TQuat)* — Quaternion
- **Returns:**
  - `c` *(TQuat)* — New quaternion
```lua
function init()
	local a = QuatEuler(0, 10, 0)
	local b = QuatEuler(0, 0, 45)
	local q = QuatRotateQuat(a, b)

	--q now represents a rotation first 10 degrees around
	--the Y axis and then 45 degrees around the Z axis.
	local x, y, z = GetQuatEuler(q)
	DebugPrint(x .. " " .. y .. " " .. z)
end
```

### [API] vec = QuatRotateVec(a, vec)
- **Args:**
  - `a` *(TQuat)* — Quaternion
  - `vec` *(TVec)* — Vector
- **Returns:**
  - `vec` *(TVec)* — Rotated vector
```lua
function init()
	local q = QuatEuler(0, 10, 0)
	local v = Vec(1, 0, 0)
	local r = QuatRotateVec(q, v)
	
	--r is now vector a rotated 10 degrees around the Y axis
	DebugPrint(VecStr(r))
end
```

### [API] transform = Transform([pos], [rot])
- **Args:**
  - `pos` *(TVec, optional)* — Vector representing transform position
  - `rot` *(TQuat, optional)* — Quaternion representing transform rotation
- **Returns:**
  - `transform` *(TTransform)* — New transform
```lua
function init()
	--Create transform located at {0, 0, 0} with no rotation
	local t1 = Transform()

	--Create transform located at {10, 0, 0} with no rotation
	local t2 = Transform(Vec(10, 0,0))

	--Create transform located at {10, 0, 0}, rotated 45 degrees around Y axis
	local t3 = Transform(Vec(10, 0,0), QuatEuler(0, 45, 0))

	DebugPrint(TransformStr(t1))
	DebugPrint(TransformStr(t2))
	DebugPrint(TransformStr(t3))
end
```

### [API] new = TransformCopy(org)
- **Args:**
  - `org` *(TTransform)* — Transform
- **Returns:**
  - `new` *(TTransform)* — Copy of org transform
```lua
function init()
	--Do this to assign a quaternion
	local right1 = Transform(Vec(1,0,0), QuatEuler(0, 90, 0))
	local right2 = TransformCopy(right1)

	--Never do this unless you REALLY know what you're doing
	local wrong1 = Transform(Vec(1,0,0), QuatEuler(0, 90, 0))
	local wrong2 = wrong1
end
```

### [API] str = TransformStr(transform)
- **Args:**
  - `transform` *(TTransform)* — Transform
- **Returns:**
  - `str` *(string)* — String representation
```lua
function init()
	local eye = Vec(0, 10, 0)
	local target = Vec(0, 1, 5)
	local rot = QuatLookAt(eye, target)
	local t = Transform(eye, rot)
	DebugPrint(TransformStr(t))
end
```

### [API] transform = TransformToParentTransform(parent, child)
- **Args:**
  - `parent` *(TTransform)* — Transform
  - `child` *(TTransform)* — Transform
- **Returns:**
  - `transform` *(TTransform)* — New transform
```lua
function init()
	local b = GetBodyTransform(body)
	local s = GetShapeLocalTransform(shape)

	--b represents the location of body in world space
	--s represents the location of shape in body space

	local w = TransformToParentTransform(b, s)

	--w now represents the location of shape in world space
	DebugPrint(TransformStr(w))
end
```

### [API] transform = TransformToLocalTransform(parent, child)
- **Args:**
  - `parent` *(TTransform)* — Transform
  - `child` *(TTransform)* — Transform
- **Returns:**
  - `transform` *(TTransform)* — New transform
```lua
function init()
	local b = GetBodyTransform(body)
	local w = GetShapeWorldTransform(shape)

	--b represents the location of body in world space
	--w represents the location of shape in world space
	
	local s = TransformToLocalTransform(b, w)

	--s now represents the location of shape in body space.
	DebugPrint(TransformStr(s))
end
```

### [API] r = TransformToParentVec(t, v)
- **Args:**
  - `t` *(TTransform)* — Transform
  - `v` *(TVec)* — Vector
- **Returns:**
  - `r` *(TVec)* — Transformed vector
```lua
function init()
	local t = GetBodyTransform(body)
	local localUp = Vec(0, 1, 0)
	local up = TransformToParentVec(t, localUp)

	--up now represents the local body up direction in world space
	DebugPrint(VecStr(up))
end
```

### [API] r = TransformToLocalVec(t, v)
- **Args:**
  - `t` *(TTransform)* — Transform
  - `v` *(TVec)* — Vector
- **Returns:**
  - `r` *(TVec)* — Transformed vector
```lua
function init()
	local t = GetBodyTransform(body)
	local localUp = Vec(0, 1, 0)
	local up = TransformToParentVec(t, localUp)

	--up now represents the local body up direction in world space
	DebugPrint(VecStr(up))
end
```

### [API] r = TransformToParentPoint(t, p)
- **Args:**
  - `t` *(TTransform)* — Transform
  - `p` *(TVec)* — Vector representing position
- **Returns:**
  - `r` *(TVec)* — Transformed position
```lua
function init()
	local t = GetBodyTransform(body)
	local bodyPoint = Vec(0, 0, -1)
	local p = TransformToParentPoint(t, bodyPoint)

	--p now represents the local body point {0, 0, -1 } in world space
	DebugPrint(VecStr(p))
end
```

### [API] r = TransformToLocalPoint(t, p)
- **Args:**
  - `t` *(TTransform)* — Transform
  - `p` *(TVec)* — Vector representing position
- **Returns:**
  - `r` *(TVec)* — Transformed position
```lua
function init()
	local t = GetBodyTransform(body)
	local worldOrigo = Vec(0, 0, 0)
	local p = TransformToLocalPoint(t, worldOrigo)

	--p now represents the position of world origo in local body space
	DebugPrint(VecStr(p))
end
```

### [API] SetRandomSeed(seed)
- **Args:**
  - `seed` *(number)* — Random seed
```lua
function init()
	SetRandomSeed(42)
	result = RollDie()
end
```

### [API] result = GetRandomBool()
- **Returns:**
  - `result` *(boolean)* — Random true/false
```lua
function init()
	isHeads = GetRandomBool()

	if isHeads then
		win = true
	end
end
```

### [API] result = GetRandomInt(min, max)
- **Args:**
  - `min` *(number)* — Lower number
  - `max` *(number)* — Upper number
- **Returns:**
  - `result` *(number)* — Random number in given range, including max.
```lua
function init()
	dieRoll = GetRandomInt(1,6)
	-- dieRoll is 1,2,3,4,5 or 6
end
```

### [API] result = GetRandomFloat(min, max)
- **Args:**
  - `min` *(number)* — Lower number
  - `max` *(number)* — Upper number
- **Returns:**
  - `result` *(number)* — Random number in given range, including max.
```lua
function init()
	-- Generate a random angle in range [0, 360]
	randomAngleDeg = GetRandomFloat(0.0f, 360.0f)
end
```

### [API] vector = GetRandomDirection([length])
- **Args:**
  - `length` *(number, optional)* — Optional length use to scale the generated direction.
- **Returns:**
  - `vector` *(Vec3)* — Random direction with unit length
```lua
function init()
	-- Generate a random direction.
	ricochetDirection = GetRandomDirection()
end
```

---
**Navigation:** [_INDEX](_INDEX.md)