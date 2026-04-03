# HMPP快速入门

本文提供调用HMPP接口函数的示例代码，帮助用户快速使用HMPP函数库。

## 特殊使用说明

部分复杂功能如采样、滤波、音频等含有配套接口实现初始化等辅助功能，请按照指南严格遵守配套使用的关系，以保证功能正确，具体信息请参考各接口说明。

- 信号库接口请使用HMPPS_Malloc和HMPPS_Free系列函数来申请和释放内存空间，并根据使用空间的数据类型选择对应的接口函数，以此保证HMPP库功能安全性并发挥HMPP库最佳性能。
- 图像库接口对图像宽和高组合不作限制，要求输入的图像存储大小（宽×高×通道数×数据类型大小）不超过INT32_MAX字节数，推荐采用业界通用的图像存储规格，例如1080P。

## 编译方法说明

- （必选）设置HMPP环境变量和添加HMPP编译选项。
    通过以下命令保证HMPP相关的环境变量被正确设置:

    ```shell
    export LD_LIBRARY_PATH=/usr/local/lib/HMPP:$LD_LIBRARY_PATH
    ```
    
    添加HMPP头文件、动态库路径相关编译选项：

    ```shell
    -I /usr/local/include/HMPP -L /usr/local/lib/HMPP
    ```

- （必选）通过以下命令保证libboundscheck相关的环境变量被正确设置:

    ```shell
    export LD_LIBRARY_PATH=/usr/local/lib/libboundscheck:$LD_LIBRARY_PATH
    ```

    添加libboundscheck头文件、动态库所在路径到编译选项：

    ```shell
    -I /usr/local/include/libboundscheck -L /usr/local/lib/libboundscheck -lboundscheck
    ```

- （可选）如需使用HMPP基础函数，请添加以下编译选项：

    ```shell
    -lHMPP_core
    ```

- （可选）如需适用HMPP信号库、HMPP图像库，通过以下命令保证KML相关的环境变量被正确设置：

    ```shell
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/nolocking:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/locking:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/omp:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/pthread:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kspblas/single:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kspblas/multi:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch/kvml/single:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch/kvml/multi:$LD_LIBRARY_PATH
    ```

    添加kml头文件、动态库所在路径到编译选项：

    ```shell
    -I /usr/local/kml/include -L /usr/local/kml/lib/neon -lkfft -lkfftf -L /usr/local/kml/lib/noarch/kvml/single -lkvml -L /usr/local/kml/lib/noarch/ -lkm -lm -L /usr/local/kml/lib/neon/kblas/locking -lkblas
    ```
    
- （可选）如需使用HMPP信号库，请添加以下编译选项：

    ```shell
    -lHMPP_signal -lHMPP_core -lpthread
    ```

- （可选）如需使用HMPP图像库，请添加以下编译选项：

    ```shell
    -lHMPP_image -lHMPP_core -lm -lHMPP_signal -lpthread
    ```

## 接口调用示例

提供的示例代码引用的HMPP头文件为hmpp.h。

- 创建test.c文件并添加以下代码内容：

    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <stdint.h>
    #include "hmpp.h"
    #define BUFFER_SIZE_T 20
    void AddExample()
    {
        uint32_t *src1 = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        uint32_t *src2 = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        uint32_t *dst = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        int32_t i, result;
    
        if (src1 == NULL || src2 == NULL || dst == NULL) {
            return;
        }
        for (i = 0; i < BUFFER_SIZE_T; ++i) {
             src1[i] = i;
             src2[i] = i + 1;
        }
        
        result = HMPPS_Add_32u(src1, src2, dst, BUFFER_SIZE_T);
        printf("result = %d \n", result);
        printf("src1: ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", src1[i]);
        }
        printf("\nsrc2: ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", src2[i]);
        }
        printf("\ndst : ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", dst[i]);
        }
        printf("\n");

        HMPPS_Free(src1);
        HMPPS_Free(src2);
        HMPPS_Free(dst);
    }
    int main(void)
    {
        AddExample();
        return 0;
    }
    ```

- 编译文件。

    ```shell
    gcc test.c -o test -L /usr/local/lib/HMPP -I /usr/local/include/HMPP -lHMPP_signal  -lHMPP_core -lpthread -I /usr/local/include/libboundscheck -L /usr/local/lib/libboundscheck -lboundscheck -I /usr/local/kml/include -L /usr/local/kml/lib/neon -lkfft -lkfftf -L /usr/local/kml/lib/noarch/kvml/single -lkvml -L /usr/local/kml/lib/noarch/ -lkm -lm -L /usr/local/kml/lib/neon/kblas/locking -lkblas
    ```

- 执行文件。

    ```shell
    ./test
    ```

    运行结果如下：

    ```shell
    result = 0 
    src1:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 
    src2:  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
    dst :  1  3  5  7  9 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39 
    ```
