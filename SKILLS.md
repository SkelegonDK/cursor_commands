# Manuel Skills — Branching Skill System

This project uses a single **manuel-skills** skill that automatically routes to the correct workflow based on context.

## How It Works

1. **One skill, many branches** — The agent applies `manuel-skills` when your task matches any of the domains below.
2. **Context-based routing** — The skill includes a branch selection table; the agent picks the matching workflow.
3. **Progressive loading** — Full details for each branch live in `_cursor/skills/manuel-skills/references/`.

## When It Applies

| Domain | Branch |
|--------|--------|
| Brainstorming, ideation | Idea Generation |
| Evidence-based research | Evidence-Based |
| Debugging, bugs | Debugging |
| New feature implementation | New Features |
| General clarification | Probing |
| Security audit | Security Scan |
| Web component architecture | Web Components State Diagram |
| Danish copy | Dansk Copy |
| Prompt crafting | Prompt Template |

## Usage

- **Automatic:** The agent applies the skill when relevant.
- **Manual:** Type `/manuel-skills` in Agent chat.

## Source

Workflows are derived from `_cursor/commands/`. The skill system replaces the need to invoke individual commands; context determines the branch.
