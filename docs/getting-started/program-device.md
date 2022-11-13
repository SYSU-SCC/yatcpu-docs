# 烧板验证

By: [:material-github: wu-kan](https://github.com/wu-kan)

## 生成比特流二进制文件


执行下述指令，可以根据 `verilog/basys3/Top.v` 生成二进制文件 `vivado/basys3/riscv-basys3/riscv-basys3.runs/impl_1/Top.bit`。

=== "Windows"
    假设你的 Vivado 安装目录是 `C:\Xilinx`（其他目录自行修改）：
    ```bash
    cd vivado\basys3
    C:\Xilinx\Vivado\2020.1\bin\vivado -mode batch -source generate_bitstream.tcl
    ```

=== "Linux"
    假设你的 Vivado 安装目录是 `~/Xilinx`（其他目录自行修改）：

    ```bash
    cd vivado/basys3
    ~/Xilinx/Vivado/2020.1/bin/vivado -mode batch \
        -source ./generate_bitstream.tcl
    ```

## 烧板

=== "Windows"
    假设你的 Vivado 安装目录是 `C:\Xilinx`（其他目录自行修改）：
    ```bash
    cd vivado\basys3
    C:\Xilinx\Vivado\2020.1\bin\vivado -mode batch -source program_device.tcl
    ```

=== "Linux"
    假设你的 Vivado 安装目录是 `~/Xilinx`（其他目录自行修改）：

    ```bash
    cd vivado/basys3
    ~/Xilinx/Vivado/2020.1/bin/vivado -mode batch \
        -source ./program_device.tcl
    ```

=== "WSL"
    由于 WSL 下可以调用 `powershell`，我们可以回到 Windows 下烧板。Windows 下可以只装 Vivado Lab，和完整版的 Vivado 相比只有烧板功能，精简很多，安装包体积约为 1 GB。

    ```bash
    # 假设你在本项目的 vivado 目录下，同时 Windows 下 Vivado Lab 2020.1 安装在 C:\Xilinx 目录下
    powershell.exe 'C:\Xilinx\Vivado_Lab\2020.1\bin\vivado_lab.bat -mode batch -source .\program_device.tcl'
    ```

后续将上述 `program_device.tcl` 换成 `generate_and_program.tcl` 可以将生成比特流和烧板在一个脚本中完成。

## 烧板结果

如果一切正常，在所有的开关处于下面的位置的时候，板上数码管应当显示 “SYSU” 的字母，如下图所示：

![SYSU on Board](images/board.png)
