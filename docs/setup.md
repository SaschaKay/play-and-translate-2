# Setup Guide

Complete guide to setting up the Word Game App development environment.

## Prerequisites

### Required Software

1. **Flutter SDK** (latest stable channel)
   - Download: https://flutter.dev/docs/get-started/install
   - Verify: `flutter --version`

2. **Dart SDK** (included with Flutter)
   - Verify: `dart --version`

3. **Git**
   - Download: https://git-scm.com/
   - Verify: `git --version`

4. **Chrome** (for web development)
   - Recommended for rapid iteration
   - Download: https://www.google.com/chrome/

### Recommended Software

1. **VS Code** or **Android Studio**
   - VS Code: https://code.visualstudio.com/
   - Android Studio: https://developer.android.com/studio

2. **Android SDK** (for Android testing)
   - Included with Android Studio
   - Or install via Android Studio's SDK Manager

3. **Xcode** (for iOS testing - Mac only)
   - Download from Mac App Store

## Installation Steps

### 1. Install Flutter

**macOS (using Homebrew):**
```bash
brew install flutter
```

**Or download and extract manually:**
```bash
# Download Flutter from flutter.dev
cd ~/development
unzip ~/Downloads/flutter_[version].zip

# Add to PATH (add to ~/.zshrc or ~/.bashrc)
export PATH="$PATH:$HOME/development/flutter/bin"
```

**Windows:**
1. Download Flutter SDK from flutter.dev
2. Extract to `C:\src\flutter`
3. Add `C:\src\flutter\bin` to PATH

**Verify installation:**
```bash
flutter doctor
```

### 2. Install IDE

**VS Code (Recommended for this project):**

1. Download and install VS Code
2. Install Flutter extension:
   - Open Extensions (Ctrl+Shift+X)
   - Search "Flutter"
   - Install "Flutter" by Dart Code

3. Install additional extensions:
   - **Dart** (included with Flutter extension)
   - **Mermaid Editor** (for viewing diagrams)
   - **Error Lens** (helpful for debugging)

**Android Studio:**

1. Download and install Android Studio
2. Install Flutter and Dart plugins:
   - File → Settings → Plugins
   - Search "Flutter" and install
   - Restart Android Studio

### 3. Clone Repository

```bash
# Clone the repository
git clone <repository-url>
cd word-game-app

# Verify Flutter is working
flutter doctor
```

### 4. Install Dependencies

```bash
# Install Dart packages
flutter pub get

# Generate Freezed models (if already created)
flutter pub run build_runner build --delete-conflicting-outputs
```

## Running the App

### Web (Recommended for Development)

**Why start with web:**
- ✅ Fastest hot reload
- ✅ No emulator needed
- ✅ Easy debugging with Chrome DevTools
- ✅ Perfect for UI development

**Run in Chrome:**
```bash
flutter run -d chrome
```

**Run with hot reload:**
```bash
flutter run -d chrome --hot
# Press 'r' to hot reload
# Press 'R' to hot restart
# Press 'q' to quit
```

### Android Emulator

**Setup:**
1. Open Android Studio
2. Tools → AVD Manager
3. Create Virtual Device
4. Select device (Pixel 5 recommended)
5. Select system image (latest stable Android)
6. Finish and start emulator

**Run app:**
```bash
# List available devices
flutter devices

# Run on Android
flutter run -d <device-id>
# Or simply:
flutter run
# (will prompt to select device)
```

### iOS Simulator (Mac only)

**Setup:**
1. Install Xcode from App Store
2. Open Xcode → Preferences → Locations
3. Set Command Line Tools to Xcode version

**Run app:**
```bash
# Open iOS Simulator
open -a Simulator

# Run app
flutter run -d <simulator-id>
```

### Physical Device

**Android:**
1. Enable Developer Options on device
2. Enable USB Debugging
3. Connect via USB
4. Run `flutter devices` to verify
5. Run `flutter run`

**iOS:**
1. Connect iPhone via USB
2. Trust computer on device
3. Open Xcode and add Apple ID
4. Run from Xcode first time
5. Then can use `flutter run`

## Development Workflow

### Recommended Setup for This Project

1. **Primary**: Web browser (Chrome)
   ```bash
   flutter run -d chrome
   ```

2. **Testing**: Android emulator (when needed)
   ```bash
   flutter run
   ```

3. **Final testing**: Physical device before release

### Hot Reload vs Hot Restart

**Hot Reload (r):**
- Fast (~1 second)
- Preserves app state
- Updates UI changes
- Updates most code changes

**Hot Restart (R):**
- Slower (~3-5 seconds)
- Resets app state
- Updates everything
- Use when hot reload doesn't work

**Full Restart (q then flutter run):**
- Slowest
- Complete rebuild
- Use for structural changes
- Use after adding dependencies

## Code Generation

This project uses Freezed for immutable models and build_runner for code generation.

### When to Run Code Generation

Run after:
- Creating new Freezed models
- Modifying existing Freezed models
- Adding new fields to models
- Changing model structure

### Commands

**Build once:**
```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

**Watch mode (automatic rebuilds):**
```bash
flutter pub run build_runner watch --delete-conflicting-outputs
```

**Clean and rebuild:**
```bash
flutter pub run build_runner clean
flutter pub run build_runner build --delete-conflicting-outputs
```

### Generated Files

For each model (e.g., `word.dart`), generates:
- `word.freezed.dart` - Freezed code (copyWith, ==, hashCode)
- `word.g.dart` - JSON serialization
- `word.hive.dart` - Hive type adapter (if using Hive annotations)

**Never edit generated files** - they're regenerated on each build.

## Project Structure

```
word-game-app/
├── lib/
│   ├── models/              # Freezed data models
│   │   ├── word.dart
│   │   ├── tag.dart
│   │   ├── word_pack.dart
│   │   ├── game_state.dart
│   │   └── learning_list.dart
│   ├── repositories/        # Data access layer
│   │   ├── word_repository.dart
│   │   └── game_repository.dart
│   ├── services/            # Business logic
│   │   ├── game_logic_service.dart
│   │   └── learning_list_service.dart
│   ├── providers/           # Riverpod providers
│   │   ├── word_providers.dart
│   │   └── game_providers.dart
│   ├── screens/             # UI screens
│   │   ├── home_screen.dart
│   │   ├── game_screen.dart
│   │   └── learning_list_screen.dart
│   ├── widgets/             # Reusable components
│   │   ├── word_grid.dart
│   │   └── word_list.dart
│   ├── utils/               # Helpers and constants
│   │   ├── languages.dart
│   │   └── tag_categories.dart
│   └── main.dart            # App entry point
├── test/                    # Tests
├── docs/                    # Documentation
├── pubspec.yaml             # Dependencies
└── README.md                # Project overview
```

## Key Dependencies

### Core Dependencies
```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State management
  flutter_riverpod: ^2.4.0
  riverpod_annotation: ^2.3.0
  
  # Local storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  
  # Immutable models
  freezed_annotation: ^2.4.1
  json_annotation: ^4.8.1
  
  # UI components
  lucide_icons: ^0.1.0

dev_dependencies:
  # Build tools
  build_runner: ^2.4.6
  freezed: ^2.4.5
  json_serializable: ^6.7.1
  hive_generator: ^2.0.1
  riverpod_generator: ^2.3.0
  
  # Testing
  flutter_test:
    sdk: flutter
  mockito: ^5.4.2
```

### Adding Dependencies

```bash
# Add a package
flutter pub add package_name

# Add a dev dependency
flutter pub add --dev package_name

# Update all packages
flutter pub upgrade
```

## Common Issues & Solutions

### Issue: "Waiting for another flutter command to release the startup lock"

**Solution:**
```bash
# Kill all Flutter processes
killall -9 dart
# Or delete lock file
rm ~/development/flutter/bin/cache/lockfile
```

### Issue: "CocoaPods not installed" (iOS/Mac)

**Solution:**
```bash
sudo gem install cocoapods
pod setup
```

### Issue: Build runner fails

**Solution:**
```bash
# Clean build cache
flutter clean
flutter pub get

# Delete conflicting outputs and rebuild
flutter pub run build_runner build --delete-conflicting-outputs
```

### Issue: Hive box already open

**Solution:**
```bash
# Hot restart the app (R in terminal)
# Or close and reopen all boxes properly in code
```

### Issue: "Unable to find bundled Java version"

**Solution:**
```bash
# Set JAVA_HOME (macOS)
export JAVA_HOME=/Applications/Android\ Studio.app/Contents/jre/Contents/Home

# Or in Android Studio: Preferences → Build Tools → Gradle
# Set Gradle JDK to bundled JDK
```

## Testing

### Run All Tests
```bash
flutter test
```

### Run Specific Test File
```bash
flutter test test/models/word_test.dart
```

### Run Tests with Coverage
```bash
flutter test --coverage
```

### Widget Testing in Chrome
```bash
flutter run -d chrome test/widget_test.dart
```

## Performance Tips

### Development Mode
- Use web for rapid UI iteration
- Use hot reload (not restart) when possible
- Enable DevTools for performance profiling

### Production Build
```bash
# Android release build
flutter build apk --release

# iOS release build
flutter build ios --release

# Web release build
flutter build web --release
```

## IDE Configuration

### VS Code Settings

Create `.vscode/settings.json`:
```json
{
  "dart.lineLength": 100,
  "editor.formatOnSave": true,
  "editor.rulers": [80, 100],
  "[dart]": {
    "editor.defaultFormatter": "Dart-Code.dart-code",
    "editor.formatOnSave": true
  }
}
```

### VS Code Tasks

Create `.vscode/tasks.json` for common commands:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Build Runner - Build",
      "type": "shell",
      "command": "flutter pub run build_runner build --delete-conflicting-outputs",
      "group": "build"
    },
    {
      "label": "Build Runner - Watch",
      "type": "shell",
      "command": "flutter pub run build_runner watch --delete-conflicting-outputs",
      "isBackground": true,
      "group": "build"
    }
  ]
}
```

### Keyboard Shortcuts

Useful VS Code shortcuts:
- `Ctrl+Shift+P` - Command palette
- `Ctrl+Space` - Autocomplete
- `F12` - Go to definition
- `Shift+F12` - Find all references
- `Ctrl+.` - Quick fix
- `Ctrl+Shift+R` - Refactor

## Next Steps

1. ✅ Complete setup following this guide
2. ✅ Run `flutter doctor` - ensure no issues
3. ✅ Run app on web: `flutter run -d chrome`
4. ✅ Read [Architecture Documentation](architecture.md)
5. ✅ Review [Class Diagram](architecture/class-diagram.md)
6. ✅ Start coding!

## Getting Help

### Resources
- [Flutter Documentation](https://flutter.dev/docs)
- [Riverpod Documentation](https://riverpod.dev/)
- [Freezed Documentation](https://pub.dev/packages/freezed)
- [Hive Documentation](https://docs.hivedb.dev/)

### Troubleshooting
- Run `flutter doctor -v` for detailed info
- Check Flutter GitHub issues
- Search Stack Overflow
- Ask in Flutter Discord/Reddit
