# 处理器架构

By: [:material-github: howardlau1999](https://github.com/howardlau1999)

本项目支持的指令集是 [RISC-V](https://riscv.org/) 的 RV32I 整数运算指令集。微架构设计上，采用的是和 [Z-scale](https://github.com/ucb-bar/zscale) 以及 [riscv-mini](https://github.com/ucb-bar/riscv-mini) 类似的三级流水线单发射顺序执行架构。流水线总体架构分为取指（IF）、译码（DE）、执行/写回/访存（EX、WB、MEM）三个阶段，如下图所示：

![pipeline](images/pipeline.png)

除寄存器堆以外，本项目默认外部存储设备为**同步读写**模式，也就是只有在时钟上升沿到来时数据才会发生变化，因此所有访存指令都需要提前一个时钟周期发出地址信号，并最快在下一个时钟周期取得数据。