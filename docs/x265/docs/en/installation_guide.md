# x265 Installation Guide

This document describes how to install x265.

## Environment Requirements

**Hardware**

| Project| Description|
| ---- | ---- |
| CPU | New Kunpeng 920 model|

**OS**

| Project | Version   |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |

**Software**

| Project | Version   |
| ------------ | ------------ |
| Compiler| GCC 10.3.1 or later|
| make |  4.3 or later|
| cmake | 3.10 or later|

## Installing x265

After obtaining the x265 software package, perform the following steps to install it:

1. Create an installation directory, which can be customized.

    ```shell
    mkdir -p /home/x265/
    ```

2. Decompress the software package and copy it to the installation directory.

    ```shell
    unzip x265_install.zip
    cp -r x265_install /home/x265/
    ```

3. Change the prefix in the **pkg-config** configuration file to the x265 installation directory.

    ```shell
    sed -i 's|^prefix=.*|prefix=/home/x265/x265_install|' /home/x265/x265_install/lib/pkgconfig/x265.pc
    ```

## Installing FFmpeg

To use x265 in video encoding, you need to install FFmpeg. After the installation process is complete, view the FFmpeg version to check whether the installation is successful. The installation directory, source code directory, and compilation directory provided in the following procedure are all examples. Replace them with actual ones.

1. Obtain the FFmpeg package and decompress it.

    ```shell
    cd /home
    wget https://ffmpeg.org/releases/ffmpeg-6.0.1.tar.gz
    tar -zxvf ffmpeg-6.0.1.tar.gz
    ```

2. Create the compilation and installation directories.

    ```shell
    mkdir -p /home/ffmpeg-6.0.1/build
    mkdir -p /home/ffmpeg-6.0.1/install
    ```

3. Set compilation parameters. The following table describes the related parameters.

    ```shell
    export PKG_CONFIG_PATH=/home/x265/x265_install/lib/pkgconfig/
    export LD_LIBRARY_PATH=/home/ffmpeg-6.0.1/install/lib/:/home/x265/x265_install/lib/:${LD_LIBRARY_PATH}
    cd /home/ffmpeg-6.0.1/build
    /home/ffmpeg-6.0.1/configure --enable-libx265 --enable-gpl --enable-pthreads --disable-autodetect --enable-static --extra-cflags="-I/home/x265/x265_install/include" --extra-ldflags="-L/home/x265/x265_install/lib -lm -lstdc++" --prefix=/home/ffmpeg-6.0.1/install
    ```

    |Parameter|Description|
    |--|--|
    |export PKG_CONFIG_PATH|Sets the x265 installation location so that the x265 library files can be found in the subsequent compilation process.|
    |export LD_LIBRARY_PATH|Adds the FFmpeg and x265 library file directories to the dynamic link library (DLL) search path so that these library files can be found in the subsequent linking process.|
    |/home/ffmpeg/src/configure|Specifies the FFmpeg source code directory and runs the **configure** script in the directory to configure compilation options.|
    |--enable-libx265|Enables support for libx265, which will enable FFmpeg to encode and decode HEVC/H.265 videos.|
    |--enable-gpl|Enables the General Public License (GPL) to allow FFmpeg to use some libraries governed by the GPL license.|
    |--enable-pthreads|Enables thread support to enhance parallel performance on multi-core processors.|
    |--disable-autodetect|Disables automatic detection. You need to manually set the paths and options of the dependency libraries.|
    |--enable-static|Enables static linking. The dependency libraries are statically linked to the generated executable file during the compilation. If you require dynamic linking, set this parameter to **--enable-shared**.|
    |--extra-cflags="-I/home/x265/x265_install/include"|Adds the additional C compilation parameter to specify the header file path of the x265 library.|
    |--extra-ldflags="-L/home/x265/x265_install/lib -lm -lstdc++"|Adds the additional linker parameters to specify the link library path of the x265 library and link to the standard math library and C++ standard library.|
    |--prefix=/home/ffmpeg/install|Specifies the prefix of the installation directory. After the compilation is complete, the software is installed in the specified directory.|

4. Compile and install the package.

    ```shell
    make -j 32
    make install
    ```

5. Go to the FFmpeg installation directory.

    ```shell
    cd /home/ffmpeg/install/bin
    ```

6. View the FFmpeg version.

    ```shell
    ./ffmpeg
    ```

    If the command output reads `ffmpeg version 6.0.1 Copyright (c) 2000-2023 the FFmpeg developers`, the installation is successful.
