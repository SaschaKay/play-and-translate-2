# Architecture Overview

Product and technical documentation for the Word Game App.

## Product Vision

Build a small, focused Android word game that helps players grow vocabulary through short sessions. The first release is a single-player experience with monetization built in and **no multiplayer**. Monetization should be modest and non-intrusive (e.g., ad-supported with optional ad removal or premium word packs).

## Core Features (Phase 1)

- Word search gameplay (single player)
- CEFR-based difficulty and tag-based word packs
- Learning list for tracked vocabulary
- Save/resume games
- Translation hints (play language vs. translation language)
- Monetization hooks (ads or premium packs)

## Non-Goals (Phase 1)

- Multiplayer or real-time competition
- Social features (friends, chat, leaderboards)
- Cloud sync (offline-first local data)

## Technical Architecture

This project follows clean architecture with a clear separation of concerns:

- **Models**: Immutable data models (Freezed)
- **Repositories**: Data access layer (Hive)
- **Services**: Business logic (game logic, learning list)
- **Providers**: Riverpod state management
- **UI**: Screens and widgets

### Folder Layout

```
lib/
├── models/
├── repositories/
├── services/
├── providers/
├── screens/
├── widgets/
└── utils/
```

## Data Principles

- **Normalized storage**: Store IDs in game state instead of full objects.
- **Soft deletes**: Use `deletedAt` for sync-ready data.
- **Tag-based filtering**: Word packs are composed via tags (CEFR, topic, type).

## Monetization Strategy

- Support a lightweight ad layer for free users.
- Allow premium word packs and/or ad removal via in-app purchase.
- Keep monetization out of core gameplay flow to preserve UX.

## Development Phases

1. **Phase 1 (MVP)**
   - Single-player word search
   - Tag-based packs
   - Learning list
   - Android-first monetization
2. **Phase 2 (Polish)**
   - Additional language pairs
   - Stats and UX improvements
3. **Phase 3 (Advanced)**
   - Optional cloud sync
   - More game modes

## Related Docs

- [Class Diagram](architecture/class-diagram.md)
- [Setup Guide](setup.md)
- [Data Flow](data-flow.md)
- [API Reference](api.md)
