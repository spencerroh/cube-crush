# Content Schema

This document defines the recommended content model for Cube Crush.

## Principles

- Definitions are static, authorable data.
- Runtime state is separate and mutable.
- Systems interpret definitions and mutate state.
- Future content should fit by adding definitions, not rewriting core systems.

## BoardShapeDefinition

```json
{
  "id": "cross_9",
  "width": 9,
  "height": 9,
  "activeMask": [
    [0,0,0,1,1,1,0,0,0],
    [0,0,0,1,1,1,0,0,0],
    [0,0,0,1,1,1,0,0,0],
    [1,1,1,1,1,1,1,1,1],
    [1,1,1,1,1,1,1,1,1],
    [1,1,1,1,1,1,1,1,1],
    [0,0,0,1,1,1,0,0,0],
    [0,0,0,1,1,1,0,0,0],
    [0,0,0,1,1,1,0,0,0]
  ],
  "clearPatterns": [
    { "id": "row_4", "cells": [{ "x": 0, "y": 4 }, { "x": 1, "y": 4 }] }
  ]
}
```

### Required fields
- `id`
- `width`
- `height`
- `activeMask`
- `clearPatterns`

## BlockDefinition

```json
{
  "id": "L_3",
  "cells": [
    { "x": 0, "y": 0 },
    { "x": 0, "y": 1 },
    { "x": 1, "y": 1 }
  ],
  "tags": ["basic"],
  "weight": 10
}
```

### Notes
- `cells` are local offsets.
- Add `rotations` later only if needed.
- Keep blocks definition-only; runtime placement lives elsewhere.

## ItemDefinition

```json
{
  "id": "bomb_3x3",
  "trigger": "onCleared",
  "conditions": [],
  "effects": [
    { "type": "destroyArea", "radius": 1 }
  ],
  "tags": ["explosive"],
  "rarity": "common"
}
```

### Model
- `trigger`: when the item activates
- `conditions`: optional guards
- `effects`: one or more effect descriptors
- `tags`: categorization for content and analytics

## ObstacleDefinition

```json
{
  "id": "ice_lv2",
  "hp": 2,
  "reactsTo": ["clearNearby", "directHit"],
  "onDestroyedEffects": []
}
```

### Notes
- Obstacles may be passive, reactive, or spreading.
- Keep obstacle reactions declarative where possible.

## GoalDefinition

```json
{
  "type": "destroyObstacle",
  "obstacleId": "ice_lv2",
  "target": 6
}
```

Other goal types may include:
- `score`
- `triggerItem`
- `surviveTurns`
- `clearPatternGroup`

## LevelDefinition

```json
{
  "id": "stage_001",
  "boardShapeId": "cross_9",
  "blockPool": ["L_3", "I_4", "T_4"],
  "itemPool": ["bomb_3x3", "rocket_row"],
  "obstaclePool": ["ice_lv2"],
  "itemChance": 0.1,
  "goals": [
    { "type": "score", "target": 3000 }
  ],
  "modifiers": ["comboBonus_v1"]
}
```

## Runtime state sketch

Definitions above should map into runtime state such as:

```json
{
  "boardState": {
    "occupiedCells": [],
    "obstacleStates": [],
    "attachedItems": []
  },
  "turnState": {
    "turnIndex": 0,
    "tray": []
  },
  "scoreState": {
    "score": 0,
    "comboDepth": 0
  }
}
```

## Event examples

Recommended core events:
- `BlockPlaced`
- `ClearPatternsMatched`
- `CellsCleared`
- `ItemTriggered`
- `ObstacleDamaged`
- `ObstacleDestroyed`
- `ScoreApplied`
- `GoalProgressed`
- `TurnEnded`

## Validation rules

Build validators for:
- block cells in bounds of intended authoring rules
- active mask rectangular consistency
- clear pattern cells only targeting active cells
- referenced ids existing in registries
- item chances and weights staying within allowed ranges

## Authoring note

Definitions can be authored as ScriptableObjects or external data, but the repo should preserve a stable runtime schema independent of editor-only convenience.
