# 自学路径

本文档提供了四个实验以及其文档。在完成实验前，需要对 RISC-V 指令集以及 Chisel 3 语言有一定了解，并且需要在本地配置好开发环境进行实验。下面是一个自学的参考路线，如果你已经掌握了相关的知识和技能可以直接跳过。

1. 阅读学习 RISC-V 设计思想以及指令集格式
  - The RISC-V Reader: An Open Architecture Atlas（RISC-V 手册：一本开源指令集的指南） [[PDF](http://riscvbook.com/chinese/RISC-V-Reader-Chinese-v2p1.pdf)]
  - RISC-V Green Card [[PDF](http://riscvbook.com/greencard-20181213.pdf)]
2. 动手实践编写 RISC-V 汇编程序加深理解
  - 在线 RISC-V 整数指令集解释器 [[Web](https://www.cs.cornell.edu/courses/cs3410/2019sp/riscv/interpreter/)]
3. 在线实践学习 Chisel 3 语言
  - Chisel 语言在线互动教程 [[Web](https://mybinder.org/v2/gh/freechipsproject/chisel-bootcamp/master)]（只需要完成 3.6 及以前的部分即可）
  - 直接在线运行即可，不需要在本地配置环境
  - 重点学习基本语法以及测试用例的编写
4. 配置并验证本地开发环境
  - 先按照[配置开发环境](./getting-started/environment.md)一节根据自己的操作系统进行配置，需要从境外下载一些依赖包，可能需要代理
  - 然后按照[快速测试](./getting-started/test.md)测试自己的开发环境是否配置成功，需要能够通过测试
  - 按照[波形仿真](./getting-started/simulation.md)一节进行波形仿真，需要能够生成波形文件并使用相关软件打开
5. 按照实验文档完成实验
  - 实验涉及到的代码在 [https://github.com/hrpccs/2022-fall-yatcpu-repo/](https://github.com/hrpccs/2022-fall-yatcpu-repo/) 下载，按照文档要求补充完整缺失的代码并通过测试即可
  - 提供的测试可能不全面，鼓励自己编写更多的测试用例保证正确性