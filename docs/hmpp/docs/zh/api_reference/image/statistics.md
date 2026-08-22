# 统计与计算机视觉

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## FloodFill

对图像进行泛洪填充。

函数接口声明如下：

HmppResult HMPPI\_FloodFill\_4Con\_8u\_C1IR\(uint8\_t \*srcDst, int srcDstStep, HmppiSize roiSize, HmppiPoint seed, uint8\_t newVal, HmppiConnectedComp \*pRegion\);

HmppResult HMPPI\_FloodFill\_8Con\_32f\_C1IR\(float \*srcDst, int srcDstStep, HmppiSize roiSize, HmppiPoint seed, float newVal, HmppiConnectedComp \*pRegion\);

HmppResult HMPPI\_FloodFill\_8Con\_8u\_C1IR\(uint8\_t \*srcDst, int srcDstStep, HmppiSize roiSize, HmppiPoint seed, uint8\_t newVal, HmppiConnectedComp \*pRegion\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向源和目标缓冲区的指针。|非空|输入/输出|
|srcDstStep|源图像和目标图像中连续行的起点之间的距离。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|非负整数|输入|
|seed|源图像中泛洪填充的起点。|seed.x∈(0, INT_MAX]，seed.y∈(0, INT_MAX]|输入|
|newVal|用于泛洪填充的新值。|输入数据类型的范围|输入|
|pRegion|指向连通分量结构体的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst，pRegion中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcDstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcDstStep不能被srcDst所属数据类型的字节长度整除的错误条件。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
void FloodFillExample()
{
    HmppiSize roi = {7, 7};
    uint8_t srcDst[50] = { 1, 1, 1, 1, 1, 1, 1,
                        0, 0, 1, 0, 0, 0, 1,
                        0, 1, 1, 1, 0, 1, 1,
                        0, 0, 0, 0, 1, 1, 1,
                        0, 0, 1, 0, 0, 1, 1,
                        0, 0, 0, 0, 1, 0, 1,
                        1, 1, 1, 1, 1, 1, 1
                      };
    int32_t srcDstStep = 7 * sizeof(uint8_t);
    HmppiPoint seed = {3, 3};
    uint8_t newVal = 2;
    HmppiConnectedComp pRegion;
    HmppResult res = HMPPI_FloodFill_4Con_8u_C1IR(srcDst, srcDstStep, roi, seed, newVal, &pRegion);
    printf("result: %d\n", (int)res);
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%d ", srcDst[i * 7 + j]);
        }
        printf("\n");
    }
}
int main()
{
    FloodFillExample();
    return 0;
}
```

运行结果：

```text
result: 0
1 1 1 1 1 1 1 
2 2 1 0 0 0 1 
2 1 1 1 0 1 1 
2 2 2 2 1 1 1 
2 2 1 2 2 1 1 
2 2 2 2 1 0 1 
1 1 1 1 1 1 1
```

## CountInRange

此函数功能是统计源图像指定区域内像素值处于上下界区间内的像素个数，并保存至counts中。在多通道情况下，给定范围的像素数在每个通道上分别计算并存储到数组counts中。

函数接口声明如下：

- **单通道操作：**

    HmppResult HMPPI\_CountInRange\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t \*counts, float lowerBound, float upperBound\);

    HmppResult HMPPI\_CountInRange\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t \*counts, uint8\_t lowerBound, uint8\_t upperBound\);

- **多通道操作：**

    HmppResult HMPPI\_CountInRange\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t counts\[3\], uint8\_t lowerBound\[3\], uint8\_t upperBound\[3\]\);

    HmppResult HMPPI\_CountInRange\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t counts\[3\], uint8\_t lowerBound\[3\], uint8\_t upperBound\[3\]\);

    HmppResult HMPPI\_CountInRange\_32f\_C3R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t counts\[3\], float lowerBound\[3\], float upperBound\[3\]\);

    HmppResult HMPPI\_CountInRange\_32f\_AC4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, int32\_t counts\[3\], float lowerBound\[3\], float upperBound\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|counts|指向像素值在给定范围的像素数的指针（用于单通道函数）。|非负整数|输入/输出|
|counts[]|包含各个通道像素值在给定范围内像素数的数组（用于多通道函数）。|非负整数|输入/输出|
|lowerBound|给定强度范围的下界（用于单通道数据）。|输入数据类型的范围|输入|
|lowerBound[]|包含各个通道的给定强度范围的下界的数组（用于多通道数据）。|输入数据类型的范围|输入|
|upperBound|给定强度范围的上界（用于单通道数据）。|输入数据类型的范围|输入|
|upperBound[]|包含各个通道的给定强度范围的上界的数组（用于多通道数据）。|输入数据类型的范围|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep不能被src所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_RANGE_ERR|lowerBound大于upperBound。|

**示例**

```c
void CountInRangeExample()
{
    HmppiSize roi = {3, 4};
    uint8_t src[9 * 4] = {1, 1, 1, 2, 2, 2, 3, 3, 3, 
                          4, 4, 4, 5, 5, 5, 6, 6, 6, 
                          7, 7, 7, 8, 8, 8, 9, 9, 9,
                          10, 10, 10, 11, 11, 11, 12, 12, 12};
    int32_t srcStep =9*sizeof(uint8_t);
    int32_t counts[3] = {0};
    uint8_t lowerBound[3] = {0, 2, 4};
    uint8_t upperBound[3] = {8, 8, 8};
    int32_t i;
    HmppResult result =HMPPI_CountInRange_8u_C3R(src, srcStep, roi, counts, lowerBound, upperBound);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for(i =0; i <3; i++) {
        printf("%d ", counts[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
8 7 5
```

## DistanceTransform

计算源图像中每个非零像素到最近的零像素的距离。

函数接口声明如下：

HmppResult HMPPI\_DistanceTransform\_3x3\_8u32f\_C1R\(const uint8\_t\* pSrc, int srcStep, float\* pDst, int dstStep, HmppiSize roiSize, float\* pMetrics\);

其中pMetrics可以定义为如下数组：

- **曼哈顿距离**

    ```c
    Ipp32f pMetrics[3*3] = {
         1, 1, 1,
         1, 0, 1,
         1, 1, 1 
    };
    ```

- **欧几里得距离**

    ```c
    Ipp32f pMetrics[3*3] = {
         1.4142f, 1.0f, 1.4142f,
         1.0f,    0.0f, 1.0f,
         1.4142f, 1.0f, 1.4142f
     };
    ```

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|pDst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|pMetrics|该指针指向一个用户定义的邻域代价表，用于自定义距离传播规则。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrc、pDst、pMetrics中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
void DistanceTransformExample()
{
    HmppiSize roi = {5, 7};
    uint8_t src[45] = {1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5
                      };
    float dst[49] = {0};
    int32_t srcStep = 5 * sizeof(uint8_t);
    int32_t dstStep = 7 * sizeof(float);
    float pMetrics[3*3] = {
        1, 1, 1,
        1, 0, 1,
        1, 1, 1 
    };
    HmppResult res = HMPPI_DistanceTransform_3x3_8u32f_C1R(src, srcStep, dst, dstStep, roi, pMetrics);
    printf("result: %d\n", (int)res);
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%f ", dst[i * 7 + j]);
        }
        printf("\n");
    }
}
int main()
{
    DistanceTransformExample();
    return 0;
}
```

运行结果：

```text
result: 0
1.00 2.00 3.00 2.00 2.00 0.00 0.00 
1.00 2.00 3.00 2.00 1.00 0.00 0.00 
1.00 2.00 3.00 2.00 1.00 0.00 0.00 
1.00 2.00 3.00 2.00 1.00 0.00 0.00 
1.00 2.00 3.00 2.00 1.00 0.00 0.00 
1.00 4.00 3.00 2.00 1.00 0.00 0.00 
5.00 4.00 3.00 2.00 1.00 0.00 0.00
```

## Histogram

此函数计算源图像每个通道的强度直方图，并将结果存储在hist数组中。实际操作为划分几个区间，指定图像统计的区域，计算区域内在各区间中的像素值的数量：

- levels是级别数组，长度为levelsLen，一个区间由levels相邻两个值组成，左闭右开；hist数组对区间内数进行统计，因此hist数组长度为levelsLen – 1。
- hist\[k\]是源图像像素src\(x, y\)的数量，该像素满足条件levels\[k\]<=src\(x,y\)<levels\[k+1\]。

函数接口声明如下：

- **初始化函数：**

    HmppResult HMPPI\_HistogramInit\(HmppDataType dataType, const float \*levels\[\], int32\_t levelsLen\[\], int32\_t numChannels,HmppiHistogramPolicy \*\*policy\);

    HmppResult HMPPI\_HistogramUniformInit\(HmppDataType dataType, float lowerLevel\[\], float upperLevel\[\],int32\_t levelsLen\[\], int32\_t numChannels, HmppiHistogramPolicy \*\*policy\);

- **单通道主函数：**

    HmppResult HMPPI\_Histogram\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist, const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist, const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist, const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist, const HmppiHistogramPolicy \*policy\);

- **多通道主函数：**

    HmppResult HMPPI\_Histogram\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[3\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[3\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[3\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_32f\_C3R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[3\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[4\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[4\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[4\], const HmppiHistogramPolicy \*policy\);

    HmppResult HMPPI\_Histogram\_32f\_C4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, uint32\_t \*hist\[4\], const HmppiHistogramPolicy \*policy\);

- **获取区间边界函数：**

    HmppResult HMPPI\_HistogramGetLevels\(const HmppiHistogramPolicy \*policy, float \*levels\[\]\);

- **释放函数：**

    HmppResult HMPPI\_HistogramRelease\(HmppiHistogramPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像ROI的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离，以字节为单位。|(0,INT_MAX]|输入|
|roiSize|源图像ROI的大小，单位为像素。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|hist|指向计算直方图的指针。在多通道数据的情况下，hist是指向每个通道直方图的指针数组。|非空|输出|
|datatype|源图像的数据类型。|HMPP8U、HMPP16UHMPP16S、HMPP32F|输入|
|Levels|指向级别数组的指针。|非空|输入|
|lowerLevel|Uniform类型直方图的级别下边界，每个通道分开。|非空|输入|
|upperLevel|Uniform类型直方图的级别上边界，每个通道分开。|非空|输入|
|numChannels|通道数。|1、3、4|输入|
|levelsLen|级别数组的长度。每个通道都有单独的级别数量。|各通道级别数组总长度不超过50,000,000。一般情况下，整型类接口级别数组长度不会超过该类型最小最大值之间整数的数量，浮点类接口级别数组长度也远不会达到50000000这个级别的数量。|输入|
|policy（init函数中）|指向HistogramPolicy结构体的双重指针。|非空|输出|
|policy（主函数中和release函数中）|指向Histogram结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_DATETYPE_ERR|传入的数据类型不是HMPP8U、HMPP16U、HMPP16S、HMPP32F中的一种。|
|HMPP_STS_HISTOLEVELS_ERR|级别数组长度小于2。|
|HMPP_STS_NUMCHANNELS_ERR|通道数不是1、3、4其中一个。|
|HMPP_STS_MALLOC_FAILED|函数中进行内存申请失败。|
|HMPP_STS_OVERFLOW|各通道的总级别数组长度超过500000000。|
|HMPP_STS_POLICY_STATE_ERR|policy结构体标记值错误。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_RANGE_ERR|Init模式levels数组不是非递减序列，或Uniform模式lowerLevel大于upperLevel。|

**示例**

```c
void HistogramExample()
{
    const int32_t numChannels = 3;
    HmppiSize roiSize = {2,3};
    uint8_t src[28] = { 1, 2, 3, 4, 5, 6, 7, 
                        102, 103, 104, 105, 106, 107, 108,
                        203, 204, 205, 255, 255, 255, 209,
                        4, 5, 6, 7, 8, 9, 10 };
    int32_t srcStep = 7 * sizeof(uint8_t);
    int32_t levelsLen[3] = {3, 4, 5};
    float p0[3] = {0, 105, 256};
    float p1[4] = {0, 100, 200, 256};
    float p2[5] = {0, 60, 105, 205, 256};
    const float *levels[3] = {p0, p1, p2};
    uint32_t h0[2], h1[3], h2[4];
    uint32_t *hist[3] = {h0, h1, h2};

    HmppiHistogramPolicy *policy = NULL;
    HmppResult result = HMPPI_HistogramInit(HMPP8U, levels, levelsLen, numChannels, &policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Histogram Init error: %d\n", result);
    }
    result = HMPPI_Histogram_8u_C3R(src, srcStep, roiSize, hist, policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Histogram error: %d\n", result);
    }
    HMPPI_HistogramRelease(policy);

    printf("hist:\n");
    for (int32_t i = 0; i < 3; i++){
        for (int32_t j = 0; j < levelsLen[i] - 1; j++){
            printf("%d ", hist[i][j]);
        }
        printf("\n");
    }
}

int main()
{
    HistogramExample();
    return 0;
}

```

运行结果：

```text
hist:
3 3
2 2 2
2 1 1 2
```

## LabelMarkers

给图像的每个连通分量用数字标签标记。

函数接口声明如下：

- **原地操作**

    HmppResult HMPPI\_LabelMarkers\_8u\_C1IR\(uint8\_t\* pMarker, int markerStep, HmppiSize roiSize, int minLabel, int maxLabel, HmppiNorm norm, int\* pNumber\);

    HmppResult HMPPI\_LabelMarkers\_16u\_C1IR\(uint16\_t\* pMarker, int markerStep, HmppiSize roiSize, int minLabel, int maxLabel, HmppiNorm norm, int\* pNumber\);

- **非原地操作**

    HmppResult HMPPI\_LabelMarkers\_16u8u\_C1R\(uint16\_t\* pSrc, int srcMarkerStep, uint8\_t\* pMarker, int dstMarkerStep, HmppiSize roiSize, int minLabel, int maxLabel, HmppiNorm norm, int\* pNumber\);

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pMarker|指向源图像和目标图像感兴趣区域的指针（原地操作）。|非空|输入，输出|
|markerStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源图像和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|minLabel|最小标签值。|minLabel∈[1, 255], minLabel <= maxLabel|输入|
|maxLabel|最大标签值。|maxLabel∈[1, 255], minLabel <= maxLabel|输入|
|pSrcMarker|指向源图像感兴趣区域的指针（非原地操作）。|非空|输入|
|pMarker|指向目标图像感兴趣区域的指针（非原地操作）。|非空|输出|
|srcMarkerStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dstMarkerStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|norm|标志使用4连接还是8连接方式计算连通分量。|HMPP_NORML1, HMPP_NORMINF, 其中L1代表4连接，INF代表8连接|输入|
|pNumber|指向连通分量数量的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pMarker，pSrcMarker中存在空指针。|
|HMPP_STS_STEP_ERR|markerStep小于或等于0。|
|HMPP_STS_BAD_ARG_ERR|minLabel, maxLabel不在[1, 255]范围内或minLabel > maxLabel。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_MALLOC_FAILED|使用的辅助缓冲区分配失败。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
void LabelMarkersExample()
{
    HmppiSize roi = {7, 6};
    uint8_t srcDst[50] = { 0, 0, 0, 0, 1, 1, 0,
                        0, 1, 1, 0, 1, 1, 0,
                        0, 1, 1, 0, 1, 1, 0,
                        0, 0, 0, 0, 1, 1, 0,
                        0, 0, 1, 0, 0, 0, 0,
                        0, 0, 0, 0, 0, 0, 1,
                      };
    int32_t srcDstStep = 7 * sizeof(uint8_t);
    HmppiPoint seed = {3, 3};
    uint8_t minLabel = 1;
    uint8_t maxLabel = 8;
    HmppiNorm norm = HMPP_NORML1;
    int number;
    HmppResult res = HMPPI_LabelMarkers_8u_C1IR(srcDst, srcDstStep, roi, minLabel, maxLabel,norm, &number);
    printf("result: %d\n", (int)res);
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%d ", srcDst[i * 7 + j]);
        }
        printf("\n");
    }
}
int main()
{
    LabelMarkersExample();
    return 0;
}
```

运行结果：

```text
result: 0
0 0 0 0 1 1 0 
0 2 2 0 1 1 0 
0 2 2 0 1 1 0 
0 0 0 0 1 1 0 
0 0 3 0 0 0 0 
0 0 0 0 0 0 4 
0 0 0 0 0 0 0
```

## Min

计算图像像素值的最小值。

函数接口声明如下：

- **单通道数据操作：**

    HmppResult HMPPI\_Min\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t \*min\);

    HmppResult HMPPI\_Min\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t \*min\);

    HmppResult HMPPI\_Min\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t \*min\);

    HmppResult HMPPI\_Min\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float \*min\);

- **多通道数据操作：**

    HmppResult HMPPI\_Min\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t min\[3\]\);

    HmppResult HMPPI\_Min\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t min\[3\]\);

    HmppResult HMPPI\_Min\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t min\[3\]\);

    HmppResult HMPPI\_Min\_32f\_C3R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float min\[3\]\);

    HmppResult HMPPI\_Min\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t min\[4\]\);

    HmppResult HMPPI\_Min\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t min\[4\]\);

    HmppResult HMPPI\_Min\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t min\[4\]\);

    HmppResult HMPPI\_Min\_32f\_C4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float min\[4\]\);

    HmppResult HMPPI\_Min\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t min\[3\]\);

    HmppResult HMPPI\_Min\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t min\[3\]\);

    HmppResult HMPPI\_Min\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t min\[3\]\);

    HmppResult HMPPI\_Min\_32f\_AC4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float min\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源图像和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|min|指向最小像素值的指针（用于单通道数据）。|非空|输入，输出|
|min[]|包含源缓冲区中像素的最小通道值的数组（用于多通道数据）。|非空|输入，输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_NOT_EVEN_STEP_ERROR|srcStep不能被src所属数据类型的字节长度整除。|

**示例**

```c
void ThresholdExample()
{
    HmppiSize roi = {9,3};
    uint8_t src[52] = {  10, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48, 
                     11, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48, 
                     12, 4, 8, 13, 16, 20, 3, 28, 32, 36, 40, 1, 255, 
                     13, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48};
    int32_t srcStep = 13*sizeof(uint8_t);
    uint8_t min;
    HMPPI_Min_8u_C1R(src, srcStep, roi, &min);
    printf("min=%d ", min);
    printf("\n");
}

int main(){
    ThresholdExample();
    return 0;
}
```

测试结果：

```text
min = 3
```

## MinEvery

计算两个图像中每对位置相同的像素中的最小值，存储到目标图像中。

函数接口声明如下：

- **选择单通道最小像素值**：

    HmppResult HMPPI\_MinEvery\_32f\_C1R\(const float \*pSrc1, int src1Step, const float \*pSrc2, int src2Step, float \*pDst, int dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc1|指向源图像1感兴趣区域的指针。|非空|输入|
|src1Step|源图像1中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|pSrc2|指向源图像2感兴趣区域的指针。|非空|输入|
|src2Step|源图像2中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|pDst|指向目标图像的指针。|非空|输出|
|dstStep|目标图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_NOT_EVEN_STEP_ERROR|srcStep不能被src所属数据类型的字节长度整除。|

**示例**

```c
#include <hmpp.h>
#include <stdio.h>
#define BUFFER_SIZE_T 10
void MinEveryExample(void)
{
    float src1[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float src2[BUFFER_SIZE_T] = {9.54, 3.77, 2.1, -3.77, 1.59, 2.9, -3.77, 8.91, 0.19, -0.11};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppiSize roiSize = {2, 5};
    HmppResult result = HMPPI_MinEvery_32f_C1R(src1, 2 * sizeof(float), src2, 2 * sizeof(float),
        dst, 2 * sizeof(float), roiSize);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
int main() {
    MinEveryExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 1.64 1.63 -1.09 -3.77 -3.20 -0.43 -3.77 -4.83 0.19 -4.40
```

## Max

此函数计算源图像指定区域内的最大像素值。在多通道图像情况下，最大值在每个通道上分别计算并存储在数组max中。

函数接口声明如下：

- **选择单通道最大像素值：**

    HmppResult HMPPI\_Max\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t \*max\);

    HmppResult HMPPI\_Max\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t \*max\);

    HmppResult HMPPI\_Max\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t \*max\);

    HmppResult HMPPI\_Max\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float \*max\);

- **选择多通道最大像素值：**

    HmppResult HMPPI\_Max\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t max\[3\]\);

    HmppResult HMPPI\_Max\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t max\[3\]\);

    HmppResult HMPPI\_Max\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t max\[3\]\);

    HmppResult HMPPI\_Max\_32f\_C3R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float max\[3\]\);

    HmppResult HMPPI\_Max\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t max\[3\]\);

    HmppResult HMPPI\_Max\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t max\[3\]\);

    HmppResult HMPPI\_Max\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t max\[3\]\);

    HmppResult HMPPI\_Max\_32f\_AC4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float max\[3\]\);

    HmppResult HMPPI\_Max\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint8\_t max\[4\]\);

    HmppResult HMPPI\_Max\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, uint16\_t max\[4\]\);

    HmppResult HMPPI\_Max\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, int16\_t max\[4\]\);

    HmppResult HMPPI\_Max\_32f\_C4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, float max\[4\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|max|指向最大像素值的指针（用于单通道数据）。|非空|输入/输出|
|max[]|指向最大像素值的数组（用于多通道数据）。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_NOT_EVEN_STEP_ERROR|srcStep不能被src所属数据类型的字节长度整除。|

**示例**

```c
void MaxExample()
{
    HmppiSize roi = {6, 6};
    const float src[8*8] = {124.5, 913, 13453434, 57.5, 23.75, 63.375, 98.0625, 2343,
                 57.540001, 16.845346, 256.14001, 98.709999, 736.23999, 459.64999, 80.102997, 90.989998, 
                 4013, 4238, 11940, 32200, 15709, 38807, 4239, 11942,
                 95345008, 25438044, 8.5345428e+09, 1.0534482e+08, 0.2, 0.23, 0.36000001, 0.56999999,
                 0.25001001, 0.87011999, 0.99010998, 0.54004002, 0.25999999, 0.33000001, 0.63, 0.75,
                 0.75001001, 0.90012002, 53450.691, 0.34004, 16411.25, 12293.375, 12779.5, 15498.062,
                 16411.211, 12293.377, 12779.52, 15498.062, 65519, 65520, 65519, 65520,
                 65504, 65504, 0.00013, 0.00013, 0, 1, 1, 3.4028235e+38};

    float vMax;
    int32_t srcStep = 8 * sizeof(float);
    HmppResult result = HMPPI_Max_32f_C1R(src, srcStep, roi, &vMax);
    printf("result = %d \n vMax = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    printf("%f\n", vMax);
    return 0;
}

int main(void)
{
    MaxExample();
    return 0;
}
```

运行结果：

```text
result = 0
vMax = 8.5345428e+09
```

## Mean

此函数功能是计算图像像素平均值。对于多通道图像，均值是在每个通道中分别计算，并将结果保存在对应通道数组中。

函数接口声明如下：

- **单通道均值：**

    HmppResult HMPPI\_Mean\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, double \*mean\);

    HmppResult HMPPI\_Mean\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double \*mean\);

    HmppResult HMPPI\_Mean\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double \*mean\);

    HmppResult HMPPI\_Mean\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, double \*mean, HmppHintAlgorithm hint\);

- **带蒙版的单通道均值：**

    HmppResult HMPPI\_Mean\_8u\_C1MR\(const uint8\_t \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, double \*mean\);

    HmppResult HMPPI\_Mean\_16u\_C1MR\(const uint16\_t \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, double \*mean\);

    HmppResult HMPPI\_Mean\_32f\_C1MR\(const float \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, double \*mean\);

- **三通道均值**

    HmppResult HMPPI\_Mean\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[3\]\);

    HmppResult HMPPI\_Mean\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[3\]\);

    HmppResult HMPPI\_Mean\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[3\]\);

    HmppResult HMPPI\_Mean\_32f\_C3R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[3\], HmppHintAlgorithm hint\);

- **带蒙版和COI的多通道均值：**

    HmppResult HMPPI\_Mean\_8u\_C3CMR\(const uint8\_t \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, int32\_t coi, double \*mean\);

    HmppResult HMPPI\_Mean\_16u\_C3CMR\(const uint16\_t \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, int32\_t coi, double \*mean\);

    HmppResult HMPPI\_Mean\_32f\_C3CMR\(const float \*src, int32\_t srcStep, const uint8\_t \*mask, int32\_t maskStep, HmppiSize roiSize, int32\_t coi, double \*mean\);

- **四通道均值：**

    HmppResult HMPPI\_Mean\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[4\]\);

    HmppResult HMPPI\_Mean\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[4\]\);

    HmppResult HMPPI\_Mean\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[4\]\);

    HmppResult HMPPI\_Mean\_32f\_C4R\(const float \*src, int32\_t srcStep, HmppiSize roiSize, double mean\[4\], HmppHintAlgorithm hint\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|mean[]|保存源图像各通道平均像素值的数组（用于多通道函数）。|源图像像素数据类型取值范围|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep不能被src所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_COI_ERR|coi不是1、2、3。|

**示例**

```c
void MeanExample()
{
    HmppiSize roi = {3, 4};
    uint8_t src[9 * 4] = {1, 1, 1, 2, 2, 2, 3, 3, 3, 
                          4, 4, 4, 5, 5, 5, 6, 6, 6, 
                          7, 7, 7, 8, 8, 8, 9, 9, 9,
                          10, 10, 10, 11, 11, 11, 12, 12, 12};
    int32_t srcStep =9*sizeof(uint8_t);
    double mean[3] = {0.0};
    int32_t i;
    HmppResult result =HMPPI_Mean_8u_C3R(src, srcStep, roi, mean);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for(i =0; i <3; i++) {
        printf("%lf ", mean[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
6.500000 6.500000 6.500000
```

## Mean\_StdDev

此函数功能是计算图像像素值的平均值和标准差。

函数接口声明如下：

- **单通道均值和标准差：**

    HmppResult HMPPI\_Mean\_StdDev\_32f\_C1R\(const float\* pSrc, int srcStep, HmppiSize roiSize, double\* pMean, double\* pStdDev\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|pMean|指向像素值计算平均值的指针。|非空|输入/输出|
|pStdDev|指向图像中像素值计算得到的标准差的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep不能被src所属数据类型的字节长度整除的错误条件。|

**示例**

```c
void Mean_StdDevExample()
{
    HmppiSize roiSize = {3, 3};
    float pSrc[] = {1.0, 2.0, 3.0,
                    4.0, 5.0, 6.0,
                    7.0, 8.0, 9.0};
    double pMean, pStdDev; 
    int srcStep = roiSize.width * sizeof(float);
    HmppResult result = HMPPI_Mean_StdDev_32f_C1R(pSrc, srcStep, roiSize, &pMean, &pStdDev);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("%lf %lf\n", pMean, pStdDev);
}
```

运行结果：

```text
result = 0
5.000000 2.581989
```

## TrueDistanceTransform

计算源图像中每个非零像素到最近的零像素的欧式距离。

函数接口声明如下：

HmppResult HMPPI\_TrueDistanceTransform\_8u32f\_C1R\(const uint8\_t\* pSrc, int srcStep, float\* pDst, int dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|pDst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrc、pDst、pMetrics中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
void TrueDistanceTransformExample()
{
    HmppiSize roi = {5, 7};
    uint8_t src[45] = {1, 0, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 0, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 5,
                       1, 2, 3, 4, 0,
                       1, 2, 3, 4, 5
                      };
    float dst[49] = {0};
    int32_t srcStep = 5 * sizeof(uint8_t);
    int32_t dstStep = 7 * sizeof(float);
    HmppResult res = HMPPI_TrueDistanceTransform_8u32f_C1R(src, srcStep, dst, dstStep, roi);
    printf("result: %d\n", (int)res);
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%.2f ", dst[i * 7 + j]);
        }
        printf("\n");
    }
}
int main()
{
    TrueDistanceTransformExample();
    return 0;
}
```

运行结果：

```text
result: 0
1.00 0.00 1.00 2.00 3.00 0.00 0.00 
1.41 1.00 1.41 2.24 2.83 0.00 0.00 
2.24 1.41 1.00 1.41 2.24 0.00 0.00 
2.00 1.00 0.00 1.00 2.00 0.00 0.00 
2.24 1.41 1.00 1.41 2.24 0.00 0.00 
2.83 2.24 2.00 2.24 2.83 0.00 0.00 
3.61 3.16 3.00 3.16 3.61 0.00 0.00
```
