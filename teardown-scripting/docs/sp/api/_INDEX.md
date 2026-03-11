# Teardown SP Scripting API — Knowledge Base Index

> **Version:** 1.7.0 | **Mode:** Singleplayer | **Engine:** Lua 5.1

## Quick Reference

| File | Section | Functions |
|------|---------|-----------|
| [00_overview.md](00_overview.md) | Teardown scripting | 0 |
| [01_parameters.md](01_parameters.md) | Parameters | 5 |
| [02_script_control.md](02_script_control.md) | Script control | 20 |
| [03_registry.md](03_registry.md) | Registry | 19 |
| [04_vector_math.md](04_vector_math.md) | Vector math | 33 |
| [05_entity.md](05_entity.md) | Entity | 16 |
| [06_body.md](06_body.md) | Body | 30 |
| [07_shape.md](07_shape.md) | Shape | 40 |
| [08_location.md](08_location.md) | Location | 3 |
| [09_joint.md](09_joint.md) | Joint | 16 |
| [10_animation.md](10_animation.md) | Animation | 33 |
| [11_light.md](11_light.md) | Light | 11 |
| [12_trigger.md](12_trigger.md) | Trigger | 13 |
| [13_screen.md](13_screen.md) | Screen | 5 |
| [14_vehicle.md](14_vehicle.md) | Vehicle | 14 |
| [15_player.md](15_player.md) | Player | 64 |
| [16_sound.md](16_sound.md) | Sound | 22 |
| [17_sprite.md](17_sprite.md) | Sprite | 2 |
| [18_scene_queries.md](18_scene_queries.md) | Scene queries | 24 |
| [19_particles.md](19_particles.md) | Particles | 15 |
| [20_spawning.md](20_spawning.md) | Spawning | 2 |
| [21_miscellaneous.md](21_miscellaneous.md) | Miscellaneous | 50 |
| [22_ui.md](22_ui.md) | User Interface | 98 |

## Key Constraints

> **[CONSTRAINTS]**
> - **Lua 5.1 only**: no bitwise operators (`&`, `|`, `~`), no `goto`, no integer division `//`
> - Arrays are 1-indexed
> - Use `--` for comments
> - `tick(dt)` is for physics/gameplay; `update(dt)` for input; `draw()` for UI/HUD
> - UI functions (`Ui*`) are **only valid inside `draw()`**
> - Entity handles are plain numbers; transforms are `{pos=Vec(...), rot=Quat(...)}`

## Section Descriptions

- **[Teardown scripting](00_overview.md)**: Script lifecycle, callback signatures, and architecture overview.
- **[Parameters](01_parameters.md)**: Reading script parameters defined in level XML.
- **[Script control](02_script_control.md)**: Version checks, time, input handling, pausing, level transitions.
- **[Registry](03_registry.md)**: Global key/value store for inter-script communication and persistence.
- **[Vector math](04_vector_math.md)**: Vec, Quat, Transform construction and math operations.
- **[Entity](05_entity.md)**: Generic entity queries (tags, properties, transforms, existence).
- **[Body](06_body.md)**: Rigid body physics: velocity, mass, force, sleep state.
- **[Shape](07_shape.md)**: Voxel shapes: material queries, painting, bounds.
- **[Location](08_location.md)**: Named transform locations in the scene.
- **[Joint](09_joint.md)**: Constraints between bodies: motors, limits, breakage.
- **[Animation](10_animation.md)**: Animating objects along paths.
- **[Light](11_light.md)**: Dynamic lights: position, color, intensity, flicker.
- **[Trigger](12_trigger.md)**: Volume triggers: overlap detection with bodies/players.
- **[Screen](13_screen.md)**: In-world screen entities for rendering textures.
- **[Vehicle](14_vehicle.md)**: Vehicle state: speed, driver, damage, part control.
- **[Player](15_player.md)**: Player state: position, health, tool, camera, movement.
- **[Sound](16_sound.md)**: Positional audio: playing, looping, 3D sound.
- **[Sprite](17_sprite.md)**: World-space billboards and 2D sprites.
- **[Scene queries](18_scene_queries.md)**: Raycasting, sphere/box overlap, line-of-sight checks.
- **[Particles](19_particles.md)**: Particle effects and explosions.
- **[Spawning](20_spawning.md)**: Dynamic entity spawning from XML.
- **[Miscellaneous](21_miscellaneous.md)**: Debug utilities, file I/O, HTTP, terrain tools.
- **[User Interface](22_ui.md)**: 2D HUD/UI rendering, text, rectangles, images (draw() only).