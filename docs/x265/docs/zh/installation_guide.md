# x265安装指南

本文提供x265的安装指南，请按照以下步骤进行安装。

## 环境配置

硬件环境：

| 项目 | 说明 |
| ---- | ---- |
| CPU | 鲲鹏920新型号 |

软件环境：

| 软件  | 版本    |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| 编译器 | gcc ≥ 10.3.1 |
| make |  ≥ 4.3 |
| cmake | ≥ 3.10 |

## 安装x265

- 用户获取x265的软件包后，按照如下步骤进行安装。

- 创建安装目录，可自定义。

    ```shell
    mkdir -p /home/x265/
    ```

- 解压软件包，并拷贝到安装目录。

    ```shell
    unzip x265_install.zip
    cp -r x265_install /home/x265/
    ```

- 修改pkg-config配置文件的prefix项为x265安装目录。

    ```shell
    sed -i 's|^prefix=.*|prefix=/home/x265/x265_install|' /home/x265/x265_install/lib/pkgconfig/x265.pc
    ```

## 安装FFmpeg

- 通过FFmpeg可以调用x265进行视频编码操作，因此需要安装FFmpeg。安装完成后通过查看FFmpeg版本检查是否安装成功。编译安装过程中使用的安装目录、源代码目录和编译目录均可自定义，本文中给出的目录仅为示例。

- 获取FFmpeg压缩包并解压。

    ```shell
    cd /home
    wget https://ffmpeg.org/releases/ffmpeg-6.0.1.tar.gz
    tar -zxvf ffmpeg-6.0.1.tar.gz
    ```

- 创建编译和安装目录。

    ```shell
    mkdir -p /home/ffmpeg-6.0.1/build
    mkdir -p /home/ffmpeg-6.0.1/install
    ```

- 配置编译参数。相关参数的解释说明如下表所示。

    ```shell
    export PKG_CONFIG_PATH=/home/x265/x265_install/lib/pkgconfig/
    export LD_LIBRARY_PATH=/home/ffmpeg-6.0.1/install/lib/:/home/x265/x265_install/lib/:${LD_LIBRARY_PATH}
    cd /home/ffmpeg-6.0.1/build
    /home/ffmpeg-6.0.1/configure --enable-libx265 --enable-gpl --enable-pthreads --disable-autodetect --enable-static --extra-cflags="-I/home/x265/x265_install/include" --extra-ldflags="-L/home/x265/x265_install/lib -lm -lstdc++" --prefix=/home/ffmpeg-6.0.1/install
    ```

    |参数|说明|
    |--|--|
    |export PKG_CONFIG_PATH|设置环境变量，指定x265的安装位置，以便后续的编译过程可以找到x265的库文件。|
    |export LD_LIBRARY_PATH|设置环境变量，将FFmpeg和x265的库文件目录添加到动态链接库搜索路径中，以便后续的链接过程可以找到这些库文件。|
    |/home/ffmpeg/src/configure|指定FFmpeg源代码目录，并运行其中的configure脚本，用于配置编译选项。|
    |--enable-libx265|开启对libx265的支持，这将使FFmpeg能够编码和解码使用H.265/HEVC格式的视频。|
    |--enable-gpl|开启GPL（General Public License）许可的功能，允许FFmpeg使用一些受GPL许可管辖的库。|
    |--enable-pthreads|开启线程支持，以便在多核处理器上更好地利用并行性能。|
    |--disable-autodetect|关闭自动检测功能，需要手动指定依赖库的路径和选项。|
    |--enable-static|开启静态链接，这将在编译时将依赖的库静态链接到生成的可执行文件中（若开启动态链接则设置为   **--enable-shared**）。|
    |--extra-cflags="-I/home/x265/x265_install/include"|添加额外的C编译参数，指定x265库的头文件路径。|
    |--extra-ldflags="-L/home/x265/x265_install/lib -lm -lstdc++"|添加额外的链接器参数，指定x265库的链接库路径，并链接标准数学库和C++标准库。|
    |--prefix=/home/ffmpeg/install|指定安装目录的前缀，编译完成后将安装到指定目录下。|

- 编译安装。

    ```shell
    make -j 32
    make install
    ```

- 进入FFmpeg安装目录。

    ```shell
    cd /home/ffmpeg/install/bin
    ```

- 查看FFmpeg版本。

    ```shell
    ./ffmpeg
    ```

    出现“ffmpeg version 6.0.1 Copyright (c) 2000-2023 the FFmpeg developers”则表示安装成功。
