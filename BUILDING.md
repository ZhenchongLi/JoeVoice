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

## 发布新版本流程

发布一个新版本的 JoeVoice 应用需要经过以下步骤。

### 1. 更新版本号

- 在 Xcode 中，打开项目导航器，选择 `JoeVoice` 项目。
- 选择 `JoeVoice` Target，然后进入 `General` 标签页。
- 更新 `Version` (例如, 从 `1.5.0` 更新到 `1.6.0`)。
- 更新 `Build` (例如, 从 `150` 更新到 `160`)。

### 2. 在 Xcode 中 Archive

- 确保在 Xcode 顶部的 Scheme 选择菜单中，选择的目标是 `JoeVoice`，设备是 `Any Mac (Apple Silicon, Intel)`。
- 从菜单栏选择 `Product > Archive`。
- Archive 过程完成后，Xcode 的 "Organizer" 窗口会自动打开，并显示你刚刚创建的归档文件。

### 3. 导出应用 (两种方式)

#### 方式 A: 签名和公证 (推荐，需要付费开发者账户)

这是为付费开发者准备的标准流程，可以避免用户看到安全警告。

1.  在 Organizer 窗口中，确保已选中最新的归档文件，然后点击右侧的 `Distribute App` 按钮。
2.  选择 `Developer ID` 作为分发方式，然后点击 `Next`。
3.  选择 `Upload` 将应用上传到 Apple 的公证服务。
4.  公证成功后，再次点击 `Distribute App`，选择 `Developer ID`，但这次选择 `Export`。
5.  将应用导出到一个文件夹中，你会得到一个经过公证的 `JoeVoice.app` 文件。

#### 方式 B: 直接导出 (免费，但用户会看到安全警告)

作为未认证的开发者，你应该遵循此路径。

1.  在 Organizer 窗口中，选中你刚刚创建的归档文件。
2.  点击右侧的 `Distribute App` 按钮。
3.  选择 `Copy App` 选项。
4.  选择一个位置导出 `JoeVoice.app` 文件。

**重要提示**: 通过这种方式导出的应用没有经过签名和公证。其他用户在打开它时会看到一个安全警告，并需要通过 `右键点击 -> 打开` 的方式才能运行。请务必在你的发布说明中告知用户这一点。

### 4. 压缩应用

- 无论你使用方式 A 还是方式 B，现在你都有一个 `JoeVoice.app` 文件。
- 将 `JoeVoice.app` 压缩成一个 `.zip` 文件。例如：`JoeVoice.1.6.0.zip`。

### 5. 更新 appcast.xml

`appcast.xml` 文件用于 Sparkle 自动更新。你需要为新版本添加一个条目。

- 打开 `appcast.xml` 文件。
- 在 `<channel>` 标签内，复制最新的 `<item>` 块，并将其粘贴在最上方。
- 更新新 `<item>` 块内的信息（版本号、发布日期、文件大小等）。
- **注意**: 如果你使用**方式 B**，`sparkle:edSignature` 字段应该删除或留空，因为你没有对 `.zip` 文件进行签名。

### 6. 创建 GitHub Release

- 访问你的项目的 GitHub 仓库页面，并进入 "Releases"。
- 点击 "Draft a new release"。
- 上传你在第 4 步中创建的 `.zip` 文件。
- 在发布说明中，如果你是使用**方式 B**发布的，请务必加上如何处理安全警告的说明。
- 发布 Release。