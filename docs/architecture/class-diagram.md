# Class Diagram

This diagram shows the complete data model and relationships for the Word Game app.

## Overview

The architecture follows clean architecture principles with:
- **Models**: Immutable data classes (using freezed)
- **Repositories**: Data access layer
- **Services**: Business logic layer
- **Providers**: State management (Riverpod)

## Full Class Diagram

```mermaid
classDiagram
    %% Core Word System
    class Word {
        +String id
        +Map~String,String~ translations
        +Map~String,String?~ audioUrls
        +List~String~ tagIds
        +String? creatorId
        +WordVisibility visibility
        +DateTime? deletedAt
        +DateTime createdAt
        +DateTime updatedAt
        +bool isSystemWord()
        +bool isUserWord()
        +bool isDeleted()
    }

    class WordVisibility {
        <<enumeration>>
        public
        private
        unlisted
    }

    %% Tag System
    class Tag {
        +String id
        +String category
        +String value
        +Map~String,String~ translations
        +String? creatorId
        +DateTime? deletedAt
        +DateTime createdAt
        +DateTime updatedAt
        +String getDisplayName(language)
        +bool isSystemTag()
    }

    class TagCategories {
        <<static>>
        +String cefr
        +String topic
        +String wordType
        +String custom
    }

    class Languages {
        <<static>>
        +String german
        +String english
        +String spanish
        +List~String~ supported
        +bool isValid(code)
    }

    %% Word Pack System
    class WordPack {
        +String id
        +Map~String,String~ names
        +Map~String,String?~ descriptions
        +List~String~ includedTagIds
        +List~String~ optionalTagIds
        +List~String~ excludedTagIds
        +List~String~ availablePlayLanguages
        +List~String~ availableTranslationLanguages
        +String? creatorId
        +bool isPublic
        +DateTime? deletedAt
        +DateTime createdAt
        +DateTime updatedAt
        +int? wordCount
    }

    %% Game System
    class GameState {
        +String id
        +List~String~ wordIds
        +Grid grid
        +Map~String,WordPlacement~ wordPlacements
        +String playLanguage
        +String translationLanguage
        +List~String~ foundWordIds
        +int hintsUsed
        +DateTime? startTime
        +Duration pausedDuration
        +GameSettings settings
        +bool isCompleted
        +DateTime createdAt
        +DateTime lastPlayedAt
        +bool isWordFound(wordId)
        +void markWordFound(wordId)
    }

    class GameSettings {
        +String name
        +GridSize gridSize
        +bool showTranslationHint
        +bool highlightFoundWords
        +String wordPackId
    }

    class WordPlacement {
        +String wordId
        +Position start
        +int length
        +Direction direction
        +List~Position~ getPositions()
    }

    class Grid {
        +List~List~String~~ cells
        +int rows
        +int cols
        +String getLetterAt(row, col)
        +bool isValidPosition(row, col)
    }

    class Position {
        +int row
        +int col
    }

    class GridSize {
        <<enumeration>>
        small_10x10
        medium_15x15
        large_20x20
    }

    class Direction {
        <<enumeration>>
        horizontal
        vertical
        diagonalDown
        diagonalUp
        horizontalReverse
        verticalReverse
        diagonalDownReverse
        diagonalUpReverse
    }

    %% Learning List System
    class LearningList {
        +String userId
        +Map~String,LearningWord~ words
        +String primaryLanguage
        +String translationLanguage
        +DateTime createdAt
        +DateTime updatedAt
        +void addWord(wordId)
        +void removeWord(wordId)
    }

    class LearningWord {
        +String wordId
        +DateTime addedAt
        +DateTime updatedAt
        +DateTime? lastReviewedAt
        +DateTime? deletedAt
        +int timesReviewed
        +String? personalNote
        +DateTime? nextReviewDate
        +int repetitionLevel
        +void markReviewed()
    }

    %% App Settings
    class AppSettings {
        +String uiLanguage
        +String defaultPlayLanguage
        +String defaultTranslationLanguage
        +bool darkMode
        +bool soundEffects
        +bool showTranslationHints
        +DateTime updatedAt
    }

    %% Service Layer (not stored, runtime only)
    class WordRepository {
        <<service>>
        +Future~Word?~ getById(id)
        +Future~List~Word~~ getByIds(ids)
        +Future~List~Word~~ getByTagIds(tagIds)
        +Future~void~ save(word)
    }

    class LearningListService {
        <<service>>
        +List~Word~ getWordsByTag(list, tagId)
        +List~Word~ getWordsNeedingReview(list)
    }

    %% Relationships - Word System
    Word --> WordVisibility : has
    Word "0..*" --> "0..*" Tag : references via tagIds
    Tag --> TagCategories : categorized by
    Word --> Languages : uses codes
    Tag --> Languages : uses codes

    %% Relationships - Word Pack
    WordPack "0..*" --> "0..*" Tag : filters via tagIds
    WordPack --> Languages : uses codes

    %% Relationships - Game
    GameState "1" --> "*" Word : references via wordIds
    GameState "1" --> "1" Grid : has
    GameState "1" --> "*" WordPlacement : has
    GameState "1" --> "1" GameSettings : configured by
    WordPlacement --> Position : has start
    WordPlacement --> Direction : has
    GameSettings --> GridSize : uses
    Grid "1" --> "*" Position : contains cells at

    %% Relationships - Learning List
    LearningList "1" --> "*" LearningWord : contains
    LearningWord --> Word : references via wordId

    %% Relationships - Services
    WordRepository ..> Word : manages
    LearningListService ..> LearningList : operates on
    LearningListService ..> Word : fetches
    GameState ..> WordRepository : fetches words via

    %% Cross-System Relationships
    GameSettings --> WordPack : references via wordPackId
    GameState --> AppSettings : respects defaults
    GameState --> Languages : uses codes
```

## Key Design Principles

### 1. Normalized Data Storage
- **GameState** stores word IDs, not full Word objects
- Reduces storage size and enables word updates without breaking saved games
- Full words fetched on-demand via WordRepository

### 2. Tag-Based Organization
- Words and WordPacks reference tags by ID string (`'cefr:A1'`, `'topic:animals'`)
- Tags have multilingual translations for UI display
- Flexible filtering: combine tags with AND/OR logic

### 3. Soft Delete Pattern
- All entities use `deletedAt` timestamp instead of hard delete
- Enables sync, undo functionality, and data recovery
- Queries filter out deleted items automatically

### 4. Multi-Language Support
Three distinct language concepts:
- **UI Language**: Interface language (buttons, menus)
- **Play Language**: Language of words in game grid
- **Translation Language**: Language for hints/translations

### 5. Immutability with Freezed
- All models use `@freezed` annotation
- Generated `copyWith`, `==`, `hashCode`, `toString`
- Type-safe JSON serialization
- Works seamlessly with Hive storage

### 6. Service Layer Separation
- **Models**: Pure data (no business logic)
- **Repositories**: Data access and queries
- **Services**: Cross-aggregate operations and business logic

## Core Entities

### Word
- Multilingual translations and audio
- Tagged with CEFR level, topic, word type
- Can be system-provided or user-created
- Supports soft delete

### Tag
- Category-value structure (e.g., `cefr:A1`)
- Multilingual display names
- Immutable ID for referencing

### WordPack
- Tag-based filtering (include/exclude/optional)
- Multilingual names and descriptions
- Specifies available play and translation languages
- Cached word count for performance

### GameState
- References words by ID only
- Efficient WordPlacement (start + direction + length)
- Full save/resume capability
- Language-specific configuration

### LearningList
- Language-pair specific (German→English separate from Spanish→English)
- Tracks review history and spaced repetition data
- Sync-ready with timestamps and tombstones

## Storage Strategy

All data stored in **Hive** (NoSQL):
- Type-safe with generated adapters
- Fast queries for tag-based filtering
- Works seamlessly with freezed models
- Cross-platform (Android, iOS, Web)

