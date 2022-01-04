# 快速测试

By: [:material-github: wu-kan](https://github.com/wu-kan)

执行下述指令可以下载代码并模拟运行本项目的测试，首次运行时需要联网自动下载必要的组件。

```bash
git clone https://github.com/howardlau1999/yatcpu 
cd yatcpu
sbt test
```
???+tips "设置代理"
    如果下载失败或者网络不稳定，可以尝试设置代理。请根据自己的实际情况填写代理地址。
    ```bash
    export HTTPS_PROXY=http://127.0.0.1:1080
    export JAVA_OPTS="$JAVA_OPTS -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=1080"
    ```

如果成功执行，你会看到类似这样的输出。

```bash
[success] Total time: 385 s (06:25), completed Dec 15, 2021, 8:45:25 PM
```

输入如下指令，可以编译生成适用于 Basys 3 开发板的 Verilog 代码 `verilog/basys3/Top.v`。

```bash
make basys3
```