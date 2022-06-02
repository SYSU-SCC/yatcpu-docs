CMake 是一个支持多平台的构建工具，它可以使用一份配置文件，就能在不同平台上构建。

## 配置编译工具链

默认情况下，CMake 会调用系统的默认编译工具链，编译出可以在计算机上运行的可执行文件。但是我们的 CPU 一般是 x86 指令集，不能在我们的 CPU 上运行。所以，我们需要通过配置文件来指定自己的编译工具链。

### 工具链配置文件

在 C 语言程序项目目录下，创建 `toolchain.cmake` 文件，并在文件中配置好我们自己的编译工具链。

```cmake
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR riscv32)

set(triple riscv32-unknown-elf)

set(CMAKE_C_COMPILER clang)
set(CMAKE_C_COMPILER_TARGET ${triple})
set(CMAKE_CXX_COMPILER clang++)
set(CMAKE_CXX_COMPILER_TARGET ${triple})
set(CMAKE_ASM_COMPILER clang)
set(CMAKE_ASM_COMPILER_TARGET ${triple})
set(CMAKE_AR llvm-ar CACHE FILEPATH "Archiver")
set(CMAKE_OBJCOPY llvm-objcopy)

set(CMAKE_C_FLAGS_INIT "-mno-relax")
set(CMAKE_CXX_FLAGS_INIT "-mno-relax")
set(CMAKE_ASM_FLAGS_INIT "-mno-relax")
set(CMAKE_EXE_LINKER_FLAGS_INIT "-fuse-ld=lld -nostdlib -static -mno-relax")
set(CMAKE_MODULE_LINKER_FLAGS_INIT "-fuse-ld=lld -nostdlib -static -mno-relax")
set(CMAKE_SHARED_LINKER_FLAGS_INIT "-fuse-ld=lld -nostdlib -static -mno-relax")
```