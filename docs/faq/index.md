# 常见问题

By: [:material-github: howardlau1999](https://github.com/howardlau1999)

!!! faq "为什么我在 Chisel 里编写的模块/IO 端口/寄存器在生成的 Verilog 代码里找不到？"
    由于 Chisel 是先生成 FIRRTL 代码，然后对 FIRRTL 代码进行逻辑简化/常量传播/死代码消除等优化，最后再根据优化后的 FIRRTL 代码生成 Verilog 代码，如果你在 Chisel 中编写的模块/IO 端口/寄存器在生成的 Verilog 代码里找不到，请检查一下你是不是忘记连接模块，或者你的逻辑是否有问题，导致某个寄存器的值为常量等。

!!! faq "报错 `Exception in thread "main" firrtl.passes.CheckInitialization$RefNotInitializedException: @[Top.scala 22:19] : [module Top] Reference cpu is not fully initialized.: cpu.io.interrupt_flag <= VOID` 是什么意思？"
    说明你的这个模块的 IO 端口悬空了，或者在某些逻辑分支里没有赋值。请检查一下报错涉及的端口是不是在所有可能的逻辑分支里都被赋值/连接了。