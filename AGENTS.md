# AGENTS.md

This file provides guidance to coding agents (e.g., Codex CLI) working in this repository. It mirrors the intent and content of `CLAUDE.md`, adapted for any agent.

## Quick Commands

### Build & Run
- Build (Xcode): Cmd+B
- Run (Xcode): Cmd+R
- Clean: Cmd+Shift+K
- Build (CLI): `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -configuration Debug -destination 'platform=macOS' build`

### Tests
- Run all tests (Xcode): Cmd+U
- Run all tests (CLI): `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -destination 'platform=macOS' test`
- Single test: `xcodebuild test -project JoeVoice.xcodeproj -scheme JoeVoice -destination 'platform=macOS' -only-testing:JoeVoiceTests/JoeVoiceTests`

### Dependencies (Required)
- whisper.cpp must be built as an XCFramework and linked before app builds:
  ```bash
  git clone https://github.com/ggerganov/whisper.cpp.git
  cd whisper.cpp
  ./build-xcframework.sh
  ```
- Add `../whisper.cpp/build-apple/whisper.xcframework` to the Xcode project.

## Project Structure
- `JoeVoice/` — main app (Swift/SwiftUI)
  - `Views/` UI
  - `Services/` app logic (AI, audio, import/export)
  - `Models/` data types
  - `Whisper/` transcription integration
  - `Notifications/`, `PowerMode/`, `Assets.xcassets/`
- `JoeVoiceTests/`, `JoeVoiceUITests/` — tests
- `JoeVoice.xcodeproj/` — Xcode project
- See `BUILDING.md` for dependency setup (whisper.cpp XCFramework)

## Architecture Overview

### Entry
- `JoeVoice.swift` — App entry. Uses SwiftData ModelContainer; wires `StateObject` services; includes onboarding and MenuBarExtra.

### Key Services
- Audio: `Recorder.swift`, `WhisperState.swift`, `AudioFileProcessor.swift`, `AudioDeviceManager.swift`
- AI: `AIService.swift`, `AIEnhancementService.swift`, `WhisperState+*` extensions
- UI management: `MenuBarManager.swift`, `HotkeyManager.swift`, `WindowManager.swift`, `MiniRecorderShortcutManager.swift`
- Power Mode: `PowerMode/` with `PowerModeSessionManager.swift`, `BrowserURLService.swift`, `ActiveWindowService.swift`
- Data: SwiftData `Transcription` model, `TranscriptionAutoCleanupService.swift`, `AudioCleanupManager.swift`, `ImportExportService.swift`

### External Integrations
- whisper.cpp (core transcription), FluidAudio/Parakeet (alt), Sparkle (auto-update), KeyboardShortcuts (hotkeys), LaunchAtLogin.

## Coding Style
- Swift API Design Guidelines; Xcode default formatting.
- Indentation 4 spaces; ~120 char soft wrap.
- Types/protocols UpperCamelCase; vars/methods lowerCamelCase; enums/cases UpperCamelCase/lowerCamelCase.
- Prefer `struct`; mark classes `final` when appropriate.
- Keep files focused; views in `Views/`, business logic in `Services/`, data in `Models/`.

## Testing
- Framework: Swift Testing (and/or XCTest in `JoeVoiceTests/`, `JoeVoiceUITests/`).
- Name tests `test...()`; arrange–act–assert.
- Add tests for new logic in `Services/` and `Models/`. Keep UI tests minimal and stable.

## Development Workflow
1. Create an Issue in GitHub for bugs/features.
2. Create a branch from `main` (`fix/...` or `feature/...`).
3. Implement changes with focused commits; follow style and tests.
4. Submit a Pull Request with summary, rationale, test plan, and screenshots/GIFs for UI.
5. Merge after approval; ensure tests pass.

## Release Process (Summary)
1. Update versions in `JoeVoice.xcodeproj/project.pbxproj`:
   - `MARKETING_VERSION` (e.g., `1.0.2`)
   - `CURRENT_PROJECT_VERSION` (e.g., `1020`)
   - Check with: `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -showBuildSettings | grep -E "(MARKETING_VERSION|CURRENT_PROJECT_VERSION)"`
2. Update `appcast.xml` (`title`, `pubDate`, `sparkle:version`, `sparkle:shortVersionString`, `description`, `enclosure`).
3. Build Release: `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -configuration Release clean build`.
4. Package DMG:
   - Copy `JoeVoice.app` from DerivedData Release path to temp dir
   - Create `Applications` symlink; run `hdiutil create ... JoeVoice-X.X.X.dmg`
   - Note exact byte size.
5. Generate Sparkle signature: `./build/SourcePackages/artifacts/sparkle/Sparkle/bin/sign_update JoeVoice-X.X.X.dmg`.
6. Update `appcast.xml` enclosure with file size, signature, download URL.
7. Commit changes and tag: `git tag vX.X.X`; push tag and main.
8. Create GitHub release and upload DMG: `gh release create vX.X.X ...` then `gh release upload vX.X.X JoeVoice-X.X.X.dmg`.
9. Verify release; test auto-update; clean temporary files.

### Important Notes
- Versions: semantic versioning `X.Y.Z`; build numbers strictly increasing per Store/TestFlight.
- Sparkle: signature and file size must match the exact DMG.
- Permissions: app needs microphone and accessibility permissions.
- Testing: verify version displays correctly after build.

## Security & Configuration
- Do not commit API keys; use in-app key UI (`UserDefaults`).
- Ensure `whisper.xcframework` is locally referenced and not vendored.

## Agent Do/Don't
- Do: follow structure, write minimal scoped changes, add/adjust tests for new logic.
- Do: use `rg` to search; read large files in chunks (≤250 lines).
- Do: prefer value semantics; keep code consistent with existing style.
- Don’t: commit secrets, reformat unrelated files, or change filenames/structures unnecessarily.

