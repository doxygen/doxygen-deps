# doxygen-deps
Build doxygen dependencies

## Available Builds

### LLVM Static Build

This repository includes a GitHub Action that automatically builds LLVM with static linking across multiple platforms, specifically configured for Doxygen dependencies.

**Features:**
- **Multi-Platform Support**: Builds on Linux, Windows, and macOS
- Downloads latest LLVM release automatically
- Builds with clang and lld projects enabled
- Static libclang library (`LIBCLANG_BUILD_STATIC=ON`)
- Platform-specific optimizations and configurations
- Optimized for Doxygen integration

**Usage:**
- Manually trigger from GitHub Actions tab
- Automatically runs on push/PR to main branch
- Produces platform-specific downloadable artifacts (tar.gz for Linux/macOS, zip for Windows)

For detailed information, see [LLVM_BUILD.md](LLVM_BUILD.md).
