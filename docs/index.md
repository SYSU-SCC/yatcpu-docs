# 前言

By: [wu-kan](https://github.com/wu-kan)

YatCPU (**Y**et **a**nother **t**oy CPU，逸芯) 是一款开源、开发中的教学用 RISC-V 处理器，基于 Chisel 硬件设计语言实现，并用于中山大学 (Sun **Yat**-sen University) 计算机学院冯班组成原理实验课程的教学。同样欢迎其他高校相关课程使用！

此处同样给出[我们在课堂上的展示](assets/slides.pdf)。

## Why another CPU

为了回答这一问题，需要先介绍一下中大往年组成原理实验课程的情况。学生需要在半个学期的时间里，从零开始实现一个 MIPS32 指令集子集的 CPU，分别完成[单周期](https://wu-kan.cn/2018/11/23/%E5%8D%95%E5%91%A8%E6%9C%9FCPU%E8%AE%BE%E8%AE%A1/)到[五周期](https://wu-kan.cn/2018/12/23/%E5%A4%9A%E5%91%A8%E6%9C%9FCPU%E8%AE%BE%E8%AE%A1/) CPU 的实现并由助教验收，少数能力强的同学可以将其扩展到流水线。计组实验被称作计院学生的第一道门槛，相较于大一的实验课程挂科率飙升，每年都有很多学生重修。

然而，门槛高的同时，这门课的上限却并没有很高。一届又一届的卷王做完多周期、流水线之后开始卷各种细节，如指令集已经从我那时的十几条增加到了五十多条，甚至于到了卷报告的程度（19 级第一名的实验报告已达三万字，五十余页）；增加了大量工作量却并不给人带来提升。

与此同时，我也在各种新闻上看到国内各大高校对计组课程的改革（“在自己写的 CPU 上跑自己写的操作系统”），心中很难不产生一些羡慕。在大四的某一天，我得知 [NelsonCheung-cn](https://github.com/NelsonCheung-cn) 开源了 [Yat-sen OS](https://github.com/NelsonCheung-cn/yatsenos-riscv)，并成功用在下一级操作系统实验课上。回想起自己前两年在操统实验（和计组实验课上）遭受的折磨，意识到自己也可以做一个 TA，以一个全新的身份对待之前的实验。怀着这种想法，我报名并成为了第一届[冯班](http://news2.sysu.edu.cn/news01/1375374.htm)计组课的助教。

### 现状与积弊

限于时间原因，在第一届实验课上我并没有对往年的实验做过多改造，只是调整了[实验要求](https://wu-kan.cn/2021/11/16/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BB%84%E6%88%90%E5%8E%9F%E7%90%86%E5%AE%9E%E9%AA%8C%E8%AF%BE%E8%AF%BE%E4%BB%B6/)，将课程的指令集从 MIPS32 改为 RV32I。通过观察和自己做实验的经历，主要找到以下几个实验课程中存在的问题：

1. 实验内容贫乏
    - 花费过多时间在从零开始实现单周期 CPU 上
    - 仅实现 CPU，未实现其他部件
2. 实验内容割裂
    - 没有和其他课程（如操作系统）“贯通”
        - 同学们此时刚学完程序设计，难以理解与组成原理的关联
3. Verilog 上手难度高
    - 中大的计组课程开设在大二上学期，早于大部分高校
    - 数字电路课程中使用 FPGA 是选做内容，半数以上的同学并没有相关经验
4. Vivado GUI 使用体验差
    - 综合与实现过程冗长且容易出错
    - Debug 与实验检查都只能看波形图
        - 作为检查实验的 TA，非常痛苦！
        - 做实验的同学更痛苦！

### 动机

基于上述问题，我们希望面向如下目标改善课程实验：

1. 上手难度低、开发效率高
    - 更简单的开发语言
        - Chisel 开发相同功能相较于 Verilog 减少数倍
    - 更丰富的工具链
2. 快速反馈、激发兴趣
    - 更快速的评测方式
    - 容易做出小的成果
    - 和其他课程“打通”

## Feature and Future

最终，我们决定开发一款尽可能简单的 RISC-V CPU（类似于 [tinyriscv](https://gitee.com/liangkangnan/tinyriscv/)），主要面向**本科低年级同学**，也即对读者的知识水平要求仅为有基本的数字电路以及 C 语言基础。

简单却不简陋，我们同时希望尽可能多地扩展其功能性（倒并不是说完全不考虑性能和节能了，只是不是当前的开发重点），并在此之上设计更多有趣且能加深同学们对体系结构理解的实验。截至目前为止，YatCPU 项目包含或计划包含以下特征：

- [x] 使用 Chisel 实现
- [x] RV32I 指令集
    - [ ] 增加 RV64 等更多指令集
- [x] 三级流水线
- [x] 支持全 Linux/WSL 开发环境
    - [x] 自动化评测
    - [x] 自动化烧板
- [x] 支持高校数电教学常用的 [Basys3](https://digilent.com/reference/programmable-logic/basys-3/start) 开发板
    - [ ] 移植到更多的开发板，如 [ZedBoard](https://digilent.com/reference/programmable-logic/zedboard/start) 等
- [x] VGA 输出，显示更多的调试信息
- [x] 运行 C 语言编译的程序
    - [x] 通过 RISC-V 指令兼容性测试
    - [x] 运行 CoreMark
- [ ] 启动操作系统
    - [ ] [FreeRTOS](https://www.freertos.org/)
    - [ ] [Yat-sen OS](https://github.com/NelsonCheung-cn/yatsenos-riscv)
    - [ ] [OpenHarmony LiteOS-A](https://gitee.com/openharmony/kernel_liteos_a)
    - [ ] [Linux Kernel](https://www.linux.org/)
    - [ ] [Debian](https://www.debian.org/)
- [ ] 完善且适用于零基础新生的文档
- [ ] 基于 YatCPU 设计更多体系结构实验
    - [ ] 缓存
    - [ ] 微码架构
    - [ ] 多发射
    - [ ] 乱序执行
    - [ ] 分支预测
    - [ ] 多核
    - [ ] 缓存一致性
    - [ ] 向量处理器

最后要感谢[广爷](http://cse.sysu.edu.cn/content/5357)和他的同学们，可以让我在课上“胡作非为”。还要由衷感谢 [howardlau1999](https://github.com/howardlau1999)，支撑了 YatCPU 前期开发的绝大部份工作量（毫不夸张地说，白白在三天内完成了我原计划一个月的工作量）。
