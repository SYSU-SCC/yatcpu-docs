# 快速测试

By: [:material-github: wu-kan](https://github.com/wu-kan)

执行下述指令可以模拟运行本项目的测试，首次运行时需要联网自动下载必要的组件。

```bash
# git clone git@github.com:SYSU-SCC/YatCPU.git
# cd YatCPU
sbt test
```

如果成功执行，你会看到类似这样的输出。

```bash
[success] Total time: 385 s (06:25), completed Dec 15, 2021, 8:45:25 PM
```

输入如下指令，可以编译生成 verilog 代码 `verilog/Top.v`。

```bash
sbt run
```