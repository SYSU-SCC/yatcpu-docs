# 编译与链接

By: [:material-github: howardlau1999](https://github.com/howardlau1999)、[:material-github: NelsonCheung](https://github.com/NelsonCheung-cn)

在之前的 C/C++ 课程中你已经学会了程序的入口是  `main()`  函数。但是 CPU 只认识地址，又如何知道一个程序从何处开始执行？我们又要如何编译一个我们自己写的 CPU 可以运行的程序并让它真正地跑起来？下面就将带你揭开  `main()`  函数幕后的秘密，并带你学习如何编译和链接，让我们写的 C 程序可以在我们自己写的 CPU 上运行起来。

## 一个简单的例子

为了便于阐述 C 程序的编译和链接的过程，我们首先准备一个简单的例子。

我们先编写一个  `print.h`  的头文件，声明一个名为  `print_something()`  的函数。

```c
#ifndef PRINT_H
#define PRINT_H

void print_something();

#endif
```

然后，我们在 `print.c` 中实现这个函数。

```c
#include <stdio.h>
#include "print.h"

void print_something() {
    printf("Hello YatCPU!\n");
}
```

最后，我们在 `main.c` 中使用这个函数打印并输出。

```c
#include "print.h"

int main() {
    print_something();
}
```

接下来，我们要开始编译运行，编译命令如下。

```shell
gcc -o main.out main.c
```

这条命令是编译 `main.c` ，然后生成可执行文件 `main.out` 。其中， `-o` 指定了生成的可执行文件的名称。但是，直接运行上述命令会出现以下错误。

```
main.c:(.text+0xa): undefined reference to `print_something'
```

这是因为 `main.c` 不知道函数 `print_something` 的实现。而函数 `print_something` 的实现在文件 `print.c` 中，我们需要在编译命令中加上它，如下所示。

```shell
gcc -o main.out main.c print.c
```

上面的命令也可以这样写。

```shell
gcc main.c print.c -o main.out 
```

在本例中，我们使用了 gcc 直接将代码 `main.c print.c print.h` 编译成可执行文件 `main.out` 的。实际上，C/C++ 编译器在编译代码时包含如下几个步骤。

- **预处理**。处理宏定义，如 `#include` ,  `#define` ,  `#ifndef` 等，生成**预处理文件**，一般以 `.i` 为后缀。
- **编译**。将预处理文件转换成**汇编代码文件**，一般以 `.S` 为后缀。
- **汇编**。将汇编代码文件文件转换成**可重定位文件**，一般以 `.o` 为后缀。
- **链接**。将多个可重定位文件链接生成**可执行文件**，一般以 `.o` 为后缀。

下面我们分别来学习这四个过程。

## 预处理

预处理又被称为预编译，主要处理宏定义，如 `#include` ,  `#define` ,  `#ifndef` 等，并删除注释行，还会添加行号和文件名标识。在编译时，编译器会使用上述信息产生调试、警告和编译错误时需要用到的行号信息。

经过预编译生成的 `.i` 文件不包含任何宏定义，因为所有的宏已经被展开，并且包含的文件也已经被插入到 `.i` 文件中。在上面的例子中，我们对 `main.c` 进行预处理，生成 `main.i` 预处理文件，命令如下。

```shell
gcc -o main.i -E main.c
```

生成的内容如下。

```
# 1 "main.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "main.c"
# 1 "print.h" 1



void print_something();
# 2 "main.c" 2

int main() {
    print_something();
}
```

## 编译

编译则是将预处理文件转换成汇编代码文件（`.S` 文件）的过程，具体的步骤主要有：词法分析 -> 语法分析 -> 语义分析及相关的优化 -> 中间代码生成 -> 目标代码生成。

我们生成 `main.c` 代码对应的汇编代码。命令如下，其中， `-masm=intel` 是为了生成 Intel 风格的汇编代码，否则默认 AT&T 风格的代码。

```shell
gcc -o hello.s -S hello.c -masm=intel
```

生成的 `hello.s` 内容如下。

```asm
	.file	"main.c"
	.intel_syntax noprefix
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	mov	eax, 0
	call	print_something
	mov	eax, 0
	pop	rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits
```

## 汇编

汇编阶段是将汇编代码编译成可重定位文件（Relocatable file）。汇编器（as）将 `main.S` 翻译成机器语言指令，把这些指令和其他附加的信息打包成可重定位文件（ `.o` 文件）。可重定位文件是一个二进制文件，它的字节编码是机器语言指令而不是字符。如果我们在文本编译器中打开可重定位文件，看到的将是一堆乱码，生成命令如下。

```shell
gcc -o main.o -c main.c
```

在 Linux 下，可重定位文件的格式是ELF文件格式，其包含了ELF头（ELF header）、程序头表（Program header table）、节（Section）和节头表（Section header table）等信息。

## 链接

链接阶段就是把若干个可重定位文件( `.o` 文件)整合成一个可执行文件（Executable file）。上面已经提到，正是因为函数的实现可能分布在多个文件中，所以在链接阶段，我们需要提供函数实现的位置，然后最终生成可执行文件。链接阶段是通过链接器(ld)完成的，链接器将输入的可重定位文件加工后合成一个可执行文件。这些目标文件中往往有相互的数据、函数引用。例如，我们在 `main.c` 的main函数中引用了 `print.c` 中 `print_something` 函数的实现。

我们可以使用gcc来进行链接，命令如下。

```shell
gcc -o main.o -c main.c
gcc -o print.o -c print.c
gcc -o main.out main.o print.o
```

链接完成后，我们使用命令 `./main.out` 即可执行。事实上，上面三条语句等价于

```shell
gcc -o main.out main.c print.c
```

生成的两个 `main.out` 也是完全一样的。

同学们可能会感到奇怪为什么预处理、编译、汇编和链接都是使用gcc，而不是使用as、ld等，这是因为gcc全名叫GNU Compiler Collection，是一个编译工具的集合，我们可以简单理解为gcc内部会自动调用as、ld等。在本例中，我们只需简单使用gcc来完成上述过程即可。

链接实际上可以分为静态链接和动态链接两种方式。上面呈现的链接方式是静态链接。在静态链接时，链接器将函数的实现代码从给出的可重定位文件或静态链接库中拷贝到最终的可执行程序中，形成一个文件。由于可执行程序可能链接了多个可重定位文件，而在每一个可重定位文件中，各个函数的地址又是独立的。为了将这些函数放到可执文件中，需要为其重新分配地址。因此，在静态链接时，链接器会做两件事，一是符号解析，把可执行程序中用到的函数的声明和其在可重定位中的实现联系起来；二是重定位，把函数的起始地址和内存地址对应起来，然后修改所有对函数的引用。

静态链接会把所有函数的实现都包含进最终的可执行文件中，但这会使得可执行文件的大小变得非常大。注意到可执行文件最终会被加载到内存中执行，有没有一种办法能够让可执行文件中的函数的实现不必包含进文件中，而是在被调用时才加载进内存？这样就能够大大减小可执行文件的大小。这种链接方式被称为动态链接。在编译的链接阶段，动态链接库只提供符号表和其他少量信息用于保证所有符号引用都有定义，保证编译顺利通过。动态链接器（ld.so）在运行过程中根据记录的共享对象的符号定义来动态加载共享库，然后完成重定位。在此可执行文件被执行时，动态链接库的全部内容将被映射到运行时相应进程的虚地址空间。动态链接程序将根据可执行程序中记录的信息找到相应的函数代码。 

两种链接方式各自都有优缺点。

静态链接的缺点很明显，一是浪费空间，因为每个可执行程序中对所有需要的目标文件都要有一份副本，所以如果多个程序对同一个目标文件都有依赖，如多个程序中都调用了 `printf` 函数，则这多个程序中都含有 `printf.o` ，所以同一个目标文件都在内存存在多个副本；另一方面就是更新比较困难，因为每当库函数的代码修改了，这个时候就需要重新进行编译链接形成可执行程序。但是静态链接的优点就是，在可执行程序中已经具备了所有执行程序所需要的任何东西，在执行的时候运行速度快。

动态链接的优点显而易见，就是即使需要每个程序都依赖同一个库，但是该库不会像静态链接那样在内存中存在多份副本，而是这多个程序在执行时共享同一份副本；另一个优点是，更新也比较方便，更新时只需要替换原来的目标文件，而无需将所有的程序再重新链接一遍。当程序下一次运行时，新版本的目标文件会被自动加载到内存并且链接起来，程序就完成了升级的目标。但是动态链接也是有缺点的，因为把链接推迟到了程序运行时，所以每次执行程序都需要进行链接，所以性能会有一定损失。

## 课后练习

1. 使用 `gcc` 将 `main.c` ， `print.c` 编译成 `main.S` ， `print.S` ，然后使用 `as` 将 `main.S` ， `print.S` 编译成 `main.o` ， `print.o` ，最后使用 `ld` 将 `main.o` ， `print.o` 链接成一个可执行文件来执行。
2. 使用 `gcc` 将 `print.c` 编译成静态链接库，然后使用 `gcc` 将 `main.c` 编译成可执行文件并在编译参数中指定静态链接库的位置。
3. 使用 `gcc` 将 `print.c` 编译成动态链接库，然后使用 `gcc` 将 `main.c` 编译成可执行文件并在编译参数中指定动态链接库的位置。