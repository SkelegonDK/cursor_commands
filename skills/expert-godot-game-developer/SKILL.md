---
name: expert-godot-game-developer
description: Applies Godot Engine best practices, scene architecture, GDScript conventions, and performance patterns from official documentation and expert sources. Use when developing Godot games, writing GDScript, designing node/scene structure, using signals or autoloads, or optimizing Godot projects.
---

# Expert Godot Game Developer

Evidence-based guidance from [Godot Engine best practices](https://docs.godotengine.org/en/stable/tutorials/best_practices/index.html), [GDScript style guide](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html), [CPU optimization](https://docs.godotengine.org/en/stable/tutorials/performance/cpu_optimization.html), and expert practitioners (e.g. GDQuest).

---

## Scene organization & coupling

**Design scenes with no dependencies when possible.** Keep everything a scene needs within itself. If a scene must interact with external context, use **dependency injection**: the parent (or high-level API) provides dependencies to the child; the child never reaches up or assumes environment.

**Ways to inject dependencies (parent → child):**

1. **Connect to a signal** — safest; use only to *respond* to behavior, not start it. Signal names: past tense (`entered`, `item_collected`, `skill_activated`).
2. **Call a method** — use to *start* behavior.
3. **Set a Callable property** — safer than passing a method name; used to start behavior.
4. **Set a Node reference or NodePath** — child uses parent-provided target.

Child stays **loosely coupled**: no hard node paths to parents/siblings, no assumptions about tree structure. Prefer **data in-house**; external dependencies are liabilities. Use `_get_configuration_warnings()` in tool scripts to self-document required setup (e.g. missing dependencies).

**Node tree as relations, not just layout:** Parent–child = “removing parent implies removing children.” If not, use siblings or separate branches. Use `Node` (no transform) between nodes to break transform inheritance, or `top_level` on CanvasItem/Node3D. For positioning across branches, use `RemoteTransform2D` / `RemoteTransform3D` with `target` assigned by a shared parent or groups.

**Suggested high-level structure:**

```
Main (main.gd) — entry point, primary controller
├── World (Node2D/Node3D) — swap children when changing levels
└── GUI (Control) — menus/widgets; keep as singleton, World child, or root child
```

Change levels by swapping World’s children (see [Change scenes manually](https://docs.godotengine.org/en/stable/tutorials/scripting/change_scenes_manually.html)), not necessarily `change_scene_to_file`. For “player persists across rooms,” consider keeping the player **outside** the room branch (e.g. under Main) to avoid special-case reparenting and doc-heavy workflows.

---

## Signals

- **Use signals to respond**, not to initiate. Name them past tense: `door_opened`, `score_changed`.
- **Avoid signal bubbling** — re-emitting child signals up through parents makes connections hard to trace. Prefer direct connections at setup or a focused event bus over deep chains.
- **Keep connection hops few** — many intermediate steps usually mean weak architecture.
- Signals are **low-cost**; use them for decoupling.

---

## Autoloads vs regular nodes

**Prefer scene-local state over globals.** Autoloads introduce global state, global access (hard-to-trace bugs), and often global resource allocation (e.g. pooled `AudioStreamPlayer`s). Prefer each scene owning its `AudioStreamPlayer`s (or small pools) and its own logic.

**Use autoloads only when** the system (1) tracks its own data, (2) must be globally accessible, and (3) exists in isolation and does **not** modify other systems’ data. Examples: quest system, dialogue manager. For shared **logic**, use `class_name` + static helpers; for shared **data**, use `Resource` or an object nodes can access (e.g. via `owner`).

Since Godot 4.1, `static var` and `static func` with `class_name` reduce the need for autoloads for pure helpers.

---

## Nodes vs lighter types

**Use nodes only when you need node features.** Tens of thousands of complex nodes can hurt performance. Prefer:

- **Object** — minimal, manual refcounting; good for custom structures (e.g. `TreeItem`-like data). Beware invalid references if the owner frees them.
- **RefCounted** — refcounted; use for most custom data that doesn’t need serialization.
- **Resource** — serialization, Inspector support; use for data/config that many nodes share.

Use these for data structures and logic that don’t require being in the scene tree.

---

## Scenes vs scripts

- **Scenes**: declarative composition, better performance for large hierarchies. Use `PackedScene.instantiate()`; engine handles creation efficiently.
- **Scripts**: imperative, good for small reusable tools. Creating big node trees purely from scripts is slower and more brittle.
- **Default**: game-specific content → **scenes**. Reusable, generic tools → **scripts** (optionally `class_name`). For a “named scene,” use a script with `extends RefCounted`, `class_name`, and a `preload` of the scene as a constant.

---

## Project organization

- **Snake_case** for folders and files (except C# scripts → PascalCase).
- **PascalCase** for node names.
- Group assets **near the scenes** that use them (e.g. `characters/player/`, `levels/riverdale/`).
- Third-party content in top-level `addons/`.
- Use **`.gdignore`** in a folder to exclude it from import (and FileSystem dock). File must be empty; no patterns.
- Use **LF**, **UTF-8**, **tabs** for indentation. Avoid case-only renames; export/PCK is case-sensitive.

---

## GDScript style (summary)

| Kind        | Convention   | Example                |
|------------|--------------|------------------------|
| Files      | snake_case   | `yaml_parser.gd`       |
| Classes    | PascalCase   | `class_name YAMLParser`|
| Nodes      | PascalCase   | `Camera3D`, `Player`   |
| Functions  | snake_case   | `load_level()`         |
| Variables  | snake_case   | `particle_effect`      |
| Signals    | snake_case, past tense | `door_opened`   |
| Constants  | CONSTANT_CASE| `const MAX_SPEED = 200`|
| Private    | leading `_`  | `_counter`, `_ready()` |
| Enums      | PascalCase + CONSTANT_CASE | `enum Element { EARTH, WATER }` |

- **Typing:** Use `:=` when type is obvious; add explicit types when ambiguous. Type `get_node()` returns (e.g. `@onready var bar: ProgressBar = get_node(...)`) or use `as` casts.
- **Formatting:** One statement per line; trailing commas in multiline arrays/dicts/enums; wrap long lines with parentheses; prefer `and` / `or` / `not`; max ~100 chars, ideally 80.
- **Code order:** `@tool` → `class_name` → `extends` → doc → signals → enums → constants → `@export` → vars → `@onready` → `_init` / `_ready` → other virtuals → public → private methods.

---

## Performance

1. **Profile first.** Use the built-in Debugger profiler; focus on the largest cost per frame.
2. **SceneTree:** Every node has a cost; `_process` / `_physics_process` propagate. Prefer fewer, slightly “heavier” nodes over huge node counts. Detach nodes with `remove_child` when not needed instead of only hiding/pausing; reattach with `add_child` when needed.
3. **Physics:** Use simplified collision shapes; disable or pool bodies off-screen; use physics interpolation for fixed timestep. Default 60 Hz physics is recommended for real-time movement.
4. **Heavy logic:** Consider C# or GDExtension for hot paths; use **threads** only with clear ownership and no shared mutable state (avoid race conditions).
5. **Manual timing:** Use `Time.get_ticks_usec()` around the code under test; run many iterations and average to reduce timer and cache noise.

---

## Checklist

- [ ] Scenes dependency-free or using dependency injection; no hard parent/sibling paths in children
- [ ] Signals for *reaction* only; past-tense names; no long signal chains
- [ ] Autoloads only for isolated, globally needed systems; prefer scene-local state
- [ ] Object/RefCounted/Resource instead of nodes where node features aren’t needed
- [ ] Game-specific content as scenes; `PackedScene.instantiate()` for hierarchies
- [ ] snake_case files/folders; PascalCase nodes; GDScript style guide applied
- [ ] Profiler used before optimizing; SceneTree and physics kept lean

---

## Additional resources

- **Official best practices:** [docs.godotengine.org – Best practices](https://docs.godotengine.org/en/stable/tutorials/best_practices/index.html)
- **GDScript style guide:** [GDScript style guide](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_styleguide.html)
- **Performance:** [CPU optimization](https://docs.godotengine.org/en/stable/tutorials/performance/cpu_optimization.html), [Using Servers](https://docs.godotengine.org/en/stable/tutorials/performance/using_servers.html)
- **Further reference:** [reference.md](reference.md)
