# 配置 RISC-V 合规性测试

RISC-V 合规性测试仅支持使用 Makefile 与 gcc 编译器运行，如果你使用的是 Windows，那么你应当配置好 mingw 环境，以便运行测试。如果你已经配置好 Verilator，那么合规性测试应当也可以正常运行。

## 编译仿真器代码

参考[波形仿真一节](simulation.md#%E4%BD%BF%E7%94%A8-verilator-%E4%BB%BF%E7%9C%9F%E7%94%9F%E6%88%90%E6%B3%A2%E5%BD%A2%E6%96%87%E4%BB%B6)先编译出仿真器可执行程序。

## 克隆合规性测试仓库

合规性测试仓库位于 [https://github.com/riscv-non-isa/riscv-arch-test](https://github.com/riscv-non-isa/riscv-arch-test)，在和 YatCPU 同级（注意不要克隆到 CPU 项目文件夹内）目录下运行：

```
git clone https://github.com/riscv-non-isa/riscv-arch-test
```

克隆完成后，进入合规性测试仓库，配置环境变量，并编译运行测试：

```bash
cd riscv-arch-test
export TARGET_SIM=~/yatcpu/verilog/verilator/obj_dir/VTop
export TARGETDIR=~/yatcpu/riscv-target
export RISCV_TARGET=yatcpu
make
```