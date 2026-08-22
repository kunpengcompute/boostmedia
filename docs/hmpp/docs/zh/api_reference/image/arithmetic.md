# 基础运算与数据交换

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Add

将两个图像相加。

函数接口声明如下:

HmppResult HMPPI\_Add\_32f\_C1R\(const float\* pSrc1, int src1Step, const float\* pSrc2, int src2Step, float\* pDst, int dstStep, HmppiSize roiSize\);

HmppResult HMPPI\_Add\_32f\_C3R\(const float\* pSrc1, int src1Step, const float\* pSrc2, int src2Step, float\* pDst, int dstStep, HmppiSize roiSize\);

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

**示例**

```c
#define BUFFER_SIZE_T 10
void AddExample(void)
{
    float src1[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float src2[BUFFER_SIZE_T] = {9.54, 3.77, 2.1, -3.77, 1.59, 2.9, -3.77, 8.91, 0.19, -0.11};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppiSize roiSize = {2, 5};
    HmppResult result = HMPPI_Add_32f_C1R(src1, 2 * sizeof(float), src2, 2 * sizeof(float),
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
    AddExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 11.18 5.40 1.01 -3.06 -1.61 2.47 -3.36 4.08 5.55 -4.51
```

## ComputeThreshold\_Otsu

计算Otsu阈值的值。计算公式为：

![](../../figures/zh-cn_formulaimage_0000002518281748.png)

![](../../figures/zh-cn_formulaimage_0000002549921497.png)

w0：前景点所占的比例。

w1：背景点所占比例\(1 - w0\)。

u0：前景点灰度值。

u1：背景点的灰度值。

u：图像整体的均值。

函数接口声明如下：

**单通道数据的阈值操作：**

HmppResult HMPPI\_ComputeThreshold\_Otsu\_8u\_C1R\(const uint8\_t\* src, int32\_t srcStep, HmppiSize roiSize, uint8\_t\* threshold\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非空|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|指向Otsu阈值的指针。|输入数据类型的范围|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#define BUFFER_SIZE 36
int ComputeThreshold_Ostu()
{
    HmppiSize roi = { 9, 4 };
    const uint8_t src[BUFFER_SIZE] = { 1, 2, 4, 8, 16, 8, 4, 2, 1,
                                       1, 2, 4, 8, 16, 8, 4, 2, 1,
                                       1, 2, 4, 8, 16, 8, 4, 2, 1,
                                       1, 2, 4, 8, 16, 8, 4, 2, 1};
    uint8_t threshold;
    int32_t srcStep = 9 * sizeof(uint8_t);
    HmppResult result = HMPPI_ComputeThreshold_Otsu_8u_C1R(src, srcStep, roi, &threshold);

    printf("result = %d \n dst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    printf("%d\n", threshold);
    return 0;
}
```

运行结果：

```text
result = 0
dst = 4
```

## CompareC

通过特定的比较方法将图像的每一个像素值与一个固定值比较。

函数接口声明如下：

HmppResult HMPPI\_CompareC\_8u\_C1R\(const uint8\_t\* pSrc, int srcStep, uint8\_t value, uint8\_t\* pDst, int dstStep, HmppiSize roiSize, HmppCmpOp cmpOp\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|value|用于比较的固定值|[-UINT8_MAX, UINT8_MAX]|输入|
|pDst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0, INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|
|cmpOp|枚举，指示使用的比较操作。|HMPP_CMP_EQ： 比较像素与固定值是否相等。HMPP_CMP_GE： 比较像素是否大于等于固定值。HMPP_CMP_LE： 比较像素是否小于等于固定值。HMPP_CMP_GT： 比较像素是否大于固定值。HMPP_CMP_LT： 比较像素是否小于固定值。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max中存在空指针。|
|HMPP_STS_STEP_ERR|srcStep小于或等于0。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
void CompareCExample()
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
    uint8_t dst[49] = {0};
    int32_t srcStep = 5 * sizeof(uint8_t);
    int32_t dstStep = 7 * sizeof(uint8_t);
    int32_t value = 3;
    HmppCmpOp cmpop = HMPP_CMP_GE;
    (void)HMPPI_CompareC_8u_C1R(src, srcStep, value, dst, dstStep, roi, cmpop);
    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%d ", dst[i * 7 + j]);
        }
        printf("\n");
    }
}
int main()
{
    CompareCExample();
    return 0;
}
```

运行结果：

```text
0 0 255 255 255 0 0 
0 0 255 255 255 0 0 
0 0 255 255 255 0 0 
0 0 255 255 255 0 0 
0 0 255 255 255 0 0 
0 0 255 255 255 0 0 
0 0 255 255 255 0 0
```

## Convert

该类接口可将图像像素值从一种数据类型转换为另一种数据类型。

函数接口声明如下：

- **将无符号整型类型转为无符号整型类型：**

    HmppResult HMPPI\_Convert\_8u16u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u8u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u8u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u8u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u8u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将无符号整型类型转为有符号整型类型：**

    HmppResult HMPPI\_Convert\_8u16s\_C1R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16s\_C3R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16s\_C4R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u16s\_AC4R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32s\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32s\_C3R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32s\_C4R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32s\_AC4R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32s\_C1R\(const uint16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32s\_C3R\(const uint16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32s\_C4R\(const uint16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32s\_AC4R\(const uint16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将无符号整型类型转为浮点类型：**

    HmppResult HMPPI\_Convert\_8u32f\_C1R\(const uint8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32f\_C3R\(const uint8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32f\_C4R\(const uint8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8u32f\_AC4R\(const uint8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32f\_C1R\(const uint16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32f\_C3R\(const uint16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32f\_AC4R\(const uint16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16u32f\_C4R\(const uint16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32u32f\_C1R\(const uint32\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将有符号整型类型转为无符号整数类型：**

    HmppResult HMPPI\_Convert\_8s8u\_C1Rs\(const int8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s16u\_C1Rs\(const int8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32u\_C1Rs\(const int8\_t \*src, int32\_t srcStep, uint32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s8u\_C1R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s8u\_C3R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s8u\_C4R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s8u\_AC4R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s16u\_C1Rs\(const int16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32u\_C1Rs\(const int16\_t \*src, int32\_t srcStep, uint32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8u\_C1R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8u\_C3R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8u\_AC4R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8u\_C4R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s32u\_C1Rs\(const int32\_t \*src, int32\_t srcStep, uint32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将有符号整型类型转为有符号整数类型：**

    HmppResult HMPPI\_Convert\_8s16s\_C1R\(const int8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32s\_C1R\(const int8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32s\_C3R\(const int8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32s\_C4R\(const int8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32s\_AC4R\(const int8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32s\_C1R\(const int16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32s\_C3R\(const int16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32s\_C4R\(const int16\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8s\_C1R\(const int32\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8s\_C3R\(const int32\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8s\_AC4R\(const int32\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s8s\_C4R\(const int32\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将有符号整型类型转为浮点类型：**

    HmppResult HMPPI\_Convert\_8s32f\_C1R\(const int8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s64f\_C1R\(const int8\_t \*src, int32\_tsrcStep, double \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32f\_C3R\(const int8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32f\_C4R\(const int8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_8s32f\_AC4R\(const int8\_t \*src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32f\_C1R\(const int16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32f\_C3R\(const int16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32f\_AC4R\(const int16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_16s32f\_C4R\(const int16\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Convert\_32s32f\_C1R\(const int32\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **将浮点数转为无符号整数类型：**

    HmppResult HMPPI\_Convert\_32f8u\_C1R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8u\_C3R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8u\_AC4R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8u\_C4R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16u\_C1R\(const float \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16u\_C3R\(const float \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16u\_AC4R\(const float \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16u\_C4R\(const float \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

- **将浮点数转为有符号整数类型：**

    HmppResult HMPPI\_Convert\_32f8s\_C1R\(const float \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8s\_C3R\(const float \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8s\_AC4R\(const float \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f8s\_C4R\(const float \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16s\_C1R\(const float \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16s\_C3R\(const float \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16s\_AC4R\(const float \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

    HmppResult HMPPI\_Convert\_32f16s\_C4R\(const float \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode\);

- **带缩放的有符号整数之间的转换：**

    HmppResult HMPPI\_Convert\_16s8s\_C1R\_S\(const int16\_t \*src, int32\_t srcStep, int8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32s16s\_C1R\_S\(const int32\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

- **带缩放的无符号整型之间的转换：**

    HmppResult HMPPI\_Convert\_32u8u\_C1R\_S\(const uint32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32u16u\_C1R\_S\(const uint32\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

- **带缩放的有符号整数转为无符号整数：**

    HmppResult HMPPI\_Convert\_32s16u\_C1R\_S\(const int32\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

- **带缩放的无符号整数转为有符号整数：**

    HmppResult HMPPI\_Convert\_8u8s\_C1R\_S\(const uint8\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_16u8s\_C1R\_S\(const uint16\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_16u16s\_C1R\_S\(const uint16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32u8s\_C1R\_S\(const uint32\_t \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32u16s\_C1R\_S\(const uint32\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32u32s\_C1R\_S\(const uint32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

- **带缩放的浮点数和整数之间的转换：**

    HmppResult HMPPI\_Convert\_32f8u\_C1R\_S\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32f8s\_C1R\_S\(const float \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32f16u\_C1R\_S\(const float \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32f16s\_C1R\_S\(const float \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32f32u\_C1R\_S\(const float \*src, int32\_t srcStep, uint32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_32f32s\_C1R\_S\(const float \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_64f8u\_C1R\_S\(const double \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_64f8s\_C1R\_S\(const double \*src, int32\_t srcStep, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_64f16u\_C1R\_S\(const double \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

    HmppResult HMPPI\_Convert\_64f16s\_C1R\_S\(const double \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

- **带缩放的浮点数和无符号整数之间的原地转换：**

    HmppResult HMPPI\_Convert\_32f32u\_C1IR\_S\(uint32\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppRoundMode roundMode, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向源图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|srcDstStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|scale|比例因子。|[INT_MIN, INT_MAX]|输入|
|roundMode|HMPP_RND_ZERO：指定将浮点值截断为零。|0|输入|
|HMPP_RND_NEAR：指定当小数部分等于0.5时，浮点值四舍五入为最接近的偶数整数；否则，浮点值四舍五入为最接近的整数。|1|输入|
|HMPP_RND_FINANCIAL：指定当小数部分小于0.5时，浮点值向下舍入为最接近的整数；如果小数部分等于或大于0.5，则向上舍入为最接近的整数。|2|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|输入模式为不支持的舍入模式。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 28
#define DST_BUFFER_SIZE_T 28

int ConvertExample(){
    int32_t i;
    HmppiSize roi = {5,4};
    float dst[DST_BUFFER_SIZE_T] = {0.0f};
    uint8_t src[SRC_BUFFER_SIZE_T] = { 1, 2, 4, 8, 16, 8, 4, 
                                       1, 2, 4, 8, 16, 8, 4, 
                                       1, 2, 4, 8, 16, 8, 4, 
                                       1, 2, 4, 8, 16, 8, 4,};
    int32_t srcStep = 7 * sizeof(uint8_t);
    int32_t dstStep = 7 * sizeof(float);
    HmppResult result = HMPPI_Convert_8u32f_C1R(src, srcStep, dst, dstStep, roi);
    printf("result = %d \ndst =", result);
    for (i = 0; i < DST_BUFFER_SIZE_T; i++) {
        printf(" %f  ", dst[i]);
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst = 1.000000  2.000000  4.000000  8.000000  16.000000  0.000000  0.000000  1.000000  2.000000  4.000000  8.000000  16.000000  0.000000  0.000000 1.000000  2.000000  4.000000  8.000000  16.000000  0.000000  0.000000 1.000000  2.000000  4.000000  8.000000  16.000000  0.000000  0.000000  
```

## Copy

该类接口将源图像缓冲区的像素值复制到目标图像缓冲区中。

函数接口声明如下：

- **复制所有颜色通道的所有像素：**

    HmppResult HMPPI\_Copy\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C1R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C3R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_AC4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C3AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C3AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C3AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C3AC4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C3AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_AC4C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_AC4C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_AC4C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_AC4C3R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_AC4C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **复制被蒙版标记的各通道的像素：**

    HmppResult HMPPI\_Copy\_8u\_C1MR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16u\_C1MR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16s\_C1MR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32s\_C1MR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32f\_C1MR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_8u\_C3MR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16u\_C3MR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16s\_C3MR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32s\_C3MR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32f\_C3MR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_8u\_C4MR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16u\_C4MR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16s\_C4MR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32s\_C4MR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32f\_C4MR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_8u\_AC4MR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16u\_AC4MR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_16s\_AC4MR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32s\_AC4MR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

    HmppResult HMPPI\_Copy\_32f\_AC4MR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t \*mask, int32\_t maskStep\);

- **复制多通道图像中的选定通道的像素：**

    HmppResult HMPPI\_Copy\_8u\_C3CR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C3CR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C3CR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C3CR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C3CR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C4CR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C4CR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C4CR\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C4CR\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C4CR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **复制所选通道的像素到单通道图像：**

    HmppResult HMPPI\_Copy\_8u\_C3C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C3C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C3C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C3C1R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C3C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C4C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C4C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C4C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C4C1R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C4C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **复制单通道图像的像素到多通道图像：**

    HmppResult HMPPI\_Copy\_8u\_C1C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C1C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C1C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C1C3R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C1C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C1C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C1C4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C1C4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C1C4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C1C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **分割多通道图像为多个单独的图像：**

    HmppResult HMPPI\_Copy\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \* const dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C3P3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \* const dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C3P3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \* const dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C3P3R\(const int32\_t \*src, int32\_t srcStep, int32\_t \* const dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C3P3R\(const float \*src, int32\_t srcStep, float \* const dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_C4P4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \* const dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_C4P4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \* const dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_C4P4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \* const dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_C4P4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \* const dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_C4P4R\(const float \*src, int32\_t srcStep, float \* const dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

- **合成多个单独的图像为多通道图像：**

    HmppResult HMPPI\_Copy\_8u\_P3C3R\(const uint8\_t \* const src\[3\], int32\_t srcStep, uint8\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_P3C3R\(const uint16\_t \* const src\[3\], int32\_t srcStep, uint16\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_P3C3R\(const int16\_t \* const src\[3\], int32\_t srcStep, int16\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_P3C3R\(const int32\_t \* const src\[3\], int32\_t srcStep, int32\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_P3C3R\(const float \* const src\[3\], int32\_t srcStep, float \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_8u\_P4C4R\(const uint8\_t \* const src\[4\], int32\_t srcStep, uint8\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16u\_P4C4R\(const uint16\_t \* const src\[4\], int32\_t srcStep, uint16\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_16s\_P4C4R\(const int16\_t \* const src\[4\], int32\_t srcStep, int16\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32s\_P4C4R\(const int32\_t \* const src\[4\], int32\_t srcStep, int32\_t \* const dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Copy\_32f\_P4C4R\(const float \* const src\[4\], int32\_t srcStep, float \* const dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入/输出|
|mask|指向蒙版图像缓冲区的指针。|非空|输入|
|maskStep|蒙版图像缓冲区中连续起点之间的距离（以字节为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

int CopyExample()
{
    HmppiSize roi = { 4, 5 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 15 * sizeof(uint8_t);
    int32_t dstStep = 18 * sizeof(uint8_t);
    HmppResult result = HMPPI_Copy_8u_C3AC4R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; i++) {
        if( i % dstWidth == 0 ){
            printf("\n");
        }
        printf("%3d ",dst[i]);       
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
result = 0
dst = 
 11  22  33   0  44  55  66   0  77  88  22   0  88  77  66   0   0   0 
 11  22  33   0  44  55  66   0  77  88  22   0  88  77  66   0   0   0 
 11  22  33   0  44  55  66   0  77  88  22   0  88  77  66   0   0   0 
 11  22  33   0  44  55  66   0  77  88  22   0  88  77  66   0   0   0 
 11  22  33   0  44  55  66   0  77  88  22   0  88  77  66   0   0   0 
  0   0   0   0   0   0   0   0   0   0
```

## CopyConstBorder

在两个图像之间复制像素值，并添加固定值边界框像素。

函数接口声明如下：

HmppResult HMPPI\_CopyConstBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize srcRoiSize, float \*dst, int32\_t dstStep, HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth, float value\);

HmppResult HMPPI\_CopyConstBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize srcRoiSize, uint8\_t \*dst, int32\_t dstStep, HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth, uint8\_t value\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcRoiSize|源感兴趣区域的大小（以像素为单位）。|srcRoiSize.width∈(0, INT_MAX]，srcRoiSize.height∈(0, INT_MAX]|输入|
|dstRoiSize|目标感兴趣区域的大小（以像素为单位）。|dstRoiSize.width∈(0, INT_MAX]，dstRoiSize.height∈(0, INT_MAX]|输入/输出|
|topBorderHeight|顶部边框的高度（以像素为单位）。|非负整数|输入|
|leftBorderWidth|左边框的宽度（以像素为单位）。|非负整数|输入|
|value|边界框像素的值。|输入数据类型的范围|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|srcRoiSize或dstRoiSize为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|srcRoiSize.width > 步长或dstRoiSize.width > 步长。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
#define SRC_BUFFER_SIZE_T 12
#define DST_BUFFER_SIZE_T 132
int CopyConstBorderExample()
{
    HmppiSize roiSrc = {3,4};
    uint8_t src[SRC_BUFFER_SIZE_T] = { 1, 2, 3,  
                                      2, 3, 4, 
                                      3, 4, 5,  
                                      4, 5, 6 };
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 3 * sizeof(uint8_t);
    int32_t dstStep = 11 * sizeof(uint8_t);
    HmppiSize roiDst = {11,12};
    int32_t topBorderHeight = 4;
    int32_t leftBorderWidth = 4;
    uint8_t value = 255;
    HmppResult result = HMPPI_CopyConstBorder_8u_C1R(src, srcStep, roiSrc, dst, dstStep, roiDst, topBorderHeight, leftBorderWidth, value);
    printf("result = %d \ndst = \n", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for (int32_t i = 0; i < 12; i++){
        for (int32_t j = 0; j < 11; j++){
            printf("%3d ",dst[i*11+j]);
        }
        printf("\n");
    }
    return 0;
}
int main() {
    CopyConstBorderExample();
    return 0;
}
```

运行结果：

```text
result = 0 
dst = 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255   1   2   3 255 255 255 255 
255 255 255 255   2   3   4 255 255 255 255 
255 255 255 255   3   4   5 255 255 255 255 
255 255 255 255   4   5   6 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255 
255 255 255 255 255 255 255 255 255 255 255
```

## CopyReplicateBorder

在两个图像之间复制像素值，并复制源图像的边界，添加边界框像素。

函数接口声明如下：

HmppResult HMPPI\_CopyReplicateBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize srcRoiSize, float \*dst, int32\_t dstStep, HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth\);

HmppResult HMPPI\_CopyReplicateBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, HmppiSize srcRoiSize, int16\_t \*dst, int32\_t dstStep, HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth\);

HmppResult HMPPI\_CopyReplicateBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, HmppiSize srcRoiSize, uint8\_t \*dst, int32\_t dstStep, HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcRoiSize|源感兴趣区域的大小（以像素为单位）。|srcRoiSize.width∈(0, INT_MAX]，srcRoiSize.height∈(0, INT_MAX]|输入|
|dstRoiSize|目标感兴趣区域的大小（以像素为单位）。|dstRoiSize.width∈(0, INT_MAX]，dstRoiSize.height∈(0, INT_MAX]|输入/输出|
|topBorderHeight|顶部边框的高度（以像素为单位）。|非负整数|输入|
|leftBorderWidth|左边框的宽度（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|srcRoiSize或dstRoiSize为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|srcRoiSize.width > 步长或dstRoiSize.width > 步长。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"
#define SRC_BUFFER_SIZE_T 12
#define DST_BUFFER_SIZE_T 132
int CopyReplicateBorderExample()
{
    HmppiSize roiSrc = {3,4};
    uint8_t src[SRC_BUFFER_SIZE_T] = { 1, 2, 3,  
                                      2, 3, 4, 
                                      3, 4, 5,  
                                      4, 5, 6 };
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 3 * sizeof(uint8_t);
    int32_t dstStep = 11 * sizeof(uint8_t);
    HmppiSize roiDst = {11,12};
    int32_t topBorderHeight = 4;
    int32_t leftBorderWidth = 4;
    HmppResult result = HMPPI_CopyReplicateBorder_8u_C1R(src, srcStep, roiSrc, dst, dstStep, roiDst, topBorderHeight, leftBorderWidth);
    printf("result = %d \ndst = \n", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for (int32_t i = 0; i < 12; i++){
        for (int32_t j = 0; j < 11; j++){
            printf("%3d ",dst[i*11+j]);
        }
        printf("\n");
    }
    return 0;
}
int main() {
    CopyReplicateBorderExample();
    return 0;
}
```

运行结果：

```text
result = 0 
dst = 
  1   1   1   1   1   2   3   3   3   3   3 
  1   1   1   1   1   2   3   3   3   3   3 
  1   1   1   1   1   2   3   3   3   3   3 
  1   1   1   1   1   2   3   3   3   3   3 
  1   1   1   1   1   2   3   3   3   3   3 
  2   2   2   2   2   3   4   4   4   4   4 
  3   3   3   3   3   4   5   5   5   5   5 
  4   4   4   4   4   5   6   6   6   6   6 
  4   4   4   4   4   5   6   6   6   6   6 
  4   4   4   4   4   5   6   6   6   6   6 
  4   4   4   4   4   5   6   6   6   6   6 
  4   4   4   4   4   5   6   6   6   6   6
```

## CopyWrapBorder

该类接口在两个图像之间复制像素值，并添加边界框像素。

函数接口声明如下：

- **非原地操作：**

    HmppResult HMPPI\_CopyWrapBorder\_32s\_C1R\(const int32\_t \*src, int32\_t srcStep, HmppiSize srcRoiSize, int32\_t \*dst, int32\_t dstStep,HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth\);

    HmppResult HMPPI\_CopyWrapBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, HmppiSize srcRoiSize, float \*dst, int32\_t dstStep,HmppiSize dstRoiSize, int32\_t topBorderHeight, int32\_t leftBorderWidth\);

- **原地操作：**

    HmppResult HMPPI\_CopyWrapBorder\_32s\_C1IR\(const int32\_t \*src, int32\_t srcDstStep, HmppiSize srcRoiSize, HmppiSize dstRoiSize,int32\_t topBorderHeight, int32\_t leftBorderwidth\);

    HmppResult HMPPI\_CopyWrapBorder\_32f\_C1IR\(const float \*src, int32\_t srcDstStep, HmppiSize srcRoiSize, HmppiSize dstRoiSize,int32\_t topBorderHeight, int32\_t leftBorderwidth\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcRoiSize|源感兴趣区域的大小（以像素为单位）。|srcRoiSize.width∈(0,INT_MAX]，srcRoiSize.height∈(0,INT_MAX]|输入|
|dstRoiSize|目标感兴趣区域的大小（以像素为单位）。|dstRoiSize.width∈(0,INT_MAX]，dstRoiSize.height∈(0,INT_MAX]|输入/输出|
|topBorderHeight|顶部边框的高度（以像素为单位）。|非负整数|输入|
|leftBorderWidth|左边框的宽度（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|srcRoiSize或dstRoiSize为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|srcRoiSize.width > 步长或dstRoiSize.width > 步长。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 12
#define DST_BUFFER_SIZE_T 132

int CopyWrapBorderExample()
{
    HmppiSize roiSrc = {3,4};
    int32_t src[SRC_BUFFER_SIZE_T] = { 1, 2, 3,  
                                      2, 3, 4, 
                                      3, 4, 5,  
                                      4, 5, 6 };
    int32_t dst[DST_BUFFER_SIZE_T] = {0};
    int32_t srcStep = 3 * sizeof(int32_t);
    int32_t dstStep = 11 * sizeof(int32_t);
    HmppiSize roiDst = {11,12};
    int32_t topBorderHeight = 4;
    int32_t leftBorderWidth = 4;
    HmppResult result = HMPPI_CopyWrapBorder_32s_C1R(src, srcStep, roiSrc, dst, dstStep, roiDst, topBorderHeight, leftBorderWidth);
    printf("result = %d \ndst = \n", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for (int32_t i = 0; i < 12; i++){
        for (int32_t j = 0; j < 11; j++){
            printf("%3d",dst[i*11+j]);
        }
        printf("\n");
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst = 
   3   1   2   3   1   2   3   1   2   3   1
   4   2   3   4   2   3   4   2   3   4   2
   5   3   4   5   3   4   5   3   4   5   3
   6   4   5   6   4   5   6   4   5   6   4
   3   1   2   3   1   2   3   1   2   3   1
   4   2   3   4   2   3   4   2   3   4   2
   5   3   4   5   3   4   5   3   4   5   3
   6   4   5   6   4   5   6   4   5   6   4
   3   1   2   3   1   2   3   1   2   3   1
   4   2   3   4   2   3   4   2   3   4   2
   5   3   4   5   3   4   5   3   4   5   3
   6   4   5   6   4   5   6   4   5   6   4
```

## Not

对图像进行像素按位取反。

函数接口声明如下：

HmppResult HMPPI_Not_8u_C1IR(uint8_t *pSrcDst, int srcDstStep, HmppiSize roiSize);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrcDst|指向源和目标图像感兴趣区域的指针（原地操作）。|非空|输入/输出|
|srcDstStep|源和目标图像中连续行起点之间的距离（以字节为单位）。|正整数|输入|
|roiSize|图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrcDst存在空指针。|
|HMPP_STS_STEP_ERR|srcDstStep小于或等于0。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|

**示例**

```c
#include "hmppi.h"
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    uint8_t srcDst[8] = {0x00, 0x0F, 0xF0, 0xAA, 0x55, 0x80, 0x7F, 0x33};
    HmppResult ret = HMPPI_Not_8u_C1IR(srcDst, 4, (HmppiSize){4, 2});
    printf("ret=%d\n", ret);
    for (int i = 0; i < 8; ++i) {
        printf("%02X ", srcDst[i]);
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
ret=0
FF F0 0F 55 AA 7F 80 CC
```

## Or

将两个图像进行按位或运算。

函数接口声明如下：

HmppResult HMPPI_Or_8u_C1R(const uint8_t \*pSrc1, int src1Step, const uint8_t \*pSrc2, int src2Step, uint8_t \*pDst, int dstStep, HmppiSize roiSize);

HmppResult HMPPI_Or_8u_C1IR(const uint8_t \*pSrc, int srcStep, uint8_t \*pSrcDst, int srcDstStep, HmppiSize roiSize);

**参数**

| 参数名   | 描述                                             | 取值范围                                                | 输入/输出 |
| -------- | ------------------------------------------------ | ------------------------------------------------------- | --------- |
| pSrc1    | 指向源图像1感兴趣区域的指针。                    | 非空                                                    | 输入      |
| src1Step | 源图像1中连续行起点之间的距离（以字节为单位）。  | 非负整数                                                | 输入      |
| pSrc2    | 指向源图像2感兴趣区域的指针。                    | 非空                                                    | 输入      |
| src2Step | 源图像2中连续行起点之间的距离（以字节为单位）。  | 非负整数                                                | 输入      |
| pDst     | 指向目标图像的指针。                             | 非空                                                    | 输出      |
| dstStep  | 目标图像中连续行起点之间的距离（以字节为单位）。 | 非负整数                                                | 输入      |
| pSrc     | 指向源图像感兴趣区域的指针。                | 非空                                                    | 输入      |
| srcStep  | 源图像中连续行起点之间的距离（以字节为单位）。| 正整数                                             | 输入      |
| pSrcDst  | 指向源图像和目标图像感兴趣区域的指针。    | 非空                                                    | 输入/输出 |
| srcDstStep | 源图像和目标图像中连续行起点之间的距离（以字节为单位）。 | 正整数                                      | 输入      |
| roiSize  | 源和目标图像感兴趣区域的大小（以像素为单位）。   | roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX] | 输出      |

**返回值**

- 成功：返回HMPP_STS_NO_ERR。
- 失败：返回错误码。

**错误码**

| 错误码                | 描述                                       |
| --------------------- | ------------------------------------------ |
| HMPP_STS_NULL_PTR_ERR | pSrc1、pSrc2、pDst、pSrc、pSrcDst中存在空指针。 |
| HMPP_STS_STEP_ERR     | src1Step、src2Step、dstStep、srcStep、srcDstStep中存在小于或等于0的值。 |
| HMPP_STS_SIZE_ERR     | roiSize.width、roiSize.height小于或等于0。 |

**示例**

```c
#include <hmpp.h>
#include <stdio.h>
#define BUFFER_SIZE_T 12
void OrExample()
{
    uint8_t src1[BUFFER_SIZE_T] = {72, 27, 3, 0, 128, 255, 5, 4, 32, 101, 169, 77};
    uint8_t src2[BUFFER_SIZE_T] = {27, 72, 54, 77, 1, 37, 59, 9, 77, 91, 19, 11};
    uint8_t dst[BUFFER_SIZE_T];
    HmppiSize roiSize = {3, 4};
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
     dst[i] = 0;
    }
    HmppResult result = HMPPI_Or_8u_C1R(src1, 3 * sizeof(uint8_t), src2, 3 * sizeof(uint8_t),
        dst, 3 * sizeof(uint8_t), roiSize);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d", dst[i]);
    }
    printf("\n");
}

int main()
{
    OrExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 91 91 55 77 129 255 63 13 109 127 187 79
```

## Scale

此类接口功能是缩放图像的像素值并将它们转换为另一种位深度。

函数接口声明如下：

- **单通道数据的缩放操作：**

    HmppResult HMPPI\_Scale\_8u16u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u16s\_C1R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32s\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32f\_C1R\(const uint8\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

    HmppResult HMPPI\_Scale\_16u8u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_16s8u\_C1R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32s8u\_C1R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32f8u\_C1R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

- **多通道数据的缩放操作：**

    HmppResult HMPPI\_Scale\_8u16u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u16s\_C3R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32s\_C3R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32f\_C3R\(const uint8\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

    HmppResult HMPPI\_Scale\_16u8u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_16s8u\_C3R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32s8u\_C3R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32f8u\_C3R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

    HmppResult HMPPI\_Scale\_8u16u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u16s\_C4R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32s\_C4R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32f\_C4R\(const uint8\_t \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

    HmppResult HMPPI\_Scale\_16u8u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_16s8u\_C4R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32s8u\_C4R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32f8u\_C4R\(const float \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, float vMin, float vMax\);

    HmppResult HMPPI\_Scale\_8u16u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u16s\_AC4R\(const uint8\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_8u32s\_AC4R\(const uint8\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_Scale\_16u8u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_16s8u\_AC4R\(const int16\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

    HmppResult HMPPI\_Scale\_32s8u\_AC4R\(const int32\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|vMin|输入数据的最小值。|可通过HMPPI_Min_*类接口获取|输入|
|vMax|输入数据的最大值。|可通过HMPPI_Max_*类接口获取|输入|
|hint|函数的算法实现方式。|HmppHintAlgorithm的枚举值之一|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|cmpOp|指定用于比较像素值和阈值的操作。可以使用“大于”或“小于”的比较。|HMPP_CMP_LT，HMPP_CMP_GT|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep不能被src所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_SCALE_RANGE_ERR|当vMin > vMax时返回错误。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|不支持的比较模式。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
void ScaleExample()
{
    HmppiSize roi = {5,4};
    uint8_t src[60] = {1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,
                       1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,
                        1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,
                        1,2,3,4,5,6,7,8,9,10,11,12,13,14,15
                               };
    int16_t dst[72] = {0};
    int32_t srcStep=15*sizeof(uint8_t);
    int32_t dstStep=18*sizeof(int16_t);
    HMPPI_Scale_8u16s_C3R(src, srcStep, dst, dstStep, roi);
 
    for(int i=0; i< 4; ++i){
        for(int j=0;j<18;++j){
          printf( "%d ", dst[i*18 + j]);
        }
        printf("\n");
    }
}

int main(){
    ScaleExample();
    return 0;
}
```

运行结果：

```text
-32511 -32254 -31997 -31740 -31483 -31226 -30969 -30712 -30455 -30198 -29941 -29684 -29427 -29170 -28913 0 0 0
-32511 -32254 -31997 -31740 -31483 -31226 -30969 -30712 -30455 -30198 -29941 -29684 -29427 -29170 -28913 0 0 0
-32511 -32254 -31997 -31740 -31483 -31226 -30969 -30712 -30455 -30198 -29941 -29684 -29427 -29170 -28913 0 0 0
-32511 -32254 -31997 -31740 -31483 -31226 -30969 -30712 -30455 -30198 -29941 -29684 -29427 -29170 -28913 0 0 0
```

## ScaleC

缩放图像的像素值并将其转换为另一个位深度。

函数接口声明如下：

HmppResult HMPPI\_ScaleC\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u8s\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u16u\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u16s\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u32s\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u32f\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u64f\_C1R\(const uint8\_t \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s8u\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s16u\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s16s\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s32s\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s32f\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s64f\_C1R\(const int8\_t \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u8u\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u8s\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u16s\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u32s\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u32f\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u64f\_C1R\(const uint16\_t \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s8u\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s8s\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s16u\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s32s\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s32f\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s64f\_C1R\(const int16\_t \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s8u\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s8s\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s16u\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s16s\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s32f\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s64f\_C1R\(const int32\_t \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f8u\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f8s\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f16u\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f16s\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f32s\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f64f\_C1R\(const float \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f8u\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f8s\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, int8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f16u\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f16s\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f32s\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f32f\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f\_C1R\(const double \*src, int32\_t srcStep, double mVal, double aVal, double \*dst, int32\_t dstStep, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_8s\_C1IR\(int8\_t \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32s\_C1IR\(int32\_t \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

HmppResult HMPPI\_ScaleC\_64f\_C1IR\(double \*srcDst, int32\_t srcDstStep, double mVal, double aVal, HmppiSize roiSize, HmppHintAlgorithm hint\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标缓冲区的指针。|非空|输入/输出|
|srcDstStep|源图像和目标图像中连续行的起点之间的距离。|非负整数|输入|
|mVal|用于缩放的乘数值。|双精度取值范围|输入|
|aVal|缩放的偏移值。|双精度取值范围|输入|
|hint|函数的算法实现方式。|HMPP_ALGHINT_FAST或HMPP_ALGHINT_ACCURATE|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src，dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep，dstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误。|
|HMPP_STS_BAD_ARG_ERR|hint != 0且hint != 2时的返回值，表示算法模式入参不合法。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#include <stdio.h>
#include "hmppi.h"
#include "hmpp_type.h"

void ScaleCExample()
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
    uint8_t dst[49] = {0};
    int32_t srcStep = 5 * sizeof(uint8_t);
    int32_t dstStep = 7 * sizeof(uint8_t);
    double aVal = 0.5;
    double mVal = 0.5;
    HmppHintAlgorithm hint = HMPP_ALGHINT_ACCURATE;

    (void)HMPPI_ScaleC_8u_C1R(src, srcStep, mVal, aVal, dst, dstStep, roi, hint);

    for (int i = 0; i < 7; i++) {
        for (int j = 0; j < 7; j++) {
            printf("%d ", dst[i * 7 + j]);
        }
        printf("\n");
    }
}

int main()
{
    ScaleCExample();
    return 0;
}
```

运行结果：

```text
1 2 2 2 3 0 0
1 2 2 2 3 0 0
1 2 2 2 3 0 0
1 2 2 2 3 0 0
1 2 2 2 3 0 0
1 2 2 2 3 0 0
1 2 2 2 3 0 0
```

## Set

初始化图像像素值为value。

函数接口声明如下：

HmppResult HMPPI_Set_64f_C1R(double value, double *dst, HmppiSize roiSize);

HmppResult HMPPI_Set_8u_C1R(uint8_t value, uint8_t *dst, int32_t dstStep, HmppiSize roiSize);

HmppResult HMPPI_Set_32f_C1R(float value, float *dst, int32_t dstStep, HmppiSize roiSize);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|value|需要初始化的像素值。|数据类型范围内的值|输入|
|dstStep|目标图像中连续行起点之间的距离（以字节为单位）。|整数|输入|
|roiSize|目标图像感兴趣区域的大小。|正整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|

**示例**

```c
#include "hmppi.h"
#include <stdio.h>

int main(void)
{
    float dst[8] = {0};
    HmppResult ret = HMPPI_Set_32f_C1R(1.25f, dst, 4 * (int)sizeof(float), (HmppiSize){4, 2});
    printf("ret=%d\n", ret);
    for (int i = 0; i < 8; ++i) {
        printf("%.2f ", dst[i]);
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
ret=0
1.25 1.25 1.25 1.25 1.25 1.25 1.25 1.25
```

## Sub

将两个图像相减。

函数接口声明如下:

HmppResult HMPPI\_Sub\_32f\_C1R\(const float\* pSrc1, int src1Step, const float\* pSrc2, int src2Step, float\* pDst, int dstStep, HmppiSize roiSize\);

HmppResult HMPPI\_Sub\_32f\_C3R\(const float\* pSrc1, int src1Step, const float\* pSrc2, int src2Step, float\* pDst, int dstStep, HmppiSize roiSize\);

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

**示例**

```c
#include <hmpp.h>
#include <stdio.h>
#define BUFFER_SIZE_T 10
void SubExample(void)
{
    float src1[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float src2[BUFFER_SIZE_T] = {9.54, 3.77, 2.1, -3.77, 1.59, 2.9, -3.77, 8.91, 0.19, -0.11};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppiSize roiSize = {2, 5};
    HmppResult result = HMPPI_Sub_32f_C1R(src1, 2 * sizeof(float), src2, 2 * sizeof(float),
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
    SubExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 7.90 2.14 3.19 -4.48 4.79 3.33 -4.18 13.74 -5.17 4.29
```

## Threshold

对图像中的像素进行阈值处理，阈值函数根据图像像素值是小于还是大于指定值来更改像素值。用于阈值像素值的比较操作的类型由HMPPCmpOp参数指定。这个操作可以是“大于”或“小于”，如果输入像素值满足比较条件，则将相应的输出像素设置为阈值，否则它不会更改或设置输出像素值，计算公式为：

- 当cmpOp=HMPP\_CMPLESS时，如果src中的像素值小于threshold时，将threshold值赋值给dst，否则将src中的像素值赋值给dst。

    ![](../../figures/zh-cn_formulaimage_0000002549921649.png)

- 当cmpOp=HMPP\_CMPGREATER时，如果src中的像素值大于threshold时，将threshold值赋值给dst，否则将src中的像素值赋值给dst。

    ![](../../figures/zh-cn_formulaimage_0000002518281888.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float threshold, HmppCmpOp cmpOp\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_C3R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_AC4R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], HmppCmpOp cmpOp\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold, HmppCmpOp cmpOp\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], HmppCmpOp cmpOp\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|任意值|输入|
|cmpOp|指定用于比较像素值和阈值的操作。可以使用“大于”或“小于”的比较。|HMPP_CMP_LT，HMPP_CMP_GT|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|正常输出。|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|不支持的比较模式。|

**示例**

```c
void ThresholdExample()
{
    HmppiSize roi = {3,4};
    uint8_t src[9*4] = { 1, 2, 4, 8, 16, 8, 4, 2, 1, 
                   1, 2, 4, 8, 16, 8, 4, 2, 1, 
                   1, 2, 4, 8, 16, 8, 4, 2, 1, 
                   1, 2, 4, 8, 16, 8, 4, 2, 1};
    uint8_t dst[9*4] = {0};
    uint8_t threshold[3] = {8,8,8};
    int srcStep=9*sizeof(uint8_t);
    int dstStep=9*sizeof(uint8_t);

    HMPPI_Threshold_8u_C3R(src, srcStep, dst, dstStep, roi, threshold, HMPP_CMP_LT);
    for (int i = 0; i < 4; i++){
        for (int j = 0; j < 9; j++){
            printf("%4d ",dst[i*9+j]);
        }
        printf("\n");
    }
}

int main(void)
{
    ThresholdExample();
    return 0;
}
```

运行结果：

```text
   8    8    8    8   16    8    8    8    8
   8    8    8    8   16    8    8    8    8
   8    8    8    8   16    8    8    8    8
   8    8    8    8   16    8    8    8    8
```

## Threshold\_GT

对图像中的像素与阈值进行比较，大于阈值的设置为阈值。

小于操作，即level是源向量上边界。计算公式为：

![](../../figures/zh-cn_formulaimage_0000002518281666.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_GT\_8u\_C1R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_C1R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_C1R\(const int16\_t\* src, int32\_t srcStep, int16\_t\* dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_C1R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize, float threshold\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_GT\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_C3R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_AC4R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\]\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_GT\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_GT\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_GT\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空。|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非空。|输入|
|dst|指向目标图像感兴趣区域的指针。|非空。|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|大于0。|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空。|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|大于0。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|任意值。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#define BUFFER_SIZE_T 52
int ThresholdLTExample()
{
    HmppiSize roi = { 4, 3 };
    uint8_t src[BUFFER_SIZE_T] = {  1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48,
                                   1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48,
                                   1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48,
                                   1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    uint8_t threshold[3] = { 18, 30, 35 };
    int32_t srcStep = 13 * sizeof(uint8_t);
    int32_t dstStep = 13 * sizeof(uint8_t);
    HmppResult result = HMPPI_Threshold_GT_8u_C3R(src, srcStep, dst, dstStep, roi, threshold);
    printf("result = %d \n dst =", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 13; j++) {
            printf("%4d ", dst[i * 13 + j]);
        }
        printf("\n");
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst =  1  4  8  13  16  20  18  28  32  18  30  35  0
       1  4  8  13  16  20  18  28  32  18  30  35  0
       1  4  8  13  16  20  18  28  32  18  30  35  0
       0  0  0   0   0   0   0   0   0   0   0   0  0
```

## Threshold\_LT

对图像中的像素与阈值进行比较，小于阈值的设置为阈值。

大于操作，即level是源向量的下边界。计算公式为：

![](../../figures/zh-cn_formulaimage_0000002549921743.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LT\_8u\_C1R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_C1R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_C1R\(const int16\_t\* src, int32\_t srcStep, int16\_t\* dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_C1R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize, float threshold\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LT\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_C3R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t\*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_AC4R\(const float \*src, int32\_t srcStep, float\*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\]\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_LT\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_LT\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\]\);

    HmppResult HMPPI\_Threshold\_LT\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空。|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非空。|输入|
|dst|指向目标图像感兴趣区域的指针。|非空。|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|大于0。|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空。|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|大于0。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|任意值。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的width、height存在零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|

**示例**

```c
#define BUFFER_SIZE_T 52

int ThresholdLTExample(){
    HmppiSize roi = {4,3};
    uint8_t src[BUFFER_SIZE_T] = {  1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48, 
                                    1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48, 
                                    1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48, 
                                    1, 4, 8, 13, 16, 20, 24, 28, 32, 36, 40, 44, 48};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    uint8_t threshold[3] = {18,30,35};
    int32_t srcStep=13*sizeof(uint8_t);
    int32_t dstStep=13*sizeof(uint8_t);
    HmppResult result = HMPPI_Threshold_LT_8u_C3R(src, srcStep, dst, dstStep, roi, threshold);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < 4; i++){
        for (int j = 0; j < 13; j++){
            printf("%4d ", dst[i * 13 + j]);
        }
        printf("\n");
    }
       return 0;
}
```

运行结果：

```text
result = 0
dst = 18 30 35 18 30 35 24 30 35 36 40 44 0
      18 30 35 18 30 35 24 30 35 36 40 44 0
      18 30 35 18 30 35 24 30 35 36 40 44 0
       0  0  0  0  0  0  0  0  0  0  0  0 0
```

## Threshold\_Val

对图像中的像素进行阈值处理，阈值函数根据图像像素值是小于还是大于阈值来更改像素值。用于阈值像素值的比较操作的类型由HMPPCmpOp参数指定。这个操作可以是“大于”或“小于”，如果输入像素值满足比较条件，则将相应的输出像素设置为参数value的值，否则它不会更改或设置输出像素值。对于具有多通道数据的图像，应分别为每个通道设置比较条件，计算公式如下：

- 当cmpOp=HMPP\_CMPLESS时，如果src中的像素值小于Threshold时，将value值赋值给dst，否则将src中的像素值赋值给dst。

    ![](../../figures/zh-cn_formulaimage_0000002518281808.png)

- 当cmpOp=HMPP\_CMPGREATER时，如果src中的像素值大于Threshold时，将value值赋值给dst，否则将src中的像素值赋值给dst。

    ![](../../figures/zh-cn_formulaimage_0000002549921569.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_Val\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float threshold, float value, HmppCmpOp cmpOp\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_Val\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\], HmppCmpOp cmpOp\);

- **单通道数据的原地阈值操作**：

    HmppResult HMPPI\_Threshold\_Val\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value, HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold, float value, HmppCmpOp cmpOp\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_Val\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\], HmppCmpOp cmpOp\);

    HmppResult HMPPI\_Threshold\_Val\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\], HmppCmpOp cmpOp\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|Threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|输入数据类型的范围|输入|
|value|像素值与相应阈值进行比较，满足比较条件（大于或小于）时，输出像素取值。多通道下使用value值数组。|输入数据类型的范围|输入|
|cmpOp|指定用于比较像素值和Threshold的操作。可以使用“小于（HMPP_CMPLESS）”或“大于（HMPP_CMPGREATER）”的比较方式。|HMPP_CMPLESS或者HMPP_CMPGREATER|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|不支持的比较模式。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define BUFFER_SIZE_T 36
int ThresholValExample()
{
    HmppiSize roi = { 3, 4 };
    uint8_t src[BUFFER_SIZE_T] = { 1, 2, 4, 8, 16, 8, 4, 2, 1,
                                   1, 2, 4, 8, 16, 8, 4, 2, 1,
                                   1, 2, 4, 8, 16, 8, 4, 2, 1,
                                   1, 2, 4, 8, 16, 8, 4, 2, 1};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    uint8_t threshold[3] = {8, 8, 8};
    uint8_t value[3] = {7, 7, 7};
    int srcStep = 9 * sizeof(uint8_t);
    int dstStep = 9 * sizeof(uint8_t);

    HmppResult result = HMPPI_Threshold_Val_8u_C3R(src, srcStep, dst, dstStep, roi, threshold, value, HMPP_CMP_LT);

    printf("result = %d \n dst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 9; j++) {
            printf("%4d ", dst[i * 9 + j]);
        }
        printf("\n");
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst =  1  4  8  13  16  20  18  28  32  18  30  35  0
       1  4  8  13  16  20  18  28  32  18  30  35  0
       1  4  8  13  16  20  18  28  32  18  30  35  0
       0  0  0   0   0   0   0   0   0   0   0   0  0
```

## Threshold\_GTVal

对图像中的像素进行阈值处理，阈值函数根据图像像素值是否大于阈值来更改像素值。如果输入像素值满足大于阈值条件，则将相应的输出像素设置为参数value的值，否则它不会更改或设置输出像素值。对于具有多通道数据的图像，应分别为每个通道设置比较条件。

计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281732.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float threshold, float value\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[4\], const uint8\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[4\], const uint16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[4\], const int16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[4\], const float value\[4\]\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold, float value\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_8u\_C4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[4\], const uint8\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16u\_C4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[4\], const uint16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_16s\_C4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[4\], const int16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_GTVal\_32f\_C4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[4\], const float value\[4\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空。|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|dst|指向目标图像感兴趣区域的指针。|非空。|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数。|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空。|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|输入数据类型的范围。|输入|
|value|像素值与相应阈值进行比较，满足大于阈值条件时，输出像素取值。多通道下使用value值数组。|输入数据类型的范围。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define BUFFER_SIZE_T 36
int ThresholGTValExample()
{
    HmppiSize roi = { 2, 4 };
    int16_t srcDst[BUFFER_SIZE_T] = { 1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1};
    int16_t threshold[3] = {8, 8, 8};
    int16_t value[3] = { 6, 6, 6 };
    int srcDstStep = 9 * sizeof(int16_t);
    HmppResult result = HMPPI_Threshold_GTVal_16s_AC4IR(srcDst, srcDstStep, roi, threshold, value);

    printf("result = %d \n dst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    int dstWidth = srcDstStep / sizeof(int16_t);
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 9; j++) {
            printf("%4d ", srcDst[i * dstWidth + j]);
        }
        printf("\n");
    }
    return 0;
}
```

运行结果：

```text
 result = 0
 dst =  1    2    4    8    6    8    4    2    1
        1    2    4    8    6    8    4    2    1
        1    2    4    8    6    8    4    2    1
        1    2    4    8    6    8    4    2    1
```

## Threshold\_LTVal

对图像中的像素进行阈值处理，阈值函数根据图像像素值是否小于阈值来更改像素值。如果输入像素值满足小于阈值条件，则将相应的输出像素设置为参数value的值，否则它不会更改或设置输出像素值。对于具有多通道数据的图像，应分别为每个通道设置比较条件。

计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281768.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float threshold, float value\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t threshold\[4\], const uint8\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t threshold\[4\], const uint16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const int16\_t threshold\[4\], const int16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, const float threshold\[4\], const float value\[4\]\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint8\_t threshold, uint8\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, uint16\_t threshold, uint16\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, int16\_t threshold, int16\_t value\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, float threshold, float value\);

- **多通道数据的原地阈值操作**：

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[3\], const uint8\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[3\], const uint16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[3\], const int16\_t value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[3\], const float value\[3\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_8u\_C4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint8\_t threshold\[4\], const uint8\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16u\_C4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const uint16\_t threshold\[4\], const uint16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_16s\_C4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const int16\_t threshold\[4\], const int16\_t value\[4\]\);

    HmppResult HMPPI\_Threshold\_LTVal\_32f\_C4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize, const float threshold\[4\], const float value\[4\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空。|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|dst|指向目标图像感兴趣区域的指针。|非空。|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数。|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空。|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|threshold|每个像素要使用的阈值水平值；多通道数据下，使用每个颜色通道的阈值数组。|输入数据类型的范围。|输入|
|value|像素值与相应阈值进行比较，满足大于阈值条件时，输出像素取值。多通道下使用value值数组。|输入数据类型的范围。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define BUFFER_SIZE_T 36
int ThresholLTValExample()
{
    HmppiSize roi = { 3, 4 };
    uint8_t src[BUFFER_SIZE_T] = { 1, 2, 4, 8, 16, 8, 4, 2, 1,
                       1, 2, 4, 8, 16, 8, 4, 2, 1,
                       1, 2, 4, 8, 16, 8, 4, 2, 1,
                       1, 2, 4, 8, 16, 8, 4, 2, 1};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    uint8_t threshold[3] = {8, 8, 8};
    uint8_t value[3] = {9, 9, 9};
    int srcStep = 9 * sizeof(uint8_t);
    int dstStep = 9 * sizeof(uint8_t);

    HmppResult result = HMPPI_Threshold_LTVal_8u_C3R(src, srcStep, dst, dstStep, roi, threshold, value);

    printf("result = %d \n dst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 9; j++) {
            printf("%4d ", dst[i * 9 + j]);
        }
        printf("\n");
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst =   9    9    9    8   16    8    9    9    9
        9    9    9    8   16    8    9    9    9
        9    9    9    8   16    8    9    9    9
        9    9    9    8   16    8    9    9    9
```

## Threshold\_LTValGTVal

对图像中的像素进行阈值处理，阈值函数根据图像像素值是小于较低阈值还是大于较高阈值，将相应的输出像素设置为低或高的输出值，否则它不会更改或设置输出像素值。对于具有多通道数据的图像，应分别为每个通道设置比较条件。

计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002550041405.png)

函数接口声明如下：

- **单通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t thresholdLT, uint8\_t valueLT, uint8\_t thresholdGT, uint8\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t thresholdLT, uint16\_t valueLT, uint16\_t thresholdGT, uint16\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, int16\_t thresholdLT, int16\_t valueLT, int16\_t thresholdGT, int16\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float thresholdLT, float valueLT, float thresholdGT, float valueGT\);

- **多通道数据的阈值操作：**

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint8\_t thresholdLT\[3\], const uint8\_t valueLT\[3\], const uint8\_t thresholdGT\[3\], const uint8\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, const uint16\_t thresholdLT\[3\], const uint16\_t valueLT\[3\], const uint16\_t thresholdGT\[3\], const uint16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep HmppiSize roiSize, const int16\_t thresholdLT\[3\], const int16\_t valueLT\[3\] const int16\_t thresholdGT\[3\], const int16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep HmppiSize roiSize, const float thresholdLT\[3\], const float valueLT\[3\] const float thresholdGT\[3\], const float valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep HmppiSize roiSize, const uint8\_t thresholdLT\[3\], const uint8\_t valueLT\[3\] const uint8\_t thresholdGT\[3\], const uint8\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep HmppiSize roiSize, const uint16\_t thresholdLT\[3\] const uint16\_t valueLT\[3\], const uint16\_t thresholdGT\[3\] const uint16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep HmppiSize roiSize, const int16\_t thresholdLT\[3\] const int16\_t valueLT\[3\], const int16\_t thresholdGT\[3\] const int16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep HmppiSize roiSize, const float thresholdLT\[3\], const float valueLT\[3\] const float thresholdGT\[3\], const float valueGT\[3\]\);

- **单通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_C1IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize uint8\_t thresholdLT, uint8\_t valueLT, uint8\_t thresholdGT uint8\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_C1IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize uint16\_t thresholdLT, uint16\_t valueLT, uint16\_t thresholdGT uint16\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_C1IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize int16\_t thresholdLT, int16\_t valueLT, int16\_t thresholdGT int16\_t valueGT\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_C1IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize float thresholdLT, float valueLT, float thresholdGT, float valueGT\);

- **多通道数据的原地阈值操作：**

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_C3IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const uint8\_t thresholdLT\[3\], const uint8\_t valueLT\[3\] const uint8\_t thresholdGT\[3\], const uint8\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_C3IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const uint16\_t thresholdLT\[3\], const uint16\_t valueLT\[3\] const uint16\_t thresholdGT\[3\], const uint16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_C3IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const int16\_t thresholdLT\[3\], const int16\_t valueLT\[3\] const int16\_t thresholdGT\[3\], const int16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_C3IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const float thresholdLT\[3\], const float valueLT\[3\] const float thresholdGT\[3\], const float valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_8u\_AC4IR\(uint8\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const uint8\_t thresholdLT\[3\], const uint8\_t valueLT\[3\] const uint8\_t thresholdGT\[3\], const uint8\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16u\_AC4IR\(uint16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const uint16\_t thresholdLT\[3\], const uint16\_t valueLT\[3\] const uint16\_t thresholdGT\[3\], const uint16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_16s\_AC4IR\(int16\_t \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const int16\_t thresholdLT\[3\], const int16\_t valueLT\[3\] const int16\_t thresholdGT\[3\], const int16\_t valueGT\[3\]\);

    HmppResult HMPPI\_Threshold\_LTValGTVal\_32f\_AC4IR\(float \*srcDst, int32\_t srcDstStep, HmppiSize roiSize const float thresholdLT\[3\], const float valueLT\[3\] const float thresholdGT\[3\], const float valueGT\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空。|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|dst|指向目标图像感兴趣区域的指针。|非空。|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数。|输入|
|srcDst|指向源和目标图像感兴趣区域的指针（用于原地操作）。|非空。|输入/输出|
|srcDstStep|原地操作的源图像和目标图像中连续行起点之间的距离（以字节为单位）。|非负整数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|thresholdLT|每个像素要使用的较低阈值。|输入数据类型的范围。|输入|
|valueLT|像素值小于较低阈值时，设定的较低输出值。|非负整数。|输入|
|thresholdGT|每个像素要使用的较高阈值。|输入数据类型的范围。|输入|
|valueGT|像素值大于较大阈值时，设定的较高输出值。|非负整数。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcDstStep中存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误条件。|
|HMPP_STS_ROI_ERR|roiSize.width > 步长。|
|HMPP_STS_THRESHOLD_ERR|thresholdLT小于thresholdGT。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define BUFFER_SIZE_T 36
int ThresholdLTValGTValExample()
{
    HmppiSize roi = { 3, 4 };
    uint16_t src[BUFFER_SIZE_T] = { 1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1,
                            1, 2, 4, 8, 16, 8, 4, 2, 1};
    uint16_t dst[BUFFER_SIZE_T] = {0};
    uint16_t thresholdLT[3] = {2, 2, 2};
    uint16_t valueLT[3] = {3, 3, 3};
    uint16_t thresholdGT[3] = {9, 9, 9};
    uint16_t valueGT[3] = {8, 8, 8};
    int srcStep = 9 * sizeof(uint16_t);
    int dstStep = 9 * sizeof(uint16_t);
    HmppResult result = HMPPI_Threshold_LTValGTVal_16u_C3R(src, srcStep, dst, dstStep, roi, thresholdLT, valueLT, thresholdGT, valueGT);
    printf("result = %d \n dst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 9; j++) {
            printf("%4d ", dst[i * 9 + j]);
        }
        printf("\n");
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst =    3    2    4    8    8    8    4    2    3
         3    2    4    8    8    8    4    2    3
         3    2    4    8    8    8    4    2    3
         3    2    4    8    8    8    4    2    3
```

## SwapChannels

此类函数实现将源图像指定区域的通道数据按照一定的顺序复制到目标图像的指定区域中。

函数接口声明如下：

HmppResult HMPPI\_SwapChannels\_8u\_C3R\(const uint8\_t \*src, int32\_t src\_step, uint8\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16u\_C3R\(const uint16\_t \*src, int32\_t src\_step, uint16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16s\_C3R\(const int16\_t \*src, int32\_t src\_step, int16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32s\_C3R\(const int32\_t \*src, int32\_t src\_step, int32\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32f\_C3R\(const float \*src, int32\_t src\_step, float \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_8u\_AC4R\(const uint8\_t \*src, int32\_t src\_step, uint8\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16u\_AC4R\(const uint16\_t \*src, int32\_t src\_step, uint16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16s\_AC4R\(const int16\_t \*src, int32\_t src\_step, int16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32s\_AC4R\(const int32\_t \*src, int32\_t src\_step, int32\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32f\_AC4R\(const float \*src, int32\_t src\_step, float \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_8u\_C4R\(const uint8\_t \*src, int32\_t src\_step, uint8\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_16u\_C4R\(const uint16\_t \*src, int32\_t src\_step, uint16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_16s\_C4R\(const int16\_t \*src, int32\_t src\_step, int16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_32s\_C4R\(const int32\_t \*src, int32\_t src\_step, int32\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_32f\_C4R\(const float \*src, int32\_t src\_step, float \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_8u\_C3IR\(uint8\_t \*srcdst, int32\_t srcdst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_8u\_C4IR\(uint8\_t \*srcdst, int32\_t srcdst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\]\);

HmppResult HMPPI\_SwapChannels\_8u\_C3C4R\(const uint8\_t \*src, int32\_t src\_step, uint8\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\], uint8\_t val\);

HmppResult HMPPI\_SwapChannels\_16u\_C3C4R\(const uint16\_t \*src, int32\_t src\_step, uint16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\], uint16\_t val\);

HmppResult HMPPI\_SwapChannels\_16s\_C3C4R\(const int16\_t \*src, int32\_t src\_step, int16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\], int16\_t val\);

HmppResult HMPPI\_SwapChannels\_32s\_C3C4R\(const int32\_t \*src, int32\_t src\_step, int32\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\], int32\_t val\);

HmppResult HMPPI\_SwapChannels\_32f\_C3C4R\(const float \*src, int32\_t src\_step, float  \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[4\], float val\);

HmppResult HMPPI\_SwapChannels\_8u\_C4C3R\(const uint8\_t \*src, int32\_t src\_step, uint8\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16u\_C4C3R\(const uint16\_t \*src, int32\_t src\_step, uint16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_16s\_C4C3R\(const int16\_t \*src, int32\_t src\_step, int16\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32s\_C4C3R\(const int32\_t \*src, int32\_t src\_step, int32\_t \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

HmppResult HMPPI\_SwapChannels\_32f\_C4C3R\(const float \*src, int32\_t src\_step, float \*dst, int32\_t dst\_step, HmppiSize roi\_size, const int32\_t dst\_order\[3\]\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标缓冲区的指针。|非空|输入/输出|
|srcDstStep|源图像和目标图像中连续行的起点之间的距离。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|非负整数|输入|
|dst_order|目标图像中通道的顺序。|0、1、2的随机组合或0、1、2、3的随机组合|输入|
|val|常量值。|任意常量值|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src，dst中存在空指针或dst_order值为空。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep，dstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep、dstStep不能被src、dst所属数据类型的字节长度整除的错误。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 36
#define DST_BUFFER_SIZE_T 40

void PrintResult(HmppResult result, uint8_t *dst, int32_t dstStep)
{
    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; i++) {
        if( i % dstWidth == 0 ){
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n\n");
}

void TestExample()
{
    HmppiSize roi = { 2, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        53, 111, 2, 61, 6, 12,
        77, 184, 5, 99, 3,  4,
        41, 233, 1, 27, 5,  6,
        62, 157, 6, 80, 7,  8
    };

    int32_t srcStep = 6 * sizeof(uint8_t);
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 8 * sizeof(uint8_t);
    const int32_t dst_order[3] = {2, 0, 1};

    HmppResult result = HMPPI_SwapChannels_8u_C3R(src, srcStep, dst, dstStep, roi, dst_order);
    PrintResult(result, dst, dstStep);
}

int main()
{
    TestExample();
    return 0;
}
```

运行结果：

```text
result = 0 
dst = 
  2  53 111  12  61   6   0   0 
  5  77 184   4  99   3   0   0 
  1  41 233   6  27   5   0   0 
  6  62 157   8  80   7   0   0 
  0   0   0   0   0   0   0   0
```
