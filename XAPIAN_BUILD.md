# Xapian Static Build GitHub Action

This GitHub Action automatically downloads the latest stable Xapian release and builds it statically across multiple platforms.

## Features

- **Multi-Platform Support**: Builds on Linux (Ubuntu), Windows, and macOS
- **Automatic Latest Version**: Fetches the latest Xapian stable release automatically
- **Static Build**: Builds Xapian with static linking (`--enable-static=yes --enable-shared=no`)
- **Manual Trigger Only**: Workflow runs only when manually triggered
- **Optimized Build**: Release build with optimizations
- **Platform-specific Caching**: Caches Xapian source code per platform to speed up subsequent builds
- **Platform-specific Artifacts**: Produces downloadable build artifacts for each platform

## Supported Platforms

| Platform | OS | Archive Format | Build System |
|----------|----|----|----------|
| **Linux** | ubuntu-latest | tar.gz | autotools (configure/make) |
| **Windows** | windows-latest | zip | MSYS2 with autotools |
| **macOS** | macos-latest | tar.gz | autotools (configure/make) |

## Build Configuration

The action uses the following configuration for all platforms:

- `--enable-static=yes` - Build static libraries
- `--enable-shared=no` - Disable shared libraries
- `--disable-documentation` - Skip documentation build for faster compilation

## Triggers

The workflow runs on:
- Manual trigger (`workflow_dispatch`) **only**

This workflow does **not** run automatically on push or pull requests.

## Usage

### Manual Trigger

You can manually trigger the build from the GitHub Actions tab:

1. Go to the "Actions" tab in your repository
2. Select "Build Xapian Static" workflow
3. Click "Run workflow"
4. Select the branch (typically "main")
5. Click the green "Run workflow" button

## Artifacts

After a successful build, the workflow produces platform-specific artifacts:

- **Linux**: `xapian-static-{version}-linux.tar.gz` - Compressed tar.gz archive
- **Windows**: `xapian-static-{version}-windows.zip` - ZIP archive  
- **macOS**: `xapian-static-{version}-macos.tar.gz` - Compressed tar.gz archive
- **Retention**: All artifacts are kept for 30 days

## Build Output

Each platform build includes:
- Xapian core static library (`.a` files)
- Xapian headers and development files
- Command-line tools (xapian-* utilities)
- Configuration files

## System Requirements

The build matrix automatically handles dependencies for each platform:

**Linux (Ubuntu Latest):**
- build-essential, autoconf, automake, libtool
- git, zlib1g-dev, uuid-dev

**Windows (Windows Latest):**
- Pre-installed: Windows SDK, MSVC
- Auto-installed: MSYS2, base-devel, mingw-w64 toolchain, autoconf, automake, libtool

**macOS (macOS Latest):**
- Pre-installed: Xcode Command Line Tools
- Auto-installed: autoconf, automake, libtool (via Homebrew)

## Build Summary

After completion, each platform build provides a detailed summary including:
- Platform and OS information
- Xapian version built
- Build configuration (static/shared)
- Configure arguments used
- List of installed static libraries
- Verification of static libraries

## Customization

To modify the build configuration, edit `.github/workflows/build-xapian.yml`:

- **Add configure options**: Modify `configure_args` in the matrix for platform-specific options
- **Change installation prefix**: Modify `--prefix` in the configure step
- **Enable additional features**: Add configure flags (e.g., `--enable-backend-chert`, `--enable-backend-glass`)
- **Add new platforms**: Extend the build matrix with additional OS configurations

## Troubleshooting

### Build Failures

1. Check the build logs in the GitHub Actions tab
2. Common issues:
   - Missing dependencies: Verify all required packages are installed
   - Bootstrap failures: Ensure autotools are properly installed
   - Disk space: Ensure sufficient storage for the build

### Windows-specific Issues

- Windows builds use MSYS2 for Unix-like build environment
- Path translations may be required for Windows paths
- Ensure MSYS2 packages are up to date

## Integration with Doxygen

This static Xapian build is specifically designed for use with Doxygen, providing:
- Static Xapian library for search functionality
- No runtime dependencies for easier distribution
- Cross-platform compatibility

The static nature ensures no runtime dependencies when distributing Doxygen with Xapian support.

## About Xapian

Xapian is an open-source search engine library that provides full-text search capabilities. It's used by Doxygen to enable advanced search features in generated documentation.

Key features:
- Probabilistic relevance ranking
- Boolean queries
- Phrase and proximity searching
- Wildcard and range queries
- Faceted search
- Unicode support
