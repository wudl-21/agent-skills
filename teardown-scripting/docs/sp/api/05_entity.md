# Teardown SP API — Entity
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

An Entity is the basis of most objects in the Teardown engine (bodies, shapes, lights, locations, etc). All entities can have tags, which is a way to store custom properties on entities for scripting purposes. Some tags are also reserved for engine use. See documentation for details.

---

### [API] FindEntity

```lua
handle = FindEntity([tag], [global], [type])
```

Returns an entity with the specified tag and type. This is a universal method that is an alternative to FindBody, FindShape, FindVehicle, etc.

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

- `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)

**Example:**

```lua
function tick()
	--You may use this function in a similar way to other "Find functions" like FindBody, FindShape, FindVehicle, etc.
	local myCar = FindEntity("myCar", false, "vehicle")

	--If you do not specify the tag, the first element found will be returned
	local joint = FindEntity("", true, "joint")

	--If the type is not specified, the search will be performed for all types of entity
	local target = FindEntity("target", true)
end
```

---

### [API] FindEntities

```lua
list = FindEntities([tag], [global], [type])
```

Returns a list of entities with the specified tag and type. This is a universal method that is an alternative to FindBody, FindShape, FindVehicle, etc.

**Arguments:**

- `tag` *(string, optional)* — Tag name

- `global` *(boolean, optional)* — Search in entire scene

- `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)

**Example:**

```lua
function tick()
	-- You may use this function in a similar way to other "Find functions" like FindBody, FindShape, FindVehicle, etc.
	local cars = FindEntities("car", false, "vehicle")

	-- You can get all the entities of the specified type by passing an empty string to the tag
	local allJoints = FindEntities("", true, "joint")

	-- If the type is not specified, the search will be performed for all types
	local allUnbreakables = FindEntities("unbreakable", true)
end
```

---

### [API] GetEntityChildren

```lua
list = GetEntityChildren(handle, [tag], [recursive], [type])
```

Returns child entities

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string, optional)* — Tag name

- `recursive` *(boolean, optional)* — Search recursively

- `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)

**Example:**

```lua
function tick()
	local car = FindEntity("car", true, "vehicle")
	DebugWatch("car", car)

	local children = GetEntityChildren(entity, "", true, "wheel")
	for i = 1, #children do
		DebugWatch("wheel " .. tostring(i), children[i])
	end
end
```

---

### [API] GetEntityParent

```lua
handle = GetEntityParent(handle, [tag], [type])
```

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string, optional)* — Tag name

- `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)

**Example:**

```lua
function tick()
	local wheel = FindEntity("", true, "wheel")
	local vehicle = GetEntityParent(wheel,  "", "vehicle")
	DebugWatch("Wheel vehicle", GetEntityType(vehicle) .. " " .. tostring(vehicle))
end
```

---

### [API] SetTag

```lua
SetTag(handle, tag, [value])
```

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string)* — Tag name

- `value` *(string, optional)* — Tag value

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	--Add "special" tag to an entity
	SetTag(handle, "special")
	DebugPrint(HasTag(handle, "special"))

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	DebugPrint(HasTag(handle, "team"))
end
```

---

### [API] RemoveTag

```lua
RemoveTag(handle, tag)
```

Remove tag from an entity. If the tag had a value it is removed too.

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string)* — Tag name

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	--Add "special" tag to an entity
	SetTag(handle, "special")
	RemoveTag(handle, "special")
	DebugPrint(HasTag(handle, "special"))

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	DebugPrint(HasTag(handle, "team"))
end
```

---

### [API] HasTag

```lua
exists = HasTag(handle, tag)
```

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string)* — Tag name

**Example:**

```lua
function init()
	local handle = FindBody("body", true)
	--Add "special" tag to an entity
	SetTag(handle, "special")
	DebugPrint(HasTag(handle, "special"))

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	DebugPrint(HasTag(handle, "team"))
end
```

---

### [API] GetTagValue

```lua
value = GetTagValue(handle, tag)
```

**Arguments:**

- `handle` *(number)* — Entity handle

- `tag` *(string)* — Tag name

**Example:**

```lua
function init()
	local handle = FindBody("body", true)

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	DebugPrint(GetTagValue(handle, "team"))
end
```

---

### [API] ListTags

```lua
tags = ListTags(handle)
```

**Arguments:**

- `handle` *(number)* — Entity handle

**Example:**

```lua
function init()
	local handle = FindBody("body", true)

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	
	--List all tags and their tag values for a particular entity
	local tags = ListTags(handle)
	for i=1, #tags do
		DebugPrint(tags[i] .. " " .. GetTagValue(handle, tags[i]))
	end
end
```

---

### [API] GetDescription

```lua
description = GetDescription(handle)
```

All entities can have an associated description. For bodies and shapes this can be provided through the editor. This function retrieves that description.

**Arguments:**

- `handle` *(number)* — Entity handle

**Example:**

```lua
function init()
	local body = FindBody("body", true)
	DebugPrint(GetDescription(body))
end
```

---

### [API] SetDescription

```lua
SetDescription(handle, description)
```

All entities can have an associated description. The description for bodies and shapes will show up on the HUD when looking at them.

**Arguments:**

- `handle` *(number)* — Entity handle

- `description` *(string)* — The description string

**Example:**

```lua
function init()
	local body = FindBody("body", true)
	SetDescription(body, "Target object")
	DebugPrint(GetDescription(body))
end
```

---

### [API] Delete

```lua
Delete(handle)
```

Remove an entity from the scene. All entities owned by this entity will also be removed.

**Arguments:**

- `handle` *(number)* — Entity handle

**Example:**

```lua
function init()
	local body = FindBody("body", true)
	--All shapes associated with body will also be removed
	Delete(body)
end
```

---

### [API] IsHandleValid

```lua
exists = IsHandleValid(handle)
```

**Arguments:**

- `handle` *(number)* — Entity handle

**Example:**

```lua
function init()
	local body = FindBody("body", true)

	--valid is true if body still exists
	DebugPrint(IsHandleValid(body))
	Delete(body)

	--valid will now be false
	DebugPrint(IsHandleValid(body))
end
```

---

### [API] GetEntityType

```lua
type = GetEntityType(handle)
```

Returns the type name of provided entity, for example "body", "shape", "light", etc.

**Arguments:**

- `handle` *(number)* — Entity handle

**Example:**

```lua
function init()
	local body = FindBody("body", true)
	DebugPrint(GetEntityType(body))
end
```

---

### [API] GetProperty

```lua
value = GetProperty(handle, property)
```

Entity type Available params Body desc (string), dynamic (boolean), mass (number), transform, velocity (vector(x, y, z)), angVelocity (vector(x, y, z)), active (boolean), friction (number), restitution (number), frictionMode (average|minimum|multiply|maximum), restitutionMode (average|minimum|multiply|maximum)Shape density (number), strength (number), size (number), emissiveScale (number), localTransform, worldTransformLight enabled (boolean), color (vector(r, g, b)), intensity (number), transform, active (boolean), type (string), size (number), reach (number), unshadowed (number), fogscale (number), fogiter (number), glare (number)Location transformWater depth (number), wave (number), ripple (number), motion (number), foam (number), color (vector(r, g, b))Joint type (string), size (number), rotstrength (number), rotspring (number); only for ropes: slack (number), strength (number), maxstretch (number), ropecolor (vector(r, g, b))Vehicle spring (number), damping (number), topspeed (number), acceleration (number), strength (number), antispin (number), antiroll (number), difflock (number), steerassist (number), friction (number), smokeintensity (number), transform, brokenthreshold (number)Wheel drive (number), steer (number), travel (vector(x, y))Screen enabled (boolean), bulge (number), resolution (number, number), script (string), interactive (boolean), emissive (number), fxraster (number), fxca (number), fxnoise (number), fxglitch (number), size (vector(x, y))Trigger transform, type (string), size (vector(x, y, z)/number)

**Arguments:**

- `handle` *(number)* — Entity handle

- `property` *(string)* — Property name

**Example:**

```lua
function tick()
	local body = FindBody("testbody", true)
	local isDynamic = GetProperty(body, "dynamic")
	DebugWatch("isDynamic", isDynamic)
end
```

---

### [API] SetProperty

```lua
SetProperty(handle, property, value)
```

Entity type Available params Body desc (string), dynamic (boolean), transform, velocity (vector(x, y, z)), angVelocity (vector(x,y,z)), active (boolean), friction (number), restitution (number), frictionMode (average|minimum|multiply|maximum), restitutionMode (average|minimum|multiply|maximum)Shape density (number), strength (number), emissiveScale (number), localTransformLight enabled (boolean), color (vector(r, g, b)), intensity (number), transform, size (number/vector(x,y)), reach (number), unshadowed (number), fogscale (number), fogiter (number), glare (number)Location transformWater type (string), depth (number), wave (number), ripple (number), motion (number), foam (number), color (vector(r, g, b))Joint size (number), rotstrength (number), rotspring (number); only for ropes: slack (number), strength (number), maxstretch (number), ropecolor (vector(r, g, b))Vehicle spring (number), damping (number), topspeed (number), acceleration (number), strength (number), antispin (number), antiroll (number), difflock (number), steerassist (number), friction (number), smokeintensity (number), transform, brokenthreshold (number) Wheel drive (number), steer (number), travel (vector(x, y))Screen enabled (boolean), interactive (boolean), emissive (number), fxraster (number), fxca (number), fxnoise (number), fxglitch (number)Trigger transform, size (vector(x, y, z)/number)

**Arguments:**

- `handle` *(number)* — Entity handle

- `property` *(string)* — Property name

- `value` *(any)* — Property value

**Example:**

```lua
function tick()
	local light = FindLight("mylight", true)
	SetProperty(light, "intensity", math.abs(math.sin(GetTime())))
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | **Entity** | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)