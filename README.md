# My First Flutter Sandbox
Beginner-level hands-on Flutter project for playing around testing and breaking things without consequences.

Created on Thursday 18th September 2025.

---

## 🔧 Flutter Version Management (FVM)

The Flutter projects in this repo use **[FVM (Flutter Version Management)](https://fvm.app/)** to pin the Flutter and Dart versions, ensuring consistency across all contributors and CI/CD.

### Setup

Option 1 - New or existing Flutter project without FVM:

```bash
mkdir your-flutter-project && cd your-flutter-project
fvm use <version>  # Writes/updates .fvmrc, sets up .fvm/ directory, downloads version (if needed) and creates a local symlink under `.fvm/`
```

Option 2 - Clone existing project already using FVM:

```bash
git clone <repository>
cd <project-directory>  # .fvmrc already exists from the repo
fvm install  # Installs the version specified in .fvmrc
```

### Usage

Key FVM (v3+) commands:

```bash
# Information commands:
fvm releases  # List all available Flutter releases
fvm list  # List versions installed locally
fvm which  # Show current Flutter SDK path

# Project-specific Flutter version management:
fvm use stable  # Set project to use stable
fvm use beta  # Set project to use beta  
fvm use 3.22.0  # Set project to use specific version

# Global Flutter version management (affects all projects without .fvmrc):
fvm global stable
fvm global beta
fvm global 3.22.0

# Cleanup commands:
rm -rf .fvm .fvmrc  # Remove FVM from current project; keep existing .gitignore
fvm cleanup  # Remove unused Flutter versions globally
fvm remove  # Remove a specific installed Flutter version globally
fvm remove --all  # Removes ALL installed Flutter versions globally
```

Always run Flutter/Dart commands through FVM to ensure the project uses the pinned version:

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

After this, IDE buttons like `Pub get`, `Run`, and `Test` will automatically use the FVM-managed SDK.

### Verification

To confirm you’re using the right SDK:

```bash
fvm flutter --version
fvm flutter doctor
```

The SDK path should point inside your `~/.fvm/versions/` directory.

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

### 📄 `.gitignore` (modern, FVM v3+)

✅ Commit `.fvmrc` (the source of truth for the Flutter version).

🚫 Ignore `.fvm/` (local SDK files, caches, and symlinks).

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

⚠️ Note for legacy projects (FVM v2 and below):

Older versions used `.fvm/fvm_config.json` instead of `.fvmrc.` If you are maintaining such a project, commit `fvm_config.json` instead, and add an exception rule to your `.gitignore` (see below).

```gitignore
# FVM (legacy v2)
.fvm/
!.fvm/fvm_config.json   # keep config, ignore everything else
```
