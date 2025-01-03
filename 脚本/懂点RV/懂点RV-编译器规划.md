## 懂点 RV - 编译器部分

### 项目概述

- 项目名称：懂点RV-编译器短视频教学系列
- 项目目标：在轻松有趣的视频中，快速普及编译器相关的内容，吸引初学者学习



### 教学内容规划

#### 1：编译器简介

- 介绍编译器的基本概念和作用。
- 编译器在哪里生效的：源代码 -> 目标代码 -> 可执行文件。
- 提到编译器的不同阶段：词法分析、语法分析、中间代码生成、优化、目标代码生成等。



#### 2：编译器的基本结构和工作原理

- 详细介绍编译器的各个阶段：词法分析、语法分析、中间代码生成、优化和目标代码生成。
- 使用简单的图示或动画展示各阶段的工作流程。
- 说明每个阶段的输入和输出。

这个地方，比如输入字符串 Arg 的词法分析，Token 流的语法分析可以用 PPT 动画的方式进行演示

右边画一个 AST 树然后通过指针链接到对应的 Token 位置



#### 3：GCC 与 LLVM

##### GCC

- 介绍 GCC（GNU Compiler Collection）的基本信息。

- 演示一个简单的 C 程序的编译过程：
  ```bash
  gcc -o hello hello.c
  ```
  
- 提到 GCC 的一些常用选项，如 `-c`、`-o`、`-Wall` 等。



##### LLVM

- 演示 LLVM 通过脚本的安装过程

- 解释 LLVM 与 GCC 的区别和优势    主要是前后端分离

- 简述 LLVM 的模块化架构和中间表示（IR）

- 演示一个简单的 C 程序的编译过程：
  ```bash
  clang -o hello hello.c
  ```
  
  

#### 4：rvcc 编译器的学习例子

- 演示 rvcc 的环境配置
- 简述 rvcc 的目标和特点
- 可以介绍一下 `rvcc.h` 文件，涉及到的一些结构体



#### 5：从源代码到可执行文件的过程

**主题：** 介绍 rvcc 的详细代码，录制具体的 debug 过程

比如在 视频4 中介绍的那些结构体是如何生效的，如何生成汇编文件，最后测试执行



#### 6：开源社区贡献  + 其他学习资源

LLVM

rvcc-course

等等