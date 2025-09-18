# My First Flutter Sandbox
Beginner-level hands-on Flutter project for playing around testing and breaking things without consequences.

Created on Thursday 18th September 2025.

---

## 🔧 Flutter Version Management (FVM)

This Flutter project uses **[FVM (Flutter Version Management)](https://fvm.app/)** to pin the Flutter and Dart versions, ensuring consistency across all contributors and CI/CD.

### Setup

Install the pinned Flutter version:

```bash
fvm install
```

This will install the version defined in `.fvmrc` and create a local symlink under `.fvm/`.

### Usage

Run all Flutter/Dart commands through FVM to ensure consistency:

```bash
fvm flutter pub get
fvm flutter run
fvm flutter test
fvm dart format .
fvm dart run tool/some_script.dart
```

In IDEs (VS Code, Android Studio, IntelliJ), point the Flutter SDK path to:

```
<project>/.fvm/flutter_sdk
```

After this, IDE buttons like Pub get, Run, and Test will use the FVM-managed SDK.

### CI/CD

Example GitHub Actions workflow:

```yaml
name: Flutter CI

on: [push, pull_request]

jobs:
  build:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/cache@v4
        with:
          path: ~/.fvm
          key: fvm-${{ runner.os }}-${{ hashFiles('**/.fvm/fvm_config.json') }}

      - name: Install FVM
        run: |
          brew tap leoafarias/fvm
          brew install fvm

      - name: Setup Flutter
        run: |
          fvm install
          fvm flutter --version

      - name: Get dependencies & run tests
        run: |
          fvm flutter pub get
          fvm flutter test
```

### Verification

To confirm you’re using the right SDK:

```bash
fvm flutter doctor
```

The SDK path should point inside your `~/.fvm/versions/` directory.

## Version Control
✅ Commit `.fvmrc` (the source of truth for the Flutter version).

🚫 Ignore `.fvm/` (local SDK files, caches, and symlinks).

⚠️ Note for legacy projects (FVM v2 and below):
Older versions used `.fvm/fvm_config.json` instead of `.fvmrc.` If you are maintaining such a project, commit `fvm_config.json` instead, and add an exception rule to your `.gitignore` (see below).

---

### 📄 `.gitignore` (modern, FVM v3+)

```gitignore
# Flutter / Dart
.dart_tool/
.packages
.pub-cache/
.pub/
build/
coverage/
lib/generated_plugin_registrant.dart

# IDE & system
.idea/
.vscode/
*.iml
*.ipr
*.iws
*.swp
*.swo
.DS_Store
*.lock
*.log

# Flutter deps
.melos_tool/
.flutter-plugins
.flutter-plugins-dependencies

# Firebase (optional)
# google-services.json
# GoogleService-Info.plist

# FVM (Flutter Version Management)
.fvm/        # ignore all local SDKs/caches
# keep .fvmrc under version control
```

### 📄 `.gitignore` (legacy FVM v2 projects)

```gitignore
# FVM (legacy v2)
.fvm/
!.fvm/fvm_config.json   # keep config, ignore everything else
```
