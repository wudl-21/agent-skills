# Teardown SP API — Vector math
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Vector math is used in Teardown scripts to represent 3D positions, directions, rotations and transforms. The base types are vectors, quaternions and transforms. Vectors and quaternions are indexed tables with three and four components. Transforms are tables consisting of one vector (pos) and one quaternion (rot)

---

### [API] Vec

```lua
vec = Vec([x], [y], [z])
```

Create new vector and optionally initializes it to the provided values. A Vec is equivalent to a regular lua table with three numbers.

**Arguments:**

- `x` *(number, optional)* — X value

- `y` *(number, optional)* — Y value

- `z` *(number, optional)* — Z value

**Example:**

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

---

### [API] VecCopy

```lua
new = VecCopy(org)
```

Vectors should never be assigned like regular numbers. Since they are implemented with lua tables assignment means two references pointing to the same data. Use this function instead.

**Arguments:**

- `org` *(TVec)* — A vector

**Example:**

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

---

### [API] VecStr

```lua
str = VecStr(vector)
```

Returns the string representation of vector

**Arguments:**

- `vector` *(TVec)* — Vector

**Example:**

```lua
function init()
	local v = Vec(0, 10, 0)
	DebugPrint(VecStr(v))
end
```

---

### [API] VecLength

```lua
length = VecLength(vec)
```

**Arguments:**

- `vec` *(TVec)* — A vector

**Example:**

```lua
function init()
	local v = Vec(1,1,0)
	local l = VecLength(v)
	--l now equals 1.4142
	DebugPrint(l)
end
```

---

### [API] VecNormalize

```lua
norm = VecNormalize(vec)
```

If the input vector is of zero length, the function returns {0,0,1}

**Arguments:**

- `vec` *(TVec)* — A vector

**Example:**

```lua
function init()
	local v = Vec(0,3,0)
	local n = VecNormalize(v)
	--n now equals {0,1,0}
	DebugPrint(VecStr(n))
end
```

---

### [API] VecScale

```lua
norm = VecScale(vec, scale)
```

**Arguments:**

- `vec` *(TVec)* — A vector

- `scale` *(number)* — A scale factor

**Example:**

```lua
function init()
	local v = Vec(1,2,3)
	local n = VecScale(v, 2)
	--n now equals {2,4,6}
	DebugPrint(VecStr(n))
end
```

---

### [API] VecAdd

```lua
c = VecAdd(a, b)
```

**Arguments:**

- `a` *(TVec)* — Vector

- `b` *(TVec)* — Vector

**Example:**

```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,0,0)
	local c = VecAdd(a, b)
	--c now equals {4,2,3}
	DebugPrint(VecStr(c))
end
```

---

### [API] VecSub

```lua
c = VecSub(a, b)
```

**Arguments:**

- `a` *(TVec)* — Vector

- `b` *(TVec)* — Vector

**Example:**

```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,0,0)
	local c = VecSub(a, b)
	--c now equals {-2,2,3}
	DebugPrint(VecStr(c))
end
```

---

### [API] VecDot

```lua
c = VecDot(a, b)
```

**Arguments:**

- `a` *(TVec)* — Vector

- `b` *(TVec)* — Vector

**Example:**

```lua
function init()
	local a = Vec(1,2,3)
	local b = Vec(3,1,0)
	local c = VecDot(a, b)
	--c now equals 5
	DebugPrint(c)
end
```

---

### [API] VecCross

```lua
c = VecCross(a, b)
```

**Arguments:**

- `a` *(TVec)* — Vector

- `b` *(TVec)* — Vector

**Example:**

```lua
function init()
	local a = Vec(1,0,0)
	local b = Vec(0,1,0)
	local c = VecCross(a, b)
	--c now equals {0,0,1}
	DebugPrint(VecStr(c))
end
```

---

### [API] VecLerp

```lua
c = VecLerp(a, b, t)
```

**Arguments:**

- `a` *(TVec)* — Vector

- `b` *(TVec)* — Vector

- `t` *(number)* — fraction (usually between 0.0 and 1.0)

**Example:**

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

---

### [API] Quat

```lua
quat = Quat([x], [y], [z], [w])
```

Create new quaternion and optionally initializes it to the provided values. Do not attempt to initialize a quaternion with raw values unless you know what you are doing. Use QuatEuler or QuatAxisAngle instead. If no arguments are given, a unit quaternion will be created: {0, 0, 0, 1}. A quaternion is equivalent to a regular lua table with four numbers.

**Arguments:**

- `x` *(number, optional)* — X value

- `y` *(number, optional)* — Y value

- `z` *(number, optional)* — Z value

- `w` *(number, optional)* — W value

**Example:**

```lua
function init()
	--These are equivalent
	local a1 = Quat()
	local a2 = {0, 0, 0, 1}

	DebugPrint(QuatStr(a1) == QuatStr(a2))
end
```

---

### [API] QuatCopy

```lua
new = QuatCopy(org)
```

Quaternions should never be assigned like regular numbers. Since they are implemented with lua tables assignment means two references pointing to the same data. Use this function instead.

**Arguments:**

- `org` *(TQuat)* — Quaternion

**Example:**

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

---

### [API] QuatAxisAngle

```lua
quat = QuatAxisAngle(axis, angle)
```

Create a quaternion representing a rotation around a specific axis

**Arguments:**

- `axis` *(TVec)* — Rotation axis, unit vector

- `angle` *(number)* — Rotation angle in degrees

**Example:**

```lua
function init()
	--Create quaternion representing rotation 30 degrees around Y axis
	local q = QuatAxisAngle(Vec(0,1,0), 30)
	DebugPrint(QuatStr(q))
end
```

---

### [API] QuatDeltaNormals

```lua
quat = QuatDeltaNormals(normal0, normal1)
```

Create a quaternion representing a rotation between the input normals

**Arguments:**

- `normal0` *(TVec)* — Unit vector

- `normal1` *(TVec)* — Unit vector

**Example:**

```lua
function init()
	--Create quaternion representing a rotation between x-axis and y-axis
	local q = QuatDeltaNormals(Vec(1,0,0), Vec(0,1,0))
end
```

---

### [API] QuatDeltaVectors

```lua
quat = QuatDeltaVectors(vector0, vector1)
```

Create a quaternion representing a rotation between the input vectors that doesn't need to be of unit-length

**Arguments:**

- `vector0` *(TVec)* — Vector

- `vector1` *(TVec)* — Vector

**Example:**

```lua
function init()
	--Create quaternion representing a rotation between two non-unit vectors aligned along x-axis and y-axis
	local q = QuatDeltaVectors(Vec(10,0,0), Vec(0,5,0))
end
```

---

### [API] QuatEuler

```lua
quat = QuatEuler(x, y, z)
```

Create quaternion using euler angle notation. The order of applied rotations uses the "NASA standard aeroplane" model: Rotation around Y axis (yaw or heading) Rotation around Z axis (pitch or attitude) Rotation around X axis (roll or bank)

**Arguments:**

- `x` *(number)* — Angle around X axis in degrees, sometimes also called roll or bank

- `y` *(number)* — Angle around Y axis in degrees, sometimes also called yaw or heading

- `z` *(number)* — Angle around Z axis in degrees, sometimes also called pitch or attitude

**Example:**

```lua
function init()
	--Create quaternion representing rotation 30 degrees around Y axis and 25 degrees around Z axis
	local q = QuatEuler(0, 30, 25)
end
```

---

### [API] QuatAlignXZ

```lua
quat = QuatAlignXZ(xAxis, zAxis)
```

Return the quaternion aligned to specified axes

**Arguments:**

- `xAxis` *(TVec)* — X axis

- `zAxis` *(TVec)* — Z axis

**Example:**

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

---

### [API] GetQuatEuler

```lua
x, y, z = GetQuatEuler(quat)
```

Return euler angles from quaternion. The order of rotations uses the "NASA standard aeroplane" model: Rotation around Y axis (yaw or heading) Rotation around Z axis (pitch or attitude) Rotation around X axis (roll or bank)

**Arguments:**

- `quat` *(TQuat)* — Quaternion

**Example:**

```lua
function init()
	--Return euler angles from quaternion q
	q = QuatEuler(30, 45, 0)
	rx, ry, rz = GetQuatEuler(q)
	DebugPrint(rx .. " " .. ry .. " " .. rz)
end
```

---

### [API] QuatLookAt

```lua
quat = QuatLookAt(eye, target)
```

Create a quaternion pointing the negative Z axis (forward) towards a specific point, keeping the Y axis upwards. This is very useful for creating camera transforms.

**Arguments:**

- `eye` *(TVec)* — Vector representing the camera location

- `target` *(TVec)* — Vector representing the point to look at

**Example:**

```lua
function init()
	local eye = Vec(0, 10, 0)
	local target = Vec(0, 1, 5)
	local rot = QuatLookAt(eye, target)
	SetCameraTransform(Transform(eye, rot))
end
```

---

### [API] QuatSlerp

```lua
c = QuatSlerp(a, b, t)
```

Spherical, linear interpolation between a and b, using t. This is very useful for animating between two rotations.

**Arguments:**

- `a` *(TQuat)* — Quaternion

- `b` *(TQuat)* — Quaternion

- `t` *(number)* — fraction (usually between 0.0 and 1.0)

**Example:**

```lua
function init()
	local a = QuatEuler(0, 10, 0)
	local b = QuatEuler(0, 0, 45)

	--Create quaternion half way between a and b
	local q = QuatSlerp(a, b, 0.5)
	DebugPrint(QuatStr(q))
end
```

---

### [API] QuatStr

```lua
str = QuatStr(quat)
```

Returns the string representation of quaternion

**Arguments:**

- `quat` *(TQuat)* — Quaternion

**Example:**

```lua
function init()
	local q = QuatEuler(0, 10, 0)
	DebugPrint(QuatStr(q))
end
```

---

### [API] QuatRotateQuat

```lua
c = QuatRotateQuat(a, b)
```

Rotate one quaternion with another quaternion. This is mathematically equivalent to c = a * b using quaternion multiplication.

**Arguments:**

- `a` *(TQuat)* — Quaternion

- `b` *(TQuat)* — Quaternion

**Example:**

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

---

### [API] QuatRotateVec

```lua
vec = QuatRotateVec(a, vec)
```

Rotate a vector by a quaternion

**Arguments:**

- `a` *(TQuat)* — Quaternion

- `vec` *(TVec)* — Vector

**Example:**

```lua
function init()
	local q = QuatEuler(0, 10, 0)
	local v = Vec(1, 0, 0)
	local r = QuatRotateVec(q, v)
	
	--r is now vector a rotated 10 degrees around the Y axis
	DebugPrint(VecStr(r))
end
```

---

### [API] Transform

```lua
transform = Transform([pos], [rot])
```

A transform is a regular lua table with two entries: pos and rot, a vector and quaternion representing transform position and rotation.

**Arguments:**

- `pos` *(TVec, optional)* — Vector representing transform position

- `rot` *(TQuat, optional)* — Quaternion representing transform rotation

**Example:**

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

---

### [API] TransformCopy

```lua
new = TransformCopy(org)
```

Transforms should never be assigned like regular numbers. Since they are implemented with lua tables assignment means two references pointing to the same data. Use this function instead.

**Arguments:**

- `org` *(TTransform)* — Transform

**Example:**

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

---

### [API] TransformStr

```lua
str = TransformStr(transform)
```

Returns the string representation of transform

**Arguments:**

- `transform` *(TTransform)* — Transform

**Example:**

```lua
function init()
	local eye = Vec(0, 10, 0)
	local target = Vec(0, 1, 5)
	local rot = QuatLookAt(eye, target)
	local t = Transform(eye, rot)
	DebugPrint(TransformStr(t))
end
```

---

### [API] TransformToParentTransform

```lua
transform = TransformToParentTransform(parent, child)
```

Transform child transform out of the parent transform. This is the opposite of TransformToLocalTransform.

**Arguments:**

- `parent` *(TTransform)* — Transform

- `child` *(TTransform)* — Transform

**Example:**

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

---

### [API] TransformToLocalTransform

```lua
transform = TransformToLocalTransform(parent, child)
```

Transform one transform into the local space of another transform. This is the opposite of TransformToParentTransform.

**Arguments:**

- `parent` *(TTransform)* — Transform

- `child` *(TTransform)* — Transform

**Example:**

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

---

### [API] TransformToParentVec

```lua
r = TransformToParentVec(t, v)
```

Transfom vector v out of transform t only considering rotation.

**Arguments:**

- `t` *(TTransform)* — Transform

- `v` *(TVec)* — Vector

**Example:**

```lua
function init()
	local t = GetBodyTransform(body)
	local localUp = Vec(0, 1, 0)
	local up = TransformToParentVec(t, localUp)

	--up now represents the local body up direction in world space
	DebugPrint(VecStr(up))
end
```

---

### [API] TransformToLocalVec

```lua
r = TransformToLocalVec(t, v)
```

Transfom vector v into transform t only considering rotation.

**Arguments:**

- `t` *(TTransform)* — Transform

- `v` *(TVec)* — Vector

**Example:**

```lua
function init()
	local t = GetBodyTransform(body)
	local localUp = Vec(0, 1, 0)
	local up = TransformToParentVec(t, localUp)

	--up now represents the local body up direction in world space
	DebugPrint(VecStr(up))
end
```

---

### [API] TransformToParentPoint

```lua
r = TransformToParentPoint(t, p)
```

Transfom position p out of transform t.

**Arguments:**

- `t` *(TTransform)* — Transform

- `p` *(TVec)* — Vector representing position

**Example:**

```lua
function init()
	local t = GetBodyTransform(body)
	local bodyPoint = Vec(0, 0, -1)
	local p = TransformToParentPoint(t, bodyPoint)

	--p now represents the local body point {0, 0, -1 } in world space
	DebugPrint(VecStr(p))
end
```

---

### [API] TransformToLocalPoint

```lua
r = TransformToLocalPoint(t, p)
```

Transfom position p into transform t.

**Arguments:**

- `t` *(TTransform)* — Transform

- `p` *(TVec)* — Vector representing position

**Example:**

```lua
function init()
	local t = GetBodyTransform(body)
	local worldOrigo = Vec(0, 0, 0)
	local p = TransformToLocalPoint(t, worldOrigo)

	--p now represents the position of world origo in local body space
	DebugPrint(VecStr(p))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | **Vector math** | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)