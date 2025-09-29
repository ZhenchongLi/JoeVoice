<div align="center">
  <img src="JoeVoice/Assets.xcassets/AppIcon.appiconset/256-mac.png" width="180" height="180" />
  <h1>JoeVoice</h1>
  <p>Privacy-focused voice transcription app for macOS with local AI processing</p>

  [![License](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
  ![Platform](https://img.shields.io/badge/platform-macOS%2014.0%2B-brightgreen)
  [![GitHub release (latest by date)](https://img.shields.io/github/v/release/ZhenchongLi/JoeVoice)](https://github.com/ZhenchongLi/JoeVoice/releases)
  ![GitHub all releases](https://img.shields.io/github/downloads/ZhenchongLi/JoeVoice/total)
  ![GitHub stars](https://img.shields.io/github/stars/ZhenchongLi/JoeVoice?style=social)

  <a href="https://github.com/ZhenchongLi/JoeVoice/releases/latest">
    <img src="https://img.shields.io/badge/Download%20Now-Latest%20Version-blue?style=for-the-badge&logo=github" alt="Download JoeVoice" width="250"/>
  </a>
</div>

---

JoeVoice is a native macOS voice transcription application built for privacy and efficiency. Using local AI models, it converts speech to text with high accuracy while keeping all data on your device. You can download the app from [GitHub Releases](https://github.com/ZhenchongLi/JoeVoice/releases/latest).

![JoeVoice Mac App](https://github.com/user-attachments/assets/new-dashboard-screenshot)

After dedicating the past 5 months to developing this app, I've decided to open source it for the greater good.

The goal is to create **the most efficient and privacy-focused voice-to-text solution for macOS** that is intuitive to use. While the source code is available for developers to build and contribute, purchasing the app supports continued development and provides automatic updates, priority support, and access to new features.

## Features

- 🎙️ **High-Accuracy Transcription**: Advanced local AI models deliver precise speech-to-text conversion with near-instant processing
- 🔒 **Privacy First**: 100% offline processing ensures your data never leaves your device
- ⚡ **Power Mode**: Intelligent context detection that automatically applies optimized settings based on your active application or browser URL
- 🧠 **Context Awareness**: Smart AI adaptation that understands your current environment and adjusts transcription accordingly
- 🎯 **Global Shortcuts**: Configurable keyboard shortcuts for quick recording and push-to-talk functionality
- 📝 **Personal Dictionary**: Customize transcription accuracy with custom vocabulary, industry terminology, and intelligent text replacements
- 🔄 **Smart Modes**: Switch between specialized AI modes optimized for different writing styles and use cases
- 🤖 **AI Assistant**: Integrated voice assistant for ChatGPT-style conversational interactions

## Get Started

### Download
Download the latest version from [GitHub Releases](https://github.com/ZhenchongLi/JoeVoice/releases/latest). This open-source project welcomes contributions and support for continued development.

#### Homebrew
Alternatively, you can install JoeVoice via `brew`:

```shell
brew install --cask joevoice
```

### Build from Source
As an open-source project, you can build JoeVoice from source by following the instructions in [BUILDING.md](BUILDING.md). The official release version provides additional benefits including automatic updates, priority support, and helps fund ongoing development.

## Requirements

- macOS 14.0 or later

## Documentation

- [Building from Source](BUILDING.md) - Detailed instructions for building the project
- [Contributing Guidelines](CONTRIBUTING.md) - How to contribute to JoeVoice
- [Code of Conduct](CODE_OF_CONDUCT.md) - Our community standards

## Contributing

We welcome contributions! However, please note that all contributions should align with the project's goals and vision. Before starting work on any feature or fix:

1. Read our [Contributing Guidelines](CONTRIBUTING.md)
2. Open an issue to discuss your proposed changes
3. Wait for maintainer feedback

For build instructions, see our [Building Guide](BUILDING.md).

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## Support

If you encounter any issues or have questions, please:
1. Check the existing issues in the GitHub repository
2. Create a new issue if your problem isn't already reported
3. Provide as much detail as possible about your environment and the problem

## Acknowledgments

### Core Technology
- [whisper.cpp](https://github.com/ggerganov/whisper.cpp) - High-performance inference of OpenAI's Whisper model
- [FluidAudio](https://github.com/FluidInference/FluidAudio) - Used for Parakeet model implementation

### Essential Dependencies
- [Sparkle](https://github.com/sparkle-project/Sparkle) - Keeping JoeVoice up to date
- [KeyboardShortcuts](https://github.com/sindresorhus/KeyboardShortcuts) - User-customizable keyboard shortcuts
- [LaunchAtLogin](https://github.com/sindresorhus/LaunchAtLogin) - Launch at login functionality
- [MediaRemoteAdapter](https://github.com/ejbills/mediaremote-adapter) - Media playback control during recording
- [Zip](https://github.com/marmelroy/Zip) - File compression and decompression utilities


---

Made with ❤️ by Pax
