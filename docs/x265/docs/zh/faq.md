# x265 FAQ

本文提供使用x265过程的常见问题以及解决办法。

## 如何解决x265编译参数与处理器类型不匹配的问题

**问题现象描述**

编译x265时提示编译参数和处理器架构不匹配，例如处理器是x86架构，却使用aarch64架构的编译参数。

**关键过程、根本原因分析**

编译参数和处理器架构不匹配。

**结论、解决方案及效果**

使用**uname -a**查看处理器的架构信息，根据回显信息选择正确的编译参数。

## 安装GCC后提示无法找到NASM的解决办法

**问题现象描述**

安装GCC后提示找不到NASM。

**关键过程、根本原因分析**

未安装NASM。

**结论、解决方案及效果**

1. 下载NASM软件包。

    下载地址：[NASM官网](https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/)。

2. 将其上传到服务器并解压。

    ```shell
    tar -xvf nasm.tar.gz
    ```

3. 执行以下命令进行安装。

    ```shell
    ./configure
    make
    sudo make install
    ```

## 使用x265执行编码程序时提示权限不足的解决办法

**问题现象描述**

使用x265执行编码程序时提示Permission denied。

**关键过程、根本原因分析**

权限不足。

**结论、解决方案及效果**

为FFmpeg执行程序授权。

```shell
chmod +x /home/ffmpeg/build/ffmpeg
```

## 使用x265执行编码程序时依赖库加载错误的解决办法

**问题现象描述**

使用x265执行编码程序提示ffmpeg: error while loading shared libraries: libavdevice.so.xx。

**关键过程、根本原因分析**

依赖库加载错误。

**结论、解决方案及效果**

1. 找到报错信息中的依赖库所在路径。

    ```shell
    find / -name libavdevice.so.xx
    ```

2. 将其添加到环境变量，即在以下命令的冒号后加上找到的路径。

    ```shell
    export LD_LIBRARY_PATH=/home/ffmpeg/install/lib/:依赖库所在路径
    ```
