# AGENTS.md

This file defines how humans and AI agents should work in this repository.

## Mission

Build **Cube Crush**, a mobile-first Unity 6 LTS puzzle game with:
- expandable board shapes
- item-triggered effects
- chain reactions
- data-driven content systems

## Primary rule

Do not optimize for short-term convenience if it damages long-term extensibility.

When there is a tradeoff, prefer:
- explicit architecture
- deterministic behavior
- testable systems
- data-driven configuration
- small, reviewable pull requests

## Non-goals

Avoid these patterns unless there is a strong, documented reason:
- hardcoding stage-specific gameplay inside generic systems
- putting item logic directly in board cells or UI scripts
- mixing authoring data with runtime state
- using singletons as a shortcut for gameplay dependencies
- burying gameplay rules inside animation or presentation scripts

## Source of truth

When making changes, follow this precedence:
1. `docs/game-design.md`
2. `docs/architecture.md`
3. `docs/content-schema.md`
4. local code comments only when they do not conflict with the docs

If code and docs diverge, update the docs in the same change unless the divergence is accidental and being removed.

## Architecture constraints

### 1. Board model
Treat the board as a set of active cells, not as a guaranteed rectangle.

That means:
- never assume every coordinate inside width/height is playable
- all placement and effect logic must check active cell validity
- clear rules must be expressed as explicit patterns, not assumed full rows/columns

### 2. Gameplay flow
The gameplay loop should be event-driven.

Expected flow:
- player places a block
- clear patterns are evaluated
- cells are cleared
- triggered items enqueue effects
- obstacles react
- score and goals update
- turn completes

Use an event queue instead of deep recursive effect calls.

### 3. Data-driven content
Items, obstacles, goals, board shapes, and levels must be addable without editing the core engine in most cases.

New content should usually mean:
- add a definition entry
- add or reuse an effect/trigger implementation
- wire it through registries

### 4. Presentation separation
Gameplay code must not depend on animation timing for correctness.
Presentation should observe gameplay state/events, not control rule outcomes.

## Coding guidance

### Preferred style
- small classes with narrow responsibility
- descriptive names
- explicit constructor dependencies where practical
- minimal hidden state
- guard clauses for invalid input
- deterministic random via seeded providers for test/replay paths

### Avoid
- giant manager classes
- switch statements that grow with every new item type when a registry/strategy is more appropriate
- magic numbers without named constants or config ownership
- UI code mutating gameplay state directly

## Recommended high-level folders

- `Assets/_Project/Scripts/Core`
- `Assets/_Project/Scripts/Board`
- `Assets/_Project/Scripts/Blocks`
- `Assets/_Project/Scripts/Items`
- `Assets/_Project/Scripts/Obstacles`
- `Assets/_Project/Scripts/Goals`
- `Assets/_Project/Scripts/Levels`
- `Assets/_Project/Scripts/UI`
- `Assets/_Project/Data`
- `Assets/_Project/Tests`

## AI contribution checklist

Before submitting a change, confirm:
- does this preserve non-rectangular board support?
- does this avoid hardcoding stage or item behavior into generic systems?
- is runtime state separate from definitions/assets?
- can future items or obstacles reuse this path?
- are docs updated if a system contract changed?

## PR shape

Prefer PRs that do one of these:
- introduce one gameplay system
- add one content type end-to-end
- refactor one unstable subsystem
- add one tool/debug workflow
- update one documentation area with matching code

Avoid mixing architecture refactors with unrelated gameplay content.

## Testing expectations

At minimum, changes touching gameplay logic should be validated for:
- placement validity
- clear pattern detection
- event queue ordering
- chain reaction stability
- obstacle reactions
- score calculation

## Documentation rule

If a new definition type, registry, event, or subsystem is introduced, document it in the relevant file under `docs/`.
