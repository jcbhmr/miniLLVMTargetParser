# `LLVMTargetParser` library

🎯 LLVM's `llvm::Triple` and associated members extracted into a standalone C++ library

## Installation

```sh
TODO
```

## Usage

```cpp
#include <iostream>
#include <llvm/TargetParser/Triple.h>

int main() {
    auto t = llvm::Triple("x86_64-unknown-linux-gnu");
    std::cout << t.getArchName() << "\n";
    return 0;
}
```

## Development

This project overlays its own build system onto the existing C++ source files in the LLVM repository. Note that we aren't using Git submodules here to track the [llvm/llvm-project](https://github.com/llvm/llvm-project) repository; it's just _too large_ to be practical. Instead, we use a stripped-down pseudo-submodule in `llvm-project/` that contains only the files we need. But wait! The LLVM source code has some code generation steps that are handled by some complex CMake infrastructure. Since this particular project doesn't have any platform-specific code generation we can just check in some pregenerated files from the LLVM vX.Y.Z release tarball and use those pregenerated files instead of the source versions.

TL;DR: We have `llvm-project/llvm/lib/*` which has only the source `*.cpp` files that we need to build the `libLLVMTargetParser.a` or similar library and a separate `LLVM-Linux-X64/include/*` directory that holds the released pregenerated header files.

Use `./task.py generate [version]` to generate `llvm-project/` and `LLVM-Linux-X64/` directories for the given LLVM version or default to fetching the latest version. This uses uv. You'll need uv installed.
