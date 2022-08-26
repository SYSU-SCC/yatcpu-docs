# 烧板验证

By: [:material-github: wu-kan](https://github.com/wu-kan)

## 生成二进制文件

假设你的 Vivado 安装目录是 `~/Xilinx`（其他目录自行修改），执行下述指令，可以根据 `verilog/Top.v` 生成二进制文件 `vivado/riscv-basys3/riscv-basys3.runs/impl_1/Top.bit`。

```bash
cd vivado
~/Xilinx/Vivado/2020.1/bin/vivado -mode batch \
    -source ./generate_bitstream.tcl
```

## 烧板

由于 WSL 下暂时不可以使用 USB 设备，因此不可以直接烧板。解决方案有两个。

### 使用 USB 方式烧板

需要一个 FAT32 格式的 U 盘。

### 在 Windows 下烧板

由于 WSL 下可以调用 `powershell`，我们可以回到 Windows 下烧板。Windows 下可以只装 Vivado Lab，和完整版的 Vivado 相比只有烧板功能，精简很多，安装包体积约为 1 GB。

```bash
# 假设你在本项目的 vivado 目录下，同时 Windows 下 Vivado Lab 2020.1 安装在 C:\Xilinx 目录下
powershell.exe 'C:\Xilinx\Vivado_Lab\2020.1\bin\vivado_lab.bat -mode batch -source .\program_device.tcl'
```

<!--
**中山大学校园网内**下载 Vivado Lab 安装包，也可以使用我们提供的的镜像：[点此下载](https://mirrors.matrix.moe/software/Xilinx_Vivado_Lab_Win_2020.1_0602_1208.tar.gz)
-->

## 烧板结果

如果一切正常，在所有的开关处于下面的位置的时候，板上数码管应当显示 “SYSU” 的字母，如下图所示：

![SYSU on Board](images/board.png)
