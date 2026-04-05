# Development Workflow

## Branching

Use short-lived branches.
Recommended prefixes:
- `feature/`
- `fix/`
- `refactor/`
- `docs/`
- `test/`

Examples:
- `feature/event-queue`
- `feature/board-shapes`
- `refactor/item-registry`

## Change size

Keep changes reviewable.
Prefer PRs under a few focused files when possible.
If a larger foundation change is necessary, split it into ordered PRs.

## Definition of done

A task is done when:
- code compiles
- docs match behavior
- the feature preserves non-rectangular board compatibility
- debug tooling exists if the feature is otherwise hard to inspect
- tests or reproducible validation steps are added for game rule changes

## Implementation order

### Milestone 1: Foundation
- Unity project bootstrap
- folder structure
- event queue
- board topology and active cell model
- block definitions and placement validation

### Milestone 2: Resolution loop
- clear pattern detection
- clear resolution
- score system
- turn flow

### Milestone 3: Expandable content
- item triggers/effects
- obstacle reactions
- goals
- level definitions and loaders

### Milestone 4: Shell and tooling
- UI shell
- save/load
- debug menu
- analytics hook points

## Testing guidance

### Unit tests
Focus on:
- placement checks
- clear pattern matching
- event ordering
- item effect targeting
- obstacle damage rules
- score and combo math

### Manual validation
For every significant gameplay feature, write reproducible steps such as:
1. Load seed X.
2. Place block Y at position Z.
3. Verify patterns A and B clear.
4. Verify item C triggers before obstacle D reaction.
5. Verify final score is N.

## Debug expectations

Every foundational system should expose at least one debug path.
Examples:
- load board shape by id
- spawn block by id
- attach item by id
- step a single event queue action
- show active clear patterns

## Documentation updates

When to update docs:
- new content type: update `docs/content-schema.md`
- system boundary change: update `docs/architecture.md`
- rule change: update `docs/game-design.md`
- repo workflow change: update this file or `AGENTS.md`

## AI-specific working style

When using AI coding tools in this repo:
- provide the target subsystem explicitly
- mention any affected docs
- ask for minimal diffs unless a refactor is intended
- require explanation when a new abstraction is introduced
- reject solutions that assume the board is always rectangular

## Commit message style

Use conventional, readable prefixes:
- `feat:`
- `fix:`
- `refactor:`
- `docs:`
- `test:`
- `chore:`

Examples:
- `feat: add active-cell board topology model`
- `feat: implement clear pattern detection`
- `docs: define item and obstacle schema`
