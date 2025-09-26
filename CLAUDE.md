# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

### Build and Development
- Build: Open in Xcode and use Cmd+B (Product > Build)
- Run: Open in Xcode and use Cmd+R (Product > Run)
- Test: Run tests in Xcode (Cmd+U) using the Testing framework
- Clean: Cmd+Shift+K (Product > Clean Build Folder)

### Dependencies
- **whisper.cpp**: Must be built and linked as XCFramework before building JoeVoice
  ```bash
  git clone https://github.com/ggerganov/whisper.cpp.git
  cd whisper.cpp
  ./build-xcframework.sh
  ```
- Add `../whisper.cpp/build-apple/whisper.xcframework` to project in Xcode

## Architecture Overview

### Core Application Structure
JoeVoice is a native macOS voice-to-text application built with SwiftUI. The app operates primarily as a menu bar utility with a main window interface.

**Main App Entry Point**: `JoeVoice.swift`
- Uses SwiftData for persistent storage with ModelContainer
- Manages multiple StateObject services for different functionalities
- Supports both onboarding flow and main app interface
- Includes MenuBarExtra for system tray functionality

### Key Services and Managers

**Audio Processing Pipeline**:
- `Recorder.swift` - Core audio recording functionality
- `WhisperState.swift` - Main transcription engine using whisper.cpp
- `AudioFileProcessor.swift` - Handles audio file processing and conversion
- `AudioDeviceManager.swift` - Manages audio input/output devices

**AI and Enhancement Services**:
- `AIService.swift` - Interfaces with external AI services
- `AIEnhancementService.swift` - Handles AI-powered text enhancement and formatting
- `WhisperState+*` extensions - Modular whisper functionality (Parakeet model, UI updates, model management)

**User Interface Management**:
- `MenuBarManager.swift` - Controls menu bar interface and interactions
- `HotkeyManager.swift` - Global keyboard shortcut handling
- `WindowManager.swift` - Window positioning and configuration
- `MiniRecorderShortcutManager.swift` - Quick recording interface

**Power Mode System**:
- Located in `PowerMode/` directory
- `PowerModeSessionManager.swift` - Manages context-aware transcription settings
- `BrowserURLService.swift` - Detects active browser URLs for context
- `ActiveWindowService.swift` - Tracks active applications for smart behavior

**Data Management**:
- Uses SwiftData with `Transcription` model for persistence
- `TranscriptionAutoCleanupService.swift` - Automatic transcript deletion for privacy
- `AudioCleanupManager.swift` - Manages temporary audio file cleanup
- `ImportExportService.swift` - Data import/export functionality

### External Integrations
- **whisper.cpp**: Core transcription engine (must be built separately)
- **FluidAudio/Parakeet**: Alternative transcription model
- **Sparkle**: Auto-update functionality
- **KeyboardShortcuts**: Global hotkey management
- **LaunchAtLogin**: System startup integration

### Testing Structure
- Uses Swift Testing framework (not XCTest)
- Test files: `JoeVoiceTests/`, `JoeVoiceUITests/`
- Tests are currently minimal and need expansion

### Development Requirements
- macOS 14.0+ required for both development and deployment
- Xcode with latest Swift version
- whisper.xcframework must be properly built and linked
- App requires microphone permissions and accessibility permissions for global hotkeys

### Privacy and Security
- 100% offline processing - no data sent to external servers for transcription
- Optional AI enhancement features can use external services
- Automatic cleanup services for maintaining user privacy
- Comprehensive entitlements in `JoeVoice.entitlements`

### Note on Project Structure
The main source code is in the `JoeVoice/` directory, aligned with the repository and target naming.
