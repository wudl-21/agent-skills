# Teardown Scripting API — Overview (v1.7.0)
> **Mode:** Singleplayer | **Language:** Lua 5.1

Teardown uses Lua version 5.1 as scripting language. The Lua 5.1 reference manual can be found here. Each Teardown script runs in its own Lua context and can only interact with the engine and other scripts through API functions and the registry. The registry is a database of hierarchical global variables that is used both internally in the engine, for communication between scripts and as a way to save persistent data.

The Teardown API uses only native lua types. Handles to objects are plain Lua numbers. Vector types are represented as plain Lua tables, and so on. Each script has up-to 6 OPTIONAL callback functions that will be called by the game engine.

## Script Callbacks

| Callback | Description |
|----------|-------------|
| `init()` | Called once at load time |
| `tick(dt)` | Every frame; dt ∈ (0, 0.0333333] |
| `update(dt)` | Fixed 60 Hz; dt = 0.0166667; may not fire every frame |
| `draw()` | 2D overlay; UI functions only valid here |
| `render(dt)` | Once per frame before actual draw |
| `postUpdate()` | After physics; used by animators |

> **[CONSTRAINTS]**
> - Teardown uses **Lua 5.1** — no bitwise operators, no `goto`
> - Each script runs in its own Lua context (isolated)
> - Handles are plain Lua numbers; vectors/quats are plain tables
> - Scripts communicate via the global registry

---
**Navigation:**
**Teardown scripting** | [Parameters](01_parameters.md) | [Script control](02_script_control.md) | [Registry](03_registry.md) | [Vector math](04_vector_math.md) | [Entity](05_entity.md) | [Body](06_body.md) | [Shape](07_shape.md) | [Location](08_location.md) | [Joint](09_joint.md) | [Animation](10_animation.md) | [Light](11_light.md) | [Trigger](12_trigger.md) | [Screen](13_screen.md) | [Vehicle](14_vehicle.md) | [Player](15_player.md) | [Sound](16_sound.md) | [Sprite](17_sprite.md) | [Scene queries](18_scene_queries.md) | [Particles](19_particles.md) | [Spawning](20_spawning.md) | [Miscellaneous](21_miscellaneous.md) | [User Interface](22_ui.md)