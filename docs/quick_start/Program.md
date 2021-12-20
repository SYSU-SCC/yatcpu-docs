# 烧板验证

By: [:material-github: wu-kan](https://github.com/wu-kan)

## 生成二进制文件

假设你的 Vivado 安装目录是 `~/Xilinx`（其他目录自行修改），执行下述指令，可以根据 `verilog/Top.v` 生成二进制文件 `vivado/riscv-basys3/riscv-basys3.runs/impl_1/Top.bit`。

```bash
cd vivado
~/Xilinx/Vivado/2020.1/bin/vivado -mode batch -source ./generate_bitstream.tcl
```

## 烧板

由于 WSL 下暂时不可以使用 USB 设备，因此不可以直接烧板。解决方案有两个。

### 方案一：使用 USB 方式烧板（WIP）

需要一个 Fat32 格式的 U 盘。

### 方案二：在 Windows 下烧板

由于 WSL 下可以调用 powershell，我们可以回到 Windows 下烧板。Windows 下可以只装 Vivado_Lab，和完整版的 Vivado 相比只有烧板功能，精简很多。

```bash
# 假设你在 vivado 目录下，同时 Windows 下 Vivado_Lab 安装在 `C:\Xilinx`
powershell.exe 'C:\Xilinx\Vivado_Lab\2020.1\bin\vivado_lab.bat -mode batch -source .\program_device.tcl'
```

中山大学校园网内下载 Vivado_Lab 安装包，也可以使用我们提供的的镜像。
