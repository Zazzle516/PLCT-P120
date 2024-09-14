## 懂点 RV - 3.1

视频目的：常见的 RISCV 模拟器介绍



先从官方的模拟器 spike 介绍开始，然后介绍 qemu，最后是 rars，



## 文稿

如果我们想得到程序在 RISC-V 架构上的运行结果但是又没有相应的硬件设备该怎么办呢

我们可以在本机上安装模拟器来实现，有哪些针对 RISCV 的模拟，它们又有什么特点呢



首先，RISCV 基金会提供了官方的 RISCV 模拟器 —— Spike

由 Berkeley Architecture Research（BAR）开发，和 RISC-V 架构更新同步，立刻体验最新特性并测试

但是大部分情况还是研究导向，在性能上稍差也不适合非常复杂的仿真



那么，从高性能的角度，有没有匹配的模拟器呢，这就说到大名鼎鼎的 qemu 了，在使用硬件加速后甚至可以提供到接近原生硬件的性能，但是高性能带来的自然是配置复杂



可是我只想以低成本的方式看看 RISC-V 是什么，真的要用到这么复杂的模拟器吗

只需要安装 Java 环境，就可以体验到可视化 RISC-V 模拟器 RARS，随着程序一步一步执行，你可以随时点开选项卡查看寄存器和内存的内容



只是看看运行仍然不理解？也可以自己写一个 RISC-V 模拟器，what I can't create I ca't understad！



## 参考文献

spike

[riscv-software-src/riscv-isa-sim: Spike, a RISC-V ISA Simulator (github.com)](https://github.com/riscv-software-src/riscv-isa-sim)

qemu

[RISC-V System emulator — QEMU documentation](https://www.qemu.org/docs/master/system/target-riscv.html) 

[QEMU / QEMU · GitLab](https://gitlab.com/qemu-project/qemu) 

自己写一个模拟器

[weijiew/remu: 使用 C++23 从零实现的 RISC-V 模拟器 (github.com)](https://github.com/weijiew/remu) 

