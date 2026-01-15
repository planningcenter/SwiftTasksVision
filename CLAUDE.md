# SwiftTasksVision
**SwiftTasksVision** is a Swift Package Manager wrapper for Google's MediaPipe Vision framework, enabling iOS developers to integrate powerful computer vision capabilities without using CocoaPods.
> **Note:** This is an independent project not affiliated with Google. It wraps the MediaPipe Vision CocoaPods into a Swift Package for easier integration.
## Project Overview
This package provides:
- Swift Package Manager distribution of MediaPipe Vision
- XCFramework binaries for MediaPipe's vision tasks
- Automated daily updates tracking official MediaPipe releases
- Example iOS app demonstrating pose detection
## Technologies & Stack
- **Language:** Swift 5.9+
- **Platform:** iOS 16.0+
- **Package Manager:** Swift Package Manager
- **Dependencies:** MediaPipe Vision (wrapped from CocoaPods)
- **Build Tools:** Xcode, xcodebuild
- **CI/CD:** GitHub Actions
## Project Structure
SwiftTasksVision/
├── Sources/
│   ├── SwiftTasksVision/          # Main library target (wrapper)
│   └── UpdatePackage/             # CLI tool for updating MediaPipe binaries
├── Dependencies/                   # XCFramework binaries
│   ├── MediaPipeTasksVision.xcframework
│   ├── MediaPipeTasksCommon.xcframework
│   └── MediaPipeCommonGraphLibraries.xcframework
├── ExampleApp/
│   ├── PoseLandmarker/            # Example app showing pose detection
│   ├── download_models.sh         # Downloads ML models
│   └── PoseLandmarker.xcodeproj   # Standalone Xcode project
├── MediaPipeTasksCommonGraph/     # Temporary build workspace
│   ├── Podfile                    # CocoaPods source for MediaPipe
│   └── MediaPipeCommonGraphLibraries.xcodeproj
├── Package.swift                  # Swift Package Manifest
└── .github/workflows/
    └── PackageUpdate.yaml         # Daily automation for updates
## Setup & Installation
### As a Dependency (For App Developers)
1. Add package to your Xcode project:
   - File > Add Packages...
   - Enter repository URL
   - Select version
2. Import and use:
   ```swift
   import MediaPipeTasksVision
   ```
### For Development (Contributing)
1. Clone the repository:
   ```bash
   git clone <repo-url>
   cd SwiftTasksVision
   ```
2. Build the package:
   ```bash
   swift build
   ```
3. Run the example app:
   - Open `ExampleApp/PoseLandmarker.xcodeproj` in Xcode
   - Run the models download script first:
     ```bash
     cd ExampleApp
     ./download_models.sh
     ```
   - Build and run in Xcode
## Common Development Commands
### Building
```bash
# Build the package
swift build
# Build in release mode
swift build -c release
### Running UpdatePackage Tool
```bash
# Update MediaPipe frameworks from CocoaPods
swift run UpdatePackage
This command:
1. Downloads latest MediaPipe pods via CocoaPods
2. Extracts XCFrameworks from pods
3. Builds MediaPipeCommonGraphLibraries framework
4. Copies all frameworks to Dependencies/
5. Updates ML model files for example app
6. Cleans up temporary files
### Example App
```bash
# Download ML models
cd ExampleApp
./download_models.sh
# Then open and run in Xcode:
open ExampleApp/PoseLandmarker.xcodeproj
### GitHub Actions
- **Daily automation:** Runs `swift run UpdatePackage` daily at midnight UTC
- **Auto-versioning:** Creates git tags matching MediaPipe version numbers
- **Manual trigger:** Can be triggered via `workflow_dispatch`
## Architecture Notes
### Package Design
1. **Binary Targets:** The package uses pre-built XCFrameworks rather than source code, as MediaPipe is distributed as binaries via CocoaPods.
2. **UpdatePackage Tool:** Automated CLI tool that:
   - Uses CocoaPods to fetch latest MediaPipe binaries
   - Extracts and repackages frameworks for SPM compatibility
   - Builds custom graph libraries wrapper
   - Updates ML models for examples
3. **Linker Settings:** Requires `-ObjC` flag because MediaPipe frameworks contain Objective-C categories.
### XCFramework Components
- **MediaPipeTasksVision:** Main vision APIs (face/hand/pose detection, segmentation, etc.)
- **MediaPipeTasksCommon:** Shared utilities and base classes
- **MediaPipeCommonGraphLibraries:** Graph processing static libraries wrapper
### Example App Architecture
The PoseLandmarker example demonstrates:
- Real-time camera feed processing
- Pose landmark detection (lite/full/heavy models)
- Overlay rendering for detected poses
- Bottom sheet configuration UI
- Media library integration for video processing
## Dependencies Management
### Primary Dependencies
- **MediaPipe Vision CocoaPods:** Source of truth for framework binaries
- Tracked version in `MediaPipeTasksCommonGraph/Podfile.lock`
### Update Process
1. Daily GitHub Action runs `UpdatePackage`
2. CocoaPods fetches latest MediaPipe version
3. Frameworks extracted and copied to `Dependencies/`
4. Version extracted from Podfile.lock
5. If new version: commit, tag, and create GitHub release
### Version Sync
- Package versions match MediaPipe CocoaPod versions exactly
- Git tags created automatically (e.g., `0.10.14`)
- No manual versioning required
## MediaPipe Vision Capabilities
The wrapped framework provides:
- Object detection and tracking
- Face detection and landmark detection
- Hand landmark detection
- Pose landmark detection (demonstrated in example)
- Image classification
- Image segmentation
- Image embedding
See [MediaPipe Tasks Guide](https://ai.google.dev/edge/mediapipe/solutions/tasks) for full API documentation.
## Important Notes
- **Minimum iOS Version:** 16.0 (set in Package.swift)
- **CocoaPods Requirement:** iOS 15.6 (used during build process only)
- **Build Requirement:** Requires Xcode with command line tools
- **Binary Size:** XCFrameworks are large (~100MB+) due to ML models
- **Licensing:** MIT License for wrapper, Apache 2.0 for MediaPipe
