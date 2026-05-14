# 快速入门

本文档以Aarch64机器和x265-4.1为例，详细说明如何使能KVCL。通过替换x265中的原生算子为KVCL实现，从而提升视频编码性能。

 1. 获取x265源码并解压。

     ```bash
     wget https://ftp.videolan.org/pub/videolan/x265/x265_4.1.tar.gz
     tar -zxf x265_4.1.tar.gz
     cd x265_4.1
     ```

 2. 应用KVCL Patch。<br>KVCL提供了Patch文件，用于替换x265中的原生算子。本文档提供示例[Patch](../../examples/x265_4.1-enable-kvcl.patch)，可直接合入x265_4.1。该Patch通过setupKvclIntrinsicPrimitives和setupKvclAssemblyPrimitives覆盖x265_4.1算子。

     1. 设置KVCL根目录路径。<br>以下命令中，`KVCL_PATH`表示KVCL代码仓的根目录，请根据实际情况修改。  

        ```bash        
        KVCL_PATH=/home/kvcl    
        ```
      
     2. 应用Patch。        
     
        ```bash               
        patch -p1 < $KVCL_PATH/examples/x265_4.1-enable-kvcl.patch
        ```

        由于KVCL在各环境中的安装位置不同，应用Patch后，需手动修正KVCL的安装路径。
     3. 打开`source/CMakeLists.txt`，搜索`kvcl lib path`，找到以下行：
     
         ```cmake      
         link_directories(/home/kvcl/output/lib)       
         ```

     4. 将其中路径修正为KVCL实际安装路径。例设KVCL根目录为`/opt/kvcl`，则修改为：

         ```cmake        
         link_directories(/opt/kvcl/output/lib)
         ```

 3. 编译x256。

     1. 配置编译器环境变量。<br>本文档使用Clang作为编辑器。请根据实际安装路径调整以下环境变量。

        ```bash
        CLANG_PATH=/home/compiler/clang
        export PATH=$CLANG_PATH/bin:$PATH
        export LD_LIBRARY_PATH=$CLANG_PATH/lib:$LD_LIBRARY_PATH
        export CC=$CLANG_PATH/bin/clang
        export CXX=$CLANG_PATH/bin/clang++
        ```
        
     2. 创建构建目录。<br>由于构建目录已存在，简单修改下目录名称即可。

        ```bash
        mkdir mybuild
        cd mybuild
        ```

     3. 配置构建参数。<br>请根据实际情况修改以下变量：
        - X265_INSTALL_PATH：x265安装路径
        - KVCL_INCLUDE_PATH：KVCL的编译结果目录下的include路径

        ```bash
        X265_INSTALL_PATH=/home/x265_kvcl/install \
        KVCL_INCLUDE_PATH=/home/kvcl/output/include &&\
        cmake ../source \
        -DCMAKE_BUILD_TYPE=Release \
        -DENABLE_ASSEMBLY=ON \
        -DHIGH_BIT_DEPTH=OFF \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -DENABLE_CLI=ON \
        -DENABLE_TESTS=ON \
        -DENABLE_SHARED=ON \
        -DCMAKE_C_FLAGS="-O3 -march=armv8.6-a+dotprod+i8mm+sve+sve2-I$KVCL_INCLUDE_PATH" \
        -DCMAKE_CXX_FLAGS="-O3 -march=armv8.6-a+dotprod+i8mm+sve+sve2 -I$KVCL_INCLUDE_PATH" \
        -DCMAKE_INSTALL_PREFIX=$X265_INSTALL_PATH \
        -DENABLE_NEON=ON \
        -DCMAKE_C_COMPILER=$CC \
        -DCMAKE_CXX_COMPILER=$CXX \
        -DENABLE_NEON_DOTPROD=ON \
        -DENABLE_NEON_I8MM=ON \
        -DENABLE_SVE=ON \
        -DENABLE_SVE2=ON \
        -DENABLE_LIBNUMA=OFF \
        -DCMAKE_EXE_LINKER_FLAGS="-ldl"
        ```

 4. 编译安装。

    ```bash
    make -j8 && make install
    ```

    编译完成后，x265中的对应算子被替换为KVCL实现。

 5. 运行测试。
    在x265目录下运行算子测试程序TestBench。

    ```bash
    ./mybuild/TestBench
    ```

    预期结果：测试程序应能正常启动，并可得到KVCL算子替换的收益。
