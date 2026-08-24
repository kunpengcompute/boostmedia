# 图像形态学

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Dilate3x3

**函数说明：**

使用3x3蒙版进行图像膨胀，是DilateBorder的一个特例，其默认蒙版元素全为非0有效。

膨胀就是求局部最大值的操作，运算符是“![](../../figures/zh-cn_formulaimage_0000002549921637.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002518441776.png)

该公式表示用蒙版B来对图像A进行膨胀处理，其中B是一个3x3蒙版。通过蒙版B与图像A进行卷积计算，扫描图像中的每一个像素点，计算B覆盖区域的像素点最大值，并用这个最大值替换参考点指定的像素值实现膨胀。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002550041629.png)

函数接口声明如下：

HmppResult HMPPI\_Dilate3x3\_64f\_C1R\(const double \*src, int32\_t srcStep, double \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段+2的结果小。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>源或目标图像的宽和高需比roiSize的宽和高分别大2像素，否则将返回HMPP\_STS\_ROI\_ERR。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
void Dilate3x3Example()
{
    HmppiSize roi = { 4, 4 };
    const double src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66
    };

    double dst[DST_BUFFER_SIZE_T] = { 0 };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(double);
    int32_t dstStep = dstWidth * sizeof(double);
    HmppResult result = HMPPI_Dilate3x3_64f_C1R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%.2lf ", dst[i]);
    }
    printf("\n");
}


int main(void)
{
    Dilate3x3Example();
    return 0;
}
```

运行结果：

```text
result = 0
dst =
66.00 33.00 44.00 55.00 0.00 0.00 0.00 0.00
66.00 33.00 44.00 55.00 0.00 0.00 0.00 0.00
66.00 33.00 44.00 55.00 0.00 0.00 0.00 0.00
66.00 33.00 44.00 55.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
```

## DilateBorder

使用蒙版进行图像膨胀，原理同Dilate3x3。

其中最大值的有效计算仅针对蒙版图像中的非0元素。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002518441660.png)

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 主函数执行后，调用Release释放HmppiMorphPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphInit\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppDataType dataType, int32\_t numChannels, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_DilateInit\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppDataType dataType, int32\_t numChannels, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_DilateBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_8u\_C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphPolicy\* policy\);

    HmppResult HMPPI\_DilateBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateBorder\_8u\_C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphPolicy\* policy\);

    HmppResult HMPPI\_DilateBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphRelease\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_DilateRelease\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_1u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_16u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_16s\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C3R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C3R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C4R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C4R\(HmppiMorphPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|dataType|源图像的数据类型（目标图像同源图像）。|枚举，数据类型。支持HMPP8U、HMPP16U、HMPP16S和HMPP32F。|输入|
|numChannels|源图像的通道数（目标图像同源图像）。|仅支持1、3和4通道|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|srcBitOffset|从源图像的第一个字节开始的偏移量（以bit为单位）。|(0, INT_MAX]，且与roiSize.width的和需大于等于srcStep * 8|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|dstBitOffset|从目标图像的第一个字节开始的偏移量（以bit为单位）。|(0, INT_MAX]，且与roiSize.width的和需大于等于dstStep * 8|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，其borderValue= MIN_VALUE。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|初始化不支持的数据类型。|
|HMPP_STS_NUMCHANNELS_ERR|初始化不支持的通道数。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcBitOffset或者dstBitOffset中存在零或负值。或srcBitOffset、dstBitOffset不能满足roiSize字段的关系。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002550041499.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void DilateBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = 100;
    HmppiMorphPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_DilateInit(roiSize, mask, maskSize, dataType, numChannels, &policy);
    printf("HMPPI_DilateInit result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_DilateBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_DilateBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphRelease(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    DilateBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_DilateInit result = 0
HMPPI_DilateBorder_16s_C1R result = 0
result = 0
dst =
 23  24  24  24   0   0   0   0
 33  34  34  34   0   0   0   0
 43  44  44  44   0   0   0   0
 43  44  44  44   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## Erode3x3

使用3x3蒙版进行图像腐蚀，是ErodeBorder的一个特例，其默认蒙版元素全为非0有效。

腐蚀就是求局部最小值的操作，运算符是“![](../../figures/zh-cn_formulaimage_0000002549921593.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002549921597.png)

该公式表示用蒙版B来对图像A进行腐蚀处理，其中B是一个3x3蒙版。通过蒙版B与图像A进行卷积计算，扫描图像中的每一个像素点，计算B覆盖区域的像素点最小值，并用这个最小值替换参考点指定的像素值实现腐蚀。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002550041597.png)

函数接口声明如下：

HmppResult HMPPI\_Erode3x3\_64f\_C1R\(const double \*src, int32\_t srcStep, double \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|源或者目标Step存在零或负值。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段+2的结果小。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>源或目标图像的宽和高需比roiSize的宽和高分别大2像素，否则将返回HMPP\_STS\_ROI\_ERR。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
void Erode3x3Example()
{
    HmppiSize roi = { 4, 4 };
    const double src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66
    };

    double dst[DST_BUFFER_SIZE_T] = { 0 };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(double);
    int32_t dstStep = dstWidth * sizeof(double);
    HmppResult result = HMPPI_Erode3x3_64f_C1R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%.2lf ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    Erode3x3Example();
    return 0;
}
```

运行结果：

```text
result = 0
dst =
11.00 11.00 22.00 33.00 0.00 0.00 0.00 0.00
11.00 11.00 22.00 33.00 0.00 0.00 0.00 0.00
11.00 11.00 22.00 33.00 0.00 0.00 0.00 0.00
11.00 11.00 22.00 33.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
```

## ErodeBorder

使用蒙版进行图像腐蚀，原理同Erode3x3。

其中最小值的有效计算仅针对蒙版图像中的非0元素。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002550041333.png)

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)的HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphInit\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppDataType dataType, int32\_t numChannels, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_ErodeInit\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppDataType dataType, int32\_t numChannels, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

    HmppResult HMPPI\_MorphologyBorderInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_ErodeBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_8u\_C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphPolicy\* policy\);

    HmppResult HMPPI\_ErodeBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeBorder\_8u\_C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphPolicy\* policy\);

    HmppResult HMPPI\_ErodeBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphRelease\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_ErodeRelease\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_1u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_16u\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_16s\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C1R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C3R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C3R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_8u\_C4R\(HmppiMorphPolicy \*policy\);

    HmppResult HMPPI\_MorphologyBorderRelease\_32f\_C4R\(HmppiMorphPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|dataType|源图像的数据类型（目标图像同源图像）。|枚举，数据类型。支持HMPP8U、HMPP16U、HMPP16S和HMPP32F。|输入|
|numChannels|源图像的通道数（目标图像同源图像）。|仅支持1、3和4通道|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|srcBitOffset|从源图像的第一个字节开始的偏移量（以bit为单位）。|(0, INT_MAX]，且与roiSize.width的和需大于等于srcStep * 8|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|dstBitOffset|从目标图像的第一个字节开始的偏移量（以bit为单位）。|(0, INT_MAX]，且与roiSize.width的和需大于等于dstStep * 8|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|表示边界填充类型，定义在枚举类型HmppiBorderType中。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，其borderValue= MAX_VALUE。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|初始化不支持的数据类型的错误。|
|HMPP_STS_NUMCHANNELS_ERR|初始化不支持的通道数的错误。|
|HMPP_STS_STEP_ERR|srcStep、dstStep、srcBitOffset或者dstBitOffset中存在零或负值。或srcBitOffset、dstBitOffset不能满足roiSize字段的关系。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型的错误。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002518281580.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void ErodeBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphologyBorderInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphologyBorderInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_ErodeBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_ErodeBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_ErodeRelease(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    ErodeBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphologyBorderInit_16s_C1R result = 0
HMPPI_ErodeBorder_16s_C1R result = 0
result = 0
dst =
 11  11  12  13   0   0   0   0
 11  11  12  13   0   0   0   0
 21  21  22  23   0   0   0   0
 31  31  32  33   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## GrayDilateBorder

使用蒙版进行图像灰度膨胀。灰度膨胀就是求局部源图像与蒙版元素和的最大值的操作，运算符是“![](../../figures/zh-cn_formulaimage_0000002518281828.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002549921577.png)

该公式表示用蒙版B来对图像A进行灰度膨胀处理，其中B是一个蒙版。通过蒙版B与图像A进行卷积计算，扫描图像中的每一个像素点，计算B覆盖区域的像素点与B蒙版元素值和的最大值，并用这个最大值替换参考点指定的像素值实现灰度膨胀。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002549921589.png)

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)的HmppiBorderType说明，但只支持**HMPP\_BORDER\_REPL**。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphGrayPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphGrayPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphGrayInit\_8u\_C1R\(HmppiMorphGrayPolicy\_8u \*\*policy, HmppiSize roiSize, const int32\_t \*mask, HmppiSize maskSize, HmppiPoint anchor\)；

    HmppResult HMPPI\_MorphGrayInit\_32f\_C1R\(HmppiMorphGrayPolicy\_32f \*\*policy, HmppiSize roiSize, const float \*mask, HmppiSize maskSize, HmppiPoint anchor\);

- **主函数操作：**

    HmppResult HMPPI\_GrayDilateBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, HmppiMorphGrayPolicy\_8u \*policy\);

    HmppResult HMPPI\_GrayDilateBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, HmppiMorphGrayPolicy\_32f \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphGrayRelease\_8u\(HmppiMorphGrayPolicy\_8u \*policy\);

    HmppResult HMPPI\_MorphGrayRelease\_32f\(HmppiMorphGrayPolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|anchor|锚点坐标。|anchor.x∈(0, maskSize.width-1]，anchor.y∈(0, maskSize.height-1]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_REPL：边框从边缘像素复制而来。|输入|
|policy（init函数中）|指向内存存储HmppiMorphGrayPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphGrayPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_ANCHOR_ERR|锚点anchor字段为负或大于等于maskSize的字段的错误。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|
|HMPP_STS_NOT_SUPPORTED_INPLACE_MODE_ERR|src与dst内存地址相同。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void GrayDilateBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 122, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    HmppiPoint anchor = { 0, 0 };
    int32_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, -2, 3,
        5, -1, 1, 0,
        0, 2, 0, -6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(uint8_t);
    int32_t dstStep = dstWidth * sizeof(uint8_t);
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    HmppiMorphGrayPolicy_8u *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphGrayInit_8u_C1R(&policy, roiSize, mask, maskSize, anchor);
    printf("HMPPI_MorphGrayInit_8u_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_GrayDilateBorder_8u_C1R(src, srcStep, dst, dstStep, roiSize, borderType, policy);
    printf("HMPPI_GrayDilateBorder_8u_C1R result = %d\n", result);
    (void)HMPPI_MorphGrayRelease_8u(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    GrayDilateBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphGrayInit_8u_C1R result = 0
HMPPI_GrayDilateBorder_8u_C1R result = 0
result = 0
dst =
122 123  36  36   0   0   0   0
 44  45  46  46   0   0   0   0
 46  47  48  49   0   0   0   0
 47  47  48  49   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## GrayErodeBorder

使用蒙版进行图像灰度腐蚀。灰度腐蚀就是求局部源图像与蒙版元素和的最小值的操作，运算符是“![](../../figures/zh-cn_formulaimage_0000002549921337.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002518281590.png)

该公式表示用蒙版B来对图像A进行灰度腐蚀处理，其中B是一个蒙版。通过蒙版B与图像A进行卷积计算，扫描图像中的每一个像素点，计算B覆盖区域的像素点与B蒙版元素值和的最小值，并用这个最小值替换参考点指定的像素值实现灰度腐蚀。

以下为接口输入输出图示：

![](../../figures/zh-cn_image_0000002550041337.png)

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)的HmppiBorderType说明，但只支持**HMPP\_BORDER\_REPL**。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphGrayPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphGrayPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphGrayInit\_8u\_C1R\(HmppiMorphGrayPolicy\_8u \*\*policy, HmppiSize roiSize, const int32\_t \*mask, HmppiSize maskSize, HmppiPoint anchor\)；

    HmppResult HMPPI\_MorphGrayInit\_32f\_C1R\(HmppiMorphGrayPolicy\_32f \*\*policy, HmppiSize roiSize, const float \*mask, HmppiSize maskSize, HmppiPoint anchor\);

- **主函数操作：**

    HmppResult HMPPI\_GrayErodeBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, HmppiMorphGrayPolicy\_8u \*policy\);

    HmppResult HMPPI\_GrayErodeBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, HmppiMorphGrayPolicy\_32f \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphGrayRelease\_8u\(HmppiMorphGrayPolicy\_8u \*policy\);

    HmppResult HMPPI\_MorphGrayRelease\_32f\(HmppiMorphGrayPolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|anchor|锚点坐标。|anchor.x∈(0, maskSize.width-1]，anchor.y∈(0, maskSize.height-1]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数。|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_REPL：边框从边缘像素复制而来。|输入|
|policy（init函数中）|指向内存存储HmppiMorphGrayPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphGrayPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_ANCHOR_ERR|锚点anchor字段为负或大于等于maskSize的字段。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|
|HMPP_STS_NOT_SUPPORTED_INPLACE_MODE_ERR|src与dst内存地址相同。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void GrayErodeBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 122, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    HmppiPoint anchor = { 0, 0 };
    int32_t mask[MASK_BUFFER_SIZE_T] = {
        -7, 0, -2, 3,
        5, -1, 1, 0,
        0, 2, 0, -6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(uint8_t);
    int32_t dstStep = dstWidth * sizeof(uint8_t);
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    HmppiMorphGrayPolicy_8u *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphGrayInit_8u_C1R(&policy, roiSize, mask, maskSize, anchor);
    printf("HMPPI_MorphGrayInit_8u_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_GrayErodeBorder_8u_C1R(src, srcStep, dst, dstStep, roiSize, borderType, policy);
    printf("HMPPI_GrayErodeBorder_8u_C1R result = %d\n", result);
    (void)HMPPI_MorphGrayRelease_8u(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    GrayErodeBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphGrayInit_8u_C1R result = 0
HMPPI_GrayErodeBorder_8u_C1R result = 0
result = 0
dst =
  4  12   6   7   0   0   0   0
 14  15  16  17   0   0   0   0
 24  25  26  27   0   0   0   0
 34  35  36  37   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphBlackhatBorder

使用蒙版进行图像黑帽操作，其默认蒙版元素全为非0有效。其定义如下：

![](../../figures/zh-cn_formulaimage_0000002550041649.png)

该公式表示用蒙版B来对图像A进行黑帽处理。形态学中的黑帽操作表示闭操作（通过蒙版B对图像A执行闭操作）与源图像A的差。

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphAdvPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphAdvPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphAdvInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_MorphBlackhatBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphBlackhatBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphAdvRelease\_1u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16s\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C4R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C4R\(HmppiMorphAdvPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，填充值依据基础操作选用填充的固定值。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphAdvPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002518281896.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphBlackhatBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphAdvPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphAdvInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphAdvInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphBlackhatBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_MorphBlackhatBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphAdvRelease_16s_C1R(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphBlackhatBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphAdvInit_16s_C1R result = 0
HMPPI_MorphBlackhatBorder_16s_C1R result = 0
result = 0
dst =
 12  11  10  10   0   0   0   0
  2   1   0   0   0   0   0   0
  2   1   0   0   0   0   0   0
  2   1   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphCloseBorder

使用蒙版进行图像闭操作，其默认蒙版元素全为非0有效。运算符是“![](../../figures/zh-cn_formulaimage_0000002550041711.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002549921717.png)

该公式表示用蒙版B来对图像A进行闭操作。形态学中的闭操作表示使用蒙版B对图像A进行膨胀操作后再对结果进行腐蚀操作。

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphAdvPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphAdvPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphAdvInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_MorphCloseBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphCloseBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphAdvRelease\_1u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16s\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C4R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C4R\(HmppiMorphAdvPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，填充值依据基础操作选用填充的固定值。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphAdvPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002518281956.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphCloseBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphAdvPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphAdvInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphAdvInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphCloseBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_MorphCloseBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphAdvRelease_16s_C1R(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphCloseBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphAdvInit_16s_C1R result = 0
HMPPI_MorphCloseBorder_16s_C1R result = 0
result = 0
dst =
 23  23  23  24   0   0   0   0
 23  23  23  24   0   0   0   0
 33  33  33  34   0   0   0   0
 43  43  43  44   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphGradientBorder

使用蒙版计算图像的梯度，其默认蒙版元素全为非0有效。其定义如下：

![](../../figures/zh-cn_formulaimage_0000002518441490.png)

该公式表示用蒙版B来计算图像A的梯度。形态学中梯度的计算表示从源图像A的膨胀操作中减去源图像A的腐蚀操作的结果。

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphAdvPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphAdvPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphAdvInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_MorphGradientBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphGradientBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphAdvRelease\_1u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16s\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C4R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C4R\(HmppiMorphAdvPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，填充值依据基础操作选用填充的固定值。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphAdvPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002550041343.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphGradientBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphAdvPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphAdvInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphAdvInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphGradientBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_MorphGradientBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphAdvRelease_16s_C1R(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphGradientBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphAdvInit_16s_C1R result = 0
HMPPI_MorphGradientBorder_16s_C1R result = 0
result = 0
dst =
 12  13  12  11   0   0   0   0
 22  23  22  21   0   0   0   0
 22  23  22  21   0   0   0   0
 12  13  12  11   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphOpenBorder

使用蒙版进行图像开操作，其默认蒙版元素全为非0有效。运算符是“![](../../figures/zh-cn_formulaimage_0000002518281604.png)”，其定义如下：

![](../../figures/zh-cn_formulaimage_0000002518441512.png)

该公式表示用蒙版B来对图像A进行开处理。形态学中的开操作表示使用蒙版B对图像A进行腐蚀操作后再对结果进行膨胀操作。

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphAdvPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphAdvPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphAdvInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_MorphOpenBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue,    const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphOpenBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

- **释放内存操作：**

    HmppResult HMPPI\_MorphAdvRelease\_1u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16s\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C4R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C4R\(HmppiMorphAdvPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，填充值依据基础操作选用填充的固定值。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphAdvPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002550041347.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphOpenBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphAdvPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphAdvInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphAdvInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphOpenBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_MorphOpenBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphAdvRelease_16s_C1R(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphOpenBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphAdvInit_16s_C1R result = 0
HMPPI_MorphOpenBorder_16s_C1R result = 0
result = 0
dst =
 11  12  13  13   0   0   0   0
 21  22  23  23   0   0   0   0
 31  32  33  33   0   0   0   0
 31  32  33  33   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphTophatBorder

使用蒙版进行图像顶帽操作，其默认蒙版元素全为非0有效。其定义如下：

![](../../figures/zh-cn_formulaimage_0000002518281860.png)

该公式表示用蒙版B来对图像A进行顶帽处理。形态学中的顶帽操作表示开操作（通过蒙版B对图像A执行开操作）与源图像A的差。

此类函数支持的边界方式说明可参照[枚举类型](../common.md#枚举类型)中HmppiBorderType的说明。

同时支持混合填充方式：如HMPP\_BORDERREPL**|**HMPP\_BORDERINMEM\_TOP，表示上边界以HMPP\_BORDERINMEM方式填充，其余边界以HMPP\_BORDERREPL方式填充。

该函数调用流程如下：

1. 调用对应类型Init初始化HmppiMorphAdvPolicy结构体，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放HmppiMorphAdvPolicy函数所包含内存，否则将产生内存泄漏。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphAdvInit\_1u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16u\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_16s\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C1R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C3R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_8u\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

    HmppResult HMPPI\_MorphAdvInit\_32f\_C4R\(HmppiSize roiSize, const uint8\_t \*mask, HmppiSize maskSize, HmppiMorphAdvPolicy \*\*policy\);

- **主函数操作：**

    HmppResult HMPPI\_MorphTophatBorder\_1u\_C1R\(const uint8\_t \*src, int32\_t srcStep, int32\_t srcBitOffset, uint8\_t \*dst, int32\_t dstStep, int32\_t dstBitOffset, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint8\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, uint16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, int16\_t borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, float borderValue, const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[3\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const uint8\_t borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphTophatBorder\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiBorderType borderType, const float borderValue\[4\], const HmppiMorphAdvPolicy \*policy\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphAdvRelease\_1u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16u\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_16s\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C1R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C3R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_8u\_C4R\(HmppiMorphAdvPolicy \*policy\);

    HmppResult HMPPI\_MorphAdvRelease\_32f\_C4R\(HmppiMorphAdvPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mask|指向蒙版的指针。|非空|输入|
|maskSize|蒙版图像的大小（以像素为单位）。|maskSize.width∈(0,INT_MAX]，maskSize.height∈(0,INT_MAX]|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|borderType|边界填充类型。|枚举，边界填充类型。HMPP_BORDER_DEFAULT：边框设置为HMPP_BORDER_CONST，填充值依据基础操作选用填充的固定值。HMPP_BORDER_REPL：边框从边缘像素复制而来。HMPP_BORDER_IN_MEM：边框是从内存中的源图像像素获得的。HMPP_BORDER_CONST：所有边框像素的值都设置为常数。HMPP_BORDER_MIRROR：边框像素从源图像边界像素镜像而来。还支持混合边框。|输入|
|borderValue|常量值，分配给常量边框HMPP_BORDER_CONST的像素。|数据类型范围内的值|输入|
|policy（init函数中）|指向内存存储HmppiMorphAdvPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- src不能和dst是同一数组，或内存重叠，否则可能导致结果错误。
>- 当使用**HMPP\_BORDER\_IN\_MEM**边界填充模式时，需保证入参的源图像有足够的额外数据以供计算使用，调用主函数时将src偏移最少offset长度，否则甚至可能产生崩溃等情况。offset需满足：
> ![](../../figures/zh-cn_formulaimage_0000002518441768.png)
> 其中T为源图像数据类型。
>- **1u\_C1R**类型接口不支持HMPP\_BORDER\_MIRROR模式，否则将返回HMPP\_STS\_BORDER\_ERR错误。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphTophatBorderExample()
{
    HmppiSize roiSize = { 4, 4 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16S;
    int32_t numChannels = 1;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    int16_t borderValue = -100;
    HmppiMorphAdvPolicy *policy = NULL;
    HmppResult result;

    result = HMPPI_MorphAdvInit_16s_C1R(roiSize, mask, maskSize, &policy);
    printf("HMPPI_MorphAdvInit_16s_C1R result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphTophatBorder_16s_C1R(src, srcStep, dst, dstStep, roiSize, borderType, borderValue, policy);
    printf("HMPPI_MorphTophatBorder_16s_C1R result = %d\n", result);
    (void)HMPPI_MorphAdvRelease_16s_C1R(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphTophatBorderExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphAdvInit_16s_C1R result = 0
HMPPI_MorphTophatBorder_16s_C1R result = 0
result = 0
dst =
  0   0   0   1   0   0   0   0
  0   0   0   1   0   0   0   0
  0   0   0   1   0   0   0   0
 10  10  10  11   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphReconstructDilate

通过膨胀重建图像。

该函数调用流程如下：

1. 调用对应类型Init初始化主函数执行所需内存buffer，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放buffer内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphReconstructInit\_32f\(HmppiSize roiSize, HmppDataType dataType, int32\_t numChannels, float \*\*buffer\);

    HmppResult HMPPI\_MorphReconstructInit\_8u\(HmppiSize roiSize, HmppDataType dataType, int32\_t numChannels, uint8\_t \*\*buffer\);

- **主函数操作：**

    HmppResult HMPPI\_MorphReconstructDilate\_8u\_C1IR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructDilate\_16u\_C1IR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructDilate\_64f\_C1IR\(const double \*src, int32\_t srcStep, double \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructDilate\_32f\_C1IR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float \*buffer, HmppiNorm Norm\);

- **释放内存操作：**

    HmppResult HMPPI\_MorphReconstructRelease\_8u\(uint8\_t \*buffer\);

    HmppResult HMPPI\_MorphReconstructRelease\_32f\(float \*buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dataType|源图像的数据类型（目标图像同源图像）。|枚举，数据类型。支持HMPP8U、HMPP16U、HMPP32F和HMPP64F。|输入|
|numChannels|源图像的通道数（目标图像同源图像）。|仅支持1、3和4通道。|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数。|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|Norm|蒙版标准类型。|HMPP_NORMINF：无穷范数，8连通，3x3矩形掩码。HMPP_NORML1：L1标准，4连通，3x3交叉蒙版。|输入|
|buffer（init函数中）|指向辅助内存buffer的指针的指针。|非空|输出|
|buffer（主函数中和release函数中）|指向辅助内存buffer的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|初始化不支持的数据类型。|
|HMPP_STS_NUMCHANNELS_ERR|初始化不支持的通道数。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_NORM_ERR|蒙版的类型不支持。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>8u、16u、64f类型主函数使用8u类型初始化接口，32f类型主函数使用32f类型初始化接口，否则调用主函数可能会产生崩溃等情况。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphReconstructDilateExample()
{
    HmppiSize roiSize = { 4, 4 };
    const uint16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    uint16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    HmppiSize maskSize = { 4, 3 };
    uint8_t mask[MASK_BUFFER_SIZE_T] = {
        1, 0, 2, 3,
        5, 1, 1, 0,
        0, 2, 0, 6,
    };

    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(int16_t);
    int32_t dstStep = dstWidth * sizeof(int16_t);
    HmppDataType dataType = HMPP16U;
    int32_t numChannels = 1;
    uint8_t *buffer = NULL;
    HmppiNorm Norm = HMPP_NORMINF;
    HmppResult result;

    result = HMPPI_MorphReconstructInit_8u(roiSize, dataType, numChannels, &buffer);
    printf("HMPPI_MorphReconstructInit_8u result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphReconstructDilate_16u_C1IR(src, srcStep, dst, dstStep, roiSize, buffer, Norm);
    printf("HMPPI_MorphReconstructDilate_16u_C1IR result = %d\n", result);
    (void)HMPPI_MorphReconstructRelease_8u(buffer);
    buffer = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphReconstructDilateExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphReconstructInit_8u result = 0
HMPPI_MorphReconstructDilate_16u_C1IR result = 0
result = 0
dst =
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphReconstructErode

通过腐蚀重建图像。

该函数调用流程如下：

1. 调用对应类型Init初始化主函数执行所需内存buffer，否则主函数无法调用成功。
2. 调用主函数。
3. 最后调用Release释放buffer内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPI\_MorphReconstructInit\_32f\(HmppiSize roiSize, HmppDataType dataType, int32\_t numChannels, float \*\*buffer\);

    HmppResult HMPPI\_MorphReconstructInit\_8u\(HmppiSize roiSize, HmppDataType dataType, int32\_t numChannels, uint8\_t \*\*buffer\);

- **主函数操作：**

    HmppResult HMPPI\_MorphReconstructErode\_8u\_C1IR\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructErode\_16u\_C1IR\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructErode\_64f\_C1IR\(const double \*src, int32\_t srcStep, double \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t \*buffer, HmppiNorm Norm\);

    HmppResult HMPPI\_MorphReconstructErode\_32f\_C1IR\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, float \*buffer, HmppiNorm Norm\);

- **释放内存操作**：

    HmppResult HMPPI\_MorphReconstructRelease\_8u\(uint8\_t \*buffer\);

    HmppResult HMPPI\_MorphReconstructRelease\_32f\(float \*buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dataType|源图像的数据类型（目标图像同源图像）。|枚举，数据类型。支持HMPP8U、HMPP16U、HMPP32F和HMPP64F。|输入|
|numChannels|源图像的通道数（目标图像同源图像）。|仅支持1、3和4通道|输入|
|src|指向源图像的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为src数据类型的字节整倍数。|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数，且为dst数据类型的字节整倍数。|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|Norm|蒙版标准类型。|HMPP_NORMINF：无穷范数，8连通，3x3矩形掩码。HMPP_NORML1：L1标准，4连通，3x3交叉蒙版。|输入|
|buffer（init函数中）|指向辅助内存buffer的指针的指针。|非空|输出|
|buffer（主函数中和release函数中）|指向辅助内存buffer的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_SIZE_ERR|roiSize或者maskSize的字段为零或负值。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|初始化不支持的数据类型。|
|HMPP_STS_NUMCHANNELS_ERR|初始化不支持的通道数。|
|HMPP_STS_STEP_ERR|源或者目标Step中存在零或负值。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或者dstStep不为其图像所属数据类型的字节长度的整倍数。|
|HMPP_STS_ROI_ERR|源或者目标图像的宽和高比roiSize的字段小。|
|HMPP_STS_NORM_ERR|蒙版的类型不支持。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>8u、16u、64f类型主函数使用8u类型初始化接口，32f类型主函数使用32f类型初始化接口，否则调用主函数时将产生甚至崩溃等情况。

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 64
#define MASK_BUFFER_SIZE_T 12
void MorphReconstructErodeExample()
{
    HmppiSize roiSize = { 4, 4 };
    const uint16_t src[SRC_BUFFER_SIZE_T] = {
        11, 12, 13, 14, 15, 16, 17, 18, 12, 19, 13, 15,
        21, 22, 23, 24, 25, 26, 27, 28, 22, 29, 23, 25,
        31, 32, 33, 34, 35, 36, 37, 38, 32, 39, 33, 35,
        41, 42, 43, 44, 45, 46, 47, 48, 42, 49, 43, 45,
        51, 52, 53, 54, 55, 56, 57, 58, 52, 59, 53, 55,
        61, 62, 63, 64, 65, 66, 67, 68, 62, 69, 63, 65,
        71, 72, 73, 74, 75, 76, 77, 78, 72, 79, 73, 75,
        81, 82, 83, 84, 85, 86, 87, 88, 82, 89, 83, 85
    };

    uint16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    const int32_t srcWidth = 12;
    const int32_t dstWidth = 8;
    int32_t srcStep = srcWidth * sizeof(uint16_t);
    int32_t dstStep = dstWidth * sizeof(uint16_t);
    HmppDataType dataType = HMPP16U;
    int32_t numChannels = 1;
    uint8_t *buffer = NULL;
    HmppiNorm Norm = HMPP_NORMINF;
    HmppResult result;

    result = HMPPI_MorphReconstructInit_8u(roiSize, dataType, numChannels, &buffer);
    printf("HMPPI_MorphReconstructInit_8u result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_MorphReconstructErode_16u_C1IR(src, srcStep, dst, dstStep, roiSize, buffer, Norm);
    printf("HMPPI_MorphReconstructErode_16u_C1IR result = %d\n", result);
    (void)HMPPI_MorphReconstructRelease_8u(buffer);
    buffer = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }

    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; ++i) {
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ", dst[i]);
    }
    printf("\n");
}

int main(void)
{
    MorphReconstructErodeExample();
    return 0;
}
```

运行结果：

```text
HMPPI_MorphReconstructInit_8u result = 0
HMPPI_MorphReconstructErode_16u_C1IR result = 0
result = 0
dst =
 11  12  13  14   0   0   0   0
 21  22  23  24   0   0   0   0
 31  32  33  34   0   0   0   0
 41  42  43  44   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0
```

## MorphSetMode

设置针对高级形态学操作的蒙版处理模式。

函数接口声明如下：

HmppResult HMPPI\_MorphSetMode\(int32\_t mode, HmppiMorphAdvPolicy\* policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mode|蒙版处理模式。|支持的值：0，黑帽/顶帽在进行第二次操作前，使像素阈值大于零。1，不翻转蒙版。4，黑帽/顶帽不设置阈值。|输入|
|policy|指向HmppiMorphAdvPolicy结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空。|
|HMPP_STS_NOT_SUPPORTED_MODE_ERR|mode非0、1、4。|
