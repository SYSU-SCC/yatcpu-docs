# 波形仿真

By: [:material-github: howardlau1999](https://github.com/howardlau1999)

在烧板验证之前，可以使用波形仿真再进行一次测试。你可以使用 Vivado 或者 Verilator 进行波形仿真。使用 Verilator 仿真需要安装 C++ 编译器。

## 生成波形文件

### 测试时生成波形文件

在运行测试时，如果设置环境变量 `WRITE_VCD` 为 `1`，则会生成波形文件。

=== "Linux/macOS"
    ```bash
    WRITE_VCD=1 sbt test
    ```

=== "Windows"
    Powershell:
    ```powershell
    $Env:WRITE_VCD=1; sbt test
    ```
    命令提示符:
    ```cmd
    set WRITE_VCD=1
    sbt test
    ```

=== "Intellij IDEA"
    先点击右上角三角形左边的下拉菜单，点 “Edit configurations...”，如果没有就先运行一次测试。 
    ![](images/write-vcd-idea-1.png)
    在 “Environment Variables” 选项中，添加一个环境变量，名称为 `WRITE_VCD`，值为 `1`。
    ![](images/write-vcd-idea-2.png)
    之后点 OK 保存即可。

之后可以在 `test_run_dir` 目录下的各个子目录中找到 `.vcd` 文件，使用 GTKWave 打开即可，参考[查看波形文件](#查看波形文件)一节。
### 使用 Vivado 仿真生成波形文件

确保你的 PATH 路径中包含 Vivado 的安装目录，然后运行命令：

```bash
make vivado-sim
```

将会生成 `vivado/riscv-basys3/riscv-basys3.sim/sim_1/behav/xsim/dump.vcd` 文件。

## 查看波形文件
### 使用 GTKWave 查看 VCD 格式波形

不同软件生成的 VCD 文件路径不一样：

- Vivado: `vivado/riscv-basys3/riscv-basys3.sim/sim_1/behav/xsim/dump.vcd`
- 测试时生成：`test_run_dir` 目录下的各个子目录中的 `.vcd` 文件

如果你的操作系统带有 GUI 图形界面，可以使用 [GTKWave](http://gtkwave.sourceforge.net/)，点击 "File->Open New Tab..." 打开对应的文件查看波形。

![gtkwave-windows](images/gtkwave.png)

### 使用 Vivado 查看 WDB 格式波形

你也可以打开 Vivado 来查看波形，Vivado 支持的格式为 `.wdb`，路径是 `vivado/riscv-basys3/riscv-basys3.sim/sim_1/behav/xsim/test_behav.wdb`。在启动 Vivado 后，选择菜单栏的 "Flow->Open Static Simulation..."，选择这个文件，然后点击 "Open" 按钮。

![vivado-1](images/vivado-1.png)

![vivado-2](images/vivado-2.png)

打开后，在左侧的窗口找到你感兴趣的模块，右键，选择 "Add to Wave Window"，即可在波形窗口查看波形。

![vivado-3](images/vivado-3.png)

![vivado-4](images/vivado-4.png)