.. rvgpu documentation master file, created by
   sphinx-quickstart on Tue May 23 14:44:14 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

RVGPU
==============================

RVGPU是一个基于RISC-V指令集实现的开源GPU项目，项目目标是提供一套完整的基于RISC-V指令集的开源GPU软硬件方案。

代码仓库：https://gitee.com/rvgpu

文档：https://rvgpu.gitee.io/pages

目前已经发布 v0.2版本，可以支持简单的cuda程序在cmodel上运行。

项目包含的代码仓库如下：
::

   .
   └── rvgpu                  项目的顶层仓库，通过git submodule的方式来管理所有子项目
       ├── docs               项目文档
       ├── gvm                gpu runtime的实现
       ├── kmod-drv           内核态驱动
       ├── qemu               实现一个虚拟的gpu设备
       ├── rvgpu-cmodel       gpu的C模型，用来实现指令集、架构的仿真。
       ├── rvgpu-llvm         编译器工具链，用于实现cuda编译以及rvgpu后端  
       ├── rvgpu-mesa         OpenGL、Vulkan的实现
       └── tools              一些开发工具

项目将实现的框架如下，目前还处于早期阶段，大多数代码还在开发过程中：

.. image:: ./guide/pics/rvgpu_softstack.PNG
   :scale: 75 %

Release Notes
=================================
.. toctree::
   :maxdepth: 1

   release/notes

使用手册
=================================
.. toctree::
   :maxdepth: 1

   guide/build

开发文档
=================================
.. toctree::
   :maxdepth: 1

   develop/developer
   develop/rvgpu/command_stream
   develop/mesa/environment

参考资料
=================================
.. toctree::
   :maxdepth: 1

   docsref/docsref
