# Word Game App

A Flutter-based language learning application featuring word search and anagram games with built-in translations, CEFR-based difficulty levels, and vocabulary tracking.

## 🎯 Features

- **Word Search Gameplay** - Find hidden words in multilingual grids
- **CEFR-Based Difficulty** - Organized by official language proficiency levels (A1-C2)
- **Personal Learning List** - Track vocabulary with spaced repetition support
- **Save/Resume Games** - Pick up where you left off
- **Translation Hints** - Get help in your native language
- **Tag-Based Word Packs** - Flexible organization by topics and levels
- **Multilingual Support** - Play in one language, translate to another

## 🚀 Tech Stack

- **Framework**: Flutter (cross-platform: Android, iOS, Web)
- **State Management**: Riverpod
- **Local Storage**: Hive (NoSQL)
- **Models**: Freezed (immutable data classes)
- **Language**: Dart

## 📚 Documentation

Complete technical documentation is available in the `docs/` folder:

- **[Architecture Overview](docs/architecture.md)** - Full product and technical documentation
- **[Class Diagram](docs/architecture/class-diagram.md)** - Data model and relationships
- **[Setup Guide](docs/setup.md)** - Development environment setup

## 🛠️ Getting Started

### Prerequisites

- Flutter SDK (latest stable)
- Dart SDK (comes with Flutter)
- VS Code or Android Studio
- Chrome (for web testing)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd word-game-app

# Install dependencies
flutter pub get

# Run on web (recommended for development)
flutter run -d chrome

# Or run on Android emulator
flutter run
```

### Development Workflow

1. Start with web browser testing for rapid iteration
2. Use hot reload for instant updates (`r` in terminal)
3. Add Android emulator testing when needed

## 🏗️ Project Structure

```
lib/
├── models/          # Freezed data models
├── repositories/    # Data access layer
├── services/        # Business logic
├── providers/       # Riverpod providers
├── screens/         # UI screens
├── widgets/         # Reusable components
└── utils/           # Helpers and constants

docs/
├── architecture.md           # Complete documentation
├── architecture/
│   └── class-diagram.md     # Data model diagram
└── setup.md                 # Setup instructions
```

## 📖 Key Concepts

### Three Language Systems

The app handles three distinct language concepts:

1. **UI Language** - App interface (buttons, menus, instructions)
2. **Play Language** - Language of words in the game grid (e.g., German)
3. **Translation Language** - Language for hints and translations (e.g., English)

### Tag-Based Organization

Words are organized using a flexible tag system:
- **CEFR Levels**: A1, A2, B1, B2, C1, C2
- **Topics**: animals, food, travel, family, work, etc.
- **Word Types**: noun, verb, adjective, etc.

Users can combine tags to create custom word packs.

## 🎮 Game Mechanics

### Word Search
- Find hidden words in a letter grid
- Words can be horizontal, vertical, or diagonal
- Support for reversed directions
- Efficient placement algorithm
- Translation hints available

### Learning Integration
- Add found words directly to learning list
- Track review history
- Spaced repetition support (future)
- Personal notes on words

## 🗄️ Data Architecture

### Key Design Decisions

1. **Normalized Storage** - GameState stores word IDs, not full objects (90% size reduction)
2. **Tag References** - Words reference tags by ID for flexibility
3. **Soft Deletes** - All entities use `deletedAt` timestamp for sync-ready architecture
4. **Immutable Models** - Freezed ensures type safety and prevents bugs
5. **Repository Pattern** - Clean separation of data access and business logic

### Storage Strategy

- **Hive** for local storage (NoSQL, type-safe, cross-platform)
- All models use Freezed for immutability and code generation
- Offline-first design with sync-ready timestamps

## 🔧 Development

### Code Generation

When you modify Freezed models, run:

```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

### Testing

```bash
# Run all tests
flutter test

# Run specific test
flutter test test/models/word_test.dart
```

## 🚧 Development Phases

### Phase 1: MVP (Current)
- ✅ Word search game mechanics
- ✅ Tag-based word organization
- ✅ Learning list functionality
- ✅ Save/resume games
- ✅ German-English language pair

### Phase 2: Polish & Expand
- Audio pronunciation
- More language pairs (Spanish, French)
- Custom word packs (user-created)
- Statistics dashboard
- Improved UI/UX

### Phase 3: Advanced Features
- Cloud sync (Firebase)
- Premium word packs
- Achievement system
- Anagram game mode
- Daily challenges

## 📝 Contributing

1. Check the [Architecture Documentation](docs/architecture.md)
2. Follow the established patterns (Freezed, Riverpod, Repository)
3. Keep business logic in services, not models
4. Write tests for new features
5. Update documentation as needed

## 🙏 Acknowledgments

- CEFR levels based on Common European Framework of Reference for Languages
- Initial word lists from Goethe Institute and similar sources
- Translations from Dict.cc and community contributions

---

For detailed architecture information, see [docs/architecture.md](docs/architecture.md)
