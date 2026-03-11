# Animation

An animator manages a prefab hierarchy using a matching skeleton and a set of animation sequences. These animations are processed sequentially, generating a "blend-tree."

There are two types of animations: looping and single-shot. Looping animations must be called every frame to keep them active; otherwise, they will stop. In contrast, single-shot animations are triggered once and will play to completion.

Single-shot animations are automatically processed after all looping animations, but they can be executed earlier if necessary. To ensure that single-shot animations are processed in the correct order within the blend-tree, an instance API is available.

### [API] SetAnimatorPositionIK(handle, begname, endname, target, [weight], [history], [flag])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `begname` *(string)* — Name of the start-bone of the chain
  - `endname` *(string)* — Name of the end-bone of the chain
  - `target` *(TVec)* — World target position that the "endname" bone should reach
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `history` *(number, optional)* — How much of the previous frames result [0,1] that should be used when start the IK search, default is 0.0
  - `flag` *(boolean, optional)* — TRUE if constraints should be used, default is TRUE
```lua
SetAnimatorPositionIK(animator, "shoulder_l", "hand_l", Vec(10, 0, 0), 1.0, 0.9, true)
```

### [API] SetAnimatorTransformIK(handle, begname, endname, transform, [weight], [history], [locktarget], [useconstraints])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `begname` *(string)* — Name of the start-bone of the chain
  - `endname` *(string)* — Name of the end-bone of the chain
  - `transform` *(TTransform)* — World target transform that the "endname" bone should reach
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `history` *(number, optional)* — How much of the previous frames result [0,1] that should be used when start the IK search, default is 0.0
  - `locktarget` *(boolean, optional)* — TRUE if the end-bone should be fixed to the target-transform, FALSE if IK solution is used
  - `useconstraints` *(boolean, optional)* — TRUE if constraints should be used, default is TRUE
```lua
SetAnimatorTransformIK(animator, "shoulder_l", "hand_l", Transform(10, 0, 0), 1.0, 0.9, false, true)
```

### [API] length = GetBoneChainLength(handle, begname, endname)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `begname` *(string)* — Name of the start-bone of the chain
  - `endname` *(string)* — Name of the end-bone of the chain
- **Returns:**
  - `length` *(number)* — Length of the bone chain between "start-bone" and "end-bone"
```lua
local length = GetBoneChainLength(animator, "shoulder_l", "hand_l")
```

### [API] handle = FindAnimator([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `handle` *(number)* — Handle to first animator with specified tag or zero if not found
```lua
--Search for the first animator in script scope
local animator = FindAnimator()

--Search for an animator tagged "anim" in script scope
local animator = FindAnimator("anim")

--Search for an animator tagged "anim2" in entire scene
local anim2 = FindAnimator("anim2", true)
```

### [API] list = FindAnimators([tag], [global])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all animators with specified tag
```lua
--Search for animators tagged "target" in script scope
local targets = FindAnimators("target")
for i=1, #targets do
	local target = targets[i]
	...
end
```

### [API] transform = GetAnimatorTransform(handle)
- **Args:**
  - `handle` *(number)* — Animator handle
- **Returns:**
  - `transform` *(TTransform)* — World space transform of the animator
```lua
local pos = GetAnimatorTransform(animator).pos
```

### [API] transform = GetAnimatorAdjustTransformIK(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Name of the location node
- **Returns:**
  - `transform` *(TTransform)* — World space transform of the animator
```lua
--This will adjust the target transform so that the grip defined by a location node in editor called "ik_hand_l" will reach the target
local target = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
local adj = GetAnimatorAdjustTransformIK(animator, "ik_hand_l")
if adj ~= nil then
    target = TransformToParentTransform(target, adj)
end
SetAnimatorTransformIK(animator, "shoulder_l", "hand_l", target, 1.0, 0.9)
```

### [API] SetAnimatorTransform(handle, transform)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `transform` *(TTransform)* — Desired transform
```lua
local t = Transform(Vec(10, 0, 0), QuatEuler(0, 90, 0))
SetAnimatorTransform(animator, t)
```

### [API] MakeRagdoll(handle)
- **Args:**
  - `handle` *(number)* — Animator handle
```lua
MakeRagdoll(animator)
```

### [API] UnRagdoll(handle, [time])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `time` *(number, optional)* — Transition time
```lua
--Take control of bodies and do a blend during one sec between the animation state and last physics state
UnRagdoll(animator, 1.0)
```

### [API] handle = PlayAnimation(handle, name, [weight], [filter])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `filter` *(string, optional)* — Name of the bone and its subtree that will be affected
- **Returns:**
  - `handle` *(number)* — Handle to the instance that can be used with PlayAnimationInstance, zero if clip reached its end
```lua
--This will play a single animation "Shooting" with a 80% influence but only on the skeleton starting at bone "Spine"
PlayAnimation(animator, "Shooting", 0.8, "Spine")
```

### [API] PlayAnimationLoop(handle, name, [weight], [filter])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `filter` *(string, optional)* — Name of the bone and its subtree that will be affected
```lua
--This will play an animation loop "Walking" with a 100% influence on the whole skeleton
PlayAnimationLoop(animator, "Walking")
```

### [API] handle = PlayAnimationInstance(handle, instance, [weight], [speed])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `instance` *(number)* — Instance handle
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `speed` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
- **Returns:**
  - `handle` *(number)* — Handle to the instance that can be used with PlayAnimationInstance, zero if clip reached its end
```lua
--This will play a single animation "Shooting" with a 80% influence but only on the skeleton starting at bone "Spine"
PlayAnimation(animator, "Shooting", 0.8, "Spine")
```

### [API] StopAnimationInstance(handle, instance)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `instance` *(number)* — Instance handle

### [API] PlayAnimationFrame(handle, name, time, [weight], [filter])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `time` *(number)* — Time in the animation
  - `weight` *(number, optional)* — Weight [0,1] of this animation, default is 1.0
  - `filter` *(string, optional)* — Name of the bone and its subtree that will be affected
```lua
--This will play an animation "Walking" at a specific time of 1.5s with a 80% influence on the whole skeleton
PlayAnimationFrame(animator, "Walking", 1.5, 0.8)
```

### [API] BeginAnimationGroup(handle, [weight], [filter])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `weight` *(number, optional)* — Weight [0,1] of this group, default is 1.0
  - `filter` *(string, optional)* — Name of the bone and its subtree that will be affected
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

### [API] EndAnimationGroup(handle)
- **Args:**
  - `handle` *(number)* — Animator handle

### [API] PlayAnimationInstances(handle)
- **Args:**
  - `handle` *(number)* — Animator handle
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

### [API] list = GetAnimationClipNames(handle)
- **Args:**
  - `handle` *(number)* — Animator handle
- **Returns:**
  - `list` *(table)* — Indexed table with animation names
```lua
local list = GetAnimationClipNames(animator)
for i=1, #list do
	local name = list[i]
	..
end
```

### [API] time = GetAnimationClipDuration(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
- **Returns:**
  - `time` *(number)* — Total duration of the animation

### [API] SetAnimationClipFade(handle, name, fadein, fadeout)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `fadein` *(number)* — Fadein time of the animation
  - `fadeout` *(number)* — Fadeout time of the animation
```lua
SetAnimationClipFade(animator, "fire", 0.5, 0.5)
```

### [API] SetAnimationClipSpeed(handle, name, speed)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `speed` *(number)* — Sets the speed factor of the animation
```lua
--This will make the clip run 2x as normal speed
SetAnimationClipSpeed(animator, "walking", 2)
```

### [API] TrimAnimationClip(handle, name, begoffset, [endoffset])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `begoffset` *(number)* — Time offset from the beginning of the animation
  - `endoffset` *(number, optional)* — Time offset, positive value means from the beginning and negative value means from the end, zero(default) means at end
```lua
--This will "remove" 1s from the beginning and 2s from the end.
TrimAnimationClip(animator, "walking", 1, -2)
```

### [API] time = GetAnimationClipLoopPosition(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
- **Returns:**
  - `time` *(number)* — Time of the current playposition in the animation

### [API] time = GetAnimationInstancePosition(handle, instance)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `instance` *(number)* — Instance handle
- **Returns:**
  - `time` *(number)* — Time of the current playposition in the animation

### [API] SetAnimationClipLoopPosition(handle, name, time)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Animation name
  - `time` *(number)* — Time in the animation
```lua
--This will set the current playposition to one second
SetAnimationClipLoopPosition(animator, "walking", 1)
```

### [API] SetBoneRotation(handle, name, quat, [weight])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
  - `quat` *(TQuat)* — Orientation of the bone
  - `weight` *(number, optional)* — Weight [0,1] default is 1.0
```lua
--This will set the existing rotation by QuatEuler(...)
SetBoneRotation(animator, "spine", QuatEuler(0, 180, 0), 1.0)
```

### [API] SetBoneLookAt(handle, name, point, [weight])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
  - `point` *(table)* — World space point as vector
  - `weight` *(number, optional)* — Weight [0,1] default is 1.0
```lua
--This will set the existing local-rotation to point to world space point
SetBoneLookAt(animator, "upper_arm_l", Vec(10, 20, 30), 1.0)
```

### [API] RotateBone(handle, name, quat, [weight])
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
  - `quat` *(TQuat)* — Additive orientation
  - `weight` *(number, optional)* — Weight [0,1] default is 1.0
```lua
--This will offset the existing rotation by QuatEuler(...)
RotateBone(animator, "spine", QuatEuler(0, 5, 0), 1.0)
```

### [API] list = GetBoneNames(handle)
- **Args:**
  - `handle` *(number)* — Animator handle
- **Returns:**
  - `list` *(table)* — Indexed table with bone-names
```lua
local list = GetBoneNames(animator)
for i=1, #list do
	local name = list[i]
	..
end
```

### [API] handle = GetBoneBody(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
- **Returns:**
  - `handle` *(number)* — Handle to the bone's body, or zero if no bone is present.
```lua
local body = GetBoneBody(animator, "head")
end
```

### [API] transform = GetBoneWorldTransform(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
- **Returns:**
  - `transform` *(TTransform)* — World space transform of the bone
```lua
    local animator = GetPlayerAnimator()
    local bones = GetBoneNames(animator)
    for i=1, #bones do
        local bone = bones[i]
        local t = GetBoneWorldTransform(animator,bone)
        DebugCross(t.pos)
    end
```

### [API] transform = GetBoneBindPoseTransform(handle, name)
- **Args:**
  - `handle` *(number)* — Animator handle
  - `name` *(string)* — Bone name
- **Returns:**
  - `transform` *(TTransform)* — Local space transform of the bone in bindpose
```lua
local lt = getBindPoseTransform(animator, "lefthand")
```

---
**Navigation:** [_INDEX](_INDEX.md)