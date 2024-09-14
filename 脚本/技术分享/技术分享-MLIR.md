[MLIR Toy Tutorial 概述 - 张洪滨 - 20200226 - PLCT实验室技术分享_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1p7411K7jd/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074) 

[MLIR & Python Binding 简介 - 20200205 - PLCT研究生张洪滨_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1m7411H7GR/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074)

[MLIR GSoC Proposal 分享 - 张洪滨同学 - 20200506 - PLCT实验室_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1CV411d7Te/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074) 

[张洪滨 - ONNX-MLIR LeNet 端到端编译过程 - 20210221 - OSDT Meetup_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Vr4y1A7Cg/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074) 

[张洪滨 - 一年一度 MLIR 分享 - 20211217 - PLCT OpenDay 2021_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1K44y1J7fX/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074) 

[207-李枫-IREE--基于MLIR的端对端机器学习编译器和运行时_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV12T411R7nc/?spm_id_from=333.999.0.0&vd_source=def395bf81634ebee1a6e15ae3527074) 



分类：MLIR

观看顺序：从简单到困难



# 技术分享视频1



### 前期知识点

TensorFlow是什么



LLVM 是什么



XLA 是什么



Dialect 是什么



MLIR 是什么，解决了什么问题



从 TensorFlow 到 LLVM 的逐级下降过程



#### 视频内容

```
Multi-Level Intermediate Representation
```



- MLIR 是什么

虽然不是 ML，但却是为了 ML 而生的，一个可重用可扩展的开源代码优化框架，辅助 tenserflow 



- MLIR 的功能

处理软件碎片化，为面向多种硬件的编译器提供支持，为领域专用编译器的开发减少开销

连接已有的编译器

**逐级抽象，代码优化，避免组合爆炸**



#### 从源文件到表达式的生成

初始是对 tensorflow 的支持 2017

从 TensorFlow 图变为 XLA Graph，从 XLA 再变为 LLVM 再调用各种后端



- Toy Tutorial 的目录介绍



# 技术分享视频2

介绍从 .toy 源文件开始生成 AST到 MLIRGen

在 MLIRGen 之后的优化 pass 功能

PassManger 在不同下降阶段的模块，下降优化的方面





# 技术分享视频3



#### 视频内容

演示了逐级下降过程

- MLIR 的产生

10'20	MLIRGen 模块 ->  Dialect 模块 -> TableGen 模块



- TableGen 模块的好处 & 如何开发一个简单的 TableGen



- Google 编程之夏的介绍

MLIR与 Python Binding 的工作