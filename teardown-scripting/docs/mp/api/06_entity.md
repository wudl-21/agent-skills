# Entity

An Entity is the basis of most objects in the Teardown engine (bodies, shapes, lights, locations, etc).
All entities can have tags, which is a way to store custom properties on entities for scripting purposes.
Some tags are also reserved for engine use. See documentation for details.

### [API] handle = FindEntity([tag], [global], [type])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
  - `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)
- **Returns:**
  - `handle` *(number)* — Handle to first entity with specified tag or zero if not found
```lua
function client.tick()
	--You may use this function in a similar way to other "Find functions" like FindBody, FindShape, FindVehicle, etc.
	local myCar = FindEntity("myCar", false, "vehicle")

	--If you do not specify the tag, the first element found will be returned
	local joint = FindEntity("", true, "joint")

	--If the type is not specified, the search will be performed for all types of entity
	local target = FindEntity("target", true)
end
```

### [API] list = FindEntities([tag], [global], [type])
- **Args:**
  - `tag` *(string, optional)* — Tag name
  - `global` *(boolean, optional)* — Search in entire scene
  - `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)
- **Returns:**
  - `list` *(table)* — Indexed table with handles to all entities with specified tag
```lua
function client.tick()
	-- You may use this function in a similar way to other "Find functions" like FindBody, FindShape, FindVehicle, etc.
	local cars = FindEntities("car", false, "vehicle")

	-- You can get all the entities of the specified type by passing an empty string to the tag
	local allJoints = FindEntities("", true, "joint")

	-- If the type is not specified, the search will be performed for all types
	local allUnbreakables = FindEntities("unbreakable", true)
end
```

### [API] list = GetEntityChildren(handle, [tag], [recursive], [type])
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string, optional)* — Tag name
  - `recursive` *(boolean, optional)* — Search recursively
  - `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)
- **Returns:**
  - `list` *(table)* — Indexed table with child elements of the entity
```lua
function client.tick()
	local car = FindEntity("car", true, "vehicle")
	DebugWatch("car", car)

	local children = GetEntityChildren(entity, "", true, "wheel")
	for i = 1, #children do
		DebugWatch("wheel " .. tostring(i), children[i])
	end
end
```

### [API] handle = GetEntityParent(handle, [tag], [type])
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string, optional)* — Tag name
  - `type` *(string, optional)* — Entity type ("body", "shape", "light", "location" etc.)
- **Returns:**
  - `handle` *(number)* — 
```lua
function client.tick()
	local wheel = FindEntity("", true, "wheel")
	local vehicle = GetEntityParent(wheel,  "", "vehicle")
	DebugWatch("Wheel vehicle", GetEntityType(vehicle) .. " " .. tostring(vehicle))
end
```

### [API] SetTag(handle, tag, [value])
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string)* — Tag name
  - `value` *(string, optional)* — Tag value
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

### [API] RemoveTag(handle, tag)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string)* — Tag name
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

### [API] exists = HasTag(handle, tag)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string)* — Tag name
- **Returns:**
  - `exists` *(boolean)* — Returns true if entity has tag
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

### [API] value = GetTagValue(handle, tag)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `tag` *(string)* — Tag name
- **Returns:**
  - `value` *(string)* — Returns the tag value, if any. Empty string otherwise.
```lua
function init()
	local handle = FindBody("body", true)

	--Add "team" tag to an entity and give it value "red"
	SetTag(handle, "team", "red")
	DebugPrint(GetTagValue(handle, "team"))
end
```

### [API] tags = ListTags(handle)
- **Args:**
  - `handle` *(number)* — Entity handle
- **Returns:**
  - `tags` *(table)* — Indexed table of tags on entity
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

### [API] description = GetDescription(handle)
- **Args:**
  - `handle` *(number)* — Entity handle
- **Returns:**
  - `description` *(string)* — The description string
```lua
function init()
	local body = FindBody("body", true)
	DebugPrint(GetDescription(body))
end
```

### [API] SetDescription(handle, description)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `description` *(string)* — The description string
```lua
function init()
	local body = FindBody("body", true)
	SetDescription(body, "Target object")
	DebugPrint(GetDescription(body))
end
```

### [API] Delete(handle)
- **Args:**
  - `handle` *(number)* — Entity handle
```lua
function init()
	local body = FindBody("body", true)
	--All shapes associated with body will also be removed
	Delete(body)
end
```

### [API] exists = IsHandleValid(handle)
- **Args:**
  - `handle` *(number)* — Entity handle
- **Returns:**
  - `exists` *(boolean)* — Returns true if the entity pointed to by handle still exists
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

### [API] type = GetEntityType(handle)
- **Args:**
  - `handle` *(number)* — Entity handle
- **Returns:**
  - `type` *(string)* — Type name of the provided entity
```lua
function init()
	local body = FindBody("body", true)
	DebugPrint(GetEntityType(body))
end
```

### [API] value = GetProperty(handle, property)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `property` *(string)* — Property name
- **Returns:**
  - `value` *(any)* — Property value
| Entity type | Available params |
| --- | --- |
| Body | desc (string), dynamic (boolean), mass (number), transform, velocity (vector(x, y, z)), angVelocity (vector(x, y, z)), active (boolean), friction (number), restitution (number), frictionMode (average\|minimum\|multiply\|maximum), restitutionMode (average\|minimum\|multiply\|maximum) |
| Shape | density (number), strength (number), size (number), emissiveScale (number), localTransform, worldTransform |
| Light | enabled (boolean), color (vector(r, g, b)), intensity (number), transform, active (boolean), type (string), size (number), reach (number), unshadowed (number), fogscale (number), fogiter (number), glare (number) |
| Location | transform |
| Water | depth (number), wave (number), ripple (number), motion (number), foam (number), color (vector(r, g, b)) |
| Joint | type (string), size (number), rotstrength (number), rotspring (number); only for ropes: slack (number), strength (number), maxstretch (number), ropecolor (vector(r, g, b)) |
| Vehicle | spring (number), damping (number), topspeed (number), acceleration (number), strength (number), antispin (number), antiroll (number), difflock (number), steerassist (number), friction (number), smokeintensity (number), transform, brokenthreshold (number) |
| Wheel | drive (number), steer (number), travel (vector(x, y)) |
| Screen | enabled (boolean), bulge (number), resolution (number, number), script (string), interactive (boolean), emissive (number), fxraster (number), fxca (number), fxnoise (number), fxglitch (number), size (vector(x, y)) |
| Trigger | transform, type (string), size (vector(x, y, z)/number) |
```lua
function client.tick()
	local body = FindBody("testbody", true)
	local isDynamic = GetProperty(body, "dynamic")
	DebugWatch("isDynamic", isDynamic)
end
```

### [API] SetProperty(handle, property, value)
- **Args:**
  - `handle` *(number)* — Entity handle
  - `property` *(string)* — Property name
  - `value` *(any)* — Property value
| Entity type | Available params |
| --- | --- |
| Body | desc (string), dynamic (boolean), transform, velocity (vector(x, y, z)), angVelocity (vector(x,y,z)), active (boolean), friction (number), restitution (number), frictionMode (average\|minimum\|multiply\|maximum), restitutionMode (average\|minimum\|multiply\|maximum) |
| Shape | density (number), strength (number), emissiveScale (number), localTransform |
| Light | enabled (boolean), color (vector(r, g, b)), intensity (number), transform, size (number/vector(x,y)), reach (number), unshadowed (number), fogscale (number), fogiter (number), glare (number) |
| Location | transform |
| Water | type (string), depth (number), wave (number), ripple (number), motion (number), foam (number), color (vector(r, g, b)) |
| Joint | size (number), rotstrength (number), rotspring (number); only for ropes: slack (number), strength (number), maxstretch (number), ropecolor (vector(r, g, b)) |
| Vehicle | spring (number), damping (number), topspeed (number), acceleration (number), strength (number), antispin (number), antiroll (number), difflock (number), steerassist (number), friction (number), smokeintensity (number), transform, brokenthreshold (number) |
| Wheel | drive (number), steer (number), travel (vector(x, y)) |
| Screen | enabled (boolean), interactive (boolean), emissive (number), fxraster (number), fxca (number), fxnoise (number), fxglitch (number) |
| Trigger | transform, size (vector(x, y, z)/number) |
```lua
function tick()
	local light = FindLight("mylight", true)
	SetProperty(light, "intensity", math.abs(math.sin(GetTime())))
end
```

---
**Navigation:** [_INDEX](_INDEX.md)