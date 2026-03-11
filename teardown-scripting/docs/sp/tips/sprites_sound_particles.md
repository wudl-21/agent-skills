# Sprites, Sound & Particles

> Related: [scripting_basics.md](scripting_basics.md)

## Sprites

Sprites are flat images placed in **world space** (not screen-space UI).

- Load from `init()`; draw from `tick()` (not `draw()`).
- Format: JPEG or PNG.

```lua
local spriteHandle

function init()
    spriteHandle = LoadSprite("MOD/images/icon.png")
end

function tick(dt)
    DrawSprite(
        spriteHandle,
        transform,    -- Transform: world position & orientation
        size,         -- float: size in meters
        r, g, b, a,   -- color multiplier (0.0–1.0)
        depthTest,    -- bool: occluded by world geometry?
        additive      -- bool: use additive blending?
    )
end
```

> **[CONSTRAINTS]**
> - `DrawSprite` must be called from `tick()`, **not** `draw()`.
> - Must call every frame (sprites are not persistent).

## Sound Effects

Only **OGG Vorbis** (`.ogg`) files are supported. Load in `init()`, play in `tick()`.

```lua
local sndHandle

function init()
    sndHandle = LoadSound("MOD/sound/bang.ogg")
end

function tick(dt)
    if fired then
        PlaySound(sndHandle, worldPosition, volume)  -- positional, affected by acoustics
    end
end
```

### UI Sounds (Menu / Non-positional)

```lua
-- In draw() callback only:
UiSound("click.ogg", volume)
```

### Random Sound Variants

Name files with incrementing suffixes (`bang0.ogg`, `bang1.ogg`, `bang2.ogg`, ...):

```lua
-- Load any one; engine randomly picks a variant on each PlaySound call
LoadSound("bang0.ogg")
```

### Looping Sounds & Music

```lua
local loopHandle  = LoadLoop("engine_loop.ogg")
PlayLoop(loopHandle, worldPosition, volume)   -- plays looped at world position

local musicHandle = LoadMusic("MOD/music/theme.ogg")
PlayMusic(musicHandle, volume)
```

## Particles

### 1. Set Up State (before spawning)

```lua
ParticleReset()                          -- reset all properties to defaults
ParticleType(1)                          -- atlas index (1 = smoke)
ParticleRadius(0.3, 1.0)                 -- start radius → end radius (meters)
ParticleColor(0.5, 0.3, 0.1,  1, 1, 1)  -- start RGB → end RGB
ParticleAlpha(1.0, 0.0)                  -- start opacity → end opacity
ParticleGravity(2.0)                     -- gravity scale
```

### 2. Spawn Particles

```lua
SpawnParticle(
    position,   -- Vec: world position
    velocity,   -- Vec: initial velocity
    lifetime    -- float: seconds
)
```

### Particle Burst — Full Pattern

```lua
local function RandVec(scale)
    return Vec(
        (math.random() - 0.5) * scale,
        (math.random() - 0.5) * scale,
        (math.random() - 0.5) * scale
    )
end

local function SpawnExplosion(pos)
    ParticleReset()
    ParticleRadius(0.3, 1.0)
    ParticleColor(0.5, 0.3, 0.1,   1.0, 1.0, 1.0)  -- brown → white
    ParticleAlpha(1.0, 0.0)
    ParticleGravity(2.0)

    for i = 1, 100 do
        SpawnParticle(
            pos,
            RandVec(5),                        -- random velocity spread
            1.5 + math.random() * 1.0          -- randomized lifetime 1.5–2.5s
        )
    end
end
```

> **[CONSTRAINTS]**
> - Particle state set before `SpawnParticle` applies to **all** spawned particles until the next state change.
> - Call `ParticleReset()` before setting up a new effect to avoid inheriting previous state.
> - Teardown particles support physics **collision with the environment** and **fluid dynamics simulation** (smoke behavior).
> - Particles are spawned from `tick()`, not `draw()`.
