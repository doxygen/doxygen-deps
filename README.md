# doxygen-deps
Build doxygen dependencies

## Available Builds

### LLVM Static Build

This repository includes a GitHub Action that automatically builds LLVM with static linking, specifically configured for Doxygen dependencies.

**Features:**
- Downloads latest LLVM release automatically
- Builds with clang and lld projects enabled
- Static libclang library (`LIBCLANG_BUILD_STATIC=ON`)
- Optimized for Doxygen integration

**Usage:**
- Manually trigger from GitHub Actions tab
- Automatically runs on push/PR to main branch
- Produces downloadable artifacts

For detailed information, see [LLVM_BUILD.md](LLVM_BUILD.md).
