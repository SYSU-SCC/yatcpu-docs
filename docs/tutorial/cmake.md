CMake 是一个支持多平台的构建工具，它可以使用一份配置文件，就能在不同平台上构建。

## 配置编译工具链

默认情况下，CMake 会调用系统的默认编译工具链，编译出可以在计算机上运行的可执行文件。但是我们的 CPU 一般是 x86 指令集，不能在我们的 CPU 上运行。所以，我们需要通过配置文件来指定自己的编译工具链。

### 工具链配置文件

在 C 语言程序项目目录下，创建 `toolchain.cmake` 文件，并在文件中配置好我们自己的编译工具链。

通过设置 `CMAKE_***_COMPILER` 变量，我们可以指定使用 clang 作为编译器。而 `CMAKE_***_COMPILER_TARGET` 则设置了交叉编译的编译目标三元组。编译目标三元组中，第一个分量代表目标指令集，第二个分量代表目标操作系统，第三个分量代表是可执行程序的格式。`riscv32-unknown-elf` 就表示我们编译的目标是 RISC-V 32 位指令集，在未知操作系统上运行的 ELF 格式的可执行程序。

另外，我们还需要指定编译器的初始额外参数，这些参数可以在 `CMAKE_***_FLAGS` 变量中指定。其中重要的是 `-fuse-ld=lld`、`-no-std` 和 `-static`。第一个参数告诉 CMake 使用 lld 作为链接器，而不是默认的 ld。第二个参数告诉 CMake 不使用标准库，而是使用自己的库。最后一个参数告诉编译使用静态链接，而不是动态链接。

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