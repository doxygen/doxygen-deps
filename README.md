# doxygen-deps
The goal of the repository is to provide Github actions that allow to build the external dependencies 
for Doxygen for all platforms supported and used for the official binary release.

These dependencies are optional, so they are not shipped as part of Doxygen's repository.

The dependencies consist of (statically) build libraries of:
- libxapian (part of Xapian)
- libclang (part of LLVM)
- Qt6 (only the `qtbase` and `qtsvg` modules are needed)

Since there package are normally provided as shared libraries, the resulting doxygen binaries cannot easiliy be moved to a different system without recompilation.
By using statically linked versions of the dependent libraries, this can be avoided.

These package are too big and slow to compile every time, so the idea is to trigger the Github action manually, and then use the produced artifacts.
