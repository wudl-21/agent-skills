# Teardown Scripting API — Overview

> **Version:** 2.0.0 (Experimental)  
> Lua 5.1. Each script runs in its own Lua context.

Teardown uses Lua version 5.1 as scripting language. The Lua 5.1 reference manual can be found here.
Each Teardown script runs in its own Lua context and can only interact with the engine and other scripts
through API functions and the registry. The registry is a database of hierarchical global variables that is used both internally
in the engine, for communication between scripts and as a way to save persistent data.

The Teardown API uses only native lua types. Handles to objects are plain Lua numbers. Vector types are represented
as plain Lua tables, and so on.

Starting with version 2.0, the Teardown API supports networked multiplayer using a client/server architecture.
The same script runs both on the server and on each client, but different parts of the script are used. This is implemented 
through the server and client tables. Teardown does not use dedicated servers, so the player hosting a session will be 
the server for that session while also acting as one of the clients. Hence, the host is both the server and one of the clients,
while everyone else is just a client.

Each script has the following server callback functions that will be called by the game engine. Note that all of them
are optional. In many cases, you will only need the init and tick. Most of the game logic should be implemented on the server.

The following optional callback functions are available on the client. The client part of a script is typically used for overlay graphics
and user interfaces, but it can also be used for optimization purposes to spawn local particle effects, sounds or animations.

## Built-in Tables

| Built in table | Description |
| --- | --- |
| server | Only exists on the server. You can put your own global variables in here, but they will only be available on the server. |
| client | This is similar to the server table, but only exists on clients. |
| shared | Automatically synchronized data from server to client parts of the same script. Read-only from the client part of the script. The server can put any data type in the shared table, including tables, but having a lot of data that changes often can consume a lot of bandwith. |

## Server Callbacks

| Server function | Description |
| --- | --- |
| function server.init() | Called once at load time |
| function server.tick(dt) | Called exactly once per frame. The time step is variable but always between 0.0 and 0.0333333 |
| function server.update(dt) | Called at a fixed update rate, but at the most two times per frame. Time step is always 0.0166667 (60 updates per second). Depending on frame rate it might not be called at all for a particular frame. |
| function server.postUpdate() | Called like update, but after physics. Because update can trigger physics updates, it can be necessary to do some additional calculations afterwards. |
| function server.destroy() | For game mode scripts, this is called when the game mode is stopped |

## Client Callbacks

| Client function | Description |
| --- | --- |
| function client.init() | Called once at load time |
| function client.tick(dt) | Called exactly once per frame. The time step is variable but always between 0.0 and 0.0333333 |
| function client.update(dt) | Called at a fixed update rate, but at the most two times per frame. Time step is always 0.0166667 (60 updates per second). Depending on frame rate it might not be called at all for a particular frame. |
| function client.postUpdate() | Called like update, but after physics. Because update can trigger physics updates, it can be necessary to do some additional calculations afterwards. This is usually used by animators. |
| function client.draw() | Called when the 2D overlay is being draw, after the scene but before the standard HUD. Ui functions can only be used from this callback. |
| function client.render(dt) | Called exactly once per frame, right before things are actually drawn to the screen. |
| function client.destroy() | For game mode scripts, this is called when the game mode is stopped |

> **[CONSTRAINTS]**
> - Handles to objects are plain Lua numbers.
> - Vector types are plain Lua tables (`{x, y, z}` for TVec, `{x, y, z, w}` for TQuat).
> - `shared` table is auto-synced server→client; read-only on client.
> - Dedicated servers not used — host is both server and client.

---
**Navigation:** [_INDEX](_INDEX.md)