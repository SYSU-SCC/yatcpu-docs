# 开发环境

By: [:material-github: wu-kan](https://github.com/wu-kan)

本项目主要搭建在 Linux 或 WSL （Windows Subsystem for Linux）环境中。~~Windows 环境和 MacOS 理论上也可以，但需要读者自行摸索。~~[这里](https://liuhaohua.com/server-programming-guide/appendix/build-env/)给出搭建相关环境的一个参照。

这里假设你使用的 Linux 或 WSL 系统是 Debian 11。对于使用其他 Linux 系统的同学，操作是类似的，相信你有足够的能力参考下面的指令搭建环境。

## 必要工具

```bash
sudo apt install git  # 用于将本项目 clone 下来
sudo apt install gcc-riscv64-unknown-elf # 用于生成 risc-v 的测试指令
sudo apt install make # build-essential
sudo apt install curl gnupg2 # 下载工具
sudo apt install scala # 本项目的语言
sudo apt install libtinfo5 # vivado 需要一个这个，否则启动报错
```

## 包管理器 SBT

[sbt](https://packages.debian.org/sid/sbt)目前仍然在 Debian 的[不稳定](https://www.debian.org/releases/sid/)版本中。对于不希望使用 sid 系统的同学，可以按照[文档](https://www.scala-sbt.org/1.x/docs/zh-cn/Installing-sbt-on-Linux.html#Ubuntu%E5%92%8C%E5%85%B6%E4%BB%96%E5%9F%BA%E4%BA%8EDebian%E7%9A%84%E5%8F%91%E8%A1%8C%E7%89%88)的指示下载安装。

```bash
echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | sudo tee /etc/apt/sources.list.d/sbt.list
echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | sudo tee /etc/apt/sources.list.d/sbt_old.list
curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo apt-key add
sudo apt update
sudo apt install sbt
```

## Vivado

中山大学组成原理实验课使用的 [Basys3](https://www.stepfpga.com/doc/_media/basys3_ss.pdf) 实验板型号为 XC7A35T，需要 Xilinx 的工具进行烧板。这里安装 `Vivado HL WebPACK`即可，不需要许可或激活许可密钥。

中山大学校园网内下载 Vivado 安装包，可以使用我们提供的镜像。

```bash
curl -O https://mirrors.matrix.moe/software/Xilinx_Unified_2020.1_0602_1208.tar.gz
# 也可以在 Xilinx 的官网下载
# <https://china.xilinx.com/support/download/index.html/content/xilinx/zh/downloadNav/vivado-design-tools/archive.html>
tar -zxf Xilinx_Unified_2020.1_0602_1208.tar.gz
cd Xilinx_Unified_2020.1_0602_1208
./xsetup -b ConfigGen
```

先输入 `2` 再输入 `1`，这时会选中 `Vivado HL WebPACK`。最后会产生一个默认配置文件 `~/.Xilinx/install_config.txt`。

随后再次执行下述指令，安装`Vivado HL WebPACK`。

```bash
./xsetup -a XilinxEULA,3rdPartyEULA,WebTalkTerms -b Install -c ~/.Xilinx/install_config.txt -l ~/Xilinx
```

其中`~/Xilinx`是安装目录，可以自行定义。
