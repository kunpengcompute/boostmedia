# 几何与频域变换

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Transpose

对图像执行转置操作。

函数接口声明如下：

HmppResult HMPPI_Transpose_16s_C1R(const int16_t \*pSrc, int srcStep, int16_t \*pDst, int dstStep, HmppiSize roiSize);

HmppResult HMPPI_Transpose_32s_C1R(const int32_t \*pSrc, int srcStep, int32_t \*pDst, int dstStep, HmppiSize roiSize);

HmppResult HMPPI_Transpose_32f_C1R(const float \*pSrc, int srcStep, float \*pDst, int dstStep, HmppiSize roiSize);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|整数|输入|
|pDst|指向目标图像感兴趣区域的指针。|非空|输出|
|dstStep|目标图像中连续行起点之间的距离（以字节为单位）。|整数|输入|
|roiSize|源图像感兴趣区域大小（以像素为单位）。|roiSize.width∈(0, INT_MAX]，roiSize.height∈(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|pSrc或pDst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize.width或roiSize.height小于或等于0。|

**示例**

```c
#include "hmppi.h"
#include <stdint.h>
#include <stdio.h>

int main(void)
{
    int32_t src[6] = {10, 20, 30, 40, 50, 60};
    int32_t dst[6] = {0};
    HmppResult ret = HMPPI_Transpose_32s_C1R(src, 3 * (int)sizeof(int32_t), dst, 2 * (int)sizeof(int32_t), (HmppiSize){3, 2});
    printf("ret=%d\n", ret);
    for (int i = 0; i < 6; ++i) {
        printf("%d ", dst[i]);
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
ret=0
10 40 20 50 30 60
```

## FFT

对二维图像做快速傅里叶变换。

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>调用该接口计算FFT之前，必须调用HMPPS\_FFTCToCInit接口初始化HmppsFFTPolicy规范结构。

- 初始化函数

    HmppResult HMPPI\_FFTCToCInit\_32fc\(int32\_t powerX, int32\_t powerY, int32\_t direction, int32\_t flag, HmppsFFTPolicy\_32fc \*\*policyX, HmppsFFTPolicy\_32fc \*\*policyY\);

    其中policyX，policyY需要调用HMPPS\_FFTCToCInit\_32fc做初始化。

- 主函数

    HmppResult HMPPI\_FFTCToC\_32fc\_C1R\(Hmpp32fc \*src, int srcStep, Hmpp32fc \*dst, int dstStep, HmppsFFTPolicy\_32fc \*policyX, HmppsFFTPolicy\_32fc \*policyY\);

- 资源释放函数

    HmppResult HMPPI\_FFTCToCRelease\_32fc\(HmppsFFTPolicy\_32fc \*policyX, HmppsFFTPolicy\_32fc \*policyY\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|powerX|图像X轴方向的信号长度, FFT序列输入信号长度为![](../../figures/zh-cn_formulaimage_0000002549921763.png)。|[0, 27]|输入|
|powerY|图像Y轴方向的信号长度, FFT序列输入信号长度为![](../../figures/zh-cn_formulaimage_0000002550041759.png)。|[0, 27]|输入|
|direction|direction=1表示FFT正变换。direction=-1表示FFT逆变换。用于CToC模式。|±1|输入|
|flag|结果正规化模式。|HMPP_FFT_DIV_FWD_BY_N：正向变换，1/N正规化模式。HMPP_FFT_DIV_BWD_BY_N：反向变换，1/N正规化模式。HMPP_FFT_DIV_BY_SQRTN：正向或反向变换，1/N1/2正规化模式。HMPP_FFT_NODIV_BY_ANY：正向或反向变换，不做特殊处理。|输入|
|policyX（init函数中）|双重指针，指向X轴方向的HmppsFFTPolicy结构体，结构体内包含FFT计算需要的一些信息和缓存块的首地址。|非空|输出|
|policyY（init函数中）|双重指针，指向Y轴方向的HmppsFFTPolicy结构体，结构体内包含FFT计算需要的一些信息和缓存块的首地址。|非空|输出|
|policy（主函数和release函数）|指针，指向HmppsFFTPolicy结构体。|非空|输入|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|指针参数为空。|
|HMPP_STS_FFT_POWER_ERR|power小于0或大于27。|
|HMPP_STS_MALLOC_FAILED|所需的额外内存申请失败。|
|HMPP_STS_FFT_FLAG_ERR|flag不是HMPP_FFT_DIV_FWD_BY_N、HMPP_FFT_DIV_BWD_BY_N、HMPP_FFT_DIV_BY_SQRTN或HMPP_FFT_NODIV_BY_ANY。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- HmppsFFTPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。

**示例**

```c
#include <stdio.h>
#include <math.h>
#include "hmppi.h"
#include "hmpp_type.h"
#define PI 3.14159265358979323846
void FFTCToC_Example()
{
    Hmpp32fc src[64], dst[64];
    for (int32_t i = 0; i < 8; i++) {
        for (int32_t j = 0; j < 8; j++) {
            src[i * 8 + j].re = cos(2 * PI * i * 16 / 64);
            src[i * 8 + j].im = 1;
        }
    }
    HmppResult result;
    HmppsFFTPolicy_32fc *policyX = NULL;
    HmppsFFTPolicy_32fc *policyY = NULL;
    int srcStep = 8 * sizeof(Hmpp32fc);
    int dstStep = 8 * sizeof(Hmpp32fc);
    result = HMPPI_FFTCToCInit_32fc(3, 3, 1, HMPP_FFT_NODIV_BY_ANY, &policyX, &policyY);// 正向FFT
    if (result != HMPP_STS_NO_ERR) {
        printf("Create Policy Error!\n");
        return;
    }
    result = HMPPI_FFTCToC_32fc_C1R(src, srcStep, dst, dstStep, policyX, policyY);
    if (result != HMPP_STS_NO_ERR) {
        printf("FFT Error!\n");
        return;
    }
    HMPPI_FFTCToCRelease_32fc(policyX, policyY);
    printf("dstRe =");
    for (int32_t i = 0; i < 8; i++) {
        for (int j = 0; j < 8; j++) {
            printf("    %e", dst[i * 8 + j].re);
        }
    }
    printf("\ndstIm =");
    for (int32_t i = 0; i < 64; i++) {
        for (int j = 0; j < 8; j++) {
            printf("    %e", dst[i * 8 + j].im);
        }
    }
    printf("\n");
}
int main() {
    FFTCToC_Example();
    return 0;
}
```

运行结果：

```text
dstRe =    -1.959435e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    -2.771059e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    3.200000e+01    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    2.771059e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    1.959435e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    2.771059e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    3.200000e+01    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    -2.771059e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00
dstIm =    6.400000e+01    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    -7.837740e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    7.837740e-15    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    1.000000e+00    0.000000e+00    8.968310e-44    2.242078e-44    1.121039e-44    1.121039e-44    9.183409e-41    9.183409e-41    0.000000e+00    0.000000e+00    1.401298e-45    9.183409e-41    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    -1.973691e+26    9.183409e-41    -1.973691e+26    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    -1.456977e-27    9.183409e-41    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    9.183409e-41    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    0.000000e+00    9.183409e-41    0.000000e+00    9.183409e-41    0.000000e+00    0.000000e+00    -1.456977e-27    -1.979496e+26    4.794963e-39    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00    0.000000e+00
```

## ResizeLinear

用于调整图像大小，属于图像几何变换的一类功能。

在调用ResizeLinear函数之前需要先调用Init函数进行初始化，然后调用ResizeLinear主功能函数，最后调用Release函数释放相关空间。对于Resize类函数，主要有以下三种处理方式调整图像大小：

- Init之后，调整整个图像的大小，最后Release。
- Init之后，对每一个图像块调整大小，最后Release。
- 对每个图像块进行Init，之后通过Resize主函数调整大小，最后Release。

目前仅支持双线性插值算法进行图像大小调整。

函数接口声明如下：

- **初始化函数：**

    HmppResult HMPPI\_ResizeLinearInit\_8u\(HmppiSize srcSize, HmppiSize dstSize, HmppiResizePolicy\_32f\*\* policy\);

    HmppResult HMPPI_ResizeLinearInit_16s(HmppiSize srcSize, HmppiSize dstSize, HmppiResizePolicy_32f* pSpec);

    HmppResult HMPPI\_ResizeLinearInit\_32f\(HmppiSize srcSize, HmppiSize dstSize, HmppiResizePolicy\_32f\*\* pSpec\);

- **主函数：**

    HmppResult HMPPI\_ResizeLinear\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiPoint dstOffset, HmppiSize dstSize, HmppiBorderType border, const uint8\_t \*borderValue, const HmppResizePolicy\_32f \*policy\);

    HmppResult HMPPI\_ResizeLinear\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiPoint dstOffset, HmppiSize dstSize, HmppiBorderType border, const uint8\_t \*borderValue, const HmppResizePolicy\_32f \*policy\);

    HmppResult HMPPI_ResizeLinear_16s_C1R(const int16_t*pSrc, HMPP32S srcStep, int16_t* pDst, HMPP32S dstStep, HmppiPoint dstOffset, HmppiSize dstSize, HmppiBorderType border, const int16_t*pBorderValue, const HmppiResizePolicy_32f* pSpec);

    HmppResult HMPPI\_ResizeLinear\_32f\_C1R\(const float\* pSrc, int32\_t srcStep, float\* pDst, int32\_t dstStep, HmppiPoint dstOffset, HmppiSize dstSize, HmppiBorderType border, const float\* pBorderValue, const HmppiResizePolicy\_32f\* pSpec\);

- **释放函数：**

    HmppResult HMPPI\_ResizeLinearRelease\_8u\(HmppResizePolicy\_32f \*policy\);

    HmppResult HMPPI\_ResizeLinearRelease\_32f\(HmppiResizePolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src/pSrc|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|srcSize|源图像块的大小。|正整数|输入|
|dst/pDst|指向目标图像感兴趣区域的指针。|非空|输入、输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dstOffset|目标图像块的偏移值。|非负整数|输入|
|dstSize|目标图像块的大小。|正整数|输入|
|border|边界取值算法。|HMPPI_ResizeLinear_8u_*目前支持如下算法：HMPPI_BORDER_REPL、HMPPI_BORDER_IN_MEM、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_LEFT、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_RIGHT、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_LEFT | HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_LEFT | HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM_BOTTOM | HMPPI_BORDER_IN_MEM_LEFT、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM_TOP | HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_LEFT | HMPPI_BORDER_IN_MEM_BOTTOM | HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_REPL | HMPPI_BORDER_IN_MEM_LEFT | HMPPI_BORDER_IN_MEM_TOP | HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM，HMPPI_ResizeLinear_32f_C1R目前支持以下算法：HMPPI_BORDER_REPLH、MPPI_BORDER_IN_MEM、HMPPI_BORDER_MIRROR、HMPPI_BORDER_MIRROR_R|输入|
|borderValue|边界值。|border为HMPPI_BORDER_CONST时边界值|输入|
|policy/pSpec|特殊结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_OPERATION|告警，不进行操作。srcSize和dstSize中width、height存在零。|
|HMPP_STS_NULL_PTR_ERR|传入的任意一个指针存在空指针。|
|HMPP_STS_MALLOC_FAILED|Policy初始化时内存申请失败。|
|HMPP_STS_SIZE_ERR|srcSize和dstSize中width、height存在负值。或srcSize小于2x2。|
|HMPP_STS_BORDER_ERR|边界算法类型不支持。|
|HMPP_STS_OUT_OF_RANGE_ERR|目标图像块偏移值比Init函数输入的目标图像块的width/height大。|
|HMPP_STS_SIZE_WRN|目标图像块width/height比Init函数输入的目标图像块的width/height大。|

**示例**

```c
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <hmpp.h>

int main ()
{
    HmppiResizePolicy_32f *policy = NULL;
    HmppiSize srcSize = {2, 2};
    HmppiSize dstSize = {4, 4};
    int32_t numChannels = 3;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    const uint8_t borderValue = 0;
    HmppResult result = 0;
    HmppiPoint dstOffset = {0, 0};

    uint8_t src[12] = {0, 255, 3, 6, 255, 0,
                       2, 1,   3, 4, 134, 23};
    int32_t dstLen = dstSize.width * dstSize.height * numChannels;
    uint8_t *dst = HMPPS_Malloc_8u(dstLen);

    result = HMPPI_ResizeLinearInit_8u(srcSize, dstSize, &policy);
    printf("result = %d\n", result);
    int32_t srcStep = srcSize.width * numChannels * sizeof(uint8_t);
    int32_t dstStep = dstSize.width * numChannels * sizeof(uint8_t);
    result = HMPPI_ResizeLinear_8u_C3R(src, srcStep, dst, dstStep, dstOffset, dstSize, borderType, &borderValue, policy);
    printf("result = %d\n", result);

    HMPPI_ResizeLinearRelease_8u(policy);
    printf("free policy end\n");
    for (int32_t i = 0; i < dstLen; i++) {
        printf("%d ", dst[i]);
    }
    printf("\n");
    HMPPS_Free(dst);
}
```

运行结果：

```text
result = 0
result = 0
free policy end
0 255 3 2 255 2 5 255 1 6 255 0 1 192 3 2 200 4 4 216 5 6 225 6 2 65 3 2 89 7 4 139 14 5 164 17 2 1 3 3 34 8 4 101 18 4 134 23
```

## Resize

用于调整三维图像大小，属于图像几何变换的一类功能。

函数接口声明如下：

- **三维图像调整大小：**

    HmppResult HMPPI\_RResize\_8u\_C1V\(const uint8\_t \*pSrc, HmpprVolume srcVolume, int srcStep, int srcPlaneStep,

    HmpprCuboid srcVoi, uint8\_t \*pDst, int dstStep, int dstPlaneStep, HmpprCuboid dstVoi, double xFactor,

    double yFactor, double zFactor, double xShift, double yShift, double zShift, int interpolation\);

    HmppResult HMPPI\_RResize\_16u\_C1V\(const uint16\_t \*pSrc, HmpprVolume srcVolume, int srcStep, int srcPlaneStep,

    HmpprCuboid srcVoi, uint16\_t \*pDst, int dstStep, int dstPlaneStep, HmpprCuboid dstVoi, double xFactor,

    double yFactor, double zFactor, double xShift, double yShift, double zShift, int interpolation\);

    HmppResult HMPPI\_RResize\_32f\_C1V\(const float \*pSrc, HmpprVolume srcVolume, int srcStep, int srcPlaneStep,

    HmpprCuboid srcVoi, float \*pDst, int dstStep, int dstPlaneStep, HmpprCuboid dstVoi, double xFactor,

    double yFactor, double zFactor, double xShift, double yShift, double zShift, int interpolation\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源体积起点的指针。|非空|输入|
|srcVolume|源volume的大小。|正整数|输入|
|srcStep|源体积中每个平面连续行起始点之间的距离（以字节为单位）。|非负整数|输入|
|srcPlaneStep|源连续体积平面之间的距离（以字节为单位）。|非负整数|输入|
|srcVoi|源体积的感兴趣体积。|正整数|输入|
|pDst|指向目标体积原点的指针。|非空|输入/输出|
|dstStep|目标体积中每个平面连续行起始点之间的距离（以字节为单位）。|非负整数|输入|
|dstPlaneStep|目标连续体积平面之间的距离（以字节为单位）。|非负整数|输入|
|dstVoi|目标体积的感兴趣体积。|正整数|输入|
|xFactor|改变源VOI的x维度因子。|正数|输入|
|yFactor|改变源VOI的y维度因子。|正数|输入|
|zFactor|改变源VOI的z维度因子。|正数|输入|
|xShift|在x方向上的偏移值。|实数|输入|
|yShift|在y方向上的偏移值。|实数|输入|
|zShift|在z方向上的偏移值。|实数|输入|
|interpolation|插值算法的类型。|支持以下算法：HMPPI_INTER_NN|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|传入的任意一个指针存在空指针。|
|HMPP_STS_STEP_ERR|srcStep、srcPlaneStep、dstStep、dstPlaneStep存在负数或者例如srcStep小于srcVolume.width乘以输入数据类型字节长度，srcPlaneStep小于srcStep乘以srcVolume.height等。|
|HMPP_STS_SIZE_ERR|srcVolume、srcVoi和dstVoi中width、height和depth存在负值。|
|HMPP_STS_INTERPOLAION_ERR|插值算法类型不支持。|
|HMPP_STS_WRONG_INTERSECT_VOI|srcVoi的x/y/z要比srcVolume的width/height/depth大。|
|HMPP_STS_RESIZE_FACTOR_ERR|x/y/zFactor存在负数或者0。|

**示例**

```c
void ResizeExample()
{
    HmpprVolume srcVolume = {2, 2, 2};
    HmpprCuboid srcVoi = {0, 0, 0, 2, 2, 2};
    HmpprCuboid dstVoi = {0, 0, 0, 3, 3, 3};
    int srcStep = 2 * sizeof(uint16_t);
    int dstStep = 3 * sizeof(uint16_t);
    int srcPlaneStep = srcStep * 2;
    int dstPlaneStep = dstStep * 3;
    double xFactor = 1.5;
    double yFactor = 1.5;
    double zFactor = 1.5;
    double xShift = 0.;
    double yShift = 0.;
    double zShift = 0.;
    uint16_t pSrc[] = {1, 2, 3, 4, 5, 6, 7, 8};
    int dstLen = dstVoi.width * dstVoi.height * dstVoi.depth;
    uint16_t *pDst = HMPPS_Malloc_16u(dstLen); 
    HmppResult result = HMPPI_RResize_16u_C1V(pSrc, srcVolume, srcStep, srcPlaneStep, srcVoi, pDst, dstStep, dstPlaneStep,
            dstVoi, xFactor, yFactor, zFactor, xShift, yShift, zShift, HMPPI_INTER_NN);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
        for (int i = 0; i < dstLen; i++){
        printf("%d ", pDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
1 2 2 3 4 4 3 4 4 5 6 6 7 8 8 7 8 8 5 6 6 7 8 8 7 8 8
```

## ResizeNearest

用于调整图像大小，使用最近邻插值算法，属于图像几何变换的一类功能。

在调用ResizeNearest函数之前需要先调用Init函数进行初始化，然后调用ResizeLinear主功能函数，最后调用Release函数释放相关空间。

函数接口声明如下：

- **初始化函数：**

    HmppResult HMPPI_ResizeNearestInit_8u(HmppiSize srcSize, HmppiSize dstSize, HmppiResizePolicy_32f* pSpec);

    HmppResult HMPPI\_ResizeNearestInit\_32f\(HmppiSize srcSize, HmppiSize dstSize, HmppiResizePolicy\_32f \*\*pSpec\);

- **主函数：**

    HmppResult HMPPI_ResizeNearest_8u_C1R(const uint8_t*pSrc, int srcStep, uint8_t* pDst, int dstStep, HmppiPoint dstOffset, HmppiSize dstSize, const HmppiResizeSpec_32f* pSpec);

    HmppResult HMPPI\_ResizeNearest\_32f\_C1R\(const float \*pSrc, int srcStep, float \*pDst, int dstStep, HmppiPoint dstOffset, HmppiSize dstSize, const HmppiResizePolicy\_32f \*pSpec\);

- **释放函数：**

    HmppResult HMPPI\_ResizeNearestRelease\_32f\(HmppiResizePolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|srcSize|源图像块的大小。|正整数|输入|
|pDst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dstOffset|目标图像块的偏移值。|非负整数|输入|
|dstSize|目标图像块的大小。|正整数|输入|
|pSpec|特殊结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_OPERATION|告警，不进行操作。srcSize和dstSize中width、height存在零。|
|HMPP_STS_NULL_PTR_ERR|传入的任意一个指针存在空指针。|
|HMPP_STS_MALLOC_FAILED|pSpec初始化时内存申请失败。|
|HMPP_STS_SIZE_ERR|srcSize和dstSize中width、height存在负值。|
|HMPP_STS_STEP_ERR|srcStep或dstStep不是float字节长度的整数倍。srcStep或dstStep小于srcwidth或dstwidth乘以float字节长度。|
|HMPP_STS_BORDER_ERR|边界算法类型不支持。|
|HMPP_STS_OUT_OF_RANGE_ERR|目标图像块偏移值比Init函数输入的目标图像块的width或height大。|
|HMPP_STS_SIZE_WRN|目标图像块width/height比Init函数输入的目标图像块的width或height大。|

**示例**

```c
#include <stdio.h>
#include <hmpp.h>

void ResizeNearestExample()
{
    HmppiSize srcSize = {3, 3};
    HmppiSize dstSize = {6, 6};
    HmppiPoint dstOffset = {0, 0};
    int srcStep = srcSize.width * sizeof(float);
    int dstStep = dstSize.width * sizeof(float);
    HmppiResizePolicy_32f *pSpec;
    float pSrc[] = {0.0, 255.0, 3.0, 6.0, 255.0, 0.0, 2.0, 1.0, 3.0};
    int dstLen = dstSize.width * dstSize.height;
    float *pDst = HMPPS_Malloc_32f(dstLen);
    HMPPI_ResizeNearestInit_32f(srcSize, dstSize, &pSpec);
    HmppResult result = HMPPI_ResizeNearest_32f_C1R(pSrc, srcStep, pDst, dstStep, dstOffset, dstSize, pSpec);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < dstLen; i++) {
        printf("%f ", pDst[i]);
    }
    printf("\n");
    HMPPI_ResizeNearestRelease_32f(pSpec);
}

int main()
{
    ResizeNearestExample();
    return 0;
}
```

运行结果：

```text
result = 0
0.000000 255.000000 255.000000 3.000000 3.000000 0.000000 6.000000 255.000000 255.000000 0.000000 0.000000 0.000000 6.000000 255.000000 255.000000 0.000000 0.000000 0.000000 2.000000 1.000000 1.000000 3.000000 3.000000 0.000000 2.000000 1.000000 1.000000 3.000000 3.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000 0.000000
```

## ResizeCubic

用于调整图像大小，使用双参数三次插值算法，属于图像几何变换的一类功能。

在调用ResizeCubic函数之前需要先调用Init函数进行初始化，然后调用ResizeCubic主功能函数，最后调用Release函数释放相关空间。

函数接口声明如下：

- **初始化函数：**

    HmppResult HMPPI\_ResizeCubicInit\_32f\(HmppiSize srcSize, HmppiSize dstSize, float B, float C, HmppiResizePolicy\_32f\*\* pSpec\);

- **主函数：**

    HmppResult HMPPI\_ResizeCubic\_32f\_C1R\(const float\* pSrc, int32\_t srcStep, float\* pDst, int32\_t dstStep, HmppiPoint dstOffset, HmppiSize dstSize, HmppiBorderType border, const float\* pBorderValue, const HmppiResizePolicy\_32f\* pSpec\);

- **释放函数：**

    HmppResult HMPPI\_ResizeCubicRelease\_32f\(HmppiResizePolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|srcSize|源图像块的大小。|正整数|输入|
|pDst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dstOffset|目标图像块的偏移值。|非负整数|输入|
|dstSize|目标图像块的大小。|正整数|输入|
|B|三次滤波器的第一个参数。|实数|输入|
|C|三次滤波器的第二个参数。|实数|输入|
|border|边界取值算法。|目前支持如下算法：HMPPI_BORDER_REPL、HMPPI_BORDER_MIRROR、HMPPI_BORDER_MIRROR_R|输入|
|borderValue|边界值。|border为HMPPI_BORDER_CONST时边界值|输入|
|pSpec|特殊结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_OPERATION|告警，不进行操作。srcSize和dstSize中width、height存在零。|
|HMPP_STS_NULL_PTR_ERR|传入的任意一个指针存在空指针。|
|HMPP_STS_MALLOC_FAILED|Policy初始化时内存申请失败。|
|HMPP_STS_SIZE_ERR|srcSize和dstSize中width、height存在负值。或srcSize小于2x2。|
|HMPP_STS_BORDER_ERR|边界算法类型不支持。|
|HMPP_STS_STEP_ERR|srcStep或dstStep不是float字节长度的整数倍。srcStep或dstStep小于srcwidth或dstwidth乘以float字节长度。|
|HMPP_STS_OUT_OF_RANGE_ERR|目标图像块偏移值比Init函数输入的目标图像块的width/height大。|
|HMPP_STS_SIZE_WRN|目标图像块width/height比Init函数输入的目标图像块的width/height大。|

**示例**

```c
void ResizeCubicExample()
{
    HmppiSize srcSize = {4, 4};
    HmppiSize dstSize = {6, 6};
    HmppiPoint dstOffset = {0, 0};
    int srcStep = srcSize.width * sizeof(float);
    int dstStep = dstSize.width * sizeof(float);
    HmppiResizePolicy_32f *pSpec; 
    float pSrc[] = {0.0, 255.0, 3.0, 6.0, 255.0, 0.0, 2.0, 1.0, 3.0,
            0.0, 255.0, 3.0, 6.0, 255.0, 0.0, 2.0};
    int dstLen = dstSize.width * dstSize.height;
    float *pDst = HMPPS_Malloc_32f(dstLen);
    double B = 0.5;
    double C = 0.5;
    HmppResult result = HMPPI_ResizeCubicInit_32f(srcSize, dstSize, B, C, &pSpec);
    HmppiBorderType border = HMPPI_BORDER_REPL;
    float borderValue = pSrc[0];
    result = HMPPI_ResizeCubic_32f_C1R(pSrc, srcStep, pDst, dstStep, dstOffset, dstSize, border, &borderValue, pSpec);
 
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < dstLen; i++){
        printf("%f ", pDst[i]);
    }
    printf("\n");
    HMPPI_ResizeCubicRelease_32f(pSpec);
}
```

运行结果：

```text
result = 0
-4.966425 140.752594 205.351364 60.469444 -8.723660 6.077533 140.751587 141.259216 97.576118 18.680174 -10.837348 3.531568 205.383133 95.673416 7.140303 43.515594 33.608219 0.835248 60.508411 16.424671 41.026234 159.661621 113.710358 0.589375 -9.937934 58.751194 133.184296 140.705246 72.326935 1.272445 3.603872 145.281662 204.080536 56.064888 -13.772015 2.033274 
```

## WarpAffineNearest

用于使用最近邻插值方法对图像执行仿射变换。

在调用WarpAffineNearest函数之前需要先调用Init函数进行初始化，然后调用set函数对目标图像进行初始化，再调用WarpAffineNearest主功能函数，最后调用Release函数释放相关空间。

函数接口声明如下：

- **初始化函数：**

    HmppResult HMPPI\_WarpAffineNearestInit\(HmppiSize srcSize, HmppiSize dstSize, double coeffs\[2\]\[3\], double xCenter, double yCenter, double angle, HmppiWarpDirection direction, HmppiBorderType borderType, double borderValue, int smoothEdge, HmppiWarpPolicy\*\* pSpec\);

    HmppResult HMPPI\_Set\_64f\_C1R\(double value, double \*dst, HmppiSize roiSize\);

    HmppResult HMPPI\_Set\_8u\_C1R\(uint8_t value, uint8_t \*dst, int32_t dstStep, HmppiSize roiSize\);

- **主函数：**

    HmppResult HMPPI_WarpAffineNearest_64f_C1R(const double *src, int srcStep, double*dst, int dstStep, HmppiPoint dstRoiOffset, HmppiSize dstRoiSize, const HmppiWarpPolicy *pSpec);

    HmppResult HMPPI_WarpAffineNearest_8u_C1R(const uint8_t*pSrc, int srcStep, uint8_t pDst, int dstStep, HmppiPoint dstRoiOffset, HmppiSize dstRoiSize, const HmppiWarpPolicy*pSpec);

- **释放函数：**

    HmppResult HMPPI\_WarpAffineNearestRelease\_32f\(HmppiWarpPolicy \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像感兴趣区域的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|srcSize|源图像块的大小。|srcSize.width∈(1,INT_MAX]，srcSize.height∈(1,INT_MAX]|输入|
|dst|指向目标图像感兴趣区域的指针。|非空|输入/输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dstRoiOffset|目标图像块的偏移值。|非负整数|输入|
|dstRoiSize|目标图像块的大小。|dstRoiSize.width∈(1,INT_MAX]，dstRoiSize.height∈(1,INT_MAX]|输入|
|coeffs|仿射变换系数。|非空|输入|
|xCenter|旋转中心x坐标。|在srcSize范围内|输入|
|yCenter|旋转中心y坐标。|在srcSize范围内|输入|
|angle|旋转角度。|(-180, 180)|输入|
|direction|转换方向。|HMPP_WARP_FORWARD：前向变换HMPP_WARP_BACKWARD：后向变换|输入|
|smoothEdge|边缘平滑标志。|0：不支持边缘平滑|输入|
|borderType|边界取值类型。|支持如下算法：HMPPI_BORDER_REPL、HMPPI_BORDER_IN_MEM、HMPPI_BORDER_CONST、HMPPI_BORDER_TRANSP; 或者HMPPI_BORDER_TRANSP和HMPPI_BORDER_IN_MEM_TOP、HMPPI_BORDER_IN_MEM_BOTTOM、HMPPI_BORDER_IN_MEM_LEFT、HMPPI_BORDER_IN_MEM_RIGHT进行或运算后的结果|输入|
|borderValue|边界值。|border为HMPPI_BORDER_CONST时边界值|输入|
|pSpec|特殊结构体的指针。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_OPERATION|告警，不进行操作。srcSize和dstSize中width、height存在零。|
|HMPP_STS_NULL_PTR_ERR|传入的任意一个指针存在空指针。|
|HMPP_STS_MALLOC_FAILED|pSpec初始化时内存申请失败。|
|HMPP_STS_SIZE_ERR|srcSize和dstSize中width、height存在小于等于1的值。|
|HMPP_STS_BORDER_ERR|不支持的边界填充类型|

**示例**

```c
#include <stdio.h>
#include <hmpp.h>

void WarpAffineNearestExample()
{
    double angle = 45;
    int xCenter = 1;
    int yCenter = 1;
    HmppiSize srcSize = {4, 4};
    HmppiSize dstSize = {6, 6};
    double src[] = {808.744309, 27255.923492, 14949.586917, 64065.764086, 59420.504942, 54345.804493, 2688.871018, 29176.134957, 35684.298050, 52137.708563, 15309.428011, 31545.366991, 9154.513170, 54118.967098, 34390.819908, 57073.143379};
    int dstLen = dstSize.width * dstSize.height;
    double *dst = HMPPS_Malloc_64f(dstLen);
 
    HmppiPoint dstOffset = {0, 0};
    HmppiWarpPolicy *pSpec = NULL;
    double pborder = src[0];
    int smoothEdge = 0;
    double coeffs[2][3];
    HmppiWarpDirection direction = HMPP_WARP_FORWARD;
    HmppiBorderType borderType = HMPPI_BORDER_REPL;
    HmppResult result = HMPPI_WarpAffineNearestInit(srcSize, dstSize, coeffs, xCenter, yCenter, angle, direction, borderType, pborder, smoothEdge, &pSpec);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_Set_64f_C1R(0.0, dst, dstSize);
    printf("result = %d\n", result);
 
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPI_WarpAffineNearest_64f_C1R(src, srcSize.width, dst, dstSize.width, dstOffset,  dstSize, pSpec);
    printf("result = %d\n", result);
    
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < dstLen; i++){
        printf("%f ", dst[i]);
    }
    printf("\n");
    HMPPI_WarpAffineNearestRelease_32f(pSpec);
}

int main()
{
    WarpAffineNearestExample();
    return 0;
}
```

运行结果：

```text
result = 0
result = 0
result = 0
27255.923492 14949.586917 2688.871018 31545.366991 31545.366991 57073.143379 808.744309 54345.804493 15309.428011 15309.428011 57073.143379 57073.143379 59420.504942 35684.29805 52137.708563 34390.819908 34390.819908 57073.143379 35684.29805 35684.29805 9154.51317 54118.967098 34390.819908 34390.819908 35684.29805 9154.51317 9154.51317 9154.51317 54118.967098 34390.819908 9154.51317 9154.51317 9154.51317 9154.51317 9154.51317 54118.967098
```

## Mirror

此类函数实现了对图像的几何变换，可以将图像绕指定的轴进行镜像翻转。其中，翻转轴的角度由枚举参数flip决定。

函数的接口声明如下：

- **非原地操作：**

    HmppResult HMPPI\_Mirror\_8u\_C1R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C1R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C3R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_AC4R\(const uint16\_t \*src, int32\_t srcStep, uint16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C1R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C3R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_AC4R\(const int16\_t \*src, int32\_t srcStep, int16\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C1R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C3R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_AC4R\(const int32\_t \*src, int32\_t srcStep, int32\_t \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C1R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C3R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_AC4R\(const float \*src, int32\_t srcStep, float \*dst, int32\_t dstStep, HmppiSize roiSize, HmppiAxis flip\);

- **原地操作：**

    HmppResult HMPPI\_Mirror\_8u\_C1IR\(uint8\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_C3IR\(uint8\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_C4IR\(uint8\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_8u\_AC4IR\(uint8\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C1IR\(uint16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C3IR\(uint16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_C4IR\(uint16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16u\_AC4IR\(uint16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C1IR\(int16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C3IR\(int16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_C4IR\(int16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_16s\_AC4IR\(int16\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C1IR\(int32\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C3IR\(int32\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_C4IR\(int32\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32s\_AC4IR\(int32\_t\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C1IR\(float\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C3IR\(float\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_C4IR\(float\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

    HmppResult HMPPI\_Mirror\_32f\_AC4IR\(float\* srcDst, int32\_t srcDstStep, HmppiSize roiSize, HmppiAxis flip\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|srcDst|指向源和目标缓冲区的指针。|非空|输入/输出|
|srcDstStep|源图像和目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|flip|指定镜像图像翻转的轴。|HMPP_AXS_HORIZONTAL（水平轴翻转）HMPP_AXS_VERTICAL（垂直轴翻转）HMPP_AXS_BOTH（水平+垂直轴翻转）HMPP_AXS_45（45°轴翻转，仅适用非原地操作C1R类型）HMPP_AXS_135（135°轴翻转，仅适用非原地操作C1R类型）|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_NOT_EVEN_STEP_ERR|srcStep或dstStep不能被src或dst所属数据类型的字节长度整除的错误。|
|HMPP_STS_MIRROR_FLIP_ERR|flip为非法值。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 36
#define DST_BUFFER_SIZE_T 36

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
    HmppiSize roi = { 4, 4 };
    uint8_t data[SRC_BUFFER_SIZE_T] = {
        53, 111, 2, 61, 6, 12,
        77, 184, 5, 99, 3,  4,
        41, 233, 1, 27, 5,  6,
        62, 157, 6, 80, 7,  8
    };

    const uint8_t *src = data;
    int32_t srcStep = 6 * sizeof(uint8_t);
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 8 * sizeof(uint8_t);
    HmppResult result1 = HMPPI_Mirror_8u_C1R(src, srcStep, dst, dstStep, roi, HMPP_AXS_45);
    PrintResult(result1, dst, dstStep);

    int32_t srcDstStep = 6 * sizeof(uint8_t);
    HmppResult result2 = HMPPI_Mirror_8u_C1IR(data, srcDstStep, roi, HMPP_AXS_BOTH);
    PrintResult(result2, data, dstStep);
}

int main(void)
{
    TestExample();
    return 0;
}
```

运行结果：

```text
result = 0 
dst = 
 53  77  41  62   0   0   0   0 
111 184 233 157   0   0   0   0 
  2   5   1   6   0   0   0   0 
 61  99  27  80   0   0   0   0 
  0   0   0   0 

result = 0 
dst = 
 80   6 157  62   6  12 
 27   1 233  41   3   4 
 99   5 184  77   5   6 
 61   2 111  53   7   8 
  0   0   0   0   0   0 
  0   0   0   0   0   0
```
