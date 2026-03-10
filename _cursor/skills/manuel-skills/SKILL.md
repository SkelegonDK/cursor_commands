---
name: manuel-skills
description: Router skill that loads specialized sub-skills based on context. Covers Godot Engine game development, macOS/iOS automated testing with Steve CLI, PRD and product requirements, and other Manuel workflows. Use when the task involves Godot, GDScript, game development, Steve testing, accessibility automation, PRDs, product requirements, or when the user references "Manuel skills."
---

# Manuel Skills — Branching Router

This skill routes to the correct sub-skill based on context. **Read only the matched sub-skill file** to minimize tokens. Do not load multiple sub-skills unless the task clearly spans domains.

## Routing Table

Evaluate the user's request and open files. If exactly one route matches, read that file and follow it. If multiple match, pick the primary one; if none match, proceed without a sub-skill.

| If context mentions… | Read this file |
|----------------------|----------------|
| Godot, GDScript, game development, scenes, nodes, signals, autoloads | [../expert-godot-game-developer/SKILL.md](../expert-godot-game-developer/SKILL.md) |
| Steve CLI, macOS testing, iOS Simulator, accessibility API, UI automation | [../steve-testing/SKILL.md](../steve-testing/SKILL.md) |
| PRD, product requirements, stakeholders, user stories, discovery interview | [prd.md](prd.md) (same directory) |

**Fallback:** If no route matches, inform the user that no specialized skill applies and proceed normally.

## Extensibility

To add a new sub-skill:

1. Create a `.md` file in this directory (e.g. `new-skill.md`), or add a path to a sibling skill.
2. Add one row to the Routing Table above with trigger terms and the file path.
3. No other changes required.
