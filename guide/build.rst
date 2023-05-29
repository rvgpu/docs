RVGPU 构建和使用
=======================

1. 构建
#######################

1.1 编译
***********************

为了方便管理仓库，此项目使用git submodule来管理，所以只需要访问rvgpu顶层仓库就可以通过submodule来
克隆项目的所有子仓库。
::

    git clone https://gitee.com/sietium-research/rvgpu.git

如果使用的gitee上面的镜像仓库，需要更新子仓库的路径，如果是从gitlab上克隆，则跳过此步骤:
::

   ./tools/repos/updata_submodule_urls.sh


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
