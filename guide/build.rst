RVGPU 构建和使用
=======================

1. 构建RVGPU
#######################

1.1 编译
***********************

为了方便管理仓库，此项目使用git submodule来管理，所以只需要访问rvgpu顶层仓库就可以通过submodule来
克隆项目的所有子仓库。
::

    git clone http://10.100.101.58/rvgpu/rvgpu.git


接下来使用如下命令来初始化仓库：
::

    git submodule init  
    git submodule update

scripts目录下提供了一个编译脚本用来快速编译：
::

    ./tools/build/build.sh

此脚本将编译Mesa、LLVM和CModel，并且将编译生成的目标文件安装到`${PWD}/install`路径下。

1.2 运行vulkan 
***********************

运行vulkan demo时需要设置vulkan ICD环境，可以通过如下命令来设置`VK_ICD_FILENAMES`环境变量：

::

    export LD_LIBRARY_PATH=${PWD}/install/lib/
    export VK_ICD_FILENAMES=${PWD}/install/share/vulkan/icd.d/rvgpu_icd.x86_64.json

2. 源码编译第三方库
#######################

2.1 编译Vulkan SDK
***********************

下载SDK：https://vulkan.lunarg.com/

解压缩后的SDK目录是包含已经编译好的文件，也可以通过下面方式自己重新编译：

::

   ./vulkansdk --clean
   ./vulkansdk --debug --skip-installing-deps -j 8

vulkansdk的编译脚本可以加上如下的选项：
::

   sage: ./vulkansdk [--clean|--help] [OPTION...] [REPO...]
   OPTIONS:
     [--help|help] prints this usage and exits
     [--clean|-c] deletes all the build directories and exits
     [--debug|-d] build binaries as debuggable (will replace existing binaries)
     [--numjobs #|-j #] specify the number of jobs to use for building, defaults to using 1 job
     [--maxjobs] uses nproc to use max jobs possible
     [--skip-installing-deps] skips attempting to install package dependencies to build SDK

要使用vulkan SDK只需要执行如下命令：
::

   source setup-env.sh


