Release Notes
=======================

Next: v0.2.2
#######################

- 解决cuda中调用数学库问题

v0.2.1
#######################
2023-10-20
https://gitee.com/rvgpu/rvgpu/tree/v0.2.1

- 解决了cuda中调用kernel函数的传参问题
- 在CModel中添加MMU实现

v0.2
#######################
2023-10-12 
https://gitee.com/rvgpu/rvgpu/tree/v0.2

- 支持简单的CUDA程序运行
- 添加LLVM编译器功能，支持将.cu程序编译成可执行文件，device端指令为RVGPU指令
- 实现简单CUDA Runtime动态库，
- 实现GVM，封装将device可分发给Physical和Simulator设备

v0.1  
#######################
2023-09-16 
https://gitee.com/rvgpu/rvgpu/tree/v0.1

- 实现支持RISCV64GC 指令集的CModel
- 支持基于C语言的图形应用程序
