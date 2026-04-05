# Architecture

## Goals

The architecture must support:
- mobile-first casual puzzle gameplay
- non-rectangular board shapes
- chain reactions from items and obstacles
- data-driven content expansion
- safe AI-assisted iteration

## Core design

The project should be split into **definitions**, **runtime state**, and **systems**.

### Definitions
Static content that can be authored and versioned.
Examples:
- `BoardShapeDefinition`
- `BlockDefinition`
- `ItemDefinition`
- `ObstacleDefinition`
- `GoalDefinition`
- `LevelDefinition`

### Runtime state
Transient gameplay state for the active session.
Examples:
- `BoardState`
- `TurnState`
- `ScoreState`
- `ComboState`
- `LevelRuntimeState`

### Systems
Logic processors that react to events and mutate runtime state.
Examples:
- `PlacementSystem`
- `ClearDetectionSystem`
- `ClearResolutionSystem`
- `ItemTriggerSystem`
- `ObstacleSystem`
- `GoalSystem`
- `ScoreSystem`
- `TurnSystem`

## Event-driven flow

Use a central event queue for gameplay resolution.

### Canonical turn sequence
1. Player selects and places a block.
2. `BlockPlacedEvent` is published.
3. Clear patterns are evaluated.
4. If matched, `CellsClearedEvent` is published.
5. Items attached to cleared cells enqueue `ItemTriggeredEvent`.
6. Obstacles may enqueue additional reactions.
7. Score and combo systems update.
8. Goals progress.
9. Turn end is finalized.

This avoids brittle recursive logic and makes chain handling visible and replayable.

## Module boundaries

### Core
Shared interfaces, event queue, deterministic random provider, logging, service registration.

### Board
Board topology, active cell queries, placement validation, clear pattern resolution.

### Blocks
Block shapes, rotations if applicable, block generation pools, selection tray logic.

### Items
Triggers, conditions, effects, item registries, item execution context.

### Obstacles
Obstacle durability, reaction rules, spread/transform behavior, destruction outcomes.

### Goals
Stage objectives and progress tracking.

### Levels
Board shape selection, spawn rules, difficulty modifiers, win/lose conditions.

### UI
Presentation only. Reads runtime state and events, then renders animations and feedback.

## Board topology model

Do not model the board as a guaranteed fully playable rectangle.
Use:
- width/height for bounds
- active-cell mask for playable topology
- explicit clear-pattern definitions for what counts as a completed line or region

This allows square, cross, hash, ring, and future board variants.

## Suggested interfaces

```csharp
public interface IGameEvent
{
    string EventType { get; }
}

public interface IGameSystem
{
    void Handle(IGameEvent gameEvent, GameContext context);
}

public interface IEffect
{
    void Apply(EffectContext context);
}

public interface ITrigger
{
    bool Matches(IGameEvent gameEvent, GameContext context);
}
```

## Runtime determinism

For gameplay logic, prefer deterministic execution:
- seeded RNG abstraction
- event ordering that does not depend on frame timing
- gameplay state changes independent of animation timing

This helps with testing, debugging, analytics reproduction, and AI-assisted reasoning.

## Data loading strategy

Content definitions may be authored as ScriptableObjects, JSON, or both.
Recommended approach:
- ScriptableObjects for editor authoring convenience
- exportable JSON or DTO-like runtime structures for validation and testing

Important rule: ScriptableObjects are authoring assets, not mutable runtime state containers.

## Debug tooling

The repo should eventually include debug tools for:
- spawn specific blocks
- attach specific items
- load named board shapes
- replay a seed
- step through event queue resolution
- inspect active goals and score deltas

## Future-proofing notes

Plan for later addition of:
- live events and seasonal levels
- economy hooks
- rewarded continue flows
- daily missions
- content analytics
- savegame versioning

These should integrate around the core, not leak into puzzle resolution code.
