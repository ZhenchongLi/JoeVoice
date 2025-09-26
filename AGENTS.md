# Repository Guidelines

## Project Structure & Module Organization
- `JoeVoice/` — main app code (Swift/SwiftUI):
  - `Views/` UI, `Services/` app logic (AI, audio, import/export), `Models/` data types, `Whisper/` transcription integration, `Notifications/`, `PowerMode/`, `Assets.xcassets/`.
- `JoeVoiceTests/` — unit tests (`*Tests.swift`).
- `JoeVoiceUITests/` — UI tests (`*UITests.swift`).
- `JoeVoice.xcodeproj/` — Xcode project.
- See `BUILDING.md` for dependency setup (whisper.cpp XCFramework).

## Build, Test, and Development Commands
- Build (CLI):
  - `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -configuration Debug -destination 'platform=macOS' build`
- Run: Prefer Xcode (Cmd+R) after adding `whisper.xcframework` per `BUILDING.md`.
- Tests (all):
  - `xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -destination 'platform=macOS' test`
- Target a single test:
  - `xcodebuild test -project JoeVoice.xcodeproj -scheme JoeVoice -destination 'platform=macOS' -only-testing:JoeVoiceTests/JoeVoiceTests`

## Coding Style & Naming Conventions
- Swift API Design Guidelines; Xcode default formatting (use Cmd+I).
- Indentation: 4 spaces; 120‑char soft wrap.
- Types/protocols: UpperCamelCase; methods/vars: lowerCamelCase; enums/cases: UpperCamelCase/lowerCamelCase.
- Prefer `struct` over `class` when value semantics suffice; mark `final` where appropriate.
- Keep files focused (one primary type per file). Place views in `Views/`, business logic in `Services/`, data in `Models/`.

## Testing Guidelines
- Framework: XCTest. Name tests `test...()` and keep arrange‑act‑assert structure.
- Place unit tests in `JoeVoiceTests/`, UI flows in `JoeVoiceUITests/`.
- Add tests for new logic in `Services/` and `Models/`. For UI, add minimal, stable assertions.
- Ensure all tests pass before PRs; avoid flaky async tests.

## Commit & Pull Request Guidelines
- Commits: imperative mood, small scoped changes. Example: `Fix Whisper buffer underflow in Recorder`.
- PRs: include summary, rationale, linked issues, test plan, and screenshots/GIFs for UI changes.
- Update docs when changing build steps or user‑visible behavior (`README.md`, `BUILDING.md`).

## Security & Configuration Tips
- Do not commit API keys; use the in‑app key UI (stored in `UserDefaults`).
- Verify `whisper.xcframework` is locally referenced and not vendored in the repo.
