# Documentation

Complete technical documentation for the Word Game App.

## 📚 Documentation Index

### Architecture & Design
- **[Architecture Overview](architecture.md)** - Complete product and technical documentation including:
  - Product vision and features
  - Data models and relationships
  - Technical stack and justifications
  - Development phases
  - Implementation guidelines
- **[Data Flow](data-flow.md)** - High-level request and state flow
- **[API Reference](api.md)** - Planned repository and service APIs

- **[Class Diagram](architecture/class-diagram.md)** - Visual representation of data models
  - Core entities (Word, Tag, WordPack)
  - Game system (GameState, Grid, WordPlacement)
  - Learning system (LearningList, LearningWord)
  - Service layer (Repositories, Services)
  - *Note: View with Mermaid extension in VS Code*

### Setup & Development
- **[Setup Guide](setup.md)** - Development environment setup
  - Prerequisites and installation
  - Flutter setup
  - IDE configuration (VS Code, Android Studio)
  - Running the app
  - Code generation

## 🎯 Quick Links

### For New Developers
1. Start with [Architecture Overview](architecture.md) - Product section
2. Review [Class Diagram](architecture/class-diagram.md) for data model
3. Follow [Setup Guide](setup.md) to get running
4. Read Architecture Overview - Technical section for implementation details

### For Product Managers
- [Architecture Overview](architecture.md) - Product Overview section
- Feature roadmap in Architecture doc - Development Phases section

### For Architects/Tech Leads
- [Architecture Overview](architecture.md) - Technical Architecture section
- [Class Diagram](architecture/class-diagram.md)
- Design decisions and justifications in Architecture doc

## 📖 Key Concepts

### Data Model
- **Word** - Multilingual vocabulary with tags
- **Tag** - Flexible categorization (CEFR, topics, types)
- **WordPack** - Tag-based filtered collections
- **GameState** - Complete game state (save/resume capable)
- **LearningList** - Personal vocabulary tracker

### Architecture Patterns
- **Clean Architecture** - Models, Repositories, Services, Providers
- **Repository Pattern** - Data access abstraction
- **Immutable Models** - Freezed for type safety
- **Offline-First** - Sync-ready design with Hive

### Language System
Three distinct language concepts:
- **UI Language** - App interface
- **Play Language** - Words in game
- **Translation Language** - Hints and translations

## 🛠️ Technical Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Framework | Flutter | Cross-platform UI |
| Language | Dart | Application code |
| State Management | Riverpod | Reactive state |
| Local Storage | Hive | NoSQL database |
| Models | Freezed | Immutable data classes |
| Code Generation | build_runner | Generate boilerplate |

## 📝 Documentation Standards

### When to Update Docs

Update documentation when:
- Adding new features
- Changing data models
- Modifying architecture patterns
- Making breaking changes
- Adding new concepts

### Documentation Files

| File | Purpose | Update When |
|------|---------|------------|
| `architecture.md` | Product & technical overview | Major changes |
| `architecture/class-diagram.md` | Data model visualization | Model changes |
| `setup.md` | Development setup | Tool/dependency changes |
| `../README.md` | Project overview | Feature additions |

### Viewing Diagrams

The class diagram uses Mermaid syntax. To view:
1. Install "Mermaid Editor" extension in VS Code
2. Open `architecture/class-diagram.md`
3. Use the Mermaid preview feature

## 🚀 Development Workflow

1. **Read** architecture documentation
2. **Review** class diagram for data relationships
3. **Follow** setup guide
4. **Implement** following established patterns
5. **Test** with `flutter test`
6. **Update** docs if adding new concepts

## 📚 Additional Resources

### Flutter Resources
- [Flutter Documentation](https://flutter.dev/docs)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)

### Package Documentation
- [Riverpod](https://riverpod.dev/)
- [Hive](https://docs.hivedb.dev/)
- [Freezed](https://pub.dev/packages/freezed)

### Learning Resources
- [Flutter Architecture Samples](https://github.com/brianegan/flutter_architecture_samples)
- [CEFR Guidelines](https://www.coe.int/en/web/common-european-framework-reference-languages)

## 🤝 Contributing to Documentation

When contributing documentation:
1. Keep it clear and concise
2. Use examples where helpful
3. Update diagrams when models change
4. Cross-reference related docs
5. Maintain consistent formatting
