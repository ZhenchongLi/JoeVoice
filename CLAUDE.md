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

## Development Workflow

When encountering issues or implementing new features, follow this development workflow:

1. **Create an Issue**: Document the problem or feature request in GitHub Issues
2. **Create a Branch**: Create a fix or feature branch from main
   ```bash
   git checkout -b fix/issue-name
   # or
   git checkout -b feature/feature-name
   ```
3. **Submit Pull Request**: Submit code changes through a pull request for review
4. **Merge to Main**: After approval, merge the pull request to main branch
5. **Release**: Create and publish a release with the updated code

## Release Process

Follow this comprehensive process to create and publish a new version of JoeVoice:

### 1. Version Preparation

#### Update Version Numbers
```bash
# Update version in Xcode project
# Edit JoeVoice.xcodeproj/project.pbxproj
# Change MARKETING_VERSION (e.g., 1.0.2)
# Change CURRENT_PROJECT_VERSION (e.g., 1020 for version 1.0.2)

# Alternative: Use xcodebuild to check current versions
xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -showBuildSettings | grep -E "(MARKETING_VERSION|CURRENT_PROJECT_VERSION)"
```

#### Update appcast.xml
```bash
# Update the following in appcast.xml:
# - <title>: New version number
# - <pubDate>: Current date in RFC 2822 format
# - <sparkle:version>: Build number (e.g., 1020)
# - <sparkle:shortVersionString>: Version string (e.g., 1.0.2)
# - <description>: Release notes
# - <enclosure>: URL, length, and sparkle:edSignature (to be updated later)
```

### 2. Build and Package

#### Prerequisites Check
```bash
# Ensure whisper.cpp is built
ls -la ../whisper.cpp/build-apple/whisper.xcframework
```

#### Build Release Version
```bash
# Clean and build release version
xcodebuild -project JoeVoice.xcodeproj -scheme JoeVoice -configuration Release clean build
```

#### Create DMG Package
```bash
# Create temporary directory
mkdir -p /tmp/claude/dmg-temp

# Copy built app
cp -R "/Users/lizc/Library/Developer/Xcode/DerivedData/JoeVoice-*/Build/Products/Release/JoeVoice.app" /tmp/claude/dmg-temp/

# Create Applications symlink
ln -s /Applications /tmp/claude/dmg-temp/Applications

# Create DMG
hdiutil create -volname "JoeVoice" -srcfolder /tmp/claude/dmg-temp -ov -format UDZO JoeVoice-X.X.X.dmg

# Get file size
ls -la JoeVoice-X.X.X.dmg
```

#### Generate Sparkle Signature
```bash
# Generate EdDSA signature for auto-updates
./build/SourcePackages/artifacts/sparkle/Sparkle/bin/sign_update JoeVoice-X.X.X.dmg

# Output format: sparkle:edSignature="..." length="..."
```

### 3. Update Release Configuration

#### Update appcast.xml with DMG Details
```bash
# Update the enclosure tag with:
# - Correct file size (length attribute)
# - New EdDSA signature (sparkle:edSignature attribute)
# - Correct download URL
```

#### Commit Version Changes
```bash
git add JoeVoice.xcodeproj/project.pbxproj appcast.xml
git commit -m "release: bump version to X.X.X

- Update app version to X.X.X (build XXXX)
- Update appcast.xml with new version information
- Add release notes for version X.X.X

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

### 4. Create GitHub Release

#### Create Git Tag and Push
```bash
# Create and push tag
git tag vX.X.X
git push origin main
git push origin vX.X.X
```

#### Create GitHub Release
```bash
# Create release with notes
gh release create vX.X.X --title "JoeVoice vX.X.X" --notes "$(cat <<'EOF'
## What's New in Version X.X.X

### Features & Improvements
- List of new features
- List of improvements

### Bug Fixes
- List of bug fixes

### Technical Updates
- Technical changes
- Development improvements

**Full Changelog**: https://github.com/ZhenchongLi/JoeVoice/compare/vX.X.X-1...vX.X.X
EOF
)"
```

#### Upload DMG to Release
```bash
# Upload DMG file to GitHub release
gh release upload vX.X.X JoeVoice-X.X.X.dmg
```

### 5. Verification and Cleanup

#### Verify Release
```bash
# Check release details
gh release view vX.X.X

# Verify DMG download URL matches appcast.xml
# Test auto-update functionality if possible
```

#### Clean Up
```bash
# Remove temporary files
rm JoeVoice-X.X.X.dmg
rm -rf /tmp/claude/dmg-temp
```

### Important Notes

- **Version Numbering**: Use semantic versioning (X.Y.Z)
- **Build Numbers**: Use format XXYY for version X.Y (e.g., 1020 for 1.0.2)
- **Sparkle Signatures**: Must match the exact DMG file being distributed
- **File Sizes**: Must be exact byte count for proper auto-update verification
- **Testing**: Always verify the app version displays correctly after building
- **Security**: Private keys for Sparkle signing are automatically detected by generate_keys tool

### Troubleshooting

#### Version Mismatch Issues
If users report version still shows old number after update:
1. Verify MARKETING_VERSION and CURRENT_PROJECT_VERSION in project.pbxproj
2. Rebuild the app completely (clean build)
3. Create new DMG with updated app
4. Generate new signature for the corrected DMG
5. Update appcast.xml with new signature and file size
6. Replace DMG file in GitHub release using `gh release upload vX.X.X JoeVoice-X.X.X.dmg --clobber`

#### Auto-Update Issues
If auto-updates fail:
1. Verify sparkle:edSignature matches the actual DMG file
2. Check file size (length attribute) is correct
3. Ensure download URL is accessible
4. Confirm SUPublicEDKey in Info.plist matches the private key used for signing
