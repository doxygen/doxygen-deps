# LLVM Static Build GitHub Action

This GitHub Action automatically downloads the latest LLVM repository and builds it statically with the clang and lld projects enabled.

## Features

- **Automatic Latest Version**: Fetches the latest LLVM release automatically
- **Static Build**: Builds LLVM with static linking (`LLVM_BUILD_STATIC=ON`)
- **libclang Static**: Enables static libclang library (`LIBCLANG_BUILD_STATIC=ON`)
- **Essential Projects**: Builds clang and lld (`LLVM_ENABLE_PROJECTS="clang;lld"`)
- **Optimized Build**: Release build with optimizations
- **Caching**: Caches LLVM source code to speed up subsequent builds
- **Artifacts**: Produces downloadable build artifacts

## Build Configuration

The action uses the following CMake configuration:

- `CMAKE_BUILD_TYPE=Release` - Optimized release build
- `LLVM_ENABLE_PROJECTS="clang;lld"` - Enable clang compiler and lld linker
- `LIBCLANG_BUILD_STATIC=ON` - Build static libclang library
- `LLVM_BUILD_STATIC=ON` - Build static LLVM libraries
- `LLVM_LINK_LLVM_DYLIB=OFF` - Disable dynamic LLVM library
- `CLANG_LINK_CLANG_DYLIB=OFF` - Disable dynamic clang library
- `LLVM_ENABLE_PIC=OFF` - Disable position-independent code for static builds
- `LLVM_TARGETS_TO_BUILD="X86;AArch64"` - Build for x86 and ARM64 targets

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

After a successful build, the workflow produces:

- **llvm-static-{version}.tar.gz**: Compressed archive containing the complete LLVM installation
- **Retention**: Artifacts are kept for 30 days

## Build Output

The build includes:
- LLVM core libraries (static)
- Clang compiler
- LLD linker
- libclang static library
- Headers and development files

## System Requirements

The build runs on Ubuntu Latest with the following dependencies:
- build-essential
- cmake
- ninja-build
- git
- python3
- zlib1g-dev
- libedit-dev
- libffi-dev
- libxml2-dev
- libtinfo-dev

## Build Summary

After completion, the workflow provides a detailed summary including:
- LLVM version built
- Build configuration
- List of installed components
- Verification of static libraries

## Customization

To modify the build configuration, edit `.github/workflows/build-llvm.yml`:

- **Add more projects**: Modify `LLVM_ENABLE_PROJECTS` (e.g., add `;compiler-rt;libcxx`)
- **Change targets**: Modify `LLVM_TARGETS_TO_BUILD` (e.g., add `;ARM;NVPTX`)
- **Build type**: Change `BUILD_TYPE` environment variable
- **Additional options**: Add more CMake flags in the configure step

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