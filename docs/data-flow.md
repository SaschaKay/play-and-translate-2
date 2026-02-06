# Data Flow

High-level overview of how data moves through the app.

## Overview

1. UI triggers user intent (e.g., start game, view pack).
2. Providers request data from services.
3. Services coordinate repositories and domain rules.
4. Repositories read/write models in Hive.
5. Results flow back to UI via Riverpod.

## Example: Start Game

1. Home screen triggers `startGame` action.
2. `GameLogicService` resolves a word pack and generates placements.
3. `GameRepository` persists a new `GameState`.
4. UI watches provider to render the grid.
