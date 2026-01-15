The README.md is relatively minimal and missing several important pieces of information that would help developers understand and use this package. Based on the CLAUDE.md documentation, I can see the README lacks:

1. Requirements (iOS version, Swift version)
2. Repository URL for installation
3. Information about the example app setup (download_models.sh)
4. Development/contribution setup
5. Technology stack details
6. Available MediaPipe capabilities
7. Important notes about linker flags and framework size
8. Architecture overview

I'll provide an updated, comprehensive README:

# SwiftTasksVision

**SwiftTasksVision** is a Swift Package Manager wrapper for Google's MediaPipe Vision framework, enabling iOS developers to integrate powerful computer vision capabilities without using CocoaPods.

> **Disclaimer:** This project is an independent endeavor and is **not affiliated with or endorsed by Google**. MediaPipe is a trademark of Google.

## Features

- **Swift Package Integration:** Access MediaPipe Vision through Swift Package Manager, eliminating the need for CocoaPods in your app
- **Automated Daily Updates:** Package automatically tracks and updates to the latest MediaPipe Vision CocoaPods releases
- **Example Application:** Includes a complete iOS app demonstrating real-time pose detection
- **Full Vision API Support:** Object detection, face detection, hand landmarks, pose detection, image segmentation, and more

## Requirements

- **iOS:** 16.0+
- **Swift:** 5.9+
- **Xcode:** Latest version with command line tools
- **Platform:** iOS only

## Installation

### Adding to Your Xcode Project

1. In Xcode, navigate to `File` > `Add Packages...`
2. Enter the repository URL: `https://github.com/pco-abhi/SwiftTasksVision`
3. Select the desired version and add the package
4. Add the `-ObjC` linker flag to your target:
   - Select your target in Xcode
   - Go to `Build Settings`
   - Search for `Other Linker Flags`
   - Add `-ObjC`

### Using in Your Code

```swift
import MediaPipeTasksVision

// Use MediaPipe Vision APIs as documented


## Example Application

The package includes a complete example iOS app demonstrating real-time pose detection with camera input.

### Running the Example

1. Clone the repository:
   ```bash
   git clone https://github.com/pco-abhi/SwiftTasksVision.git
   cd SwiftTasksVision
   ```

2. Download the required ML models:
   ```bash
   cd ExampleApp
   ./download_models.sh
   ```

3. Open and run the example project:
   ```bash
   open ExampleApp/PoseLandmarker.xcodeproj
   ```

The example demonstrates:
- Real-time camera feed processing
- Pose landmark detection with multiple model options (lite/full/heavy)
- Visual overlay rendering for detected poses
- Video file processing from media library

## MediaPipe Vision Capabilities

This package provides access to MediaPipe's full suite of vision tasks:

- **Object Detection & Tracking**
- **Face Detection & Landmarks**
- **Hand Landmark Detection**
- **Pose Landmark Detection**
- **Image Classification**
- **Image Segmentation**
- **Image Embedding**

For detailed API documentation, see the [MediaPipe Tasks Guide](https://ai.google.dev/edge/mediapipe/solutions/tasks).

## Development

### Building the Package

```bash
swift build

# Release build
swift build -c release


### Updating MediaPipe Frameworks

The package includes an automated tool to fetch and update MediaPipe binaries:

```bash
swift run UpdatePackage


This command:
1. Downloads the latest MediaPipe CocoaPods
2. Extracts XCFrameworks
3. Builds the MediaPipeCommonGraphLibraries wrapper
4. Updates all dependencies in the `Dependencies/` folder
5. Updates ML models for the example app

### Automation

A GitHub Action runs daily to automatically:
- Check for new MediaPipe releases
- Update frameworks if a new version is available
- Create version tags matching MediaPipe versions
- Generate GitHub releases

## Architecture

### Package Structure


SwiftTasksVision/
├── Sources/
│   ├── SwiftTasksVision/          # Main library wrapper
│   └── UpdatePackage/             # Framework update automation
├── Dependencies/                   # XCFramework binaries
│   ├── MediaPipeTasksVision.xcframework
│   ├── MediaPipeTasksCommon.xcframework
│   └── MediaPipeCommonGraphLibraries.xcframework
├── ExampleApp/                    # Pose detection demo app
└── Package.swift                  # SPM manifest


### Binary Distribution

This package uses pre-built XCFrameworks rather than source code, as MediaPipe is distributed as binaries via CocoaPods. The frameworks are:

- **MediaPipeTasksVision:** Main vision APIs
- **MediaPipeTasksCommon:** Shared utilities and base classes  
- **MediaPipeCommonGraphLibraries:** Graph processing libraries wrapper

## Important Notes

- **Binary Size:** XCFrameworks are large (~100MB+) due to embedded ML models
- **Linker Flags:** The `-ObjC` flag is required because MediaPipe frameworks contain Objective-C categories
- **Versioning:** Package versions match MediaPipe CocoaPod versions exactly (e.g., 0.10.14)

## About MediaPipe

[MediaPipe](https://ai.google.dev/edge/mediapipe/solutions/guide) is a cross-platform framework by Google that offers customizable machine learning solutions for live and streaming media. The Vision component provides state-of-the-art models and tools for various computer vision tasks.

For complete documentation, refer to the [MediaPipe Solutions Guide](https://ai.google.dev/edge/mediapipe/solutions/guide).

## License

This wrapper project is licensed under the MIT License. MediaPipe itself is licensed under the Apache 2.0 License.

## Contributing

Contributions are welcome! This project primarily maintains the SPM wrapper and automation for MediaPipe Vision. For issues with MediaPipe itself, please refer to the [official MediaPipe repository](https://github.com/google/mediapipe).
