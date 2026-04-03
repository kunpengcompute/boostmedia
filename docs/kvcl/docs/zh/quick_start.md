# 快速开始

本文档以Aarch64机器和x265-4.1作为环境举例，说明如何使能kvcl。

## 编译x265

1. 获取x265代码仓并解压。

```bash
wget https://ftp.videolan.org/pub/videolan/x265/x265_4.1.tar.gz
tar -zxf x265_4.1.tar.gz
```

2. 修改代码。

由于需要修改编译配置、替换x265中的kvcl算子。
本文档提供示例[patch](../../examples/x265_4.1-enable-kvcl.patch)，可直接合入x265_4.1。
该patch通过setupKvclIntrinsicPrimitives和setupKvclAssemblyPrimitives覆盖x265_4.1算子。

```bash
#KVCL_PATH表示KVCL代码仓的根目录，根据实际情况修改
KVCL_PATH=/home/kvcl
cd x265_4.1
patch -p1 < $KVCL_PATH/examples/x265_4.1-enable-kvcl.patch
```

kvcl在各环境中安装位置不同，所以合入patch后需要修正kvcl安装路径：
在x265_4.1中打开source/CMakeLists.txt

```cmake
#搜索kvcl lib path，找到下行
link_directories(/home/kvcl/output/lib)   # kvcl lib path
#将其中的/home/kvcl/output/lib修正为kvcl实际安装路径下的output/lib路径即可
```

 3. 编译

 ```bash
# 用户需自行安装编译器，本文档以clang为例
# 以下环境变量根据实际情况配置
CLNAG_PATH=/home/compiler/clang
export PATH=$CLNAG_PATH/bin:$PATH
export LD_LIBRARY_PATH=$CLNAG_PATH/lib:$LD_LIBRARY_PATH
export CC=$CLNAG_PATH/bin/clang
export CXX=$CLNAG_PATH/bin/clang++

 # 创建build目录，由于build目录已存在，简单修改下目录名称
mkdir mybuild
cd mybuild

# 按实际配置路径参数
# X265_INSTALL_PATH：x265安装路径，根据实际情况修改
# KVCL_INCLUDE_PATH：KVCL的编译结果目录下的include路径，根据实际情况修改
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
-DCMAKE_C_FLAGS="-O3 -march=armv8.6-a+dotprod+i8mm+sve+sve2 -I$KVCL_INCLUDE_PATH" \
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

# 编译安装
make -j8 && make install
```

编译通过后，x265中对应算子被替换为了kvcl实现。

## 运行测试
运行算子测试程序TestBench。
```bash
# 在x265目录下
./mybuild/TestBench
```
预期结果：测试程序应能正常启动，并可得到kvcl算子替换的收益。