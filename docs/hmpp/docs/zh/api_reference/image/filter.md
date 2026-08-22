# 图像滤波

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Conv

对两幅图像进行二维卷积运算。

函数接口声明如下：

HmppResult HMPPI_Conv_8u_C1R(const uint8_t *pSrc1, int src1Step, HmppiSize src1Size, const uint8_t*pSrc2, int src2Step, HmppiSize src2Size, uint8_t *pDst, int dstStep, int divisor, int algType);

HmppResult HMPPI_Conv_32f_C1R(const float *pSrc1, int src1Step, HmppiSize src1Size, const float*pSrc2, int src2Step, HmppiSize src2Size, float *pDst, int dstStep, int algType);

- 如果设置了HMPPI_ROI_FULL标志，该函数执行由pSrc1和pSrc2参数指向的两个源图像之间的完整二维有限线性卷积。目标图像h[i, j]通过以下公式计算：

  ![](../../figures/iconv-1.png)

  其中，

  - Mh = Mf + Mg - 1

    - Mf 是第一个源图像矩阵 f 的行数
    - Mg 是第二个源图像矩阵 g 的行数

  - Nh = Nf + Ng - 1

    - Nf 是第一个源图像矩阵 f 的列数
    - Ng 是第二个源图像矩阵 g 的列数

  - 0 ≤ i < Mh, 0 ≤ j < Nh

  - 计算f、g所有可能的 i,j 位置，超出下标部分使用零填充计算：

    ![](../../figures/iconv-2.png)
    
    ![](../../figures/iconv-3.png)

- 如果设置了 HMPP_ROI_VALID 标志，该函数执行由 pSrc1 和 pSrc2 参数指向的两个源图像之间的有效二维有限线性卷积。函数操作得到的目标图像 h[i, j] 通过以下公式计算：

  ![](../../figures/iconv-4.png)

  其中，
  
  - Mh = |Mf - Mg| + 1
    - Mf 是第一个源图像矩阵 f 的行数
    - Mg 是第二个源图像矩阵 g 的行数
  - Nh = |Nf - Ng| + 1
    - Nf 是第一个源图像矩阵 f 的列数
    - Ng 是第二个源图像矩阵 g 的列数
  - 0 ≤ i < Mh, 0 ≤ j < Nh
  
  这种情况假定 Mf ≥ Mg 且 Nf ≥ Ng。当 Mf < Mg 且 Nf < Ng 时，公式中的下标索引 g 必须替换为 f。对于源图像尺寸的任何其他组合，该函数不执行任何操作。

注意：接受 float 类型输入数据的函数风格使用相同的求和公式，但不对结果进行缩放（假定除数为 1）。

以下示例说明了函数的操作。对于大小为 3 x 5 的源图像 f、g，表示为：

```text
f = g = [1 1 1
   1 0 0
   1 1 1
   0 0 1
   1 1 1]
```

对于 HMPPI_ROI_FULL 情况，生成的卷积图像 h 大小为 5 x 9，包含以下数据：

```text
h = [1 2 3 2 1 
  2 2 2 0 0 
  3 4 6 4 2
  2 2 4 2 2
  3 6 11 6 3
  2 2 4 2 2
  2 4 6 4 2
  0 0 2 2 2
  1 2 3 2 1]
```

对于 HMPPI_ROI_VALID 情况，生成的卷积图像 h 大小为 1 x 1，包含以下数据：

```text
h = [11]
```

**参数**

| 参数名   | 描述                                               | 取值范围                                                     | 输入/输出 |
| -------- | -------------------------------------------------- | ------------------------------------------------------------ | --------- |
| pSrc1    | 指向源图像1感兴趣区域的指针。                      | 非空                                                         | 输入      |
| src1Step | 源图像1中连续行起点之间的距离（以字节为单位）。    | 非负整数                                                     | 输入      |
| src1Size | 源图像1的像素尺寸。                                | src1Size.width∈(0,INT_MAX]，src1Size.height∈(0,INT_MAX]      | 输入      |
| pSrc2    | 指向源图像2感兴趣区域的指针。                      | 非空                                                         | 输入      |
| src2Step | 源图像2中连续行起点之间的距离（以字节为单位）。    | 非负整数                                                     | 输入      |
| src2Size | 源图像2的像素尺寸。                                | src2Size.width∈(0,INT_MAX]，src2Size.height∈(0,INT_MAX]      | 输入      |
| pDst     | 指向目标图像感兴趣区域的指针。                     | 非空                                                         | 输出      |
| dstStep  | 目标图像中连续行起点之间的距离（以字节为单位）。   | 非负整数                                                     | 输入      |
| divisor  | 用于整除计算结果的整数值（仅整数数据操作时需要）。 | 非零                                                         | 输入      |
| algType  | 算法类型定义的位域掩码。                           | 由HmppAlgMode（HMPP_ALG_AUTO、HMPP_ALG_DEFAULT、HMPP_ALG_FFT）和HmppiROIShape（HMPPI_ROI_FULL、HMPPI_ROI_VALID）进行或运算组合的有效值 | 输入      |

**返回值**

- 成功：返回HMPP_STS_NO_ERR。
- 失败：返回错误码。

**错误码**

| 错误码                | 描述                                    |
| --------------------- | --------------------------------------- |
| HMPP_STS_NULL_PTR_ERR | 任何指定的指针为空。                    |
| HMPP_STS_SIZE_ERR     | src1Size或src2Size的字段为零或负值。    |
| HMPP_STS_STEP_ERR     | src1Step、src2Step或dstStep为零或负值。 |
| HMPP_STS_DIV_BY_ZERO  | divisor为零。                           |
| HMPP_STS_ALG_TYPE_ERR | algType值非法。                         |

**示例**

```c
#include <stdio.h>
#include "hmpp.h"

#define SRC1_WIDTH 5
#define SRC1_HEIGHT 5
#define SRC2_WIDTH 3
#define SRC2_HEIGHT 3

void ConvExample()
{
    HmppiSize src1Size = {5, 5};
    HmppiSize src2Size = {3, 3};
    uint8_t src1[5 * 5] = {
        1, 2, 3, 4, 5,
        6, 7, 8, 9, 10,
        11, 12, 13, 14, 15,
        16, 17, 18, 19, 20,
        21, 22, 23, 24, 25
    };
    uint8_t src2[3 * 3] = {
        1, 0, 1,
        0, 1, 0,
        1, 0, 1
    };
    uint8_t dst[7 * 7] = {0}; // 7 = 5 + 3 - 1;
    
 
    int divisor = 2;
    int algType = HMPP_ALG_DEFAULT | HMPPI_ROI_FULL;

    HmppResult res = HMPPI_Conv_8u_C1R(src1, 5 * sizeof(uint8_t), src1Size,
                                       src2, 3 * sizeof(uint8_t), src2Size,
                                       dst, 7 * sizeof(uint8_t), divisor, algType);
    printf("result = %d\n", res);
    printf("dst = \n");
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%3d ", dst[7 * i + j]);
        }
        printf("\n");
    }
    
}

int main()
{
    ConvExample();
    return 0;
}
```

运行结果

```text
result = 0
dst = 
  0   1   2   3   4   2   2 
  3   4   8   9  11   7   5 
  6  10  17  20  22  14  10 
 11  17  30  32  35  21  15 
 16  25  42  45  47  29  20 
  8  19  28  29  31  22  10 
 10  11  22  23  24  12  12 
```

## FilterLaplacianBorder

此函数将拉普拉斯滤波器应用于源图像src并将结果存储到目标图像dst。边框像素的值根据边框类型和边框值赋值参数获得。此过滤器的内核是参数指定的3x3或5x5大小的矩阵掩码。内核具有以下值，锚点位于中心单元格。

2    4     4     4    2

2    0    2                         4    0    -8     0    4

0   -8    0                         4   -8  -24   -8    4

2    0    2                         4    0    -8     0    4

2    4     4     4    2

此函数所需应用到的buffer空间，需要先使用FilterLaplacianInit来申请，并在主函数中使用该空间。

函数接口声明如下：

- **辅助空间buffer申请与释放函数：**

    HmppResult HMPPI\_FilterLaplacianInit\_8u16s\_C1R\(HmppiSize roiSize, HmppiMaskSize mask, uint8\_t \*\*buffer\);

    HmppResult HMPPI\_FilterLaplacianInit\_32f\_C1R\(HmppiSize roiSize, HmppiMaskSize mask, uint8\_t \*\*buffer\);

    HmppResult HMPPI\_FilterLaplacianRelease\(uint8\_t \*buffer\);

- **主函数：**

    HmppResult HMPPI\_FilterLaplacianBorder\_8u16s\_C1R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiMaskSize maskSize, HmppiBorderType borderType, uint8\_t borderValue, uint8\_t \*buffer\);

    HmppResult HMPPI\_FilterLaplacianBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep,HmppiSize roiSize, HmppiMaskSize maskSize, HmppiBorderType borderType, float borderValue, uint8\_t \*buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|maskSize|预定义掩码。|HmppMskSize3x3或HmppMskSize5x5|输入|
|borderType|边界类型。|以下HmppiBorderType的枚举值之一：HMPPI_BORDER_CONST、HMPPI_BORDER_REPL、HMPPI_BORDER_MIRROR|输入|
|borderValue|指定常量边框像素的常量值，仅当borderType = HMPPI_BORDER_CONST时有意义。|数据类型范围内的值|输入|
|buffer|指向计算所需缓冲区的指针。在init中申请空间与初始化，在release中释放空间，在主函数中使用申请的空间。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize.width、roiSize.height这两个入参中存在值小于等于0。|
|HMPP_STS_STEP_ERR|srcStep、dstStep小于等于0。|
|HMPP_STS_ROI_ERR|roiSize.width * 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除。|
|HMPP_STS_BAD_ARG_ERR|不支持的borderType类型值或者不支持的maskSize值。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 28
#define DST_BUFFER_SIZE_T 16

int LaplacianBorderExample()
{
    HmppiSize roi = { 2, 4 };
    const float src[SRC_BUFFER_SIZE_T] = { 9.8375815e-16, 4.5484828e-16, 1.4972698e-14, -1.3397389e+25, 1.9061152e+27, -8.8007769e+17, -217025.86, -6.2768196e+14, 4.4335606e+33, 1.0258707e-13, -9.3150633e-11, -5192.7163, 2.536292e+25, 6.0832354e+15, 8.5712402e+29, -0.15451884, -0.2848247, -2.1714717e+10, 2.7212473e+17, -1.3591454e-22, 7.6473188e+21, -2.2729285e-30, 1129877.1, -1.0089557e-05, -1.1054221e+21, -3.8945858e-19, -6.8345717e-34, 60685416};
    float dst[DST_BUFFER_SIZE_T] = {0};

    int32_t srcStep = 4 * sizeof(float);
    int32_t dstStep = 4 * sizeof(float);
    uint8_t *buffer = NULL;
    HmppResult result = HMPPI_FilterLaplacianInit_32f_C1R(roi, HmppMskSize3x3, &buffer);

    if (result == HMPP_STS_NO_ERR) {
        result = HMPPI_FilterLaplacianBorder_32f_C1R(src, srcStep, dst, dstStep, roi, HmppMskSize3x3, HMPPI_BORDER_MIRROR, 0.0f, buffer);
    }
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    printf("dst= ");
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        printf("%f ",dst[i]);
    }
    printf("\n");
    HMPPI_FilterLaplacianRelease(buffer);
    buffer = NULL;
    return 0;
}
```

运行结果：

```text
result = 0
dst = -7.0406216e+18 1.5248922e+28 -3.9418209e+28 7.7666443e+28 -1.5248922e+28 1.7734242e+34 5.1225988e+27 -2.04904e+27 -3.5468485e+34 7.7259126e+27 -2.2689171e+31 2.2694294e+31 -2.0290336e+26 3.5468485e+34 0 0
```

## FilterBorder

将均值滤波器应用于源图像并将结果存储到目标图像。这里处理的图像是三维图像。边框像素的值根据边框类型和边框值赋值参数获得。

本函数使用的滤波核是由HMPPI\_CreateKernel3D\_32f来进行初始化，同时，此函数所需应用到的pSpec空间大小计算需要先使用HMPPI\_FilterBorderGetSize来申请，并在HMPPI\_FilterBorderInit\_32f中进行初始化包括创建好的滤波核。

函数接口声明如下：

- **辅助函数：**

    HmppResult HMPPI\_CreateKernel3D\_32f\(float\* kernel, HmpprVolume kernelSize\);

    HmppResult HMPPI\_FilterBorderGetSize\(HmpprVolume kernelVolume, HmpprVolume dstRoiVolume, int numChannels, int\* pSpecSize\);

    HmppResult HMPPI\_FilterBorderInit\_32f\(const float\* pKernel, HmpprVolume kernelVolume, int numChannels, HmpprFilterBorderSpec\* pSpec\);

- **主函数：**

    HmppResult HMPPI\_FilterBorder\_32f\_C1V\(const float\* pSrc, int srcPlaneStep, int srcStep, float\* pDst, int dstPlaneStep, int dstStep, HmpprVolume dstRoiVolume, HmpprBorderType borderType, const float borderValue\[1\], const HmpprFilterBorderSpec\* pSpec\);

- **释放函数：**

    HmppResult HMPPI\_FilterBorderRelease\_32f\(HmpprFilterBorderSpec \*pSpec\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源体积起点的指针。|非空|输入|
|srcStep|源体积中每个平面连续行起始点之间的距离（以字节为单位）。|非负整数|输入|
|srcPlaneStep|源连续体积平面之间的距离（以字节为单位）。|非负整数|输入|
|pDst|指向目标体积原点的指针。|非空|输入/输出|
|dstStep|目标体积中每个平面连续行起始点之间的距离（以字节为单位）。|非负整数|输入|
|dstPlaneStep|目标连续体积平面之间的距离（以字节为单位）。|非负整数|输入|
|kernel|指向滤波核值的指针。|非空|输入/输出|
|kernelSize|滤波核大小。|正奇数|输入|
|kernelVolume|滤波核体积。|正奇数|输入|
|numChannels|图像通道数。|1|输入|
|pSpecSize|pSpec分配空间的大小。|非空|输入/输出|
|dstRoiVolume|目标体积的感兴趣体积。|正整数|输入|
|borderType|边界类型。|以下HmppiBorderType的枚举值之一：HMPPI_BORDER_CONST、HMPPI_BORDER_REPL、HMPPR_BORDER_IN_MEM|输入|
|borderValue|指定常量边框像素的常量值，仅当borderType = HMPPI_BORDER_CONST时有意义。|数据类型范围内的值|输入|
|pSpec|特殊结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|入参中存在空指针。|
|HMPP_STS_SIZE_ERR|dstRoiVolume、kernelVolume入参中存在值小于等于0。|
|HMPP_STS_STEP_ERR|srcStep、srcPlaneStep、dstStep、dstPlaneStep小于0。|
|HMPP_STS_CHANNEL_ERR|通道数不为1。|
|HMPP_STS_OVERFLOW|kernelVolume的width、height、depth乘积的值溢出。|

**示例**

```c
void FilterBorderExample()
{
    HmpprVolume kernelVolume = {3, 3, 3};
    HmpprVolume dstRoiVolume = {4, 4, 4};
    int pSpecSize;
    HmppResult result;
    HmpprFilterBorderSpec* pSpec;
    float* pKernel;
    float pSrc[] = {38.014484, 82.609879, 82.758270, 81.367149, 89.961128, 86.196541, 2.654759, 8.122633, 20.915607, 39.117149, 48.680553, 74.702560, 89.141975, 0.049839, 96.961235, 40.884659, 0.073048, 82.266838, 43.245163, 39.711945, 92.387321, 63.789082, 39.633656, 33.714893, 68.096214, 25.101330, 62.661972, 17.430357, 60.464512, 33.217445, 37.544556, 51.155441, 67.719810, 20.685936, 32.905922, 57.153358, 6.772291, 86.404984, 17.585430, 27.213909, 25.116919, 17.515392, 53.564182, 66.432114, 18.855532, 49.816486, 59.853039, 70.812675, 32.314438, 2.026977, 9.393250, 76.235771, 65.080223, 0.228351, 62.949459, 85.970299, 26.796694, 24.186850, 55.365528, 86.225479, 58.701576, 92.171112, 37.857521, 25.761415};
    float* pDst;
    HmpprBorderType borderType = HMPPR_BORDER_REPL;
    int srcStep = dstRoiVolume.width * sizeof(float);
    int srcPlaneStep = dstRoiVolume.width * dstRoiVolume.height * sizeof(float);
    int dstStep = srcStep;
    int dstPlaneStep = srcPlaneStep;
    int kernelTotalSize = kernelVolume.width * kernelVolume.height * kernelVolume.depth;
    pKernel = HMPPS_Malloc_32f(kernelTotalSize);
    int dataTotalSize = dstRoiVolume.width * dstRoiVolume.height * dstRoiVolume.depth;
    pDst = HMPPS_Malloc_32f(dataTotalSize);
 
    result = HMPPI_FilterBorderGetSize(kernelVolume, dstRoiVolume, 1, &pSpecSize);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    
    pSpec = (HmpprFilterBorderSpec*)malloc(pSpecSize);
    result = HMPPI_CreateKernel3D_32f(pKernel, kernelVolume);
    printf("result = %d\n", result);
    
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_FilterBorderInit_32f(pKernel, kernelVolume, 1, pSpec);
    printf("result = %d\n", result);
    
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    float borderValue = pSrc[0];
    result = HMPPI_FilterBorder_32f_C1V(pSrc, srcPlaneStep, srcStep,
                                       pDst, dstPlaneStep, dstStep,
                                       dstRoiVolume, borderType,
                                       &borderValue, pSpec);
    printf("result = %d\n", result);
    
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < dataTotalSize; i++){
        printf("%f ", pDst[i]);
    }
    HMPPI_FilterBorderRelease_32f(pSpec);
}
```

运行结果：

```text
result = 0
result = 0
result = 0
result = 0
58.525280 59.930946 61.054619 50.821186 55.691948 54.039684 52.591640 46.378212 59.807632 52.972359 42.925503 42.069408 49.808098 51.124119 45.393032 55.123226 52.184692 51.331261 52.249077 45.508347 48.955585 48.016075 47.897266 44.273628 49.801155 47.854038 44.845074 44.447330 42.592846 46.391796 47.622082 56.376503 40.344097 37.633095 42.397793 47.313328 40.404427 40.143230 44.059200 50.277069 45.813835 45.989544 47.561592 50.238838 44.789623 46.495136 49.068993 50.859116 34.800060 29.068176 37.096851 54.951031 32.350891 32.776085 43.875008 61.288776 40.396778 43.783665 51.504829 57.929409 45.802296 48.920952 53.821930 51.945923 
```

## FilterMedianBorder

将中值滤波器应用于源图像src并将结果存储到目标图像dst。边框像素的值根据边框类型和边框值赋值参数获得。此过滤器的内核大小由参数指定。

函数声明如下：

HmppResult HMPPI\_FilterMedianBorder\_16s\_C1R\(const int16\_t\* pSrc, int srcStep, int16\_t\* pDst, int dstStep, HmppiSize dstRoiSize, HmppiSize maskSize, HmppiBorderType borderType, int16\_t borderValue\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|pDst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|maskSize|掩码大小。|maskSize.width >=3 且为奇数, maskSize.height >= 3且为奇数|输入|
|borderType|边界类型。|以下HmppiBorderType的枚举值之一：HMPPI_BORDER_CONST、HMPPI_BORDER_REPL、HMPPI_BORDER_MIRROR|输入|
|borderValue|指定常量边框像素的常量值，仅当borderType = HMPPI_BORDER_CONST时有意义。|数据类型范围内的值|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrc、pDst的入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize.width、roiSize.height这两个入参中存在值小于等于0。|
|HMPP_STS_STEP_ERR|srcStep、dstStep小于等于0。|
|HMPP_STS_ROI_ERR|roiSize.width * 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除。|
|HMPP_STS_BAD_ARG_ERR|不支持的borderType类型值或者不支持的maskSize值。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
#define SRC_BUFFER_SIZE_T 28
#define DST_BUFFER_SIZE_T 16
int MedianBorderExample()
{
    HmppiSize roi = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = { 13, 17, 5, -1, 9, -8, 44, -23, -5, 1, 7, 54, 75, -32, -9, -10, 11, -5, 15, -33, 7, -45, 112, 99, -45, 28, 64, 60};
    int16_t dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 4 * sizeof(int16_t);
    int32_t dstStep = 4 * sizeof(int16_t);
    HmppiSize mskSize = {3, 3}; 
    HmppResult result = HMPPI_FilterMedianBorder_16s_C1R(src, srcStep, dst, dstStep, roi, mskSize, HMPPI_BORDER_CONST, -1);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    } else {
        printf("result: %d\n", (int)result);
    }
    printf("dst= ");
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        printf("%d ",dst[i]);
    }
    printf("\n");
    return 0;
}
int main() {
    MedianBorderExample();
    return 0;
}
```

运行结果：

```text
result: 0
dst= -1 5 -1 -1 -1 7 5 -1 -1 1 -8 -1 -1 -1 -1 -1
```

## FilterMinBorder

对8位单通道图像应用最小值滤波，支持边界处理。

**函数接口声明如下：**

HmppResult HMPPI_FilterMinBorder_8u_C1R(const uint8_t *pSrc, int srcStep, uint8_t*pDst, int dstStep, HmppiSize dstRoiSize, HmppiSize maskSize, HmppiBorderType borderType, uint8_t borderValue);

滤波核中心位置定义如下：

    x = (markSize.width - 1) / 2

    y = (markSize.height - 1) / 2

**参数**

| 参数名      | 描述                                             | 取值范围                                                     | 输入/输出 |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ | --------- |
| pSrc        | 指向源图像感兴趣区域的指针。                     | 非空                                                         | 输入      |
| srcStep     | 源图像中连续行起点之间的距离（以字节为单位）。   | 非负整数                                                     | 输入      |
| pDst        | 指向目标图像感兴趣区域的指针。                   | 非空                                                         | 输出      |
| dstStep     | 目标图像中连续行起点之间的距离（以字节为单位）。 | 非负整数                                                     | 输入      |
| dstRoiSize  | 目标图像ROI的大小（以像素为单位）。              | dstRoiSize.width∈(0,INT_MAX]，dstRoiSize.height∈(0,INT_MAX]  | 输入      |
| maskSize    | 滤波核的大小。                                   | maskSize.width∈(0,INT_MAX],  maskSize.height∈(0,INT_MAX]，且为正奇数 | 输入      |
| borderType  | 边界填充类型。                                   | 单一边界类型：HMPPI_BORDER_CONST、HMPPI_BORDER_REPL、HMPPI_BORDER_IN_MEM；<br />混合边界类型：HMPPI_BORDER_REPL、HMPPI_BORDER_CONST、HMPPI_BORDER_MIRROR和HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_IN_MEM_LEFT、HMPPI_BORDER_IN_MEM_RIGHT进行或运算后的结果。 | 输入      |
| borderValue | 当borderType为HMPPI_BORDER_CONST时使用的常量值。 | 0-255                                                        | 输入      |

**返回值**

- 成功：返回HMPP_STS_NO_ERR。
- 失败：返回错误码。

**错误码**

| 错误码                | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| HMPP_STS_NULL_PTR_ERR | pSrc、pDst存在空指针。                                       |
| HMPP_STS_SIZE_ERR     | dstRoiSize.width、dstRoiSize.height、srcStep、dstStep小于等于0。 |
| HMPP_STS_BORDER_ERR   | maskSize.width、markSize.height小于等于0或不为正奇数；borderType非法。 |

**示例**

```c
#include "hmpp.h"
#include <stdio.h>

void FilterMinExample()
{
    uint8_t src[25] = {3,  12, 99, 108, 22,
                       6,  70,  32,  56, 77,
                       101, 23, 211, 85, 15,
                       1,  171, 18, 190, 165,
                       201, 12, 23, 42,  5};
    uint8_t dst[9] = {0};
    uint8_t* srcStart = &src[6];
    HmppiSize dstRoiSize = {3, 3};
    HmppiSize maskSize = {3, 3};
    HmppiBorderType borderType = HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_BOTTOM | HMPPI_BORDER_IN_MEM_RIGHT; 
    uint8_t borderValue = 0;

    HmppResult res = HMPPI_FilterMinBorder_8u_C1R(srcStart, 5, dst, 3, dstRoiSize, maskSize, borderType, borderValue);

    printf("result = %d\n", res);
    printf("dst = \n");

    for (int i = 0; i < 3; ++i) {
        for (int j = 0; j < 3; ++j) {
            printf("%3u ", dst[i * 3 + j]);
        }
        printf("\n");
    }
}

int main()
{
    FilterMinExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 
 23  23  15 
 18  18  15 
 12  12   5
```

## FilterSobelBorder

将Sobel滤波器应用于源图像src并将结果存储到目标图像dst。边框像素的值根据边框类型和边框值赋值参数获得。此过滤器的内核是参数指定的3x3或5x5大小的矩阵掩码。内核具有以下值，锚点位于中心单元格。

1 2 1

0 0 0

-1 -2 -1

1 4 6 4 1

2 8 12 8 2

0 0 0 0 0

-2 -8 -12 -8 -2

-1 -4 -6 -4 -1

函数声明如下：

HmppResult HMPPI\_FilterSobelHorizBorder\_32f\_C1R\(const float\* pSrc, int srcStep, float\* pDst, int dstStep, HmppiSize roi, HmppiMaskSize maskSize, HmppiBorderType borderType, float borderValue\);

HmppResult HMPPI\_FilterSobelNegVertBorder\_32f\_C1R\(const float\* pSrc, int srcStep, float\* pDst, int dstStep, HmppiSize roi, HmppiMaskSize maskSize, HmppiBorderType borderType, float borderValue\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|pDst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|maskSize|预定义掩码。|HmppMskSize3x3或HmppMskSize5x5|输入|
|borderType|边界类型。|以下HmppiBorderType的枚举值之一：HMPPI_BORDER_CONST、HMPPI_BORDER_REPL、HMPPI_BORDER_MIRROR|输入|
|borderValue|指定常量边框像素的常量值，仅当borderType = HMPPI_BORDER_CONST时有意义。|数据类型范围内的值|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrc、pDst的入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize.width、roiSize.height的入参中存在值小于等于0。|
|HMPP_STS_STEP_ERR|srcStep、dstStep小于等于0。|
|HMPP_STS_ROI_ERR|roiSize.width * 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除。|
|HMPP_STS_BAD_ARG_ERR|不支持的borderType类型值或者不支持的maskSize值。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
#define SRC_BUFFER_SIZE_T 28
#define DST_BUFFER_SIZE_T 16
int SobelBorderExample()
{
    HmppiSize roi = { 2, 4 };
    const float src[SRC_BUFFER_SIZE_T] = { 9.8375815e-16, 4.5484828e-16, 1.4972698e-14, -1.3397389e+25, 1.9061152e+27, -8.8007769e+17, -217025.86, -6.2768196e+14, 4.4335606e+33, 1.0258707e-13, -9.3150633e-11, -5192.7163, 2.536292e+25, 6.0832354e+15, 8.5712402e+29, -0.15451884, -0.2848247, -2.1714717e+10, 2.7212473e+17, -1.3591454e-22, 7.6473188e+21, -2.2729285e-30, 1129877.1, -1.0089557e-05, -1.1054221e+21, -3.8945858e-19, -6.8345717e-34, 60685416};
    float dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 4 * sizeof(int16_t);
    int32_t dstStep = 4 * sizeof(int16_t);
    HmppiMaskSize mskSize = HmppMskSize3x3; 
    HmppResult result = HMPPI_FilterSobelHorizBorder_32f_C1R(src, srcStep, dst, dstStep, roi, mskSize, HMPPI_BORDER_CONST, 0);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    } else {
        printf("result: %d\n", (int)result);
    }
    printf("dst= ");
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        printf("%e ",dst[i]);
    }
    printf("\n");
    return 0;
}
int main() {
    SobelBorderExample();
    return 0;
}
```

运行结果：

```text
result: 0
dst= -1.339739e+25 -2.679478e+25 3.812230e+27 1.906115e+27 1.339739e+25 2.679478e+25 -3.812230e+27 -1.906115e+27 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00 0.000000e+00
```
