# Particles

Functions to configure and emit particles, used for fire, smoke and other visual effects. There are
two types of particles in Teardown - plain particles and smoke particles. Plain particles are simple
billboard particles simulated with gravity and velocity that can be used for fire, debris, rain, snow and such.
Smoke particles are only intended for smoke and they are simulated with fluid dynamics internally and rendered
with some special tricks to get a more smoke-like appearance.

All functions in the particle API, except for SpawnParticle modify properties in the particle state, which is
then used when emitting particles, so the idea is to set up a state, and then emit one or several particles
using that state.

Most properties in the particle state can be either constant or animated over time. Supply a single argument
for constant, two argument for linear interpolation, and optionally a third argument for other types of
interpolation. There are also fade in and fade out parameters that fade from and to zero.

### [API] ParticleReset()
```lua
function client.init()
	ParticleReset()
end
```

### [API] ParticleType(type)
- **Args:**
  - `type` *(string)* — Type of particle. Can be "smoke" or "plain".
```lua
function client.init()
	ParticleType("smoke")
end
```

### [API] ParticleTile(type)
- **Args:**
  - `type` *(number)* — Tile in the particle texture atlas (0-15)
```lua
function client.init()
	--Smoke particle
	ParticleTile(0)

	--Fire particle
	ParticleTile(5)
end
```

### [API] ParticleColor(r0, g0, b0, [r1], [g1], [b1])
- **Args:**
  - `r0` *(number)* — Red value
  - `g0` *(number)* — Green value
  - `b0` *(number)* — Blue value
  - `r1` *(number, optional)* — Red value at end
  - `g1` *(number, optional)* — Green value at end
  - `b1` *(number, optional)* — Blue value at end
```lua
function client.init()
	--Constant red
	ParticleColor(1,0,0)

	--Animating from yellow to red
	ParticleColor(1,1,0, 1,0,0)
end
```

### [API] ParticleRadius(r0, [r1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `r0` *(number)* — Radius
  - `r1` *(number, optional)* — End radius
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Constant radius 0.4 meters
	ParticleRadius(0.4)

	--Interpolate from small to large
	ParticleRadius(0.1, 0.7)
end
```

### [API] ParticleAlpha(a0, [a1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `a0` *(number)* — Alpha (0.0 - 1.0)
  - `a1` *(number, optional)* — End alpha (0.0 - 1.0)
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Interpolate from opaque to transparent
	ParticleAlpha(1.0, 0.0)
end
```

### [API] ParticleGravity(g0, [g1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `g0` *(number)* — Gravity
  - `g1` *(number, optional)* — End gravity
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Move particles slowly upwards
	ParticleGravity(2)
end
```

### [API] ParticleDrag(d0, [d1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `d0` *(number)* — Drag
  - `d1` *(number, optional)* — End drag
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Slow down fast moving particles
	ParticleDrag(0.5)
end
```

### [API] ParticleEmissive(d0, [d1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `d0` *(number)* — Emissive
  - `d1` *(number, optional)* — End emissive
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Highly emissive at start, not emissive at end
	ParticleEmissive(5, 0)
end
```

### [API] ParticleRotation(r0, [r1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `r0` *(number)* — Rotation speed in radians per second.
  - `r1` *(number, optional)* — End rotation speed in radians per second.
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Rotate fast at start and slow at end
	ParticleRotation(10, 1)
end
```

### [API] ParticleStretch(s0, [s1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `s0` *(number)* — Stretch
  - `s1` *(number, optional)* — End stretch
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Stretch particle along direction of motion
	ParticleStretch(1.0)
end
```

### [API] ParticleSticky(s0, [s1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `s0` *(number)* — Sticky (0.0 - 1.0)
  - `s1` *(number, optional)* — End sticky (0.0 - 1.0)
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Make particles stick to objects
	ParticleSticky(0.5)
end
```

### [API] ParticleCollide(c0, [c1], [interpolation], [fadein], [fadeout])
- **Args:**
  - `c0` *(number)* — Collide (0.0 - 1.0)
  - `c1` *(number, optional)* — End collide (0.0 - 1.0)
  - `interpolation` *(string, optional)* — Interpolation method: linear, smooth, easein, easeout or constant. Default is linear.
  - `fadein` *(number, optional)* — Fade in between t=0 and t=fadein. Default is zero.
  - `fadeout` *(number, optional)* — Fade out between t=fadeout and t=1. Default is one.
```lua
function client.init()
	--Disable collisions
	ParticleCollide(0)

	--Enable collisions over time
	ParticleCollide(0, 1)

	--Ramp up collisions very quickly, only skipping the first 5% of lifetime
	ParticleCollide(1, 1, "constant", 0.05)
end
```

### [API] ParticleFlags(bitmask)
- **Args:**
  - `bitmask` *(number)* — Particle flags (bitmask 0-65535)
```lua
function client.tick()
	--Fire extinguishing particle
	ParticleFlags(256)
	SpawnParticle(Vec(0, 10, 0), -0.1, math.random() + 1)
end
```

### [API] SpawnParticle(pos, velocity, lifetime)
- **Args:**
  - `pos` *(TVec)* — World space point as vector
  - `velocity` *(TVec)* — World space velocity as vector
  - `lifetime` *(number)* — Particle lifetime in seconds
```lua
function client.tick()
	ParticleReset()
	ParticleType("smoke")
	ParticleColor(0.7, 0.6, 0.5)
	--Spawn particle at world origo with upwards velocity and a lifetime of ten seconds
	SpawnParticle(Vec(0, 5, 0), Vec(0, 1, 0), 10.0)
end
```

---
**Navigation:** [_INDEX](_INDEX.md)