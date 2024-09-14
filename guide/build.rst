RVGPU 构建和使用
=======================

1. 如何运行
#######################

1.1 环境
***********************

实验操作系统为：ubuntu22.04

依赖第三方软件：
::

    # 编译LLVM 依赖
    sudo apt install git cmake python3 python-pip

    sudo pip3 install ninja

    # 编译CModel 依赖
    sudo apt install pkg-config libdrm-dev libgtest-dev

1.2 编译源码
***********************
项目整体使用git submodule的方式管理，所以只需要clone顶层仓库就可以拉取所有的代码：
::

    git clone https://gitee.com/rvgpu/rvgpu.git

目前v0.2.1的tag分支可以使用，最新的代码还在努力开发中，代码切换到v0.2.1的tag上，并且初始化子模块。

务必切换到v0.2.1分支上，子模块也从v0.2.1初始化，其他分支编译不过：
::

    cd rvgpu
    git checkout -b v0.2.1 tags/v0.2.1

    git submodule init 
    git submodule update


涉及到的构建代码比较多，所以在tools仓库下实现了一个构建脚本，可以编译安装所有需要的代码仓库：
::

    cd rvgpu
    ./tools/build/build.sh

编译时间较长，正确执行完脚本后，将安装到当前路径下的install目录下：
::

    qihangkong@st-ubuntu:~/git/rvgpu$ ls install/
    bin  include  lib  libexec  share

1.3 使用Docker镜像编译
***********************
使用打包好的Docker镜像进行编译, Dockerfile位于：https://gitee.com/rvgpu/tools/blob/main/docker/Dockerfile
::

    docker image build -t rvgpu/rvgpu:v0.2.1 .
    docker run -it rvgpu/rvgpu:v0.2.1

启动镜像后，rvgpu的仓库位于：/root/rvgpu，编译使用如下的命令：
::

    cd /root/rvgpu
    ./tools/build/build.sh

1.4 运行cuda程序
***********************
cuda程序mul.cu如下：
::

    // file: mul.cu
    #include <iostream>

    __global__ void mul(int a, float* x, float* y) {
        y[threadIdx.x] = a * x[threadIdx.x];
    }

    int main(int argc, char* argv[]) {
        const int kDataLen = 4;

        int a = 2;
        float host_x[kDataLen] = {1.0f, 2.0f, 3.0f, 4.0f};
        float host_y[kDataLen];

        // Copy input data to device.
        float* device_x;
        float* device_y;
        cudaMalloc(&device_x, kDataLen * sizeof(float));
        cudaMalloc(&device_y, kDataLen * sizeof(float));
        cudaMemcpy(device_x, host_x, kDataLen * sizeof(float), cudaMemcpyHostToDevice);

        // Launch the kernel.
        mul<<<1, kDataLen>>>(a, device_x, device_y);

        // Copy output data to host.
        cudaDeviceSynchronize();
        cudaMemcpy(host_y, device_y, kDataLen * sizeof(float), cudaMemcpyDeviceToHost);

        // Print the results.
        for (int i = 0; i < kDataLen; ++i) {
            std::cout << "y[" << i << "] = " << host_y[i] << "\n";
        }

        cudaDeviceReset();
        return 0;
    }


使用clang编译cuda：
::

    export CUDA_INSTALL_PATH=/root/rvgpu/install
    export LD_LIBRARY_PATH=${CUDA_INSTALL_PATH}/lib
    export PATH=${CUDA_INSTALL_PATH}/bin:${PATH}

    clang++ mul.cu -o a.out --cuda-gpu-arch=rv64g -L${CUDA_INSTALL_PATH}/lib -lcudart -ldl -lrt

通过--cuda-gpu-arch=rv64g指定编译cuda的目标设备为rvgpu。

运行设置环境变量 LD_LIBRARY_PATH 如下：
::

    export LD_LIBRARY_PATH=${PWD}/install/lib


    # 确保应用程序已经链接到指定的动态库上
    qihangkong@st-ubuntu:~/git/rvgpu$ ldd ./mul
    ./mul: /home/qihangkong/git/rvgpu/install/lib/libcudart.so.11.0: no version information available (required by ./mul)
            linux-vdso.so.1 (0x00007ffdc52c9000)
            libcudart.so.11.0 => /home/qihangkong/git/rvgpu/install/lib/libcudart.so.11.0 (0x00007f7f09c3f000)
            libstdc++.so.6 => /lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f7f09800000)
            libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f7f09b3c000)
            libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f7f09b1c000)
            libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f7f09400000)
            librvgpu.so.1.0 => /home/qihangkong/git/rvgpu/install/lib/librvgpu.so.1.0 (0x00007f7f09b15000)
            /lib64/ld-linux-x86-64.so.2 (0x00007f7f09c51000)
            librvgsim.so.0.0.1 => /home/qihangkong/git/rvgpu/install/lib/librvgsim.so.0.0.1 (0x00007f7f09af0000)
            libsoftfloat.so => /home/qihangkong/git/rvgpu/install/lib/libsoftfloat.so (0x00007f7f09ace000)

运行如下：
::

    qihangkong@st-ubuntu:~/git/rvgpu$ ./mul 
    ./mul: /home/qihangkong/git/rvgpu/install/lib/libcudart.so.11.0: no version information available (required by ./mul)
    this is RVGSim
    Run Kernel
    y[0] = 2
    y[1] = 4
    y[2] = 6
    y[3] = 8


2. 问题
#########################
如果有什么问题欢迎随时联系我们：

https://gitee.com/rvgpu 

3. 使用源码编译第三方库
#########################
在开发过程中，往往需要编译构建第三方库，下面的列表是构建第三方仓库的参考文档：

.. toctree::
   :maxdepth: 1

   build_vulkan_demos
