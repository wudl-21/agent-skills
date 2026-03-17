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
--Retrieve blinkcount parameter, or set to 5 if omitted
parameterBlinkCount = GetIntParam("blinkcount", 5)
function init()
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
--Retrieve speed parameter, or set to 10.0 if omitted
parameterSpeed = GetFloatParam("speed", 10.0)
function init()
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
--Retrieve playsound parameter, or false if omitted
parameterPlaySound = GetBoolParam("playsound", false)
function init()
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
--Retrieve mode parameter, or "idle" if omitted
parameterMode = GetStringParam("mode", "idle")
function init()
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
--Retrieve color parameter, or set to 0.39, 0.39, 0.39 if omitted
color_r, color_g, color_b = GetColorParam("color", 0.39, 0.39, 0.39)
function init()
	DebugPrint(color_r .. " " .. color_g .. " " .. color_b)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)