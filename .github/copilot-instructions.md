# GitHub Copilot Instructions

This repository contains **Cube Crush**, a Unity 6 LTS mobile puzzle game.

## What matters most

Prioritize:
- extensible architecture
- non-rectangular board support
- event-driven resolution
- data-driven content
- small, explainable changes

## Hard constraints

- Do not assume the board is a full rectangle.
- Do not hardcode level-specific behavior into core systems.
- Do not mix runtime mutable state into content definition assets.
- Do not make animation timing part of gameplay correctness.
- Do not use giant switch statements for every item if a registry or strategy approach is better.

## Preferred implementation patterns

- `Definition` classes for static content
- `State` classes for runtime mutable state
- `System` classes for rule processing
- event queue for chained resolution
- dependency injection or explicit wiring over hidden global state

## When editing gameplay code

Always consider:
1. Does this work for cross/hash/future board masks?
2. Can future items/obstacles reuse this path?
3. Is the behavior deterministic and testable?
4. Does presentation stay separate from game rules?

## Good task framing examples

- "Implement board active-mask validation without assuming a rectangular playable area."
- "Add a clear-pattern detector that accepts explicit pattern cell lists."
- "Create an item effect registry for destroyArea, clearRow, and reroll."
- "Add unit tests for chain resolution ordering."

## Bad task framing examples

- "Just make rows clear when full."
- "Hardcode stage 12 behavior inside the board manager."
- "Put bomb logic straight in the cell view script."

## Output preferences

When proposing code changes:
- explain the chosen abstraction briefly
- keep diffs small unless a foundational refactor is requested
- mention any docs that should be updated
- prefer maintainability over cleverness
