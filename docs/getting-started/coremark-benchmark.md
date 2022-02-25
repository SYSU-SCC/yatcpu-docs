# 配置 CoreMark 性能基准测试

CoreMark 是一套用于衡量 MCU 和 CPU 等设备的计算性能的基准测试，包含矩阵乘法、链表操作等测试。CoreMark 同样只能使用 Makefile 进行编译，但可以使用 Clang 编译器编译。

## 编译波形仿真程序

CoreMark 编译出来可以像我们自己写的程序一样加载到 CPU 运行，也可以先在电脑模拟仿真。如果想在电脑模拟仿真，需要先参考[波形仿真一节](simulation.md#%E4%BD%BF%E7%94%A8-verilator-%E4%BB%BF%E7%9C%9F%E7%94%9F%E6%88%90%E6%B3%A2%E5%BD%A2%E6%96%87%E4%BB%B6)先编译出仿真可执行程序。

## 克隆 CoreMark 仓库

这里同样将 CoreMark 仓库克隆到和我们 CPU 项目同级：

```bash
git clone https://github.com/eembc/coremark
```

克隆完成后，将 CPU 项目内的可移植文件拷贝到 CoreMark 目录中（或创建软连接），然后编译 CoreMark 可执行文件，使用仿真器加载并运行：

```bash
cp -r ~/yatcpu/coremark/yatcpu ~/coremark
cd ~/coremark
make PORT_DIR=yatcpu
~/yatcpu/verilog/verilator/obj_dir/VTop -instruction coremark.bin.asmbin -time 10000000000
```

运行时间会比较长，大约需要 6 分钟才有输出。