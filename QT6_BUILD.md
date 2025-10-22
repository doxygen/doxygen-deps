# Qt6 Static Build GitHub Action

This GitHub Action automatically downloads the latest Qt6 release and builds it statically with only the qtbase and qtsvg modules across multiple platforms.

## Features

- **Multi-Platform Support**: Builds on Linux (Ubuntu), Windows, and macOS
- **Fixed Qt6 Version**: Uses Qt6 6.8.1 (LTS) for stability and reproducibility
- **Static Build**: Builds Qt6 with static linking (`BUILD_SHARED_LIBS=OFF`)
- **Minimal Modules**: Only builds qtbase and qtsvg modules (as required by Doxygen)
- **Optimized Build**: Release build with optimizations
- **Platform-specific Caching**: Caches Qt6 source code per platform to speed up subsequent builds
- **Platform-specific Artifacts**: Produces downloadable build artifacts for each platform

## Supported Platforms

| Platform | OS | Archive Format | Build System |
|----------|----|----|----------|
| **Linux** | ubuntu-latest | tar.gz | CMake + Ninja |
| **Windows** | windows-latest | zip | CMake + Ninja |
| **macOS** | macos-latest | tar.gz | CMake + Ninja |

## Build Configuration

The action uses the following common CMake configuration for all platforms:

- `CMAKE_BUILD_TYPE=Release` - Optimized release build
- `BUILD_SHARED_LIBS=OFF` - Build static libraries
- `QT_FEATURE_static=ON` - Enable Qt static build feature
- `QT_FEATURE_shared=OFF` - Disable shared libraries
- `QT_BUILD_EXAMPLES=OFF` - Skip examples for faster build
- `QT_BUILD_TESTS=OFF` - Skip tests for faster build
- Various Qt features disabled (network, sql, opengl, etc.) to minimize build size

### Modules Built

Only the following Qt6 modules are built:

1. **qtbase** - Core Qt functionality (QtCore, QtGui, QtWidgets, QtXml)
2. **qtsvg** - SVG support for Qt

These are the only modules required by Doxygen for generating documentation.

### Platform-specific Configuration

**Linux (Ubuntu):**
- `CMAKE_INTERPROCEDURAL_OPTIMIZATION=OFF` - Disable LTO for faster builds
- Full X11 and XCB development libraries installed
- OpenGL development libraries for GUI support

**Windows:**
- `CMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded` - Static runtime linking
- Uses Ninja build system with MSVC compiler

**macOS:**
- `CMAKE_OSX_DEPLOYMENT_TARGET=10.15` - Target macOS 10.15 and newer
- Framework build disabled for static linking

## Triggers

The workflow runs on:
- Manual trigger (`workflow_dispatch`) **only**

This workflow does **not** run automatically on push or pull requests.

## Usage

### Manual Trigger

You can manually trigger the build from the GitHub Actions tab:

1. Go to the "Actions" tab in your repository
2. Select "Build Qt6 Static" workflow
3. Click "Run workflow"
4. Select the branch (typically "main")
5. Click the green "Run workflow" button

## Artifacts

After a successful build, the workflow produces platform-specific artifacts:

- **Linux**: `qt6-static-{version}-linux.tar.gz` - Compressed tar.gz archive
- **Windows**: `qt6-static-{version}-windows.zip` - ZIP archive  
- **macOS**: `qt6-static-{version}-macos.tar.gz` - Compressed tar.gz archive
- **Retention**: All artifacts are kept for 30 days

## Build Output

Each platform build includes:
- Qt6 static libraries (`.a` on Unix-like, `.lib` on Windows)
- Qt6 headers and development files
- Qt6 build tools (moc, uic, rcc, etc.)
- Qt6 plugins (as static libraries)
- CMake configuration files for integration

## System Requirements

The build matrix automatically handles dependencies for each platform:

**Linux (Ubuntu Latest):**
- build-essential, cmake, ninja-build
- git, python3
- X11 development libraries (libx11-dev, libxcb-*-dev, etc.)
- XKB common development libraries
- OpenGL development libraries (libgl1-mesa-dev, libglu1-mesa-dev)
- Font libraries (libfontconfig1-dev, libfreetype6-dev)

**Windows (Windows Latest):**
- Pre-installed: Visual Studio Build Tools, Python
- Auto-installed: Ninja (via Chocolatey)

**macOS (macOS Latest):**
- Pre-installed: Xcode Command Line Tools, Python
- Auto-installed: CMake, Ninja (via Homebrew)

## Build Process

The Qt6 build is performed in two stages:

1. **Build qtbase**: First builds the base Qt module containing QtCore, QtGui, QtWidgets, and QtXml
2. **Build qtsvg**: Then builds qtsvg module which depends on qtbase

This two-stage approach is necessary because qtsvg depends on qtbase being already built and installed.

## Build Summary

After completion, each platform build provides a detailed summary including:
- Platform and OS information
- Qt6 version built
- Platform-specific build configuration
- Platform-specific CMake arguments
- List of installed static libraries
- Verification of static libraries

## Customization

To modify the build configuration, edit `.github/workflows/build-qt6.yml`:

- **Update Qt6 version**: Change the `VERSION` in the "Set Qt6 version" step
- **Add more modules**: Download additional Qt modules and add build steps
- **Enable Qt features**: Modify Qt feature flags (e.g., `-DQT_FEATURE_network=ON`)
- **Build type**: Change `BUILD_TYPE` environment variable
- **Platform-specific options**: Add more CMake flags in the matrix cmake_args for specific platforms
- **Add new platforms**: Extend the build matrix with additional OS configurations

## Troubleshooting

### Build Failures

1. Check the build logs in the GitHub Actions tab
2. Common issues:
   - Out of memory: Qt6 builds require significant RAM (8GB+ recommended)
   - Disk space: Ensure sufficient storage for the build (20GB+ recommended)
   - Dependencies: Verify all required packages are installed
   - Module dependencies: Ensure qtbase is built before qtsvg

### Large Build Times

Qt6 is a large project and builds can take 2-3 hours. The workflow includes:
- Parallel builds using all available CPU cores
- Source code caching to speed up repeated builds
- Minimal module set (only qtbase and qtsvg)
- Disabled examples and tests

### Platform-specific Issues

**Linux:**
- Ensure all X11 and XCB development packages are installed
- May need additional font or OpenGL libraries depending on configuration

**Windows:**
- Builds use MSVC compiler (not MinGW)
- Static runtime linking is used for better portability
- Ensure Visual Studio Build Tools are up to date

**macOS:**
- Framework builds are disabled for static linking
- Deployment target is set to macOS 10.15 for broad compatibility

## Integration with Doxygen

This static Qt6 build is specifically designed for use with Doxygen, providing:
- Static Qt6 libraries for GUI features
- QtCore, QtGui, QtWidgets for the doxywizard GUI tool
- QtXml for XML parsing
- QtSvg for SVG graphics support in documentation

The static nature ensures no runtime dependencies when distributing Doxygen with Qt6 support.

## About Qt6

Qt is a cross-platform application development framework used for developing GUI applications, among other things. Qt6 is the latest major version.

Key features:
- Cross-platform compatibility (Linux, Windows, macOS)
- Rich widget set for GUI applications
- Signal and slot mechanism for event handling
- Internationalization support
- Vector graphics support (SVG)
- XML parsing and manipulation

For more information, visit: https://www.qt.io/
