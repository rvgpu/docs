构建Vulkan SDK
#######################

编译Vulkan SDK
=======================

下载SDK：https://vulkan.lunarg.com/

解压缩后的SDK目录是包含已经编译好的文件，也可以通过下面方式自己重新编译：

::

   ./vulkansdk --clean
   ./vulkansdk --debug --skip-installing-deps -j 8

vulkansdk的编译脚本可以加上如下的选项：
::

   usage: ./vulkansdk [--clean|--help] [OPTION...] [REPO...]
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
