# Building JoeVoice

This guide provides detailed instructions for building JoeVoice from source.

## Prerequisites

Before you begin, ensure you have:
- macOS 14.0 or later
- Xcode (latest version recommended)
- Swift (latest version recommended)

## Building whisper.cpp Framework

1. Clone and build whisper.cpp:
```bash
git clone https://github.com/ggerganov/whisper.cpp.git
cd whisper.cpp
./build-xcframework.sh
```
This will create the XCFramework at `build-apple/whisper.xcframework`.

## Building JoeVoice

1. Clone the JoeVoice repository:
```bash
git clone https://github.com/ZhenchongLi/JoeVoice.git
cd JoeVoice
```

2. Add the whisper.xcframework to your project:
   - Drag and drop `../whisper.cpp/build-apple/whisper.xcframework` into the project navigator, or
   - Add it manually in the "Frameworks, Libraries, and Embedded Content" section of project settings

3. Build and Run
   - Build the project using Cmd+B or Product > Build
   - Run the project using Cmd+R or Product > Run

## Development Setup

1. **Xcode Configuration**
   - Ensure you have the latest Xcode version
   - Install any required Xcode Command Line Tools

2. **Dependencies**
   - The project uses [whisper.cpp](https://github.com/ggerganov/whisper.cpp) for transcription
   - Ensure the whisper.xcframework is properly linked in your Xcode project
   - Test the whisper.cpp installation independently before proceeding

3. **Building for Development**
   - Use the Debug configuration for development
   - Enable relevant debugging options in Xcode

4. **Testing**
   - Run the test suite before making changes
   - Ensure all tests pass after your modifications

## Troubleshooting

If you encounter any build issues:
1. Clean the build folder (Cmd+Shift+K)
2. Clean the build cache (Cmd+Shift+K twice)
3. Check Xcode and macOS versions
4. Verify all dependencies are properly installed
5. Make sure whisper.xcframework is properly built and linked

For more help, please check the [issues](https://github.com/ZhenchongLi/JoeVoice/issues) section or create a new issue.

## Release Process

For detailed release instructions, see the comprehensive release documentation in [CLAUDE.md](CLAUDE.md#release-process).

### Quick Release Overview

1. **Update Version Numbers**
   - Update `MARKETING_VERSION` and `CURRENT_PROJECT_VERSION` in project settings
   - Update version information in `appcast.xml`

2. **Build Release Version**
   - Clean and build release configuration in Xcode
   - Create DMG package for distribution

3. **Generate Sparkle Signature**
   - Use Sparkle tools to generate EdDSA signature for auto-updates

4. **Create GitHub Release**
   - Create git tag and push to repository
   - Upload DMG to GitHub releases
   - Update appcast.xml with final download URLs

For complete step-by-step instructions, refer to the [Release Process section in CLAUDE.md](CLAUDE.md#release-process).