# 实验一 单周期 CPU

## 程序计数器

首先我们来实现 CPU 中最简单的部件之一，程序计数器（Program Counter, PC）。在单周期 CPU 中，程序计数器在每一个周期只需要将当前指令地址 +4 （RISC-V 非变长指令长度），并将结果写入寄存器中保存并输出即可。另外，为了处理分支命令，我们还需要接收控制单元送入的跳转信号以及跳转地址，如果跳转信号使能，我们就需要将跳转目的地址写入寄存器中。

模块的输入信号以及输出信号已经定义好，相关代码文件位于 `src/main/scala/riscv/core/ProgramCounter.scala`，请在 `// Lab1(ProgramCounter)` 注释处填入代码，使其能通过 `ProgramCounterTest` 测试。

## 寄存器堆

CPU 在进行运算时，需要将结果或状态临时保存在寄存器中。通常，CPU 含有一定数量的通用寄存器（General Purpose Registers, GPR），这些寄存器组合在一起成为寄存器堆（Register File），我们可以给每个寄存器分配一个编号作为寄存器地址，并通过这个编号访问指定的寄存器。RISC-V 中规定的 GPR 有 32 个，编号分别为 0-31，其中 0 号寄存器永远返回 0 值。

寄存器堆提供两个读端口（对应两个源操作数）以及一个写端口（对应一个目的寄存器），每个读端口可以输入寄存器地址并输出对应寄存器数据。写端口输入寄存器地址、写使能信号以及写入数据，如果写使能信号激活，就将写入数据写入寄存器中，否则不进行操作。需要特别注意的是，如果一个寄存器，在同一个周期，既作为源操作数，又作为目的寄存器，则需要通过**转发**的方法，将写入端口的数据输出。

模块的输入信号以及输出信号已经定义好，相关代码文件位于 `src/main/scala/riscv/core/RegisterFile.scala`，请在 `// Lab1(RegisterFile)` 注释处填入代码，使其能通过 `RegisterFileTest` 测试。

## 译码器

当指令从存储器取出后，需要译码器将指令译码，输出适当的控制信号。译码器负责按照指令格式通过纯组合逻辑的方法，解码出对应的控制信号、操作数等信息，并输出。

模块的输入信号以及输出信号已经定义好，相关代码文件位于 `src/main/scala/riscv/core/InstructionDecode.scala`，请在 `// Lab1(InstructionDecode)` 注释处填入代码，使其能通过 `InstructionDecodeTest` 测试。

## ALU

在执行阶段，ALU 负责计算并将结果输出，为纯组合逻辑。

模块的输入信号以及输出信号已经定义好，相关代码文件位于 `src/main/scala/riscv/core/ALU.scala`，请在 `// Lab1(ALU)` 注释处填入代码，使其能通过 `ALUTest` 测试。

## 提交 Autograder

完成以上所有实验并通过相应测试后，请将以下文件打包为 `.zip` 文件，并上传到 Autograder 进行进一步的测试：

- `src/main/scala/riscv/core/ProgramCounter.scala`
- `src/main/scala/riscv/core/InstructionDecode.scala`
- `src/main/scala/riscv/core/RegisterFile.scala`
- `src/main/scala/riscv/core/ALU.scala`

请确保 Autograder 提交界面显示的完整文件路径包含上述文件路径且完全一致。多余的文件可以上传但将被忽略。