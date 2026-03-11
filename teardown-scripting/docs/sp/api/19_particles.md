# Teardown SP API — Particles
> **Version:** 1.7.0 | **Mode:** Singleplayer | **Language:** Lua 5.1

Functions to configure and emit particles, used for fire, smoke and other visual effects. There are two types of particles in Teardown - plain particles and smoke particles. Plain particles are simple billboard particles simulated with gravity and velocity that can be used for fire, debris, rain, snow and such. Smoke particles are only intended for smoke and they are simulated with fluid dynamics internally and rendered with some special tricks to get a more smoke-like appearance.

All functions in the particle API, except for SpawnParticle modify properties in the particle state, which is then used when emitting particles, so the idea is to set up a state, and then emit one or several particles using that state.

Most properties in the particle state can be either constant or animated over time. Supply a single argument for constant, two argument for linear interpolation, and optionally a third argument for other types of interpolation. There are also fade in and fade out parameters that fade from and to zero.

---

### [API] ParticleReset

```lua
ParticleReset()
```

Reset to default particle state, which is a plain, white particle of radius 0.5. Collision is enabled and it alpha animates from 1 to 0.

**Example:**

```lua
function init()
	ParticleReset()
end
```

---

### [API] ParticleType

```lua
ParticleType(type)
```

Set type of particle

**Arguments:**

- `type` *(string)* — Type of particle. Can be "smoke" or "plain".

**Example:**

```lua
function init()
	ParticleType("smoke")
end
```

---

### [API] ParticleTile

```lua
ParticleTile(type)
```

**Arguments:**

- `type` *(number)* — Tile in the particle texture atlas (0-15)

**Example:**

```lua
function init()
	--Smoke particle
	ParticleTile(0)
	
	--Fire particle
	ParticleTile(5)
end
```

---

### [API] ParticleColor

```lua
ParticleColor(r0, g0, b0, [r1], [g1], [b1])
```

Set particle color to either constant (three arguments) or linear interpolation (six arguments)

**Arguments:**

- `r0` *(number)* — Red value

- `g0` *(number)* — Green value

- `b0` *(number)* — Blue value

- `r1` *(number, optional)* — Red value at end

- `g1` *(number, optional)* — Green value at end

- `b1` *(number, optional)* — Blue value at end

**Example:**

```lua
function init()
	--Constant red
	ParticleColor(1,0,0)

	--Animating from yellow to red
	ParticleColor(1,1,0, 1,0,0)
end
```

---

### [API] ParticleRadius

```lua
ParticleRadius(r0, [r1], [interpolation], [fadein], [fadeout])
```

Set the particle radius. Max radius for smoke particles is 1.0.

**Arguments:**

- `r0` *(number)* — Radius

- `r1` *(number, optional)* — End radius

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Constant radius 0.4 meters
	ParticleRadius(0.4)

	--Interpolate from small to large
	ParticleRadius(0.1, 0.7)
end
```

---

### [API] ParticleAlpha

```lua
ParticleAlpha(a0, [a1], [interpolation], [fadein], [fadeout])
```

Set the particle alpha (opacity).

**Arguments:**

- `a0` *(number)* — Alpha (0.0 - 1.0)

- `a1` *(number, optional)* — End alpha (0.0 - 1.0)

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Interpolate from opaque to transparent
	ParticleAlpha(1.0, 0.0)
end
```

---

### [API] ParticleGravity

```lua
ParticleGravity(g0, [g1], [interpolation], [fadein], [fadeout])
```

Set particle gravity. It will be applied along the world Y axis. A negative value will move the particle downwards.

**Arguments:**

- `g0` *(number)* — Gravity

- `g1` *(number, optional)* — End gravity

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Move particles slowly upwards
	ParticleGravity(2)
end
```

---

### [API] ParticleDrag

```lua
ParticleDrag(d0, [d1], [interpolation], [fadein], [fadeout])
```

Particle drag will slow down fast moving particles. It's implemented slightly different for smoke and plain particles. Drag must be positive, and usually look good between zero and one.

**Arguments:**

- `d0` *(number)* — Drag

- `d1` *(number, optional)* — End drag

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Slow down fast moving particles
	ParticleDrag(0.5)
end
```

---

### [API] ParticleEmissive

```lua
ParticleEmissive(d0, [d1], [interpolation], [fadein], [fadeout])
```

Draw particle as emissive (glow in the dark). This is useful for fire and embers.

**Arguments:**

- `d0` *(number)* — Emissive

- `d1` *(number, optional)* — End emissive

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Highly emissive at start, not emissive at end
	ParticleEmissive(5, 0)
end
```

---

### [API] ParticleRotation

```lua
ParticleRotation(r0, [r1], [interpolation], [fadein], [fadeout])
```

Makes the particle rotate. Positive values is counter-clockwise rotation.

**Arguments:**

- `r0` *(number)* — Rotation speed in radians per second.

- `r1` *(number, optional)* — End rotation speed in radians per second.

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Rotate fast at start and slow at end
	ParticleRotation(10, 1)
end
```

---

### [API] ParticleStretch

```lua
ParticleStretch(s0, [s1], [interpolation], [fadein], [fadeout])
```

Stretch particle along with velocity. 0.0 means no stretching. 1.0 stretches with the particle motion over one frame. Larger values stretches the particle even more.

**Arguments:**

- `s0` *(number)* — Stretch

- `s1` *(number, optional)* — End stretch

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Stretch particle along direction of motion
	ParticleStretch(1.0)
end
```

---

### [API] ParticleSticky

```lua
ParticleSticky(s0, [s1], [interpolation], [fadein], [fadeout])
```

Make particle stick when in contact with objects. This can be used for friction.

**Arguments:**

- `s0` *(number)* — Sticky (0.0 - 1.0)

- `s1` *(number, optional)* — End sticky (0.0 - 1.0)

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Make particles stick to objects
	ParticleSticky(0.5)
end
```

---

### [API] ParticleCollide

```lua
ParticleCollide(c0, [c1], [interpolation], [fadein], [fadeout])
```

Control particle collisions. A value of zero means that collisions are ignored. One means full collision. It is sometimes useful to animate this value from zero to one in order to not collide with objects around the emitter.

**Arguments:**

- `c0` *(number)* — Collide (0.0 - 1.0)

- `c1` *(number, optional)* — End collide (0.0 - 1.0)

- `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.

- `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.

- `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.

**Example:**

```lua
function init()
	--Disable collisions
	ParticleCollide(0)

	--Enable collisions over time
	ParticleCollide(0, 1)

	--Ramp up collisions very quickly, only skipping the first 5% of lifetime
	ParticleCollide(1, 1, "constant", 0.05)
end
```

---

### [API] ParticleFlags

```lua
ParticleFlags(bitmask)
```

Set particle bitmask. The value 256 means fire extinguishing particles and is currently the only flag in use. There might be support for custom flags and queries in the future.

**Arguments:**

- `bitmask` *(number)* — Particle flags (bitmask 0-65535)

**Example:**

```lua
function tick()
	--Fire extinguishing particle
	ParticleFlags(256)
	SpawnParticle(Vec(0, 10, 0), -0.1, math.random() + 1)
end
```

---

### [API] SpawnParticle

```lua
SpawnParticle(pos, velocity, lifetime)
```

Spawn particle using the previously set up particle state. You can call this multiple times using the same particle state, but with different position, velocity and lifetime. You can also modify individual properties in the particle state in between calls to to this function.

**Arguments:**

- `pos` *(TVec)* — World space point as vector

- `velocity` *(TVec)* — World space velocity as vector

- `lifetime` *(number)* — Particle lifetime in seconds

**Example:**

```lua
function tick()
	ParticleReset()
	ParticleType("smoke")
	ParticleColor(0.7, 0.6, 0.5)
	--Spawn particle at world origo with upwards velocity and a lifetime of ten seconds
	SpawnParticle(Vec(0, 5, 0), Vec(0, 1, 0), 10.0)
end
```

---


---
**Navigation:**
[Teardown scripting](00_overview.md) | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | **Particles** | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)