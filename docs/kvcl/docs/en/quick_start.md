# Quick Start

This document uses an AArch64 server and x265-4.1 as an example to describe how to enable KVCL. The native operator in x265 is replaced with the KVCL implementation to improve video encoding performance.

 1. Obtain the x265 source code and decompress it.

     ```bash
     wget https://ftp.videolan.org/pub/videolan/x265/x265_4.1.tar.gz
     tar -zxf x265_4.1.tar.gz
     cd x265_4.1
     ```

 2. Apply the KVCL patch.<br>KVCL provides a patch file to replace the native operator in x265. This document provides an example [Patch](../../examples/x265_4.1-enable-kvcl.patch), which can be directly integrated into x265_4.1. This patch overwrites the x265_4.1 operator through setupKvclIntrinsicPrimitives and setupKvclAssemblyPrimitives.

     1. Set the KVCL root directory.<br>In the following command, `KVCL_PATH` indicates the root directory of the KVCL code repository. 

        ```bash        
        KVCL_PATH=/home/kvcl    
        ```
      
     2. Apply the patch.       
     
        ```bash               
        patch -p1 < $KVCL_PATH/examples/x265_4.1-enable-kvcl.patch
        ```

        The KVCL installation path varies depending on the environment. After applying the patch, you need to manually correct the KVCL installation path.
     3. Open `source/CMakeLists.txt` and search for `kvcl lib path` to find the following line:
     
         ```cmake      
         link_directories(/home/kvcl/output/lib)       
         ```

     4. Change the path to the actual KVCL installation path. For example, if the KVCL root directory is `/opt/kvcl`, change the path as follows:

         ```cmake        
         link_directories(/opt/kvcl/output/lib)
         ```

 3. Compile x256.

     1. Set environment variables for the compiler.<br>This document uses Clang as the editor. Adjust the following environment variables based on the actual installation path.

        ```bash
        CLANG_PATH=/home/compiler/clang
        export PATH=$CLANG_PATH/bin:$PATH
        export LD_LIBRARY_PATH=$CLANG_PATH/lib:$LD_LIBRARY_PATH
        export CC=$CLANG_PATH/bin/clang
        export CXX=$CLANG_PATH/bin/clang++
        ```
        
     2. Create a build directory.<br>The build directory already exists. Therefore, you only need to change the directory name.

        ```bash
        mkdir mybuild
        cd mybuild
        ```

     3. Configure build parameters.<br>Modify the following variables as required:
        - X265_INSTALL_PATH: x265 installation path
        - KVCL_INCLUDE_PATH: **include** path in the KVCL compilation result directory

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

 4. Perform compilation and installation.

    ```bash
    make -j8 && make install
    ```

    After the compilation is complete, the corresponding operator in x265 is replaced with the KVCL implementation.

 5. Run the test.
    Run the operator test program TestBench in the x265 directory.

    ```bash
    ./mybuild/TestBench
    ```

    Expected result: The test program can be started properly, and the benefits of KVCL operator replacement can be obtained.
