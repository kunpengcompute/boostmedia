# 信号滤波

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Convolve

计算src1向量（长度为src1Len）和src2向量（长度为src2Len）的线性卷积，结果存储到dst向量中。计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281792.png)

该函数调用流程如下：

1. 调用Init初始化HmppsCorrPolicy\_32f结构体。
2. 调用主函数。
3. 最后调用Release释放HmppsCorrPolicy\_32f函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_ConvInit\_32f\(int32\_t src1Len, int32\_t src2Len, HmppCalcMode calcMode, HmppsConvPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_ConvInit\_64f\(int32\_t src1Len, int32\_t src2Len, HmppCalcMode calcMode, HmppsConvPolicy\_64f \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_Convolve\_32f\(const float \*src1, int32\_t src1Len, const float \*src2, int32\_t src2Len, float \*dst, HmppsConvPolicy\_32f \*policy\);

    HmppResult HMPPS\_Convolve\_64f\(const double \*src1, int32\_t src1Len, const double \*src2, int32\_t src2Len, double \*dst, HmppsConvPolicy\_64f \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_ConvRelease\_32f\(HmppsConvPolicy\_32f \*policy\);

    HmppResult HMPPS\_ConvRelease\_64f\(HmppsConvPolicy\_64f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src1Len|第一个源向量长度。|(0, INT_MAX]|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|src2Len|第二个源向量长度。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|algMode|计算使用的算法模型。定义在枚举类型HmppAlgMode中，请参见枚举类型。|枚举体HmppAlgMode元素：HMPP_ALG_AUTOHMPP_ALG_DEFAULTHMPP_ALG_FFT|输入|
|policy（init函数中）|指向内存存储ConvPolicy的指针。|非空|输出|
|policy（主函数中和release函数中）|指向ConvPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当src1Len或src2Len小于或等于0时指示错误。|
|HMPP_STS_MISMATCH|Init函数申请内存的问题规模和主函数中实际计算的问题规模不匹配。|
|HMPP_STS_OVERFLOW_ERR|FFT加速模型的问题规模过大。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化HmppsConvPolicy\_32f规范结构。
- HmppsConvPolicy\_32f结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- src1、src2不能和dst是同一数组，否则可能导致结果错误。
- 使用HMPP\_ALG\_AUTO或HMPP\_ALG\_FFT模式时，当src1Len和src2Len较大时会出现OVERFLOW错误提示。

**示例**

```c
void Convolve_Example()
{
    const int src1Len = 10;
    const int src2Len = 10;
    float src1[src1Len];
    float src2[src2Len];
    float dst[src1Len + src2Len - 1];
    for (int i = 0; i < src1Len; ++i) src1[i] = 1;
    for (int i = 0; i < src2Len; ++i) src2[i] = 1;

    HmppsConvPolicy_32f *policy = NULL;
    HmppResult result = HMPPS_ConvInit_32f(src1Len, src2Len, HMPP_ALG_FFT, &policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Init failed");
        return;
    }
    result = HMPPS_Convolve_32f(src1, src1Len, src2, src2Len, dst, policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Convolve failed");
        return;
    }
    for (int i = 0; i < src1Len + src2Len - 1; ++i) {
        printf("%.2f ", dst[i]);
    }
    HMPPS_ConvRelease_32f(policy);

}
```

运行结果：

```text
1 2 3 4 5 6 7 8 9 10 9 8 7 6 5 4 3 2 1
```

## ConvBiased

计算src1向量（长度为src1Len）和src2向量（长度为src2Len）的线性卷积，以bias作为左偏移量指定src2开始的元素，计算得到的序列dst也以bias作为左偏移量进行移动，空位补0。计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002550041387.png)，![](../../figures/zh-cn_formulaimage_0000002518281632.png)

设src2原数组为x，x的长度为xLen。

![](../../figures/zh-cn_formulaimage_0000002518441522.png)，![](../../figures/zh-cn_formulaimage_0000002518441528.png)

函数接口声明如下：

**主函数：**

HmppResult HMPPS\_ConvBiased\_32f \(const float\* src1, int32\_t src1Len, const float\* src2, int32\_t src2Len, float\* dst, int32\_t dstLen, int32\_t bias\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src1Len|第一个源向量长度。|(0, INT_MAX]|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|src2Len|第二个源向量长度。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstLen|目标向量长度。|(0, INT_MAX]|输入|
|bias|指定卷积起始元素的参数。|[INT_MIN, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当srcLen或dstLen小于或等于0时指示错误。|

**注意**

src1、src2不能和dst是同一数组，否则可能导致结果错误。

**示例**

```c
void Hilbert_Example()
{
    const int src1Len = 5;
    const int src2Len = 4;
    const int dstLen = 10;
    const int bias = 1;
    float src1[src1Len] = {2.1, -1.5, 3.5, 4.2, 1.7};
    float src2[src2Len] = {0.6, 1.3, -1.7, 2.1};
    float dst[dstLen];
    HMPPS_ConvBiased_32f(src1, src1Len, src2, src2Len, dst, dstLen, bias);
    for (int i = 0; i < dstLen; ++i) {
        printf("%.2f ", dst[i]);
    }
}
```

运行结果：

```text
1.26 1.83 -3.42 9.62 0.529999 -4.93 -2.89 0 0 0
```

## FIRSparse

滤波器就是对特定的频率或者特定频率以外的频率进行消除的电路，被广泛用于通信系统和信号处理系统中。从功能角度，数字滤波器对输入离散信号的数字代码进行运算处理，以达到滤除频带外信号的目的。而稀疏滤波是针对滤波器中具有较多的0值，只记录非0抽头的类型滤波器并进行信号处理。

滤波目标向量dst\(y\)通过滤波系数组nzTaps\(b\)与src\(x\)向量中采样信号x做卷积运算得。

计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281680.png)

dlyLine向量支持空值：如果dlyLine为空，该函数使用全零值的延迟线。

FIRSparse函数调用流程如下：

1. 使用对应的FIRSparseInit函数进行初始化。
2. 使用FIRSparse进行滤波操作。
3. 再使用FIRSparseGetDlyLine或者FIRSparseSetDlyLine检索设置延迟线。
4. 最后使用FIRSparseRelease对FIRSparseInit申请的内存进行释放。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_FIRSparseInit\_32f\(const float \*nzTaps, const int32\_t \*nzTapsPos, int32\_t nzTapsLen, const float \*dlyLine, HmppsFIRSparsePolicy\_32f \*\*policy\);

- **获取延迟线操作：**

    HmppResult HMPPS\_FIRSparseGetDlyLine\_32f\(const HmppsFIRSparsePolicy\_32f \*policy, float \*dlyLine\);;

- **设置延迟线操作：**

    HmppResult HMPPS\_FIRSparseSetDlyLine\_32f\(HmppsFIRSparsePolicy\_32f \*policy, const float \*dlyLine\);

- **主函数操作：**

    HmppResult HMPPS\_FIRSparse\_32f\(const float \*src, float \*dst, int32\_t len, HmppsFIRSparsePolicy\_32f \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_FIRSparseRelease\_32f\(HmppsFIRSparsePolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|nzTaps|指向包含非零抽头值的指针。|非空|输入|
|nzTapPos|指向包含非零抽头值位置（从0开始计）的指针。|非空，数组中的数据值需在[0, INT_MAX]范围内，且升序|输入|
|nzTapsLen|数组中具有非零抽头值的元素的数目。|(0, INT_MAX]|输入|
|dlyLine（init和setDly函数中）|指向包含延迟线值的指针。|数组中的元素数为nzTapPos[nzTapsLen - 1]，当dlyLine为NULL时，使用0填充|输入|
|dlyLine（getDly函数中）|指向延迟线值的指针。|非空|输出|
|src|指向源向量的指针。|非空|输入|
|len|源向量中的元素长度。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|policy（init函数中）|指向内存存储FIRSparsePolicy的指针的指针。|非空|输出|
|policy（setDly函数中）|指向FIRSparsePolicy的指针。|非空|输出|
|policy（主函数、getDly函数和release函数中）|指向FIRSparsePolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当nzTapsLen或len小于或等于0时指示错误。|
|HMPP_STS_SPARSE_ERR|指示nzTapPos指针指向的数组内数值不是升序排序，或有负数或重复的数值出现。|
|HMPP_STS_OVERFLOW|指示计算溢出时错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化FIRSparsePolicy规范结构。
- FIRSparsePolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- FIRSparsePolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
- src不能和dst是同一数组，否则可能导致结果错误。

**示例**

```c
#define TPAS_LEN_S 5
#define DLYLINE_LEN_S 8
#define SRC_LEN_S 7
void FIRSparseExample(void)
{
    int32_t nzTapsLen = TPAS_LEN_S;
    float nzTaps[TPAS_LEN_S] = { 0.1, 0.2, 0.3, 0.4, 0.5 };
    int32_t nzTapsPos[TPAS_LEN_S] = { 1, 3, 4, 6, DLYLINE_LEN_S };
    float *dlyLine1 = NULL;
    float dlyLine2[DLYLINE_LEN_S] = { 0.01, 0.02, 0.03, 0.04, 0.05, 0.06, 0.07, 0.08 };
    float dlyDst[DLYLINE_LEN_S] = { 0.0f };
    int32_t len = SRC_LEN_S;
    float src[SRC_LEN_S] = { 0.2107, -40.6842, 17.1776, -15.6654, -2.407, -0.8981, 0.2883 };
    float dst[SRC_LEN_S] = { 0.0f };

    HmppsFIRSparsePolicy_32f *policy = NULL;
    HmppResult result;

    result = HMPPS_FIRSparseInit_32f(nzTaps, nzTapsPos, nzTapsLen, dlyLine1, &policy);
    printf("HMPPS_FIRSparseInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_FIRSparseSetDlyLine_32f(policy, dlyLine2);
    printf("HMPPS_FIRSparseSetDlyLine_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        HMPPS_FIRSparseRelease_32f(policy);
        return;
    }
    result = HMPPS_FIRSparse_32f(src, dst, len, policy);
    printf("HMPPS_FIRSparse_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        HMPPS_FIRSparseRelease_32f(policy);
        return;
    }
    result = HMPPS_FIRSparseGetDlyLine_32f(policy, dlyDst);
    printf("HMPPS_FIRSparseGetDlyLine_32f result = %d\n", result);
    HMPPS_FIRSparseRelease_32f(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i;
    printf("len = %d\ndst =", len);
    for(i = 0; i < len; ++i){
        printf(" %f", dst[i]);
    }
    printf("\ndlyDstLen = %d\ndlyDst =", DLYLINE_LEN_S);
    for(i = 0; i < DLYLINE_LEN_S; ++i){
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_FIRSparseInit_32f result = 0
HMPPS_FIRSparseSetDlyLine_32f result = 0
HMPPS_FIRSparse_32f result = 0
HMPPS_FIRSparseGetDlyLine_32f result = 0
len = 7
dst = 0.083000 0.089070 -4.014420 1.799900 -9.612170 -8.991440 2.024671
dlyDstLen = 8
dlyDst = 0.288300 -0.898100 -2.407000 -15.665400 17.177601 -40.684200 0.210700 0.010000
```

## FIRSR

对源矢量执行单速率FIR滤波，单速率基本FIR滤波器IP核是单速率（输入采样率=输出采样率）有限脉冲响应滤波器。

计算taps向量（长度为tapsLen）和src向量（长度为len）、dlySrc向量（长度为tapsLen-1）的线性点积运算，目标向量存储到dst向量中，延迟线向量存储在dlyDst中。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002550041511.png)。

其中，x\(0\)...x\(numIters\)是源向量；h\(0\)...h\(tapsLen -1\)是FIR滤波器系数。要计算y\(0\)...y\(tapsLen -1\)目标向量，该函数使用延迟线的dly向量。

举例如下：

y\(0\)= h\(tapsLen-1\)\* d\(0\)+h\(tapsLen-2\)\* d\(1\)+...+h\(1\)\*d\(tapsLen -2\)+h\(0\)\*x\(0\)

y\(1\)= h\(tapsLen-1\)\* d\(1\)+h\(tapsLen-2\)\* d\(2\)+... +h\(2\)\*d\(tapsLen -2\)+h\(1\)\*x\(0\)+h\(0\)\*x\(1\)

y\(tapsLen-1\)= h\(tapsLen-1\)\* x\(0\)+...+h\(1\)\* x\(tapsLen-2\)+h\(0\)\* x\(tapsLen-1\)

其中，

- d\(0\)、d\(1\)、d\(2\)和d\(tapsLen -2\)是dlySrc向量的元素。
- dlySrc向量和dlyDst支持空值：
    - 如果dlySrc为空，该函数使用全零值的延迟线。
    - 如果dlyDst为空，该函数不会将任何数据复制到目标延迟线。

该函数调用流程如下：

1. 调用Init初始化FIRPolicy结构体。
2. 调用主函数。
3. 最后调用Release释放FIRPolicy函数所包含内存（16s、16sc使用32f、32fc初始化及释放）。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_FIRSRInit\_32f\(const float \*taps, int32\_t tapsLen, int32\_t len, HmppAlgMode algType, HmppsFIRPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_FIRSRInit\_64f\(const double \*taps, int32\_t tapsLen, int32\_t len, HmppAlgMode algType, HmppsFIRPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_FIRSRInit\_32fc\(const Hmpp32fc \*taps, int32\_t tapsLen, int32\_t len, HmppAlgMode algType, HmppsFIRPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_FIRSRInit\_64fc\(const Hmpp64fc \*taps, int32\_t tapsLen, int32\_t len, HmppAlgMode algType, HmppsFIRPolicy\_64fc \*\*policy\);

    HmppResult HMPPS\_FIRSRInit32f\_32fc\(const float \*taps, int32\_t tapsLen, int32\_t len, HmppAlgMode algType, HmppsFIRPolicy\_32f \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_FIRSR\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len, HmppsFIRPolicy\_32f \*policy, const int16\_t \*dlySrc, int16\_t \*dlyDst\);

    HmppResult HMPPS\_FIRSR\_32f\(const float \*src, float \*dst, int32\_t len, HmppsFIRPolicy\_32f \*policy, const float \*dlySrc, float \*dlyDst\);

    HmppResult HMPPS\_FIRSR\_64f\(const double \*src, double \*dst, int32\_t len, HmppsFIRPolicy\_64f \*policy, const double \*dlySrc, double \*dlyDst\);

    HmppResult HMPPS\_FIRSR\_16sc\(const Hmpp16sc \*src, Hmpp16sc \*dst, int32\_t len, HmppsFIRPolicy\_32fc \*policy, const Hmpp16sc \*dlySrc, Hmpp16sc \*dlyDst\);

    HmppResult HMPPS\_FIRSR\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len, HmppsFIRPolicy\_32fc \*policy, const Hmpp32fc \*dlySrc, Hmpp32fc \*dlyDst\);

    HmppResult HMPPS\_FIRSR\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t len, HmppsFIRPolicy\_64fc \*policy, const Hmpp64fc \*dlySrc, Hmpp64fc \*dlyDst\);

    HmppResult HMPPS\_FIRSR32f\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len, HmppsFIRPolicy\_32f \*policy, const Hmpp32fc \*dlySrc, Hmpp32fc \*dlyDst\);

- **释放内存操作**：

    HmppResult HMPPS\_FIRSRRelease\_32f\(HmppsFIRPolicy\_32f \*policy\);

    HmppResult HMPPS\_FIRSRRelease\_64f\(HmppsFIRPolicy\_64f \*policy\);

    HmppResult HMPPS\_FIRSRRelease\_32fc\(HmppsFIRPolicy\_32fc \*policy\);

    HmppResult HMPPS\_FIRSRRelease\_64fc\(HmppsFIRPolicy\_64fc \*policy\);

    HmppResult HMPPS\_FIRSRRelease32f\_32fc\(HmppsFIRPolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|taps|指向滤波器系数的指针。|非空|输入|
|tapsLen|FIR滤波器系数的长度。|(0, INT_MAX]|输入|
|src|指向源向量的指针。|非空|输入|
|len|源向量中的元素长度。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|algMode|计算使用的算法模型，可能的值是：HMPP_ALG_AUTO、HMPP_ALG_DEFAULT、HMPP_ALG_FFT。|HMPP_ALG_AUTOHMPP_ALG_DEFAULTHMPP_ALG_FFT|输入|
|dlySrc|指向包含源延迟线值向量的指针。|向量可以为NULL，如果为非NULL，则数组长度定义为tapsLen - 1|输入|
|dlyDst|指向包含目标延迟线值向量的指针。|向量可以为NULL，如果为非NULL，则数组长度定义为tapsLen - 1|输出|
|policy（init函数中）|指向内存存储FIRPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向FIRPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当tapsLen或len小于或等于0时指示错误。|
|HMPP_STS_ALG_TYPE_ERR|指示algMode数值不支持时错误。|
|HMPP_STS_OVERFLOW|指示计算溢出时错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化FIRPolicy规范结构。
- FIRPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- FIRPolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
- src不能和dst是同一数组，否则可能导致结果错误。

**示例**

```c
#define TPAS_SIZE_S 8
#define SRC_SIZE_S 7
void FIRSRExample(void)
{
    float taps[TPAS_SIZE_S] = { -0.023, -3.1463, 35.5304, -0.0622, 0.2213, 0.0127, 0.0183, 59.8159 };
    float src[SRC_SIZE_S] = { 0.3065, -1.8737, -58.7455, 155.8426, -0.0294, 4.3917, -195.6575 };
    float dlySrc[TPAS_SIZE_S - 1] = { 0.2107, -40.6842, 17.1776, -15.6654, -2.407, -0.8981, 0.2883 };
    float dst[SRC_SIZE_S] = { 0.0f };
    float dlyDst[TPAS_SIZE_S - 1] = { 0.0f };
    int32_t tapsLen = TPAS_SIZE_S;
    int32_t len = SRC_SIZE_S;
    HmppAlgMode algType = HMPP_ALG_AUTO;
    HmppsFIRPolicy_32f *policy = NULL;
    HmppResult result;

    result = HMPPS_FIRSRInit_32f(taps, tapsLen, len, HMPP_ALG_AUTO, &policy);
    printf("HMPPS_FIRSRInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_FIRSR_32f(src, dst, len, policy, dlySrc, dlyDst);
    printf("HMPPS_FIRSR_32f result = %d\n", result);
    HMPPS_FIRSRRelease_32f(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("dstLen = %d\ndst =", len);
    for(i = 0; i < len; ++i){
        printf(" %f", dst[i]);
    }
    printf("\ndlyDstLen = %d\ndst2 =", TPAS_SIZE_S - 1);
    for(i = 0; i < TPAS_SIZE_S - 1; ++i){
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_FIRSRInit_32f result = 0
HMPPS_FIRSR_32f result = 0
dstLen = 7
dst = -24.064171 -2424.601318 1045.096069 -822.377380 -2721.383057 5486.669434 -15.829129
dlyDstLen = 7
dst2 = 0.306500 -1.873700 -58.745499 155.842606 -0.029400 4.391700 -195.657501
```

## FIRMR

对源矢量执行多速率FIR滤波，多速率是与单速率相对的。

计算taps向量（长度为tapsLen）和src向量（长度为numIters \* downFactor）、dlySrc向量（长度为\(tapsLen + upFactor - 1\) / upFactor）的相关线性点积运算，目标向量存储到dst向量中，延迟线目标向量存储在dlyDst中。

计算公式从FIRSR的计算公式发展而来，如下：

![](../../figures/zh-cn_formulaimage_0000002549921431.png)

![](../../figures/zh-cn_formulaimage_0000002518441596.png)

![](../../figures/zh-cn_formulaimage_0000002518441588.png)

![](../../figures/zh-cn_formulaimage_0000002550041419.png)

其中，

- src是延迟线向量和源向量的组合。
- tapsN是FIR滤波器系数组成的特定序列的二维数组。
- upFactor：滤波信号内部上行采样所依据的因子。即在输入信号的每个样本之间插入upFactor - 1个零。
- upPhase：非零样本在upFactor- 上采样输入信号的长度块内的偏移相位。
- downFactor：FIR响应内部向下采样的因子。即从上采样滤波器响应的每downFactor -上采样滤波器响应的长度输出块中丢弃downFactor-1个输出样本。
- downPhase：非丢弃样本位于上采样滤波器响应块内的偏移相位。

    dlySrc向量和dlyDst支持空值：

    - 如果dlySrc为空，该函数使用全零值的延迟线。
    - 如果dlyDst为空，该函数不会将任何数据复制到目标延迟线。

FIRMR函数调用流程如下：

1. 调用Init初始化FIRPolicy结构体，
2. 调用主函数，
3. 最后调用Release释放FIRPolicy函数所包含内存（16s、16sc使用32f、32fc初始化及释放）。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_FIRMRInit\_32f\(const float \*taps, int32\_t tapsLen, int32\_t upFactor, int32\_t upPhase, int32\_t downFactor, int32\_t downPhase, HmppsFIRPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_FIRMRInit\_64f\(const double \*taps, int32\_t tapsLen, int32\_t upFactor, int32\_t upPhase, int32\_t downFactor, int32\_t downPhase, HmppsFIRPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_FIRMRInit\_32fc\(const Hmpp32fc \*taps, int32\_t tapsLen, int32\_t upFactor, int32\_t upPhase, int32\_t downFactor, int32\_t downPhase, HmppsFIRPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_FIRMRInit\_64fc\(const Hmpp64fc \*taps, int32\_t tapsLen, int32\_t upFactor, int32\_t upPhase, int32\_t downFactor, int32\_t downPhase, HmppsFIRPolicy\_64fc \*\*policy\);

    HmppResult HMPPS\_FIRMRInit32f\_32fc\(const float \*taps, int32\_t tapsLen, int32\_t upFactor, int32\_t upPhase, int32\_t downFactor, int32\_t downPhase, HmppsFIRPolicy\_32f \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_FIRMR\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t numIters, HmppsFIRPolicy\_32f \*policy, const int16\_t \*dlySrc, int16\_t \*dlyDst\);

    HmppResult HMPPS\_FIRMR\_32f\(const float \*src, float \*dst, int32\_t numIters, HmppsFIRPolicy\_32f \*policy, const float \*dlySrc, float \*dlyDst\);

    HmppResult HMPPS\_FIRMR\_64f\(const double \*src, double \*dst, int32\_t numIters, HmppsFIRPolicy\_64f \*policy, const double \*dlySrc, double \*dlyDst\);

    HmppResult HMPPS\_FIRMR\_16sc\(const Hmpp16sc \*src, Hmpp16sc \*dst, int32\_t numIters, HmppsFIRPolicy\_32fc \*policy, const Hmpp16sc \*dlySrc, Hmpp16sc \*dlyDst\);

    HmppResult HMPPS\_FIRMR\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t numIters, HmppsFIRPolicy\_32fc \*policy, const Hmpp32fc \*dlySrc, Hmpp32fc \*dlyDst\);

    HmppResult HMPPS\_FIRMR\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t numIters, HmppsFIRPolicy\_64fc \*policy, const Hmpp64fc \*dlySrc, Hmpp64fc \*dlyDst\);

    HmppResult HMPPS\_FIRMR32f\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t numIters, HmppsFIRPolicy\_32f \*policy, const Hmpp32fc \*dlySrc, Hmpp32fc \*dlyDst\);

- **释放内存操作**：

    HmppResult HMPPS\_FIRMRRelease\_32f\(HmppsFIRPolicy\_32f \*policy\);

    HmppResult HMPPS\_FIRMRRelease\_64f\(HmppsFIRPolicy\_64f \*policy\);

    HmppResult HMPPS\_FIRMRRelease\_32fc\(HmppsFIRPolicy\_32fc \*policy\);

    HmppResult HMPPS\_FIRMRRelease\_64fc\(HmppsFIRPolicy\_64fc \*policy\);

    HmppResult HMPPS\_FIRMRRelease32f\_32fc\(HmppsFIRPolicy\_32f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|taps|指向滤波器系数的指针。|非空|输入|
|tapsLen|FIR滤波器系数的长度。|(0, INT_MAX]|输入|
|upFactor|多速率上采样因子。|(0, INT_MAX]|输入|
|upPhase|上采样信号的相位。|[0, upFactor]|输入|
|downFactor|多速率下采样因子。|(0, INT_MAX]|输入|
|downPhase|下采样信号的相位。|[0, downFactor]|输入|
|src|指向源向量的指针。|非空|输入|
|numIters|与函数过滤的样本数量相关联的迭代次数。源向量的元素(numIters*downFactor)被过滤，结果(numIters* upFactor)的样本被存储在目标数组中。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dlySrc|指向包含源延迟线值向量的指针。|向量可以为NULL，如果为非NULL，则数组长度定义为( tapsLen+ upFactor - 1) / upFactor|输入|
|dlyDst|指向包含目标延迟线值向量的指针。|向量可以为NULL，如果为非NULL，则数组长度定义为( tapsLen+ upFactor - 1) / upFactor|输出|
|policy（init函数中）|指向内存存储FIRPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向FIRPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当tapsLen或numIters小于或等于0时指示错误。|
|HMPP_STS_OVERFLOW_ERR|指示计算溢出时错误。|
|HMPP_STS_FIRMR_FACTOR_ERR|采样因子小于或等于0时错误。|
|HMPP_STS_FIRMR_PHASE_ERR|相位小于0或采样因子小于相位时错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化FIRPolicy规范结构。
- FIRPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- FIRPolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
- src不能和dst是同一数组，否则可能导致结果错误。

**示例**

```c
#define TPAS_SIZE_S 8
#define UP_FACTOR_SIZE_S 5
#define UP_PHASE_SIZE_S 4
#define DOWN_FACTOR_SIZE_S 3
#define DOWN_PHASE_SIZE_S 2
#define NUM_ITERS_SIZE_S 2
void FIRMRExample(void)
{
    int32_t tapsLen = TPAS_SIZE_S;
    int32_t upFactor = UP_FACTOR_SIZE_S;
    int32_t upPhase = UP_PHASE_SIZE_S;
    int32_t downFactor = DOWN_FACTOR_SIZE_S;
    int32_t downPhase = DOWN_PHASE_SIZE_S;
    int32_t numIters = NUM_ITERS_SIZE_S;
    float taps[TPAS_SIZE_S] = { -0.023, -3.1463, 35.5304, -0.0622, 0.2213, 0.0127, 0.0183, 59.8159 };
    float src[NUM_ITERS_SIZE_S * DOWN_FACTOR_SIZE_S] = { 0.3065, -1.8737, -58.7455, 155.8426, -0.0294, 4.3917 };
    float dlySrc[(TPAS_SIZE_S + UP_FACTOR_SIZE_S - 1) / UP_FACTOR_SIZE_S] = { 0.2107, -40.6842};
    float dst[NUM_ITERS_SIZE_S * UP_FACTOR_SIZE_S] = { 0.0f };
    float dlyDst[TPAS_SIZE_S - 1] = { 0.0f };

    HmppsFIRPolicy_32f *policy = NULL;
    HmppResult result;

    result = HMPPS_FIRMRInit_32f(taps, tapsLen, upFactor, upPhase, downFactor, downPhase, &policy);
    printf("HMPPS_FIRMRInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_FIRMR_32f(src, dst, numIters, policy, dlySrc, dlyDst);
    printf("HMPPS_FIRMR_32f result = %d\n", result);
    HMPPS_FIRMRRelease_32f(policy);
    policy = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    int32_t dstLen = numIters * upFactor;
    int32_t dlyLen = (tapsLen + upFactor - 1) / upFactor;
    printf("dstLen = %d\ndst =", dstLen);
    for(i = 0; i < dstLen; ++i){
        printf(" %f", dst[i]);
    }
    printf("\ndlyDstLen = %d\ndst2 =", dlyLen);
    for(i = 0; i < dlyLen; ++i){
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_FIRMRInit_32f result = 0
HMPPS_FIRMR_32f result = 0
dstLen = 10
dst = 2.530557 -1.708862 0.067828 -48.239738 1.327350 3.653970 -491.402649 34.487968 9320.820312 -0.101382
dlyDstLen = 2
dst2 = -0.029400 4.391700
```

## FIRGen

滤波系数生成接口主要是通过对无限滤波器系数加窗转化为有限滤波系数，此次支持低通、高通、带通和带阻滤波系数，支持的窗口为Bartlett窗、Blackman窗、Hamming窗和Hann窗。

滤波系数生成是通过对理想的无限滤波器系数hd\(n\)加窗window\(n\)转化为有限滤波器系数h\(n\)，并计算出具有截止频率freq\(F\)的FIR滤波器的len个数的系数h\(n\)。

主要公式为：![](../../figures/zh-cn_formulaimage_0000002518441814.png)。

其中窗口生成可参考HMPP相关窗口模块。

![](../../figures/zh-cn_formulaimage_0000002518441820.png)。

低通时域采用公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281936.png)

高通时域采用公式如下：

![](../../figures/zh-cn_formulaimage_0000002550041657.png)

![](../../figures/zh-cn_formulaimage_0000002518281932.png)

带通时域采用公式如下：

![](../../figures/zh-cn_formulaimage_0000002518441812.png)

带阻时域采用公式如下：

![](../../figures/zh-cn_formulaimage_0000002518441824.png)

低通时域归一化公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281940.png)

高通时域归一化公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281928.png)

带通时域归一化公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281924.png)

带阻时域归一化公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281944.png)

FIRGen函数调用流程如下：

1. 调用Init初始化buffer缓冲区。
2. 调用主函数。
3. 最后调用Release释放buffer缓冲区。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_FIRGenInit\_64f\(int32\_t tapsLen, uint8\_t \*\*buffer\);

- **主函数操作：**

    HmppResult HMPPS\_FIRGenLowpass\_64f\(double freq, double \*taps, int32\_t tapsLen, HmppWinType winType, HmppBool doNormal, uint8\_t \*buffer\);

    HmppResult HMPPS\_FIRGenHighpass\_64f\(double freq, double \*taps, int32\_t tapsLen, HmppWinType winType, HmppBool doNormal, uint8\_t \*buffer\);

    HmppResult HMPPS\_FIRGenBandpass\_64f\(double freqLow, double freqHigh, double \*taps, int32\_t tapsLen, HmppWinType winType, HmppBool doNormal, uint8\_t \*buffer\);

    HmppResult HMPPS\_FIRGenBandstop\_64f\(double freqLow, double freqHigh, double \*taps, int32\_t tapsLen, HmppWinType winType, HmppBool doNormal, uint8\_t \*buffer\);

- **释放内存操作**：

    HmppResult HMPPS\_FIRGenRelease\(uint8\_t \*buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|tapsLen|有限滤波系数的长度。|[5, INT_MAX]|输入|
|freq|截止频率。|(0, 0.5)|输入|
|freqLow|低截止频率。|(0, 0.5)且小于freqHigh|输入|
|freqHigh|高截止频率。|(0, 0.5)且大于freqLow|输入|
|taps|指向存储计算出的tap值向量的指针。|非空|输出|
|winType|指定计算中使用的窗口类型。|HMPP_WIN_BARTLETT、HMPP_WIN_BLACKMAN、HMPP_WIN_HAMMING或HMPP_WIN_HANN|输入|
|doNormal|指定计算过滤系数是否进行归一化。|HMPP_TRUE或HMPP_FALSE|输入|
|buffer（init函数中）|指向内存存储buffer缓冲区的指针的指针。|非空|输出|
|buffer（主函数中和release函数中）|指向buffer缓冲区的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|指示当tapsLen<5，或者freq<=0、freq>=0.5，或者freqLow、freqHigh<=0、freqLow、freqHigh>=0.5、freqLow>=freqHigh时的错误。|
|HMPP_STS_WIN_TYPE_ERR|指示窗口类型错误。|
|HMPP_STS_OVERFLOW|指示计算溢出时错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化buffer缓冲区规范结构。
- buffer缓冲区初始化成功后，如果执行主函数失败，必须使用Release函数释放buffer缓冲区。

**示例**

```c
#define TPAS_LEN_S 8
void FIRGenBandpassExample(void)
{
    int32_t tapsLen = TPAS_LEN_S;
    double freqLow = 0.05;
    double freqHigh = 0.48;
    HmppWinType winType = HMPP_WIN_BARTLETT;
    HmppBool doNormal = HMPP_TRUE;
    double taps[TPAS_LEN_S] = { 0.0 };

    uint8_t *buffer = NULL;
    HmppResult result;

    result = HMPPS_FIRGenInit_64f(tapsLen, &buffer);
    printf("HMPPS_FIRGenInit_64f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_FIRGenBandpass_64f(freqLow, freqHigh, taps, tapsLen, winType, doNormal, buffer);
    printf("HMPPS_FIRGenBandpass_64f result = %d\n", result);
    HMPPS_FIRGenRelease(buffer);
    buffer = NULL;
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i;
    printf("tapsLen = %d\ntaps =", tapsLen);
    for(i = 0; i < tapsLen; ++i){
        printf(" %f", taps[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_FIRGenInit_64f result = 0
HMPPS_FIRGenBandpass_64f result = 0
tapsLen = 8
taps = -0.000000 0.010000 -0.196259 0.517494 0.517494 -0.196259 0.010000 -0.000000
```

## FIRLMS

本函数最终求解出一个滤波器，求解过程使用均值方差来修正滤波器系数。主函数计算主要两部分：

1. 第一步计算dst的结果，计算公式如下：

    ![](../../figures/zh-cn_formulaimage_0000002518441728.png)

    src\[0\]前会接上dlyLine延迟线，下面举例说明：

    假设src数组长度len为4，taps数组长度tapsLen为3，则延迟线数组dlyLine长度也为tapsLen，dlyIndex范围\[0, tapsLen - 1\]

    - 如果dlyIndex=0，实际src数组可看成dlyLine\[1\]，dlyLine\[2\]，src\[0\]，src\[1\]，src\[2\]，src\[3\]。
    - 如果dlyIndex=1，实际src数组可看成dlyLine\[2\]，dlyLine\[0\]，src\[0\]，src\[1\]，src\[2\]，src\[3\]。
    - 如果dlyIndex=2，实际src数组可看成dlyLine\[0\]，dlyLine\[1\]，src\[0\]，src\[1\]，src\[2\]，src\[3\]。

2. 第二步修正taps数组，即修正滤波器系数，公式如下：

    ![](../../figures/zh-cn_formulaimage_0000002518281820.png)

    ![](../../figures/zh-cn_formulaimage_0000002550041585.png)

    实际计算过程，每计算出一个dst\[n\]的值，就用该dst\[n\]，更新一次taps数组。

该函数调用流程如下：

1. 调用Init初始化FIRLMSPolicy结构体。
2. 调用主函数。
3. 调用GetTaps获取修正后的滤波器数组。
4. 最后调用Release释放FIRPolicy函数所包含内存（16s、16sc使用32f、32fc初始化及释放）。

函数接口声明如下：

- **获取滤波器数组：**

    HmppResult HMPPS\_FIRLMSGetTaps\_32f\(const HmppsFIRLMSPolicy\_32f\* policy, float \*outTaps\);

    HmppResult HMPPS\_FIRLMSGetTaps32f\_16s\(const HmppsFIRLMSPolicy32f\_16s\* policy, float \*outTaps\);

- **设置延迟线数组和偏移：**

    HmppResult HMPPS\_FIRLMSSetDlyLine\_32f\(HmppsFIRLMSPolicy\_32f\* policy, const float \*dlyLine, int32\_t dlyLineIndex\);

    HmppResult HMPPS\_FIRLMSSetDlyLine32f\_16s\(HmppsFIRLMSPolicy32f\_16s\* policy, const int16\_t \*dlyLine, int32\_t dlyLineIndex\);

- **获取延迟线数组和偏移：**

    HmppResult HMPPS\_FIRLMSGetDlyLine\_32f\(const HmppsFIRLMSPolicy\_32f\* policy, float \*dlyLine, int32\_t \*dlyLineIndex\);

    HmppResult HMPPS\_FIRLMSGetDlyLine32f\_16s\(const HmppsFIRLMSPolicy32f\_16s\* policy, int16\_t \*dlyLine, int32\_t \*dlyLineIndex\);

- **初始化操作：**

    HmppResult HMPPS\_FIRLMSInit\_32f\(HmppsFIRLMSPolicy\_32f \*\*policy, const float \*taps, int32\_t tapsLen, const float \*dlyLine, int dlyLineIndex\);

    HmppResult HMPPS\_FIRLMSInit32f\_16s\(HmppsFIRLMSPolicy32f\_16s \*\*policy, const float \*taps, int32\_t tapsLen, const int16\_t \*dlyLine, int dlyLineIndex\);

- **主函数操作：**

    HmppResult HMPPS\_FIRLMS\_32f\(HmppsFIRLMSPolicy\_32f \*policy, const float \*src, const float \*ref, float \*dst, int32\_t len, float mu\);

    HmppResult HMPPS\_FIRLMS32f\_16s\(HmppsFIRLMSPolicy32f\_16s \*policy, const int16\_t \*src, const int16\_t \*ref, int16\_t \*dst, int32\_t len, float mu\);

- **释放内存操作**：

    HmppResult HMPPS\_FIRLMSRelease\_32f\(HmppsFIRLMSPolicy\_32f \*policy\);

    HmppResult HMPPS\_FIRLMSRelease32f\_16s\(HmppsFIRLMSPolicy32f\_16s \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|ref|指向参考向量的指针。|非空|输入|
|dst|指向目标向量指针。|非空|输出|
|len|源向量、参考向量、目标向量长度。|(0, INT_MAX]|输入|
|taps|指向滤波器向量的指针。|Init函数入参中，taps可以为NULL|输入|
|tapsLen|滤波器向量长度。|(0, INT_MAX]|输入|
|dlyLine|指向延迟线向量指针。|SetDIyLine入参中，不能为NULLInit函数入参中，可以为NULL|输入|
|dlyLineIndex|延迟线起始元素的偏移量。|[INT_MIN, INT_MAX]实际会映射到[0,tapsLen)|输入|
|policy（init函数中）|指向内存存储FIRLMSPolicy的指针的指针。|非空|输出|
|policy（主函数中和release函数中）|指向DFTPolicy结构体的指针。|非空|输入/输出|
|mu|滤波器适配系数。|float数据，需要用户根据实际数据进行调节|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当len小于或等于0时指示错误。|
|HMPP_STS_FIR_LEN_ERR|当tapsLen小于或等于0时指示错误。|
|HMPPS_STS_POLICY_STATE_ERR|policy结构体状态标识不对。|
|HMPP_STS_MALLOC_FAILED|函数中进行内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化FIRLMS规范结构。
- FIRLMSPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- FIRLMSPolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
- 主函数调用成功后，需要调用GetTaps函数从policy中获取最新的滤波器数组
- src不能和dst是同一数组，否则可能导致结果错误。

**示例**

```c
void FIRLMSExample(void)
{
    const int tapsLen = 7;
    const int len = 32;
    float src[len] = { 0.15275501, 0.143135, 0.31686401, 0.26976699, 0.72030699, 0.50592899, 0.486655, 0.706716, 0.68356198, 0.77510101, 
                              0.94214898, 1.05799, 1.29482, 1.14037, 1.43979, 1.44664, 1.55493, 1.57329, 1.68367, 1.70401, 1.85473, 1.86356, 
                              2.2176099, 2.1915801, 2.24248, 2.1015699, 2.25688, 2.26349, 2.5790999, 2.6948299, 2.4193299, 2.69561 };
    float ref[len];
    float dst[len];
    float taps[tapsLen];
    float mu = 0.0005;
    for (int i = 0; i < tapsLen; ++i) {
        taps[i] = 1.0 / tapsLen;
    }
    HmppsFIRLMSPolicy_32f *policy = NULL;

    HmppResult result = HMPPS_FIRLMSInit_32f(&policy, taps, tapsLen, NULL, 0);
    if (result != HMPP_STS_NO_ERR) {
        printf("HMPPS_FIRLMSInit_32f result = %d\n", result);
        return;
    }
    result = HMPPS_FIRLMS_32f(policy, src, ref, dst, len, mu);
    if (result != HMPP_STS_NO_ERR) {
        printf("HMPPS_FIRLMS_32f result = %d\n", result);
        return;
    }
    printf("Dst: ");
    for (int i = 0; i < len; ++i) {
        printf("%f ", dst[i]);
    }
    printf("\n");
    result = HMPPS_FIRLMSGetTaps_32f(policy, taps);
    if (result != HMPP_STS_NO_ERR) {
        printf("HMPPS_FIRLMSGetTaps_32f result = %d\n", result);
        return;
    }
    printf("Taps: ");
    for (int i = 0; i < tapsLen; ++i) {
        printf("%f ", taps[i]);
    }
    printf("\n");
    HMPPS_FIRLMSRelease_32f(policy);
}
```

运行结果：

```text
Dst: 0.021822 0.042269 0.087532 0.126056 0.228895 0.297082 0.365800 0.443564 0.519442 0.570626 0.661690 0.704063 0.808335 0.893767 0.985927 1.080792 1.174086 1.177286 1.238281 1.184714 1.245834 1.165491 1.215122 1.248639 1.280253 1.287248 1.300077 1.297933 1.313811 1.096912 1.071277 0.833319
Taps: 0.031206 0.038017 0.041954 0.050642 0.050239 0.060425 0.061261
```

## FilterMedian

计算源向量中元素的中值。

中值滤波器是一种使用掩码（mask）的非线性排序滤波器，使用相邻区间内的中值来替换源向量中的元素。中值滤波器常用于图像和信号的处理中，具有滤除脉冲噪声的作用。通常掩码长度被设为奇数，奇数长度的掩码可以使计算实现更简洁，同时保证输出信号偏移较小。HMPP函数库还实现了延迟线的特性，计算时在源向量左端补充延迟线dlySrc数组数据，dlySrc为NULL时补充maskSize-1个src\[0\]。

函数接口声明如下：

- **初始化操作**：

    HmppResult HMPPS\_FilterMedianInit\(int32\_t maskSize, HmppDataType dataType, uint8\_t \*\*buffer\);

- **主函数操作：**

    HmppResult HMPPS\_FilterMedian\_8u\(const uint8\_t \*src, uint8\_t \*dst, int32\_t len, int32\_t maskSize, const uint8\_t \*dlySrc, uint8\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len, int32\_t maskSize, const int16\_t \*dlySrc, int16\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_32s\(const int32\_t \*src, int32\_t \*dst, int32\_t len, int32\_t maskSize, const int32\_t \*dlySrc, int32\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_32f\(const float \*src, float \*dst, int32\_t len, int32\_t maskSize, const float \*dlySrc, float \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_64f\(const double \*src, double \*dst, int32\_t len, int32\_t maskSize, const double \*dlySrc, double \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_8u\_I\(uint8\_t \*srcDst, int32\_t len, int32\_t maskSize, const uint8\_t \*dlySrc, uint8\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_16s\_I\(int16\_t \*srcDst, int32\_t len, int32\_t maskSize, const int16\_t \*dlySrc, int16\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_32s\_I\(int32\_t \*srcDst, int32\_t len, int32\_t maskSize, const int32\_t \*dlySrc, int32\_t \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_32f\_I\(float \*srcDst, int32\_t len, int32\_t maskSize, const float \*dlySrc, float \*dlyDst, uint8\_t \*buffer\);

    HmppResult HMPPS\_FilterMedian\_64f\_I\(double \*srcDst, int32\_t len, int32\_t maskSize, const double \*dlySrc, double \*dlyDst, uint8\_t \*buffer\);

- **释放内存操作：**

    HmppResult HMPPS\_FilterMedianRelease\(uint8\_t \*buffer\)；

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|maskSize|中值掩码大小，若为偶数则减1后作为奇数用于中值滤波。|(0, len]|输入|
|dlySrc|源延迟线数据地址。|无|输入|
|dlyDst|目的延迟线数据地址。|无|输出|
|buffer|工作缓冲区地址。|非空|输入|
|bufferSize|工作缓冲区大小。|非空|输出|
|dataType|中值滤波支持的数据类型： 8u，16s，32s，32f，64f。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 警告：返回HMPP\_STS\_EVEN\_MEDIAN\_MASK\_SIZE。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst、src、dst、buffer这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_MASK_SIZE_ERR|中值掩码小于或等于0或者大于len。|
|HMPP_STS_EVEN_MEDIAN_MASK_SIZE|告警，中值掩码为偶数。|

**注意**

掩码长度通常是奇数，用户可以将其设为偶数，但最终会通过减法将其转变为奇数。

**示例**

```c
#define DATA_SIZE 10
#define MASK_SIZE 3
void FilterMedianExample(void)
{

    int16_t src[DATA_SIZE] = { 1, 3, 3, 1, 5, 6, 3, 8, 19, 10 };
    int16_t dst[DATA_SIZE] = { 0 };
    uint8_t *buffer = NULL;
    int16_t dlySrc[MASK_SIZE-1] = { 2, 4 };
    int16_t dlyDst[MASK_SIZE-1] = { 0 };

    HmppResult retVal;
    retVal = HMPPS_FilterMedianInit(MASK_SIZE, HMPP16S, &buffer);
    if (retVal != HMPP_STS_EVEN_MEDIAN_MASK_SIZE && retVal != HMPP_STS_NO_ERR) {
        return;
    }
 
    retVal = HMPPS_FilterMedian_16s(src, dst, DATA_SIZE, MASK_SIZE, dlySrc, dlyDst, buffer);
    printf("result = %d\n", retVal);
    if (retVal != HMPP_STS_NO_ERR) {
        return;
    }
    HMPPS_FilterMedianRelease(buffer);
    int32_t i;
    printf("dst =");
    for (i = 0; i < DATA_SIZE; ++i) {
        printf(" %d", dst[i]);
    }
    printf("\ndlyDst =");
    for (i = 0; i < MASK_SIZE - 1; ++i) {
        printf(" %d", dlyDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 2 3 3 3 3 5 5 6 8 10
dlyDst = 19 10
```

## IIR

初始化一个无限脉冲响应（IIR）滤波器并进行滤波。

HMPP支持两种IIR滤波器：任意阶滤波器和BiQuad滤波器。输入向量X\[n\]将被存储在src中，输出向量y\[n\]存储在dst中。

如图1所示描述了任意阶滤波器的结构。

**图 1**  任意阶滤波器结构  
![](../../figures/任意阶滤波器结构.png "任意阶滤波器结构")

其中X\[n\]为输入，y\[n\]为输出，order为滤波器阶数，a、b为滤波器系数，计算公式：

![](../../figures/zh-cn_formulaimage_0000002518441792.png)

其中![](../../figures/zh-cn_formulaimage_0000002550041633.png)，![](../../figures/zh-cn_formulaimage_0000002518281864.png)，滤波器初始系数向量长度为order，排列顺序为：![](../../figures/zh-cn_formulaimage_0000002549921633.png)。

其中src和dst允许是同一个数组，支持原地操作。IIR接收长度为order延迟线，延迟线运行为空，若为空，会将延迟线0填充。滤波计算完成后，延迟线将会被更新。

BiQuad滤波器是二阶IIR滤波器的级联，如图2所示描述了k个二阶滤波器组成的BiQuad滤波器。

**图 2**  k个二阶滤波器组成的BiQuad滤波器  
![](../../figures/k个二阶滤波器组成的BiQuad滤波器.png "k个二阶滤波器组成的BiQuad滤波器")

HMPP仅支持Direct Form 2（DF2）形式的延迟线。HMPPS\_IIRGetDlyLine和HMPPS\_IIRSetDlyLine函数返回/设置的延迟线也是DF2形式的。相较于Direct Form 1（DF1）形式的延迟线，存储DF2形式的延迟线元素的个数少一半。DF2形式的延迟线可以由DF1形式延迟线计算得到的信息，如果需要DF1形式的延迟线，请拷贝src数组和计算完成后dst数组的后order个元素作为DF1形式的延迟线。

IIR函数调用流程如下：

1. 使用对应的IIRInit函数进行初始化。
2. 使用IIR进行滤波操作。
3. 再使用IIRGetDlyLine或者IIRSetDlyLine检索设置延迟线。
4. 最后再使用IIRRelease对IIRInit申请的内存进行释放。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_IIRInit\_32f\(HmppsIIRPolicy\_32f \*\*policy, const float \*taps, int order, const float \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_64f\(HmppsIIRPolicy\_64f \*\*policy, const double \*taps, int order, const double \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_32fc\(HmppsIIRPolicy\_32fc \*\*policy, const Hmpp32fc \*taps, int order, const Hmpp32fc \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_64fc\(HmppsIIRPolicy\_64fc \*\*policy, const Hmpp64fc \*taps, int order, const Hmpp64fc \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_BiQuad\_32f\(HmppsIIRPolicy\_32f \*\*policy, const float \*taps, int numBq, const float \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_BiQuad\_64f\(HmppsIIRPolicy\_64f \*\*policy, const double \*taps, int numBq, const double \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_BiQuad\_32fc\(HmppsIIRPolicy\_32fc \*\*policy, const Hmpp32fc \*taps, int numBq, const Hmpp32fc \*dlyLine\);

    HmppResult HMPPS\_IIRInit\_BiQuad\_64fc\(HmppsIIRPolicy\_64fc \*\*policy, const Hmpp64fc \*taps, int numBq, const Hmpp64fc \*dlyLine\);

- **获取延迟线操作：**

    HmppResult HMPPS\_IIRGetDlyLine\_32f\(const HmppsIIRPolicy\_32f \*policy, float \*dlyLine\);

    HmppResult HMPPS\_IIRGetDlyLine\_64f\(const HmppsIIRPolicy\_64f \*policy, double \*dlyLine\);

    HmppResult HMPPS\_IIRGetDlyLine\_32fc\(const HmppsIIRPolicy\_32fc \*policy, Hmpp32fc \*dlyLine\);

    HmppResult HMPPS\_IIRGetDlyLine\_64fc\(const HmppsIIRPolicy\_64fc \*policy, Hmpp64fc \*dlyLine\);

- **设置延迟线操作：**

    HmppResult HMPPS\_IIRSetDlyLine\_32f\(HmppsIIRPolicy\_32f \*policy, const float \*dlyLine\);

    HmppResult HMPPS\_IIRSetDlyLine\_64f\(HmppsIIRPolicy\_64f \*policy, const double \*dlyLine\);

    HmppResult HMPPS\_IIRSetDlyLine\_32fc\(HmppsIIRPolicy\_32fc \*policy, const Hmpp32fc \*dlyLine\);

    HmppResult HMPPS\_IIRSetDlyLine\_64fc\(HmppsIIRPolicy\_64fc \*policy, const Hmpp64fc \*dlyLine\);

- **滤波操作：**

    HmppResult HMPPS\_IIR\_32f\(const float \*src, float \*dst, int len, HmppsIIRPolicy\_32f \*policy\);

    HmppResult HMPPS\_IIR\_64f\(const double \*src, double \*dst, int len, HmppsIIRPolicy\_64f \*policy\);

    HmppResult HMPPS\_IIR\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int len, HmppsIIRPolicy\_32fc \*policy\);

    HmppResult HMPPS\_IIR\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int len, HmppsIIRPolicy\_64fc \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_IIRRelease\_32f\(HmppsIIRPolicy\_32f \*policy\);

    HmppResult HMPPS\_IIRRelease\_64f\(HmppsIIRPolicy\_64f \*policy\);

    HmppResult HMPPS\_IIRRelease\_32fc\(HmppsIIRPolicy\_32fc \*policy\);

    HmppResult HMPPS\_IIRRelease\_64fc\(HmppsIIRPolicy\_64fc \*policy\);

    HmppResult HMPPS\_IIRRelease\_BiQuad\_32f\(HmppsIIRPolicy\_32f \*policy\);

    HmppResult HMPPS\_IIRRelease\_BiQuad\_64f\(HmppsIIRPolicy\_64f \*policy\);

    HmppResult HMPPS\_IIRRelease\_BiQuad\_32fc\(HmppsIIRPolicy\_32fc \*policy\);

    HmppResult HMPPS\_IIRRelease\_BiQuad\_64fc\(HmppsIIRPolicy\_64fc \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|taps|指向滤波器系数的指针。|非空|输入|
|order|IIR滤波器的阶数。|(0, INT_MAX]|输入|
|src|指向源向量指针|非空|输入|
|dst|指向目标向量的指针。|非空|输出|
|len|源向量和目标向量长度|(0, INT_MAX]|输入|
|numBq|BiQuad滤波器的级数。|(0, INT_MAX]|输入|
|dlyLine（init和setDly函数中）|指向包含延迟线向量的指针。|向量可以为NULL，如果为NULL，则用全零填充延迟线。|输入|
|dlyLine（getDly函数中）|指向延迟线值的指针。|非空|输出|
|policy（init函数中）|指向内存存储IIRPolicy的指针的指针。|非空|输出|
|policy（setDly函数中）|指向IIRPolicy结构体的指针。|非空|输入|
|policy（滤波函数中和release函数中）|指向IIRPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当len小于或等于0时指示错误。|
|HMPP_STS_DIV_BY_ZERO_ERR|除0错误，![](../../figures/zh-cn_image_0000002550041645.png)不能为0。|
|HMPP_STS_CONTEXT_MATCH_ERR|表示Policy状态不正确时出错（使用了错误的Init函数）。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- 调用该接口计算之前，必须调用Init接口初始化IIRPolicy规范结构。
>- IIRPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
>- IIRPolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
>- src和dst支持是同一数组，即原地（inplace）操作。
>- IIR和IIR BiQuad的初始化函数不同，其他操作共用一套函数。

**IIR示例**

```c
#define ORDER 4
#define TAPS_LEN ( (ORDER + 1) * 2)
#define SRC_LEN 10
#define DLY_LEN ORDER
 
void IIRExample(void) {
    float taps[TAPS_LEN] = {0.0390, -0.1560, 0.2340, -0.1560, 0.0390, 1.0000, 0.9532, 0.7746, 0.2338, 0.0366};
    float src[SRC_LEN] = {186, 431, 689, 206, 716, 90, 695, -153};
    float dlySrc[DLY_LEN] = {123, 312, 781, 249};
    float dlyDst[DLY_LEN];
    float dst[SRC_LEN];
    HmppsIIRPolicy_32f *policy = NULL;
    HmppResult result;
 
    result = HMPPS_IIRInit_32f(&policy, taps, ORDER, dlySrc);
    printf("HMPPS_IIRInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_IIR_32f(src, dst, SRC_LEN, policy);
    printf("HMPPS_IIR_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("dstLen = %d\ndst =", SRC_LEN);
    for (i = 0; i < SRC_LEN; ++i) {
        printf(" %f", dst[i]);
    }
    HMPPS_IIRGetDlyLine_32f(policy, dlyDst);
    printf("\ndlyDstLen = %d\ndlyDst =", DLY_LEN);
    for (i = 0; i < DLY_LEN; ++i) {
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
    HMPPS_IIRRelease_32f(policy);
    policy = NULL;
}
```

运行结果：

```text
HMPPS_IIRInit_32f result = 0
HMPPS_IIR_32f result = 0
dstLen = 10
dst = 130.253998 175.634888 515.849060 -436.819489 68.000931 -4.148865 209.873474 -393.737732 411.605988 -276.982147
dlyDstLen = 4
dlyDst = 80.536873 126.760712 49.693642 10.137547
```

**IIRBiQuad示例**

```c
#define NUM_BQ 2
#define TAPS_LEN (NUM_BQ * 6)
#define SRC_LEN 8
#define DLY_LEN (NUM_BQ * 2)

void BiQuadExample(void) {
    float taps[TAPS_LEN] = {0.1980326, -0.39606521, 0.1980326, 1., 0.40919919, 0.2013296, 0.197444, -0.394888,  0.197444, 1., 0.41195841, 0.20173442};
    float src[SRC_LEN] = {186, 431, 689, 206, 716, 90, 695, -153};
    float dlySrc[DLY_LEN] = {694.24421981, -100.19274855, -681.3183156 , -324.66495805};
    float dlyDst[DLY_LEN];
    float dst[SRC_LEN];
    HmppsIIRPolicy_32f *policy = NULL;
    HmppResult result;
 
    result = HMPPS_IIRInit_BiQuad_32f(&policy, taps, NUM_BQ, dlySrc);
    printf("HMPPS_IIRInit_BiQuad_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_IIR_32f(src, dst, SRC_LEN, policy);
    printf("HMPPS_IIR_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("dstLen = %d\ndst =", SRC_LEN);
    for (i = 0; i < SRC_LEN; ++i) {
        printf(" %f", dst[i]);
    }
    HMPPS_IIRGetDlyLine_32f(policy, dlyDst);
    printf("\ndlyDstLen = %d\ndlyDst =", DLY_LEN);
    for (i = 0; i < DLY_LEN; ++i) {
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
    HMPPS_IIRRelease_32f(policy);
    policy = NULL;
}
```

运行结果：

```text
HMPPS_IIRInit_BiQuad_32f result = 0
HMPPS_IIR_32f result = 0
dstLen = 8
dst = -536.971313 -468.691376 601.606384 -250.059616 58.091927 -136.327194 271.481598 -341.952698
dlyDstLen = 4
dlyDst = 280.199768 41.936638 291.383179 -1.857876
```

## IIRGen

生成低通或高通IIR滤波器。

HMPP支持生成Butterworth和Chebyshev1类型的滤波器。Butterworth滤波器的特点是在通带内平坦，Chebyshev1滤波器的特点在通带内有波纹波动。

函数接口声明如下：

HmppResult HMPPS\_IIRGenLowpass\_64f\(double rFreq, double ripple, int order, double \*pTaps, HmppsIIRFilterType filterType\);

HmppResult HMPPS\_IIRGenHighpass\_64f\(double rFreq, double ripple, int order, double \*pTaps, HmppsIIRFilterType filterType\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|rFreq|截止频率。|(0,0.5)|输入|
|ripple|当滤波器类型为HmppChebyshev1时指定波纹。|(0, INT_MAX]，滤波器类型为HmppChebyshev1时(0, 28]|输入|
|order|生成滤波器的阶数。|[1, 12]|输入|
|filterType|生成滤波器的类型。|HmppButterworth：表示滤波器类型为Butterworth滤波器HmppChebyshev1：表示滤波器类型为Chebyshev1滤波器|输入|
|pTaps|生成滤波器的系数。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空时错误。|
|HMPP_STS_GEN_ORDER_ERR|生成的order小于1或大于12。|
|HMPP_STS_FILTER_FREQUENCY_ERR|截止频率不在(0, 0.5)范围内。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- 滤波器类型仅支持HmppButterworth和HmppChebyshev1，请勿使用其他值，否则结果可能出错。
>- IIR滤波器的收敛性对系数a的量化误差敏感，因此将生成double类型的系数用于单精度计算时候需要验证滤波器是否收敛。

**示例**

以下是用HMPP分别生成一个高通滤波器和一个低通滤波器的示例代码。

高通滤波器的类型为Butterworth，阶数为9，采样频率为1000Hz，截止频率为400Hz。与MatLab等价的代码：**\[b,a\] = butter\(9,400/500,'high'\);**。

低通滤波器的类型为Chebyshev1，阶数为9，采样频率为1000Hz，截止频率为400Hz，通带内的波纹为0.4dB。与MatLab等价的代码：**\[b,a\] = cheby1\(9, 0.4, 400/500\);**。

```c
#define ORDER 9
#define TAPS_LEN ((ORDER + 1) * 2)
void IIRGenExample() {
    HmppResult ret;
    double taps[TAPS_LEN];
    ret = HMPPS_IIRGenHighpass_64f(400.0 / 1000.0, 0, ORDER, taps, HmppButterworth);
    printf("IIRGenHighpass ret=%d\n", ret);
    for (int i = 0; i < TAPS_LEN; ++i) {
        printf("%lf ", taps[i]);
    }
    printf("\n");
 
    ret = HMPPS_IIRGenLowpass_64f(400.0 / 1000.0, 0.4, ORDER, taps, HmppChebyshev1);
    printf("IIRGenLowpass ret=%d\n", ret);
    for (int i = 0; i < TAPS_LEN; ++i) {
        printf("%lf ", taps[i]);
    }
    printf("\n");
}
```

运行结果：

```text
IIRGenHighpass ret=0
0.000006 -0.000057 0.000229 -0.000534 0.000800 -0.000800 0.000534 -0.000229 0.000057 -0.000006 1.000000 5.386221 13.378550 19.961682 19.623982 13.137028 5.973215 1.775180 0.312381 0.024765 
IIRGenLowpass ret=0
0.080020 0.720182 2.880728 6.721698 10.082547 10.082547 6.721698 2.880728 0.720182 0.080020 1.000000 4.135535 8.510626 10.746179 9.121897 5.230419 1.958336 0.349120 -0.039936 -0.041825
```

## IIRIIR

初始化一个无限脉冲响应（IIR）滤波器并对输入进行零相位数字滤波（zero-phase digital filtering）。

滤波过程分为正向滤波和反向滤波。x\(n\)将被存储在src中，滤波输出y\(n\)存储在dst中。下面简单解释零相位数字滤波器的原理。

数字信号的时域表示：

![](../../figures/zh-cn_formulaimage_0000002549921423.png)

![](../../figures/zh-cn_formulaimage_0000002518281720.png)

![](../../figures/zh-cn_formulaimage_0000002518441614.png)

![](../../figures/zh-cn_formulaimage_0000002550041475.png)

其中，h\(n\)为对应数字滤波器的冲激响应，y<sub>4</sub>\(n\)去掉首尾L个信号点即为对应输出序列。

频域表示：

![](../../figures/zh-cn_formulaimage_0000002549921461.png)

![](../../figures/zh-cn_formulaimage_0000002549921457.png)

![](../../figures/zh-cn_formulaimage_0000002550041423.png)

![](../../figures/zh-cn_formulaimage_0000002518441584.png)

由此可以推出：

![](../../figures/zh-cn_formulaimage_0000002550041443.png)

即输出序列与输入序列之间没有相位偏移。

正向滤波器和反向滤波器的阶数由order指定。系数向量长度为2\*\(order+1\)，元素排列如下：

![](../../figures/zh-cn_image_0000002549921473.png)

IIRIIRInit接收长度为order的延迟线向量，允许为空。若为空，将会用生成一个初始条件填充延迟线。

IIRIIR函数调用流程如下：

1. 使用对应的IIRIIRInit函数进行初始化。
2. 使用IIRIIR进行滤波操作。
3. 再使用IIRIIRGetDlyLine或者IIRIIRSetDlyLine检索设置延迟线。
4. 最后再使用IIRIIRRelease对IIRIIRInit申请的内存进行释放。

函数接口声明如下：

- **初始化操作：**

HmppResult HMPPS\_IIRIIRInit\_32f\(HmppsIIRPolicy\_32f \*\*policy, const float \*taps, int order, const float \*dlyLine\);

HmppResult HMPPS\_IIRIIRInit\_64f\(HmppsIIRPolicy\_64f \*\*policy, const double \*taps, int order, const double \*dlyLine\);

- **获取延迟线操作：**

HmppResult HMPPS\_IIRIIRGetDlyLine\_32f\(const HmppsIIRPolicy\_32f \*policy, float \*dlyLine\);

HmppResult HMPPS\_IIRIIRGetDlyLine\_64f\(const HmppsIIRPolicy\_64f \*policy, double \*dlyLine\);

- **设置延迟线操作：**

HmppResult HMPPS\_IIRIIRSetDlyLine\_32f\(HmppsIIRPolicy\_32f \*policy, const float \*dlyLine\);

HmppResult HMPPS\_IIRIIRSetDlyLine\_64f\(HmppsIIRPolicy\_64f \*policy, const double \*dlyLine\);

- **滤波操作：**

HmppResult HMPPS\_IIRIIR\_32f\(const float \*src, float \*dst, int len, HmppsIIRPolicy\_32f \*policy\);

HmppResult HMPPS\_IIRIIR\_64f\(const double \*src, double \*dst, int len, HmppsIIRPolicy\_64f \*policy\);

- **释放内存操作：**

HmppResult HMPPS\_IIRIIRRelease\_32f\(HmppsIIRPolicy\_32f \*policy\);

HmppResult HMPPS\_IIRIIRRelease\_64f\(HmppsIIRPolicy\_64f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|taps|指向滤波器系数的指针。|非空|输入|
|order|IIR滤波器的阶数。|(0, INT_MAX]|输入|
|src|指向源向量指针。|非空|输入|
|dst|指向目标向量的指针。|非空|输出|
|len|源向量和目标向量长度。|[3 * order, INT_MAX]|输入|
|dlyLine（init和setDly函数中）|指向包含延迟线向量的指针。|向量可以为NULL，如果为NULL，则用初始条件填充延迟线。|输入|
|dlyLine（getDly函数中）|指向延迟线值的指针。|非空|输出|
|policy（init函数中）|指向内存存储IIRIIRPolicy的指针的指针。|非空|输出|
|policy（setDly函数中）|指向IIRIIRPolicy结构体的指针。|非空|输入|
|policy（滤波函数中和release函数中）|指向IIRIIRPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|任何指定的指针为空时错误。|
|HMPP_STS_LENGTh_ERR|len小于或等于0时错误、或len<3*order。|
|HMPP_STS_DIV_BY_ZERO_ERR|除0错误，![](../../figures/zh-cn_image_0000002549921445.png)不能为0。|
|HMPP_STS_CONTEXT_MATCH_ERR|表示Policy状态不正确时出错（使用了错误的Init函数）。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- 调用该接口计算之前，必须调用Init接口初始化IIRIIRPolicy规范结构。
>- IIRIIRPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
>- IIRIIRPolicy结构体初始化成功后，如果执行滤波失败，必须使用Release函数释放结构体。
>- IIRIIR对滤波器输入数据长度有要求，要求len≥3\*order。

**示例**

```c
#define ORDER 4
#define TAPS_LEN ( (ORDER + 1) * 2)
#define SRC_LEN 12
#define DLY_LEN ORDER

void IIRIIRExample(void) {
    float taps[TAPS_LEN] = {0.0390, -0.1560, 0.2340, -0.1560, 0.0390, 1.0000, 0.9532, 0.7746, 0.2338, 0.0366};
    float src[SRC_LEN] = {186, 431, 689, 206, 716, 90, 695, -153, 289, 291, 482, -21};
    float dlySrc[DLY_LEN] = {123, 312, 781, 249};
    float dlyDst[DLY_LEN];
    float dst[SRC_LEN];
    HmppsIIRPolicy_32f *policy = NULL;
    HmppResult result;
 
    result = HMPPS_IIRIIRInit_32f(&policy, taps, ORDER, dlySrc);
    printf("HMPPS_IIRIIRInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_IIRIIR_32f(src, dst, SRC_LEN, policy);
    printf("HMPPS_IIRIIR_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("dstLen = %d\ndst =", SRC_LEN);
    for (i = 0; i < SRC_LEN; ++i) {
        printf(" %f", dst[i]);
    }
    HMPPS_IIRIIRGetDlyLine_32f(policy, dlyDst);
    printf("\ndlyDstLen = %d\ndlyDst =", DLY_LEN);
    for (i = 0; i < DLY_LEN; ++i) {
        printf(" %f", dlyDst[i]);
    }
    printf("\n");
    HMPPS_IIRIIRRelease_32f(policy);
    policy = NULL;
}
```

运行结果：

```text
HMPPS_IIRIIRInit_32f result = 0
HMPPS_IIRIIR_32f result = 0
dstLen = 12
dst = 1265.131836 330.548798 -677.644104 32.452599 557.080872 -618.195129 370.672546 -161.901367 103.654419 -112.175522 88.226212 -16.978725
dlyDstLen = 4
dlyDst = -0.039000 0.117000 -0.117000 0.039000
```

## IIRSparse

初始化一个任意阶稀疏IIR滤波器并进行滤波。

稀疏滤波器仅对系数非零的位置进行计算，适用于高阶非零元素较少的滤波器。输入向量x\(n\)将被存储在src中，输出向量y\(n\)存储在dst中。

计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518441536.png)

在初始化接收长度为nzTapsLen1 + nzTapsLen2的非零系数向量、非零系数位置向量和延迟线向量，系数向量的排列为：![](../../figures/zh-cn_formulaimage_0000002518281630.png)。

系数位置向量的顺序为：

![](../../figures/zh-cn_formulaimage_0000002518281648.png)  。

计算完成后，存储在Policy内部的延迟线将会被更新。

IIRSparse函数调用流程如下：

1. 使用IIRSparseInit函数进行初始化。
2. 使用FIRSparse进行滤波操作。
3. 再使用FIRSparseGetDlyLine或者FirSparseSetDlyLine检索设置延迟线。
4. 最后再使用IIRSparseRelease对IIRSparseInit申请的内存进行释放。

函数接口声明如下：

- **初始化操作**：

    HmppResult HMPPS\_IIRSparseInit\_32f\(HmppsIIRSparsePolicy\_32f \*\*policy, const float \*nzTaps, const int32\_t \*nzTapPos, int32\_t nzTapsLen1, int32\_t nzTapsLen2, const float \*dlyLine\);

- **滤波操作：**

    HmppResult HMPPS\_IIRSparse\_32f\(const float \*src, float \*dst, int len, HmppsIIRSparsePolicy\_32f \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_IIRSparseRelease\_32f\(HmppsIIRSparsePolicy\_32f \*policy\);

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|nzTapsLen1|系数向量A的长度。|(0, INT_MAX]|输入|
|nzTapsLen2|系数向量B的长度。|(0, INT_MAX]|输入|
|nzTaps|指向系数向量的指针。|非空，所有元素非零|输入|
|nzTapsPos|指向系数位置向量的指针。|非空升序序列，所有元素非零|输入|
|src|指向源向量指针|非空|输入|
|dst|指向目标向量的指针。|非空|输出|
|dlyLine|指向包含延迟线向量的指针。|向量可以为NULL，如果为NULL，则用全零填充延迟线。|输入|
|len|源向量和目标向量长度|(0, INT_MAX]|输入|
|policy（init函数中）|指向内存存储IIRSparsePolicy的指针的指针。|非空|输出|
|policy（滤波函数中和release函数中）|指向IIRSparsePolicy结构体的指针。|非空|输入|

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当len小于或等于0时指示错误。|
|HMPP_STS_SPARSE_ERR|除0错误，![](../../figures/zh-cn_image_0000002518441544.png)不能为0。|
|HMPP_STS_IIR_ORDER_ERR|系数位置数组不是升序，或pNZTapPos[nzTapsLen1]为0。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- 调用该接口计算之前，必须调用Init接口初始化IIRSparsePolicy规范结构。
>- IIRSparsePolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
>- IIRSparsePolicy结构体初始化成功后，如果执行主函数失败，必须使用Release函数释放结构体。
>- IIRSparsePolicy会存储当前的dly，但是暂时不支持用户获取/设置。

**示例**

```c
#define TAPS_LEN1 3
#define TAPS_LEN2 2
#define SRC_LEN 5

void IIRSparseExample(void) {
    float nzTaps[TAPS_LEN1 + TAPS_LEN2] = {0.11381195, -0.22762391,  0.11381195, -0.8457246 , -0.30097242};
    int nzTapsPos[TAPS_LEN1 + TAPS_LEN2] = {0, 1, 2, 1, 2};
    float src[SRC_LEN] = {-609.13320264, -797.90780797, -654.27805165, -108.74137918, 24.2107372};
    float dly[TAPS_LEN1 + TAPS_LEN2] = {-578.53248547, 282.88755714, 861.85222751, 342.84958675, 769.01355443};
    float dst[SRC_LEN];
    HmppsIIRSparsePolicy_32f *policy = NULL;
    HmppResult result;

    result = HMPPS_IIRSparseInit_32f(&policy, nzTaps, nzTapsPos, TAPS_LEN1, TAPS_LEN2, dly);
    printf("HMPPS_IIRSparseInit_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_IIRSparse_32f(src, dst, SRC_LEN, policy);
    printf("HMPPS_IIR_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("\ndstLen = %d\ndst =", SRC_LEN);
    for (i = 0; i < SRC_LEN; ++i) {
        printf(" %f", dst[i]);
    }
    printf("\n");
    HMPPS_IIRSparseRelease_32f(policy);
    policy = NULL;
}
```

运行结果：

```text
HMPPS_IIRSparseInit_32f result = 0
HMPPS_IIR_32f result = 0

dstLen = 5
dst = -737.520752 346.343597 -33.106266 -30.499273 -11.198996
```

## Resample

使用对理想低通滤波器加Kaiser窗的多项滤波器对数据进行重采样，适用于可变的重采样率。

函数调用流程如下：

1. 调用Init初始化结构体。
2. 调用重采样主函数。
3. 最后调用Release释放结构体所包含的内存。

输入待重采样数据src的最左边和最右边需包含滤波所需要的延长线数据；滤波器长度filterLen跟factor、step、window关系如下：

filterLen / 2 = window \* 0.5 + 1;  factor \> 1.0

filterLen / 2 = window \* 0.5 / factor + 1.0 / \(factor \* step\); factor < 1.0

src左侧延长线数据个数：\(filterLen / 2\) - time;

src右侧延长线数据个数：time + \(filterLen / 2\);

即src长度应该为len + filterLen。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_ResamplePolyphaseInit\_32f\(float window, int32\_t step, float rollf, float alpha, HmppHintAlgorithm hint, HmppsResamplingPolyphase\_32f \*\*policy\);

    HmppResult HMPPS\_ResamplePolyphaseInit\_16s\(float window, int32\_t step, float rollf, float alpha, HmppHintAlgorithm hint, HmppsResamplingPolyphase\_16s \*\*policy\);

- **重采样操作：**

    HmppResult HMPPS\_ResamplePolyphase\_32f\(const float \*src, int32\_t len, float \*dst, double factor, float norm, double \*time,int32\_t \*outLen, const HmppsResamplingPolyphase\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphase\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*dst, double factor, float norm, double \*time,int32\_t \*outLen, const HmppsResamplingPolyphase\_16s \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_ResamplePolyphaseRelease\_32f\(HmppsResamplingPolyphase\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphaseRelease\_16s\(HmppsResamplingPolyphase\_16s \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|window|理想低通滤波器窗口大小。|(0,  (INT_MAX - 0x90) / 4 / step)|输入|
|step|多项滤波器的步长。|(0,**** (INT_MAX - 0x90) / 4 / window)|输入|
|rollf|滤波器的衰减频率。|(0, 1.0]|输入|
|alpha|Kaiser窗的可调参数。|(1.0,FLT_MAX]|输入|
|policy|指向重采样策略结构体的指针或者指针的指针。|非空|输入/输出|
|hint|重采样的模式。|HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|
|src|指向输入的待重采样的数据及延迟线。|非空|输入|
|dst|指向输出的已重采样的数据。|非空|输入/输出|
|len|待重采样的数据长度。|当factor >= 1.0 : (0,INT_MAX / factor)当factor < 1.0 : (0,INT_MAX)|输入|
|factor|重采样因子。|(0,DBL_MAX]|输入|
|norm|重采样数据的归一化系数。|float任意值|输入|
|time|指向重采样的开始时间及结束时间。|非空|输入/输出|
|outLen|指向重采样输出的数据长度。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、policy、time、outLen这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len存在小于或者等于0。|
|HMPP_STS_BAD_ARG_ERR|rollf小于或等于0或者大于1。alpha小于1。window小于2/step。factor小于或者等于0。|
|HMPP_STS_MALLOC_FAILED|申请内存失败。|
|HMPP_STS_OVER_FLOW|所需的内部buffer大小超过INT_MAX。|

**示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"
#include "hmpp_core.h"

#define BUFFER_SIZE_T 10

int main()
{
    float src[4 + BUFFER_SIZE_T + 4] = {0, 0, 0, 0, 1.0, 1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 1.9, 0, 0, 0, 0};
    float window = 6.0;
    int32_t step = 2;
    float rollf = 0.95;
    float alpha = 9.0f;
    double factor = 0.5;
    float norm = 1.0;
    double time = 4.0;
    int32_t outLen;
    float dst[BUFFER_SIZE_T/2];
    HmppsResamplingPolyphase_32f *policy;

    HmppResult result = HMPPS_ResamplePolyphaseInit_32f(window, step, rollf, alpha, HMPP_ALGHINT_FAST, &policy);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }
    result = HMPPS_ResamplePolyphase_32f(src, BUFFER_SIZE_T, dst, factor, norm, &time, &outLen, policy);
    printf("result = %d, outLen = %d\n", result, outLen);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }

    printf("dst =");
    for (int32_t i = 0; i < outLen; i++) {
        printf(" %f", dst[i]);  
    }
    printf("\n");
    
    HMPPS_ResamplePolyphaseRelease_32f(policy);

    return 0;
} 
```

运行结果：

```text
result = 0, outLen = 5
dst = 0.761617 1.231306 1.398704 1.602798 1.842311
```

## ResampleFixed

使用对理想低通滤波器加Kaiser窗的多项滤波器对数据进行重采样，仅适用于输入和输出采用率为固定有理数的情况，相比非Fixed接口提供更好的性能。

函数调用流程如下：

1. 调用Init初始化结构体。
2. 调用重采样主函数。
3. 最后调用Release释放结构体所包含的内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_ResamplePolyphaseFixedInit\_32f\(int32\_t inRate, int32\_t outRate, int32\_t len, float rollf, float alpha,HmppHintAlgorithm hint, int32\_t \*fLen, int32\_t \*fHeight,HmppsResamplingPolyphaseFixed\_32f \*\*policy\);

    HmppResult HMPPS\_ResamplePolyphaseFixedInit\_16s\(int32\_t inRate, int32\_t outRate, int32\_t len, float rollf, float alpha,HmppHintAlgorithm hint, int32\_t \*fLen, int32\_t \*fHeight,HmppsResamplingPolyphaseFixed\_16s \*\*policy\);

- **设置滤波器系数操作：**

    HmppResult HMPPS\_ResamplePolyphaseSetFixedFilter\_32f\(const float \*src, int32\_t step, int32\_t height,HmppsResamplingPolyphaseFixed\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphaseSetFixedFilter\_16s\(const int16\_t \*src, int32\_t step, int32\_t height,HmppsResamplingPolyphaseFixed\_16s \*policy\);

- **获取滤波器系数操作：**

    HmppResult HMPPS\_ResamplePolyphaseGetFixedFilter\_32f\(float \*dst, int32\_t step, int32\_t height,const HmppsResamplingPolyphaseFixed\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphaseGetFixedFilter\_16s\(int16\_t \*dst, int32\_t step, int32\_t height,const HmppsResamplingPolyphaseFixed\_16s \*policy\);

- **重采样操作：**

    HmppResult HMPPS\_ResamplePolyphaseFixed\_32f\(const float \*src, int32\_t len, float \*dst, float norm, double \*time,int32\_t \*outLen, const HmppsResamplingPolyphaseFixed\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphaseFixed\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*dst, float norm, double \*time,int32\_t \*outLen, const HmppsResamplingPolyphaseFixed\_16s \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_ResamplePolyphaseFixedRelease\_32f\(HmppsResamplingPolyphaseFixed\_32f \*policy\);

    HmppResult HMPPS\_ResamplePolyphaseFixedRelease\_16s\(HmppsResamplingPolyphaseFixed\_16s \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|inRate|固定因子重采样的输入频率。|(0,INT_MAX]|输入|
|outRate|固定因子重采样的输出频率。|(0,INT_MAX]|输入|
|len|固定因子重采样滤波器的长度或者待重采样的输入数据的长度。|滤波器长度取值范围：Init函数中会有合法性判断，跟inRate和outRate有关，(0, (INT_MAX****- 0x4) / 4)待重采样数据长度：(0,INT_MAX * inRate / outRate)|输入|
|rollf|滤波器的衰减频率。|(0,1.0]|输入|
|alpha|Kaiser窗的可调参数。|[1,FLT_MAX]|输入|
|policy|指向重采样策略结构体的指针或者指针的指针。|非空|输入/输出|
|hint|重采样的模式。|HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|
|fLen|多项滤波器实际的长度。|非空|输入/输出|
|fHeight|多项滤波器的个数。|非空|输入/输出|
|step|滤波器的步长。|(0,Init函数初始化的实际多项滤波器长度fLen]|输入|
|height|滤波器的个数。|(0,Init函数初始化的实际多项滤波器的高度fHeight]|输入|
|src|指向输入的滤波器系数或者指向输入的待重采样的数据。|非空|输入/输出|
|dst|指向输出的滤波器系数或者指向输出的已重采样的数据。|非空|输入/输出|
|norm|重采样数据的归一化系数。|float任意值|输入|
|time|指向重采样的开始时间及结束时间。|非空|输入/输出|
|outLen|指向重采样输出的数据长度。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、policy、fLen、fHeight、time、outLen这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len、step、height中存在小于或等于0。|
|HMPP_STS_BAD_ARG_ERR|rollf小于或等于0或者大于1。alpha小于1。height大于结构体中的滤波器个数。|
|HMPP_STS_MALLOC_FAILED|申请内存失败。|
|HMPP_STS_OVER_FLOW|所需的内部buffer大小超过INT_MAX。|

**示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"
#include "hmpp_core.h"

#define BUFFER_SIZE_T 10

int main()
{
    float src[4 + BUFFER_SIZE_T + 4] = {0, 0, 0, 0, 1.0, 1.1, 1.2, 1.3, 1.4, 1.5, 1.6, 1.7, 1.8, 1.9, 0, 0, 0, 0};
    int32_t inRate = 16000;
    int32_t outRate = 8000;
    int32_t inFilterLen = 6;
    float rollf = 0.95;
    float alpha = 9.0f;
    int32_t fLen;
    int32_t fHeight;
    float norm = 1.0;
    double time = 4.0;
    int32_t outLen;
    float dst[BUFFER_SIZE_T / 2];
    HmppsResamplingPolyphaseFixed_32f *policy;

    HmppResult result = HMPPS_ResamplePolyphaseFixedInit_32f(inRate, outRate, inFilterLen, rollf, alpha, 
                                                             HMPP_ALGHINT_FAST, &fLen, &fHeight, &policy);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }
    float *filter = (float *)HMPPS_Malloc_8u(fLen * fHeight * sizeof(float));
    if (filter == NULL) {
        return -1;
    }
    result = HMPPS_ResamplePolyphaseGetFixedFilter_32f(filter, fLen, fHeight, policy);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }
    printf("filter =");
    for (int32_t i = 0; i < fHeight; i++) {
       for (int32_t j = 0; j < fLen; j++) {
          printf(" %f", *(filter + i * fLen + j));
       }
       printf("\n");
    }
    result = HMPPS_ResamplePolyphaseFixed_32f(src, BUFFER_SIZE_T, dst, norm, &time, &outLen, policy);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }

    printf("dst =");
    for (int32_t i = 0; i < outLen; i++) {
        printf(" %f", dst[i]);  
    }
    printf("\n");
    
    HMPPS_ResamplePolyphaseFixedRelease_32f(policy);
    HMPPS_Free(filter);

    return 0;
} 
```

运行结果：

```text
filter = -0.000046 -0.012375 0.016357 0.493774 0.967378 0.493774 0.016357 -0.012375
result = 0
dst = 0.757003 1.183266 1.373958 1.570284 1.763205    
```
