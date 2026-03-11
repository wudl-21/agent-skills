# Teardown SP API — Sound
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Sound functions are used for playing sounds or loops in the world. There sound functions are alwyas positioned and will be affected by acoustics simulation. If you want to play dry sounds without acoustics you should use UiSound and UiSoundLoop in the User Interface section.

---

### [API] LoadSound

```lua
handle = LoadSound(path, [nominalDistance])
```

**Arguments:**

- `path` *(string)* — Path to ogg sound file

- `nominalDistance` *(number, optional)* — The distance in meters this sound is recorded at. Affects attenuation, default is 10.0

**Example:**

```lua
function init()
	local snd = LoadSound("warning-beep.ogg")
end
```

---

### [API] UnloadSound

```lua
UnloadSound(handle)
```

**Arguments:**

- `handle` *(number)* — Sound handle

**Example:**

```lua
function init()
	local snd = LoadSound("warning-beep.ogg")
	UnloadSound(snd)
end
```

---

### [API] LoadLoop

```lua
handle = LoadLoop(path, [nominalDistance])
```

**Arguments:**

- `path` *(string)* — Path to ogg sound file

- `nominalDistance` *(number, optional)* — The distance in meters this sound is recorded at. Affects attenuation, default is 10.0

**Example:**

```lua
local loop
function init()
	loop = LoadLoop("radio/jazz.ogg")
end

function tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
end
```

---

### [API] UnloadLoop

```lua
UnloadLoop(handle)
```

**Arguments:**

- `handle` *(number)* — Loop handle

**Example:**

```lua
local loop = -1
function init()
	loop = LoadLoop("radio/jazz.ogg")
end

function tick()
	if loop ~= -1 then
		local pos = Vec(0, 0, 0)
		PlayLoop(loop, pos, 1.0)
	end
		
	if InputPressed("space") then
		UnloadLoop(loop)
		loop = -1
	end
end
```

---

### [API] SetSoundLoopUser

```lua
flag = SetSoundLoopUser(handle, nominalDistance)
```

**Arguments:**

- `handle` *(number)* — Loop handle

- `nominalDistance` *(number)* — User index

**Example:**

```lua
function init()
	local loop = LoadLoop("radio/jazz.ogg")
	SetSoundLoopUser(loop, 0)
end
--This function will move (if possible) sound to gamepad of appropriate user
```

---

### [API] PlaySound

```lua
handle = PlaySound(handle, [pos], [volume], [registerVolume], [pitch])
```

**Arguments:**

- `handle` *(number)* — Sound handle

- `pos` *(TVec, optional)* — World position as vector. Default is player position.

- `volume` *(number, optional)* — Playback volume. Default is 1.0

- `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true

- `pitch` *(number, optional)* — Playback pitch. Default 1.0

**Example:**

```lua
local snd
function init()
	snd = LoadSound("warning-beep.ogg")
end

function tick()
	if InputPressed("interact") then
		local pos = Vec(0, 0, 0)
		PlaySound(snd, pos, 0.5)
	end
end

-- If you have a list of sound files and you add a sequence number, starting from zero, at the end of each filename like below,
-- then each time you call PlaySound it will pick a random sound from that list and play that sound.

-- "example-sound0.ogg"
-- "example-sound1.ogg"
-- "example-sound2.ogg"
-- "example-sound3.ogg"
-- ...
--[[
	local snd
	function init()
		snd = LoadSound("example-sound0.ogg")
	end

	-- Plays a random sound from the loaded sound series
	function tick()
		if trigSound then
			local pos = Vec(100, 0, 0)
			PlaySound(snd, pos, 0.5)
		end
	end
]]
```

---

### [API] PlaySoundForUser

```lua
handle = PlaySoundForUser(handle, user, [pos], [volume], [registerVolume], [pitch])
```

**Arguments:**

- `handle` *(number)* — Sound handle

- `user` *(number)* — Index of user to play.

- `pos` *(TVec, optional)* — World position as vector. Default is player position.

- `volume` *(number, optional)* — Playback volume. Default is 1.0

- `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true

- `pitch` *(number, optional)* — Playback pitch. Default 1.0

**Example:**

```lua
local snd
function init()
	snd = LoadSound("warning-beep.ogg")
end

function tick()
	if InputPressed("interact") then
		PlaySoundForUser(snd, 0)
	end
end

-- If you have a list of sound files and you add a sequence number, starting from zero, at the end of each filename like below,
-- then each time you call PlaySoundForUser it will pick a random sound from that list and play that sound.

-- "example-sound0.ogg"
-- "example-sound1.ogg"
-- "example-sound2.ogg"
-- "example-sound3.ogg"
-- ...

--[[
	local snd
	function init()
		snd = LoadSound("example-sound0.ogg")
	end

	-- Plays a random sound from the loaded sound series
	function tick()
		if trigSound then
			local pos = Vec(100, 0, 0)
			PlaySoundForUser(snd, 0, pos, 0.5)
		end
	end
]]
```

---

### [API] StopSound

```lua
StopSound(handle)
```

**Arguments:**

- `handle` *(number)* — Sound play handle

**Example:**

```lua
local snd
function init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function tick()
	if InputPressed("interact") then
		if not IsSoundPlaying(sndPlay) then
			local pos = Vec(0, 0, 0)
			sndPlay = PlaySound(snd, pos, 0.5)
		else
			StopSound(sndPlay)
		end
	end
end
```

---

### [API] IsSoundPlaying

```lua
playing = IsSoundPlaying(handle)
```

**Arguments:**

- `handle` *(number)* — Sound play handle

**Example:**

```lua
local snd
function init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function tick()
	if InputPressed("interact") then
		if not IsSoundPlaying(sndPlay) then
			local pos = Vec(0, 0, 0)
			sndPlay = PlaySound(snd, pos, 0.5)
		else
			StopSound(sndPlay)
		end
	end
end
```

---

### [API] GetSoundProgress

```lua
progress = GetSoundProgress(handle)
```

**Arguments:**

- `handle` *(number)* — Sound play handle

**Example:**

```lua
local snd
function init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function tick()
	if InputPressed("interact") then
		if not IsSoundPlaying(sndPlay) then
			local pos = Vec(0, 0, 0)
			sndPlay = PlaySound(snd, pos, 0.5)
		else
			SetSoundProgress(sndPlay, GetSoundProgress(sndPlay) - 1.0)
		end
	end
end
```

---

### [API] SetSoundProgress

```lua
SetSoundProgress(handle, progress)
```

**Arguments:**

- `handle` *(number)* — Sound play handle

- `progress` *(number)* — Progress in seconds

**Example:**

```lua
local snd
function init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function tick()
	if InputPressed("interact") then
		if not IsSoundPlaying(sndPlay) then
			local pos = Vec(0, 0, 0)
			sndPlay = PlaySound(snd, pos, 0.5)
		else
			SetSoundProgress(sndPlay, GetSoundProgress(sndPlay) - 1.0)
		end
	end
end
```

---

### [API] PlayLoop

```lua
PlayLoop(handle, [pos], [volume], [registerVolume], [pitch])
```

Call this function continuously to play loop

**Arguments:**

- `handle` *(number)* — Loop handle

- `pos` *(TVec, optional)* — World position as vector. Default is player position.

- `volume` *(number, optional)* — Playback volume. Default is 1.0

- `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true

- `pitch` *(number, optional)* — Playback pitch. Default 1.0

**Example:**

```lua
local loop
function init()
	loop = LoadLoop("radio/jazz.ogg")
end

function tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
end
```

---

### [API] GetSoundLoopProgress

```lua
progress = GetSoundLoopProgress(handle)
```

**Arguments:**

- `handle` *(number)* — Loop handle

**Example:**

```lua
function init()
	loop = LoadLoop("radio/jazz.ogg")
end

function tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
	if InputPressed("interact") then
		SetSoundLoopProgress(loop, GetSoundLoopProgress(loop) - 1.0)
	end
end
```

---

### [API] SetSoundLoopProgress

```lua
SetSoundLoopProgress(handle, [progress])
```

**Arguments:**

- `handle` *(number)* — Loop handle

- `progress` *(number, optional)* — Progress in seconds. Default 0.0.

**Example:**

```lua
function init()
	loop = LoadLoop("radio/jazz.ogg")
end

function tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
	if InputPressed("interact") then
		SetSoundLoopProgress(loop, GetSoundLoopProgress(loop) - 1.0)
	end
end
```

---

### [API] PlayMusic

```lua
PlayMusic(path)
```

**Arguments:**

- `path` *(string)* — Music path

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end
```

---

### [API] StopMusic

```lua
StopMusic()
```

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputDown("interact") then
		StopMusic()
	end
end
```

---

### [API] IsMusicPlaying

```lua
playing = IsMusicPlaying()
```

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputPressed("interact") and IsMusicPlaying() then
		DebugPrint("music is playing")
	end
end
```

---

### [API] SetMusicPaused

```lua
SetMusicPaused(paused)
```

**Arguments:**

- `paused` *(boolean)* — True to pause, false to resume.

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputPressed("interact") then
		SetMusicPaused(IsMusicPlaying())
	end
end
```

---

### [API] GetMusicProgress

```lua
progress = GetMusicProgress()
```

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputPressed("interact") then
		DebugPrint(GetMusicProgress())
	end
end
```

---

### [API] SetMusicProgress

```lua
SetMusicProgress([progress])
```

**Arguments:**

- `progress` *(number, optional)* — Progress in seconds. Default 0.0.

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputPressed("interact") then
 		SetMusicProgress(GetMusicProgress() - 1.0)
	end
end
```

---

### [API] SetMusicVolume

```lua
SetMusicVolume(volume)
```

Override current music volume for this frame. Call continuously to keep overriding.

**Arguments:**

- `volume` *(number)* — Music volume.

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputDown("interact") then
 		SetMusicVolume(0.3)
	end
end
```

---

### [API] SetMusicLowPass

```lua
SetMusicLowPass(wet)
```

Override current music low pass filter for this frame. Call continuously to keep overriding.

**Arguments:**

- `wet` *(number)* — Music low pass filter 0.0 - 1.0.

**Example:**

```lua
function init()
	PlayMusic("about.ogg")
end

function tick()
	if InputDown("interact") then
 		SetMusicLowPass(0.6)
	end
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | **Sound** | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)