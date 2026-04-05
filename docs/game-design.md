# Game Design

## Overview

Cube Crush is a block placement puzzle game for mobile.
Players place blocks onto a board, complete clear patterns, trigger item effects, and solve level goals.

The game should feel easy to enter, but offer strategy through:
- item timing
- obstacle management
- chain reactions
- board-specific routing and planning

## Core loop

1. Present the player with a set of candidate blocks.
2. Player places one block on the board.
3. Evaluate completed clear patterns.
4. Clear matched cells.
5. Trigger attached items and obstacle reactions.
6. Apply score, combo, and goal progress.
7. Refresh the tray/turn state.
8. Continue until win or fail condition.

## Design pillars

### 1. Readable
The player should understand why something happened.
Effects need clear ordering and strong feedback.

### 2. Strategic
Good play should involve planning for future shapes, item timing, and chain setup.

### 3. Expandable
New boards, items, obstacles, and goals must layer onto the same engine.

### 4. Mobile-friendly
Short sessions, low input friction, and strong moment-to-moment satisfaction.

## Board shapes

The engine must support multiple board shapes, including:
- rectangle
- cross
- hash (`#`-like)
- future custom masks

Board shape affects:
- valid placement cells
- clear pattern definitions
- difficulty tuning
- suitable block pools

## Blocks

Blocks are shape definitions composed of occupied local cells.

Initial MVP target:
- line
- square
- L
- T
- zigzag-like variant

Potential future expansion:
- larger shapes
- rare special shapes
- mode-specific shape pools

## Items

Some spawned blocks or occupied cells may carry an attached item.
The item activates when its trigger conditions are met, most commonly when the cell is cleared.

### Item categories
- **board control**: destroy area, clear row, clear column
- **economy/score**: score multiplier, combo boost
- **flow control**: reroll next tray, reserve block, emergency save
- **obstacle control**: direct damage, chain unlock, spread suppression

### Initial MVP item candidates
- bomb: clear nearby cells
- rocket row: clear a horizontal pattern line
- rocket column: clear a vertical pattern line
- reroll: replace remaining tray choices
- double score: multiply current clear score

## Obstacles

Obstacles add texture and level-specific tension.
They occupy cells or modify cell behavior.

Examples:
- ice: requires multiple hits to remove
- chain: locks a cell until adjacent clears happen
- stone: only removed by specific effects
- slime: spreads after turns if ignored

## Goals

Levels may define one or more goals, such as:
- reach target score
- clear N obstacles of a type
- trigger items N times
- survive N turns
- clear specific pattern groups

## Failure states

Potential fail conditions:
- no legal placement remains
- turn limit exceeded
- timer expired in future timed modes

## Scoring

Base scoring should reward:
- number of cells cleared
- number of patterns cleared simultaneously
- chain depth
- obstacle clears
- goal-specific milestones if desired

Avoid overly opaque score formulas in early versions.

## Combo and chain behavior

A combo is a meaningful multi-step resolution sequence inside one turn.
Items and obstacles can extend the chain.

Requirements:
- event ordering must be deterministic
- chain depth must be trackable
- UI should clearly show continuation vs final resolution

## Content strategy

The game should be designed as a platform for content growth.
Expansion axes:
- new board masks
- new item triggers/effects
- new obstacle behaviors
- new goals
- new modifiers
- themed seasons/events

## MVP scope

A first playable target should include:
- 2 board shapes
- 5 or more block shapes
- 3 item types
- 2 obstacle types
- 10 levels
- score and obstacle-clear goals
- debug spawn tools for development

## Out of scope for first playable

Not required initially:
- meta economy
- live events backend
- social systems
- PvP
- infinite mode balancing beyond smoke testing
