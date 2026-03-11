# Parameters

Scripts can have parameters defined in the level XML file. These serve as
input to a specific instance of the script and can be used to configure
various options and parameters of the script. While these parameters can
be read at any time in the script, it's recommended to copy them to a global
variable in or outside the init function.

### [API] value = GetIntParam(name, default)
- **Args:**
  - `name` *(string)* — Parameter name
  - `default` *(number)* — Default parameter value
- **Returns:**
  - `value` *(number)* — Parameter value
```lua
function init()
	--Retrieve blinkcount parameter, or set to 5 if omitted
	local parameterBlinkCount = GetIntParam("blinkcount", 5)
	DebugPrint(parameterBlinkCount)
end
```

### [API] value = GetFloatParam(name, default)
- **Args:**
  - `name` *(string)* — Parameter name
  - `default` *(number)* — Default parameter value
- **Returns:**
  - `value` *(number)* — Parameter value
```lua
function init()
	--Retrieve speed parameter, or set to 10.0 if omitted
	local parameterSpeed = GetFloatParam("speed", 10.0)
	DebugPrint(parameterSpeed)
end
```

### [API] value = GetBoolParam(name, default)
- **Args:**
  - `name` *(string)* — Parameter name
  - `default` *(boolean)* — Default parameter value
- **Returns:**
  - `value` *(boolean)* — Parameter value
```lua
function init()
	--Retrieve playsound parameter, or false if omitted
	local parameterPlaySound = GetBoolParam("playsound", false)
	DebugPrint(parameterPlaySound)
end
```

### [API] value = GetStringParam(name, default)
- **Args:**
  - `name` *(string)* — Parameter name
  - `default` *(string)* — Default parameter value
- **Returns:**
  - `value` *(string)* — Parameter value
```lua
function init()
	--Retrieve mode parameter, or "idle" if omitted
	local parameterMode = GetStringParam("mode", "idle")
	DebugPrint(parameterMode)
end
```

### [API] value = GetColorParam(name, default)
- **Args:**
  - `name` *(string)* — Parameter name
  - `default` *(number)* — Default parameter value
- **Returns:**
  - `value` *(number)* — Parameter value
```lua
function init()
	--Retrieve color parameter, or set to 0.39, 0.39, 0.39 if omitted
	local color_r, color_g, color_b = GetColorParam("color", 0.39, 0.39, 0.39)
	DebugPrint(color_r .. " " .. color_g .. " " .. color_b)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)