CMake 是一个支持多平台的构建工具，它可以使用一份配置文件，就能在不同平台上构建。

## 配置编译工具链

默认情况下，CMake 会调用系统的默认编译工具链，编译出可以在计算机上运行的可执行文件。但是我们自己的电脑的 CPU 一般是 x86/ARM 指令集，编译出来的二进制不能在我们的 RISC-V CPU 上运行。所以，我们需要通过配置文件来指定自己的编译工具链。

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

## 使用 CMake 生成项目并编译

一个 CMake 文件中最重要的是 `add_library` 和 `add_executable` 指令。`add_library` 指令用于添加一个库目标，`add_executable` 指令用于添加一个可执行文件目标。为了方便测试，初始化程序运行栈的代码和链接脚本已经提供了，后续只需要在 `csrc` 文件夹中添加自己的 C 语言程序，并修改 `CMakeLists.txt` 文件添加可执行程序目标即可。

CMake 项目的编译需要两步，第一步是运行 `cmake` 生成平台相关的编译配置文件，在 Linux 下通常是 `Makefile`，在 Windows 下通常是 `Visual Studio` 的解决方案文件，或者微软的 `nmake` 工具的配置文件。第二步是运行 `make` 或者 `nmake` 等工具进行编译。

为了避免污染源代码目录，配置和编译通常在单独的文件夹中进行。项目提供的编译脚本 `build.sh`（Linux）或 `build.bat`（Windows）将配置和编译文件放在 `build` 文件夹中。

如果想要添加 C 语言程序，只需要在 `CMakeLists.txt` 的开头的：

```cmake
set(C_PROGRAMS tetris hello fibonacci quicksort)
```

添加自己的 C 语言程序文件名，保存后然后重新运行一次编译脚本即可。

ELF 格式的文件可以使用 `llvm-objdump` 工具反汇编，在 `build` 文件夹中运行：

```bash
llvm-objdump -d --arch-name=riscv32 程序名
```

编译出来的 CPU 可执行文件放置在 `src/main/resources` 文件夹中，文件名后缀是 `.asmbin`。
