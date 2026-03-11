# Teardown SP API — Animation
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

An animator manages a prefab hierarchy using a matching skeleton and a set of animation sequences. These animations are processed sequentially, generating a "blend-tree."

There are two types of animations: looping and single-shot. Looping animations must be called every frame to keep them active; otherwise, they will stop. In contrast, single-shot animations are triggered once and will play to completion.

Single-shot animations are automatically processed after all looping animations, but they can be executed earlier if necessary. To ensure that single-shot animations are processed in the correct order within the blend-tree, an instance API is available.

Inverse Kinematics (IK) can be used, typically as the final step, to control specific parts of the skeleton, such as reaching for an object.

---

### [API] SetAnimatorPositionIK

```lua
SetAnimatorPositionIK(handle, begname, endname, target, [weight], [history], [flag])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `begname` *(string)* — Name of the start-bone of the chain

- `endname` *(string)* — Name of the end-bone of the chain

- `target` *(TVec)* — World target position that the "endname" bone should reach

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `history` *(number, optional)* — How much of the previous frames result [0,1] that should be used when start the IK search, default is 0.0

- `flag` *(boolean, optional)* — TRUE if constraints should be used, default is TRUE

**Example:**

```lua
SetAnimatorPositionIK(animator, "shoulder_l", "hand_l", Vec(10, 0, 0), 1.0, 0.9, true)
```

---

### [API] SetAnimatorTransformIK

```lua
SetAnimatorTransformIK(handle, begname, endname, transform, [weight], [history], [locktarget], [useconstraints])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `begname` *(string)* — Name of the start-bone of the chain

- `endname` *(string)* — Name of the end-bone of the chain

- `transform` *(TTransform)* — World target transform that the "endname" bone should reach

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `history` *(number, optional)* — How much of the previous frames result [0,1] that should be used when start the IK search, default is 0.0

- `locktarget` *(boolean, optional)* — TRUE if the end-bone should be fixed to the target-transform, FALSE if IK solution is used

- `useconstraints` *(boolean, optional)* — TRUE if constraints should be used, default is TRUE

**Example:**

```lua
SetAnimatorTransformIK(animator, "shoulder_l", "hand_l", Transform(10, 0, 0), 1.0, 0.9, false, true)
```

---

### [API] GetBoneChainLength

```lua
length = GetBoneChainLength(handle, begname, endname)
```

This will calculate the length of the bone-chain between the endpoints. If the skeleton have a chain like this (shoulder_l -> upper_arm_l -> lower_arm_l -> hand_l) it will return the length of the upper_arm_l+lower_arm_l

**Arguments:**

- `handle` *(number)* — Animator handle

- `begname` *(string)* — Name of the start-bone of the chain

- `endname` *(string)* — Name of the end-bone of the chain

**Example:**

```lua
local length = GetBoneChainLength(animator, "shoulder_l", "hand_l")
```

---

### [API] FindAnimator

```lua
handle = FindAnimator([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
--Search for the first animator in script scope
local animator = FindAnimator()

--Search for an animator tagged "anim" in script scope
local animator = FindAnimator("anim")

--Search for an animator tagged "anim2" in entire scene
local anim2 = FindAnimator("anim2", true)
```

---

### [API] FindAnimators

```lua
list = FindAnimators([tag], [global])
```

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

**Example:**

```lua
--Search for animators tagged "target" in script scope
local targets = FindAnimators("target")
for i=1, #targets do
	local target = targets[i]
	...
end
```

---

### [API] GetAnimatorTransform

```lua
transform = GetAnimatorTransform(handle)
```

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
local pos = GetAnimatorTransform(animator).pos
```

---

### [API] GetAnimatorAdjustTransformIK

```lua
transform = GetAnimatorAdjustTransformIK(handle, name)
```

When using IK for a character you can use ik-helpers to define where the

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Name of the location node

**Example:**

```lua
--This will adjust the target transform so that the grip defined by a location node in editor called "ik_hand_l" will reach the target
local target = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
local adj = GetAnimatorAdjustTransformIK(animator, "ik_hand_l")
if adj ~= nil then
    target = TransformToParentTransform(target, adj)
end
SetAnimatorTransformIK(animator, "shoulder_l", "hand_l", target, 1.0, 0.9)
```

---

### [API] SetAnimatorTransform

```lua
SetAnimatorTransform(handle, transform)
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `transform` *(TTransform)* — Desired transform

**Example:**

```lua
local t = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
SetAnimatorTransform(animator, t)
```

---

### [API] MakeRagdoll

```lua
MakeRagdoll(handle)
```

Make all prefab bodies physical and leave control to physics system

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
MakeRagdoll(animator)
```

---

### [API] UnRagdoll

```lua
UnRagdoll(handle, [time])
```

Take control if the prefab bodies and do an optional blend between the current ragdoll state and current animation state

**Arguments:**

- `handle` *(number)* — Animator handle

- `time` *(number, optional)* — Transition time

**Example:**

```lua
--Take control of bodies and do a blend during one sec between the animation state and last physics state
UnRagdoll(animator, 1.0)
```

---

### [API] PlayAnimation

```lua
handle = PlayAnimation(handle, name, [weight], [filter])
```

Single animations, one-shot, will be processed after looping animations.

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Animation name

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `filter` *(string, optional)* — Name of the bone and its subtree that will be affected

**Example:**

```lua
--This will play a single animation "Shooting" with a 80% influence but only on the skeleton starting at bone "Spine"
PlayAnimation(animator, "Shooting", 0.8, "Spine")
```

---

### [API] PlayAnimationLoop

```lua
PlayAnimationLoop(handle, name, [weight], [filter])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Animation name

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `filter` *(string, optional)* — Name of the bone and its subtree that will be affected

**Example:**

```lua
--This will play an animation loop "Walking" with a 100% influence on the whole skeleton
PlayAnimationLoop(animator, "Walking")
```

---

### [API] PlayAnimationInstance

```lua
handle = PlayAnimationInstance(handle, instance, [weight], [speed])
```

Single animations, one-shot, will be processed after looping animations.

**Arguments:**

- `handle` *(number)* — Animator handle

- `instance` *(number)* — Instance handle

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `speed` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

**Example:**

```lua
--This will play a single animation "Shooting" with a 80% influence but only on the skeleton starting at bone "Spine"
PlayAnimation(animator, "Shooting", 0.8, "Spine")
```

---

### [API] StopAnimationInstance

```lua
StopAnimationInstance(handle, instance)
```

This will stop the playing anim-instance

**Arguments:**

- `handle` *(number)* — Animator handle

- `instance` *(number)* — Instance handle

---

### [API] PlayAnimationFrame

```lua
PlayAnimationFrame(handle, name, time, [weight], [filter])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0

- `filter` *(string, optional)* — Name of the bone and its subtree that will be affected

**Example:**

```lua
--This will play an animation "Walking" at a specific time of 1.5s with a 80% influence on the whole skeleton
PlayAnimationFrame(animator, "Walking", 1.5, 0.8)
```

---

### [API] BeginAnimationGroup

```lua
BeginAnimationGroup(handle, [weight], [filter])
```

You can group looping animations together and use the result of those to blend to target. PlayAnimation will not work here since they are processed last separately from blendgroups.

**Arguments:**

- `handle` *(number)* — Animator handle

- `weight` *(number, optional)* — Weight [0,1] of this group, default is 1.0

- `filter` *(string, optional)* — Name of the bone and its subtree that will be affected

**Example:**

```lua
--This will blend an entire group with 50% influence
BeginAnimationGroup(animator, 0.5)
	PlayAnimationLoop(...)
	PlayAnimationLoop(...)
EndAnimationGroup(animator)

--You can also create a tree of groups, blending is performed in a depth-first order
BeginAnimationGroup(animator, 0.5)
	PlayAnimationLoop(animator, "anim_a", 1.0)
	PlayAnimationLoop(animator, "anim_b", 0.2)
	BeginAnimationGroup(animator, 0.75)
		PlayAnimationLoop(animator, "anim_c", 1.0)
		PlayAnimationLoop(animator, "anim_d", 0.25)
	EndAnimationGroup(animator)
EndAnimationGroup(animator)
```

---

### [API] EndAnimationGroup

```lua
EndAnimationGroup(handle)
```

Ends the group created by BeginAnimationGroup

**Arguments:**

- `handle` *(number)* — Animator handle

---

### [API] PlayAnimationInstances

```lua
PlayAnimationInstances(handle)
```

Single animations, one-shot, will be processed after looping animations. By calling PlayAnimationInstances you can force it to be processed earlier and be able to "overwrite" the result of it if you want

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
--First we play a single jump animation affecting the whole skeleton
--Then we play an aiming animation on the upper-body, filter="Spine1", keeping the lower-body unaffected
--Then we force the single-animations to be processed, this will force the "jump" to be processed.
--Then we overwrite just the spine-bone with a mouse controlled rotation("rot")
--Result will be a jump animation with the upperbody playing an aiming animation but the pitch of the spine controlled by the mouse("rot")

if InputPressed("jump") then
	PlayAnimation(animator, "Jump")
end
PlayAnimationLoop(animator, "Pistol Idle", aimWeight, "Spine1")
PlayAnimationInstances(animator)
SetBoneRotation(animator, "Spine1", rot, 1)
```

---

### [API] GetAnimationClipNames

```lua
list = GetAnimationClipNames(handle)
```

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
local list = GetAnimationClipNames(animator)
for i=1, #list do
	local name = list[i]
	..
end
```

---

### [API] GetAnimationClipDuration

```lua
time = GetAnimationClipDuration(handle, name)
```

**Arguments:**

- `handle` *(number)* — Animator handle

---

### [API] SetAnimationClipFade

```lua
SetAnimationClipFade(handle, name, fadein, fadeout)
```

**Arguments:**

- `fadeout` *(number)* — Fadeout time of the animation

**Example:**

```lua
SetAnimationClipFade(animator, "fire", 0.5, 0.5)
```

---

### [API] SetAnimationClipSpeed

```lua
SetAnimationClipSpeed(handle, name, speed)
```

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
--This will make the clip run 2x as normal speed
SetAnimationClipSpeed(animator, "walking", 2)
```

---

### [API] TrimAnimationClip

```lua
TrimAnimationClip(handle, name, begoffset, [endoffset])
```

**Arguments:**

- `begoffset` *(number)* — Time offset from the beginning of the animation

- `endoffset` *(number, optional)* — Time offset, positive value means from the beginning and negative value means from the end, zero(default) means at end

**Example:**

```lua
--This will "remove" 1s from the beginning and 2s from the end.
TrimAnimationClip(animator, "walking", 1, -2)
```

---

### [API] GetAnimationClipLoopPosition

```lua
time = GetAnimationClipLoopPosition(handle, name)
```

**Arguments:**

- `handle` *(number)* — Animator handle

---

### [API] GetAnimationInstancePosition

```lua
time = GetAnimationInstancePosition(handle, instance)
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `instance` *(number)* — Instance handle

---

### [API] SetAnimationClipLoopPosition

```lua
SetAnimationClipLoopPosition(handle, name, time)
```

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
--This will set the current playposition to one second
SetAnimationClipLoopPosition(animator, "walking", 1)
```

---

### [API] SetBoneRotation

```lua
SetBoneRotation(handle, name, quat, [weight])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `weight` *(number, optional)* — Weight [0,1] default is 1.0

**Example:**

```lua
--This will set the existing rotation by QuatEuler(...)
SetBoneRotation(animator, "spine", QuatEuler(0, 180, 0), 1.0)
```

---

### [API] SetBoneLookAt

```lua
SetBoneLookAt(handle, name, point, [weight])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `weight` *(number, optional)* — Weight [0,1] default is 1.0

**Example:**

```lua
--This will set the existing local-rotation to point to world space point
SetBoneLookAt(animator, "upper_arm_l", Vec(10, 20, 30), 1.0)
```

---

### [API] RotateBone

```lua
RotateBone(handle, name, quat, [weight])
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `weight` *(number, optional)* — Weight [0,1] default is 1.0

**Example:**

```lua
--This will offset the existing rotation by QuatEuler(...)
RotateBone(animator, "spine", QuatEuler(0, 5, 0), 1.0)
```

---

### [API] GetBoneNames

```lua
list = GetBoneNames(handle)
```

**Arguments:**

- `handle` *(number)* — Animator handle

**Example:**

```lua
local list = GetBoneNames(animator)
for i=1, #list do
	local name = list[i]
	..
end
```

---

### [API] GetBoneBody

```lua
handle = GetBoneBody(handle, name)
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Bone name

**Example:**

```lua
local body = GetBoneBody(animator, "head")
end
```

---

### [API] GetBoneWorldTransform

```lua
transform = GetBoneWorldTransform(handle, name)
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Bone name

**Example:**

```lua
    local animator = GetPlayerAnimator()
    local bones = GetBoneNames(animator)
    for i=1, #bones do
        local bone = bones[i]
        local t = GetBoneWorldTransform(animator,bone)
        DebugCross(t.pos)
    end
```

---

### [API] GetBoneBindPoseTransform

```lua
transform = GetBoneBindPoseTransform(handle, name)
```

**Arguments:**

- `handle` *(number)* — Animator handle

- `name` *(string)* — Bone name

**Example:**

```lua
local lt = getBindPoseTransform(animator, "lefthand")
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | **Animation** | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)