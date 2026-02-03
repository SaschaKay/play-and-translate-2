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
