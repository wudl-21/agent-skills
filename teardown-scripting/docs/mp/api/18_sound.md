# Sound

Sound functions are used for playing sounds or loops in the world. There sound functions are
always positioned and will be affected by acoustics simulation. If you want to play dry sounds
without acoustics you should use UiSound and UiSoundLoop in the User Interface section.

### [API] handle = LoadSound(path, [nominalDistance])
- **Args:**
  - `path` *(string)* — Path to ogg sound file
  - `nominalDistance` *(number, optional)* — The distance in meters this sound is recorded at. Affects attenuation, default is 10.0
- **Returns:**
  - `handle` *(number)* — Sound handle
```lua
function client.init()
	local snd = LoadSound("warning-beep.ogg")
end
```

### [API] UnloadSound(handle)
- **Args:**
  - `handle` *(number)* — Sound handle
```lua
function client.init()
	local snd = LoadSound("warning-beep.ogg")
	UnloadSound(snd)
end
```

### [API] handle = LoadLoop(path, [nominalDistance])
- **Args:**
  - `path` *(string)* — Path to ogg sound file
  - `nominalDistance` *(number, optional)* — The distance in meters this sound is recorded at. Affects attenuation, default is 10.0
- **Returns:**
  - `handle` *(number)* — Loop handle
```lua
local loop
function client.init()
	loop = LoadLoop("radio/jazz.ogg")
end

function client.tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
end
```

### [API] UnloadLoop(handle)
- **Args:**
  - `handle` *(number)* — Loop handle
```lua
local loop = -1
function client.init()
	loop = LoadLoop("radio/jazz.ogg")
end

function client.tick()
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

### [API] flag = SetSoundLoopUser(handle, nominalDistance)
- **Args:**
  - `handle` *(number)* — Loop handle
  - `nominalDistance` *(number)* — User index
- **Returns:**
  - `flag` *(boolean)* — TRUE if sound applied to gamepad speaker, FALSE otherwise.
```lua
function client.init()
	local loop = LoadLoop("radio/jazz.ogg")
	SetSoundLoopUser(loop, 0)
end
--This function will move (if possible) sound to gamepad of appropriate user
```

### [API] handle = PlaySound(handle, [pos], [volume], [registerVolume], [pitch])
- **Args:**
  - `handle` *(number)* — Sound handle
  - `pos` *(TVec, optional)* — World position as vector. Default is player position.
  - `volume` *(number, optional)* — Playback volume. Default is 1.0
  - `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true
  - `pitch` *(number, optional)* — Playback pitch. Default 1.0
- **Returns:**
  - `handle` *(number)* — Sound play handle
```lua
local snd
function client.init()
	snd = LoadSound("warning-beep.ogg")
end

function client.tick()
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
	function client.init()
		snd = LoadSound("example-sound0.ogg")
	end

	-- Plays a random sound from the loaded sound series
	function client.tick()
		if trigSound then
			local pos = Vec(100, 0, 0)
			PlaySound(snd, pos, 0.5)
		end
	end
]]
```

### [API] handle = PlaySoundForUser(handle, user, [pos], [volume], [registerVolume], [pitch])
- **Args:**
  - `handle` *(number)* — Sound handle
  - `user` *(number)* — Index of user to play.
  - `pos` *(TVec, optional)* — World position as vector. Default is player position.
  - `volume` *(number, optional)* — Playback volume. Default is 1.0
  - `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true
  - `pitch` *(number, optional)* — Playback pitch. Default 1.0
- **Returns:**
  - `handle` *(number)* — Sound play handle
```lua
local snd
function client.init()
	snd = LoadSound("warning-beep.ogg")
end

function client.tick()
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
	function client.init()
		snd = LoadSound("example-sound0.ogg")
	end

	-- Plays a random sound from the loaded sound series
	function client.tick()
		if trigSound then
			local pos = Vec(100, 0, 0)
			PlaySoundForUser(snd, 0, pos, 0.5)
		end
	end
]]
```

### [API] StopSound(handle)
- **Args:**
  - `handle` *(number)* — Sound play handle
```lua
local snd
function client.init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function client.tick()
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

### [API] playing = IsSoundPlaying(handle)
- **Args:**
  - `handle` *(number)* — Sound play handle
- **Returns:**
  - `playing` *(boolean)* — True if sound is playing, false otherwise.
```lua
local snd
function client.init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function client.tick()
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

### [API] progress = GetSoundProgress(handle)
- **Args:**
  - `handle` *(number)* — Sound play handle
- **Returns:**
  - `progress` *(number)* — Current sound progress in seconds.
```lua
local snd
function client.init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function client.tick()
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

### [API] SetSoundProgress(handle, progress)
- **Args:**
  - `handle` *(number)* — Sound play handle
  - `progress` *(number)* — Progress in seconds
```lua
local snd
function client.init()
	snd = LoadSound("radio/jazz.ogg")
end

local sndPlay
function client.tick()
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

### [API] PlayLoop(handle, [pos], [volume], [registerVolume], [pitch])
- **Args:**
  - `handle` *(number)* — Loop handle
  - `pos` *(TVec, optional)* — World position as vector. Default is player position.
  - `volume` *(number, optional)* — Playback volume. Default is 1.0
  - `registerVolume` *(boolean, optional)* — Register position and volume of this sound for GetLastSound. Default is true
  - `pitch` *(number, optional)* — Playback pitch. Default 1.0
```lua
local loop
function client.init()
	loop = LoadLoop("radio/jazz.ogg")
end

function client.tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
end
```

### [API] progress = GetSoundLoopProgress(handle)
- **Args:**
  - `handle` *(number)* — Loop handle
- **Returns:**
  - `progress` *(number)* — Current music progress in seconds.
```lua
function client.init()
	loop = LoadLoop("radio/jazz.ogg")
end

function client.tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
	if InputPressed("interact") then
		SetSoundLoopProgress(loop, GetSoundLoopProgress(loop) - 1.0)
	end
end
```

### [API] SetSoundLoopProgress(handle, [progress])
- **Args:**
  - `handle` *(number)* — Loop handle
  - `progress` *(number, optional)* — Progress in seconds. Default 0.0.
```lua
function client.init()
	loop = LoadLoop("radio/jazz.ogg")
end

function client.tick()
	local pos = Vec(0, 0, 0)
	PlayLoop(loop, pos, 1.0)
	if InputPressed("interact") then
		SetSoundLoopProgress(loop, GetSoundLoopProgress(loop) - 1.0)
	end
end
```

### [API] PlayMusic(path)
- **Args:**
  - `path` *(string)* — Music path
```lua
function client.init()
	PlayMusic("about.ogg")
end
```

### [API] StopMusic()
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputDown("interact") then
		StopMusic()
	end
end
```

### [API] playing = IsMusicPlaying()
- **Returns:**
  - `playing` *(boolean)* — True if music is playing, false otherwise.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputPressed("interact") and IsMusicPlaying() then
		DebugPrint("music is playing")
	end
end
```

### [API] SetMusicPaused(paused)
- **Args:**
  - `paused` *(boolean)* — True to pause, false to resume.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputPressed("interact") then
		SetMusicPaused(IsMusicPlaying())
	end
end
```

### [API] progress = GetMusicProgress()
- **Returns:**
  - `progress` *(number)* — Current music progress in seconds.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputPressed("interact") then
		DebugPrint(GetMusicProgress())
	end
end
```

### [API] SetMusicProgress([progress])
- **Args:**
  - `progress` *(number, optional)* — Progress in seconds. Default 0.0.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputPressed("interact") then
 		SetMusicProgress(GetMusicProgress() - 1.0)
	end
end
```

### [API] SetMusicVolume(volume)
- **Args:**
  - `volume` *(number)* — Music volume.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputDown("interact") then
 		SetMusicVolume(0.3)
	end
end
```

### [API] SetMusicLowPass(wet)
- **Args:**
  - `wet` *(number)* — Music low pass filter 0.0 - 1.0.
```lua
function client.init()
	PlayMusic("about.ogg")
end

function client.tick()
	if InputDown("interact") then
 		SetMusicLowPass(0.6)
	end
end
```

---
**Navigation:** [_INDEX](_INDEX.md)