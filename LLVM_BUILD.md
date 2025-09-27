# LLVM Static Build GitHub Action

This GitHub Action automatically downloads the latest LLVM repository and builds it statically with the clang and lld projects enabled across multiple platforms.

## Features

- **Multi-Platform Support**: Builds on Linux (Ubuntu), Windows, and macOS
- **Automatic Latest Version**: Fetches the latest LLVM release automatically
- **Static Build**: Builds LLVM with static linking (`LIBCLANG_BUILD_STATIC=ON`)
- **libclang Static**: Enables static libclang library for all platforms
- **Essential Projects**: Builds clang and lld (`LLVM_ENABLE_PROJECTS="clang;lld"`)
- **Optimized Build**: Release build with optimizations
- **Platform-specific Caching**: Caches LLVM source code per platform to speed up subsequent builds
- **Platform-specific Artifacts**: Produces downloadable build artifacts for each platform

## Supported Platforms

| Platform | OS | Archive Format | Build Options |
|----------|----|----|-------|
| **Linux** | ubuntu-latest | tar.gz | PIC disabled, Terminfo disabled |
| **Windows** | windows-latest | zip | MSVC static runtime, MultiThreaded CRT |
| **macOS** | macos-latest | tar.gz | PIC disabled, macOS 10.15+ deployment target |

All platforms build for **X86** and **AArch64** targets.

## Build Configuration

The action uses the following common CMake configuration for all platforms:

- `CMAKE_BUILD_TYPE=Release` - Optimized release build
- `LLVM_ENABLE_PROJECTS="clang;lld"` - Enable clang compiler and lld linker
- `LIBCLANG_BUILD_STATIC=ON` - Build static libclang library
- `LLVM_TARGETS_TO_BUILD="X86;AArch64"` - Build for x86 and ARM64 targets
- Various optimizations and disabled features for faster, smaller builds

### Platform-specific Configuration

**Linux (Ubuntu):**
- `LLVM_ENABLE_PIC=OFF` - Disable position-independent code for static builds
- `LLVM_ENABLE_TERMINFO=OFF` - Disable terminfo dependency

**Windows:**
- `LLVM_USE_CRT_RELEASE=MT` - Use static MSVC runtime
- `CMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded` - Static runtime linking

**macOS:**
- `LLVM_ENABLE_PIC=OFF` - Disable position-independent code for static builds
- `CMAKE_OSX_DEPLOYMENT_TARGET=10.15` - Target macOS 10.15 and newer

## Triggers

The workflow runs on:
- Manual trigger (`workflow_dispatch`)
- Push to main branch
- Pull requests to main branch

## Usage

### Manual Trigger

You can manually trigger the build from the GitHub Actions tab:

1. Go to the "Actions" tab in your repository
2. Select "Build LLVM Static" workflow
3. Click "Run workflow"

### Automatic Trigger

The workflow will automatically run when you:
- Push commits to the main branch
- Create a pull request targeting the main branch

## Artifacts

After a successful build, the workflow produces platform-specific artifacts:

- **Linux**: `llvm-static-{version}-linux.tar.gz` - Compressed tar.gz archive
- **Windows**: `llvm-static-{version}-windows.zip` - ZIP archive  
- **macOS**: `llvm-static-{version}-macos.tar.gz` - Compressed tar.gz archive
- **Retention**: All artifacts are kept for 30 days

## Build Output

Each platform build includes:
- LLVM core libraries (static)
- Clang compiler (with platform-appropriate extension)
- LLD linker (with platform-appropriate extension)
- libclang static library (`.a` on Unix-like, `.lib` on Windows)
- Headers and development files

## System Requirements

The build matrix automatically handles dependencies for each platform:

**Linux (Ubuntu Latest):**
- build-essential, cmake, ninja-build
- git, python3, python3-pip
- zlib1g-dev, libedit-dev, libffi-dev, libxml2-dev, libtinfo-dev

**Windows (Windows Latest):**
- Pre-installed: Visual Studio Build Tools, Python
- Auto-installed: Ninja (via Chocolatey)

**macOS (macOS Latest):**
- Pre-installed: Xcode Command Line Tools, Python
- Auto-installed: CMake, Ninja (via Homebrew)

## Build Summary

After completion, each platform build provides a detailed summary including:
- Platform and OS information
- LLVM version built
- Platform-specific build configuration
- Platform-specific CMake arguments
- List of installed components
- Verification of static libraries

## Customization

To modify the build configuration, edit `.github/workflows/build-llvm.yml`:

- **Add more projects**: Modify `LLVM_ENABLE_PROJECTS` (e.g., add `;compiler-rt;libcxx`)
- **Change targets**: Modify `LLVM_TARGETS_TO_BUILD` in the matrix cmake_args (e.g., add `;ARM;NVPTX`)
- **Build type**: Change `BUILD_TYPE` environment variable
- **Platform-specific options**: Add more CMake flags in the matrix cmake_args for specific platforms
- **Add new platforms**: Extend the build matrix with additional OS configurations

## Troubleshooting

### Build Failures

1. Check the build logs in the GitHub Actions tab
2. Common issues:
   - Out of memory: LLVM builds require significant RAM
   - Disk space: Ensure sufficient storage for the build
   - Dependencies: Verify all required packages are installed

### Large Build Times

LLVM is a large project and builds can take 1-2 hours. The workflow includes:
- Parallel builds using all available CPU cores
- Source code caching to speed up repeated builds
- Optimized tablegen for faster builds

## Integration with Doxygen

This static LLVM build is specifically designed for use with Doxygen, providing:
- Static libclang for parsing C/C++ code
- Clang compiler for analysis
- LLD linker for toolchain completeness

The static nature ensures no runtime dependencies when distributing Doxygen with LLVM support.