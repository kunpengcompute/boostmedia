# 统计与分析

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## AutoCorrNorm

计算src向量（长度为srcLen）的归一化自相关，结果存储到dst向量中。归一化支持正常、有偏和无偏自相关三种模式，计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281692.png)

![](../../figures/zh-cn_formulaimage_0000002549921427.png)

![](../../figures/zh-cn_formulaimage_0000002550041415.png)

![](../../figures/zh-cn_formulaimage_0000002550041429.png)

![](../../figures/zh-cn_formulaimage_0000002518281684.png)

该函数调用流程如下：

1. 调用Init初始化HmppsCorrPolicy\_32f结构体。
2. 再调用主函数。
3. 最后调用Release释放HmppsCorrPolicy\_32f函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_AutoCorrInit\_32f\(int32\_t srcLen, int32\_t dstLen, HmppAlgMode calcMode, HmppsCorrPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_AutoCorrInit\_64f\(int32\_t srcLen, int32\_t dstLen, HmppAlgMode calcMode, HmppsCorrPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_AutoCorrInit\_32fc\(int32\_t srcLen, int32\_t dstLen, HmppAlgMode calcMode, HmppsCorrPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_AutoCorrInit\_64fc\(int32\_t srcLen, int32\_t dstLen, HmppAlgMode calcMode, HmppsCorrPolicy\_64fc \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_AutoCorrNorm\_32f\(const float \*src, int32\_t srcLen, float \*dst, int32\_t dstLen, HmppNormMode normMode, HmppsCorrPolicy\_32f \*policy\);

    HmppResult HMPPS\_AutoCorrNorm\_64f\(const double \*src, int32\_t srcLen, double \*dst, int32\_t dstLen, HmppNormMode normMode, HmppsCorrPolicy\_64f \*policy\);

    HmppResult HMPPS\_AutoCorrNorm\_32fc\(const Hmpp32fc \*src, int32\_t srcLen, Hmpp32fc \*dst, int32\_t dstLen, HmppNormMode normMode, HmppsCorrPolicy\_32fc \*policy\);

    HmppResult HMPPS\_AutoCorrNorm\_64fc\(const Hmpp64fc \*src, int32\_t srcLen, Hmpp64fc \*dst, int32\_t dstLen, HmppNormMode normMode, HmppsCorrPolicy\_64fc \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_CorrRelease\_32f\(HmppsCorrPolicy\_32f \*policy\);

    HmppResult HMPPS\_CorrRelease\_64f\(HmppsCorrPolicy\_64f \*policy\);

    HmppResult HMPPS\_CorrRelease\_32fc\(HmppsCorrPolicy\_32fc \*policy\);

    HmppResult HMPPS\_CorrRelease\_64fc\(HmppsCorrPolicy\_64fc \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量指针。|非空|输入|
|srcLen|源向量长度。|(0, INT_MAX]|输入|
|dst|指向目标向量指针。|非空|输出|
|dstLen|目标向量长度。|非空|输入|
|algMode|计算使用的算法模型。定义在枚举类型HmppAlgMode中，请参见枚举类型。|枚举体HmppAlgMode元素：HMPP_ALG_AUTO、HMPP_ALG_DEFAULT、HMPP_ALG_FFT|输入|
|normMode|数据归一化模式。定义在枚举类型HmppNormMode中，请参见枚举类型。|枚举体HmppNormMode元素：HMPP_NORM_NORMAL、HMPP_NORM_BIASED、HMPP_NORM_UNBIASED|输入|
|policy（init函数中）|指向内存存储CorrPolicy的指针。|非空|输出|
|policy（主函数中和release函数中）|指向CorrPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当srcLen或dstLen小于或等于0时指示错误。|
|HMPP_STS_MISMATCH|Init函数申请内存的问题规模和主函数中实际计算的问题规模不匹配。|
|HMPP_STS_OVERFLOW_ERR|FFT加速模型的问题规模过大。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化HmppsCorrPolicy\_32f规范结构。
- HmppsCorrPolicy\_32f结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- src和dst不能是同一数组，否则可能导致结果错误。
- 使用HMPP\_ALG\_AUTO或HMPP\_ALG\_FFT模式时，当srcLen和dstLen较大时会出现OVERFLOW错误提示。

**示例**

```c
void AutoCorrNorm_Example()
{
    const int len = 10;
    float src[len];
    float dst[len];
    int32_t srcLen = len;
    int32_t dstLen = len;

    for (int i = 0; i < srcLen; ++i) {
        src[i] = 1;
    }
    HmppsCorrPolicy_32f *policy = NULL;
    HmppResult result = HMPPS_AutoCorrInit_32f(srcLen, dstLen, HMPP_ALG_AUTO, &policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Init failed");
        return;
    }
    result = HMPPS_AutoCorrNorm_32f(src, srcLen, dst, dstLen, HMPP_NORM_NORMAL, policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("AutoCorr failed");
        return;
    }
    for (int i = 0; i < dstLen; ++i) {
        printf("%.2f ", dst[i]);
    }
    HMPPS_CorrRelease_32f(policy);
}
```

运行结果：

```text
10 9 8 7 6 5 4 3 2 1
```

## CountInRange

统计向量中在特定区间范围内数据的个数。

当向量元素满足_lowerBound < src\[n \] < upperBound_时，pCounts = pCounts + 1。

函数接口声明如下：

HmppResult HMPPS\_CountInRange\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*counts, int32\_t lowerBound, int32\_t upperBound\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|counts|指向统计结果的指针。|非空|输出|
|lowerBound|区间下界。|[INT_MIN,INT_MAX]|输入|
|upperBound|区间上界。|[INT_MIN,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、counts这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void CountInRangeExample(void)
{
    int32_t src[BUFFER_SIZE_T] = {3, 5, 9, 9, 11, 10, 0, -7, -3, 1};
    int32_t lower =0;
    int32_t upper =10;
    int32_t count;

    HmppResult result = HMPPS_CountInRange_32s(src, BUFFER_SIZE_T, &count, lower, upper);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("countInRange = %d\n", count);
}
```

运行结果：

```text
result = 0  countInRange = 5
```

## CrossCorrNorm

计算src1向量（长度为src1Len）和src2向量（长度为src2Len）的归一化互相关，结果存储到dst向量中。归一化支持正常、有偏和无偏自相关三种模式，计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002549921749.png)

![](../../figures/zh-cn_formulaimage_0000002550041751.png)

![](../../figures/zh-cn_formulaimage_0000002550041745.png)

![](../../figures/zh-cn_formulaimage_0000002549921755.png)

该函数调用流程如下：

1. 调用Init初始化HmppsCorrPolicy\_32f结构体。
2. 调用主函数。
3. 调用Release释放HmppsCorrPolicy\_32f函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_CrossCorrInit\_32f\(int32\_t src1Len, int32\_t src2Len, int32\_t dstLen, int32\_t lowLag, HmppCalcMode calcMode, HmppsCorrPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_CrossCorrInit\_64f\(int32\_t src1Len, int32\_t src2Len, int32\_t dstLen, int32\_t lowLag, HmppCalcMode calcMode, HmppsCorrPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_CrossCorrInit\_32fc\(int32\_t src1Len, int32\_t src2Len, int32\_t dstLen, int32\_t lowLag, HmppCalcMode calcMode, HmppsCorrPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_CrossCorrInit\_64fc\(int32\_t src1Len, int32\_t src2Len, int32\_t dstLen, int32\_t lowLag, HmppCalcMode calcMode, HmppsCorrPolicy\_64fc \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_CrossCorrNorm\_32f\(const float \*src1, int32\_t src1Len, const float \*src2, int32\_t src2Len, float \*dst, int32\_t dstLen, int32\_t lowLag, HmppNormMode normMode, HmppsCorrPolicy\_32f \*policy\);

    HmppResult HMPPS\_CrossCorrNorm\_64f\(const double \*src1, int32\_t src1Len, const double \*src2, int32\_t src2Len, double \*dst, int32\_t dstLen, int32\_t lowLag, HmppNormMode normMode, HmppsCorrPolicy\_64f \*policy\);

    HmppResult HMPPS\_CrossCorrNorm\_32fc\(const Hmpp32fc \*src1, int32\_t src1Len, const Hmpp32fc \*src2, int32\_t src2Len, Hmpp32fc \*dst, int32\_t dstLen, int32\_t lowLag, HmppNormMode normMode, HmppsCorrPolicy\_32fc \*policy\);

    HmppResult HMPPS\_CrossCorrNorm\_64fc\(const Hmpp64fc \*src1, int32\_t src1Len, const Hmpp64fc \*src2, int32\_t src2Len, Hmpp64fc \*dst, int32\_t dstLen, int32\_t lowLag, HmppNormMode normMode, HmppsCorrPolicy\_64fc \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_CorrRelease\_32f\(HmppsCorrPolicy\_32f \*policy\);

    HmppResult HMPPS\_CorrRelease\_64f\(HmppsCorrPolicy\_64f \*policy\);

    HmppResult HMPPS\_CorrRelease\_32fc\(HmppsCorrPolicy\_32fc \*policy\);

    HmppResult HMPPS\_CorrRelease\_64fc\(HmppsCorrPolicy\_64fc \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src1Len|第一个源向量长度。|(0, INT_MAX]|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|src2Len|第二个源向量长度。|(0, INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstLen|目标向量长度。|(0, INT_MAX]|输入|
|lowLag|互相关最小滞后。|[INT_MIN,INT_MAX]|输入|
|algMode|计算使用的算法模型。定义在枚举类型HmppAlgMode中，请参见枚举类型。|枚举体HmppAlgMode元素：HMPP_ALG_AUTO、HMPP_ALG_DEFAULT、HMPP_ALG_FFT|输入|
|normMode|数据归一化模式。定义在枚举类型HmppNormMode中，请参见枚举类型。|枚举体HmppNormMode元素：HMPP_NORM_NORMAL、HMPP_NORM_BIASED、HMPP_NORM_UNBIASED|输入|
|policy（init函数中）|指向内存存储CorrPolicy的指针。|非空|输出|
|policy（主函数中和release函数中）|指向CorrPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当src1Len、src2Len或dstLen ≤ 0时指示错误。|
|HMPP_STS_MISMATCH|Init函数申请内存的问题规模和主函数中实际计算的问题规模不匹配。|
|HMPP_STS_OVERFLOW_ERR|FFT加速模型的问题规模过大。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化HmppsCorrPolicy\_32f规范结构。
- HmppsCorrPolicy\_32f结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。
- src1、src2不能和dst是同一数组，否则可能导致结果错误。
- 使用HMPP\_ALG\_AUTO或HMPP\_ALG\_FFT模式时，当src1Len、src2Len、dstLen较大时会出现OVERFLOW错误提示。

**示例**

```c
void CrossCorrNorm_Example()
{
    const int src1Len = 10;
    const int src2Len = 5;
    const int dstLen = 10;
    float src1[src1Len];
    float src2[src2Len];
    float dst[dstLen];

    HmppsCorrPolicy_32f *policy = NULL;
    for (int i = 0; i < src1Len; ++i) src1[i] = 1;
    for (int i = 0; i < src2Len; ++i) src2[i] = 1;
    int lowLag = -1;

    HmppResult result = HMPPS_CrossCorrInit_32f(src1Len, src2Len, dstLen, lowLag, HMPP_ALG_AUTO, &policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("Init failed");
        return;
    }
    result = HMPPS_CrossCorrNorm_32f(src1, src1Len, src2, src2Len, dst, dstLen, lowLag, HMPP_NORM_NORMAL, policy);
    if (result != HMPP_STS_NO_ERR) {
        printf("CrossCorr failed");
        return;
    }
    for (int i = 0; i < dstLen; ++i) {
        printf("%.2f ", dst[i]);
    }
    HMPPS_CorrRelease_32f(policy);

}
```

运行结果：

```text
5 5 4 3 2 1 0 0 0 0
```

## DotProd

计算两个向量的点积。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_DotProd\_32f\(const float \*src1, const float \*src2, int32\_t len, float \*dp\);

    HmppResult HMPPS\_DotProd\_32fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, int32\_t len, Hmpp32fc \*dp\);

    HmppResult HMPPS\_DotProd\_64f\(const double \*src1, const double \*src2, int32\_t len, double \*dp\);

    HmppResult HMPPS\_DotProd\_64fc\(const Hmpp64fc \*src1, const Hmpp64fc \*src2, int32\_t len, Hmpp64fc \*dp\);

    HmppResult HMPPS\_DotProd\_32f32fc\(const float \*src1, const Hmpp32fc \*src2, int32\_t len, Hmpp32fc \*dp\);

    HmppResult HMPPS\_DotProd\_32f64f\(const float \*src1, const float \*src2, int32\_t len, double \*dp\);

    HmppResult HMPPS\_DotProd\_32fc64fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, int32\_t len, Hmpp64fc \*dp\);

    HmppResult HMPPS\_DotProd\_32f32fc64fc\(const float \*src1, const Hmpp32fc \*src2, int32\_t len, Hmpp64fc \*dp\);

    HmppResult HMPPS\_DotProd\_64f64fc\(const double \*src1, const Hmpp64fc \*src2, int32\_t len, Hmpp64fc \*dp\);

- **整型数的操作：**

    HmppResult HMPPS\_DotProd\_16s64s\(const int16\_t \*src1, const int16\_t \*src2, int32\_t len, int64\_t \*dp\);

    HmppResult HMPPS\_DotProd\_16s32f\(const int16\_t \*src1, const int16\_t \*src2, int32\_t len, float \*dp\);

    HmppResult HMPPS\_DotProd\_16sc64sc\(const Hmpp16sc \*src1, const Hmpp16sc \*src2, int32\_t len, Hmpp64sc \*dp\);

    HmppResult HMPPS\_DotProd\_16s16sc64sc\(const int16\_t \*src1, const Hmpp16sc \*src2, int32\_t len, Hmpp64sc \*dp\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_DotProd\_32s\_S\(const int32\_t \*src1, const int32\_t \*src2, int32\_t len, int32\_t \*dp, double scale\);

    HmppResult HMPPS\_DotProd\_16s32s\_S\(const int16\_t \*src1, const int16\_t \*src2, int32\_t len, int32\_t \*dp, double scale\);

    HmppResult HMPPS\_DotProd\_16s32s32s\_S\(const int16\_t \*src1, const int32\_t \*src2, int32\_t len, int32\_t \*dp, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|dp|指向结果向量的指针。|非空|输出|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dp这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

void DotProdExample(void)
{
    float src1[BUFFER_SIZE_T] = {2.85, 5.44, 7.68, 11.25, 8.56, 8.34, -0.43, 9.70, 0.68, -1.38};
    float src2[BUFFER_SIZE_T] = {3.93, 2.30, 3.38, 3.92, 0.20, 3.42, 2.34, -1.36, 1.53, -1.15};
    float dst;

    HmppResult result = HMPPS_DotProd_32f(src1, src2, BUFFER_SIZE_T, &dst);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dotProd = %.2f\n", dst);
}
```

运行结果：

```text
result = 0  dotProd = 112.43
```

## FindNearest

在表中查找最接近指定值向量的元素集合。查找到的每个元素及其索引分别存储在向量outVals和outIndexes中。

其中表中元素必须满足条件：table\[n\] ≤table\[n+1\]。最接近指的是：min\(|inVals\[k\] -table\[n\]|\)。

函数接口声明如下：

HmppResult HMPPS\_FindNearest\_16u\(const uint16\_t \*inVals, uint16\_t \*outVals, int32\_t \*outIndexes, int32\_t len, const uint16\_t \*table, int32\_t tblLen\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|inVals|指向指定元素向量的指针。|非空|输入|
|outVals|指向查找结果元素向量的指针。|非空|输出|
|outIndexes|指向查找结果元素索引向量的指针。|非空|输出|
|len|inVals向量的长度|(0,INT_MAX]|输入|
|table|指向单调不减向量表。|非空|输入|
|tblLen|向量表的长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|table、inVals、outVals、outIndexes这四个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|tblLen、len小于或等于0。|

**示例**

```c
#define  TABLE_SIZE_T 13
#define  BUFFER_SIZE_T 11
void FindNearestExample()
{
    uint16_t table[TABLE_SIZE_T] = {32, 434, 486, 545, 766, 976, 1222, 1534, 1687, 3452, 8556, 32452, 56422};
    uint16_t inVals[BUFFER_SIZE_T] = {32, 545, 766, 876, 1222, 1334, 1687, 3452, 4556, 32452, 45422};
    uint16_t outVals[BUFFER_SIZE_T];
    int32_t outIndexes[BUFFER_SIZE_T];
    HmppResult result = HMPPS_FindNearest_16u(inVals, outVals, outIndexes, BUFFER_SIZE_T, table, TABLE_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("outVals :");
        for (int i = 0; i < BUFFER_SIZE_T; i++) {
            printf(" %d", outVals[i]);
        }
        printf("\n");
        printf("outIndexes:");
        for (int i = 0; i < BUFFER_SIZE_T; i++) {
            printf(" %d", outIndexes[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
outVals : 32 545 766 976 1222 1222 1687 3452 3452 32452 56422
outIndexes: 0 3 4 5 6 6 8 9 9 11 12
```

## FindNearestOne

查找表中最接近指定值的元素。查找到的元素及其索引分别存储在outVal和outIndex中。

其中表中元素必须满足条件：table\[n\] ≤table\[n+1\]。最接近指的是：min\(|inVal -table\[n\]|\)。

函数接口声明如下：

HmppResult HMPPS\_FindNearestOne\_16u\(uint16\_t inVal, uint16\_t \*outVal, int32\_t \*outIndex, const uint16\_t \*table, int32\_t tblLen\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|inVal|指定元素。|(0,UINT16_MAX]|输入|
|outVal|查找结果元素的值。|(0,UINT16_MAX]|输出|
|outIndex|查找结果元素的索引。|[0,tblLen-1]|输出|
|table|指向单调不减向量表。|非空|输入|
|tblLen|向量表的长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|table为空指针。|
|HMPP_STS_SIZE_ERR|tblLen小于或等于0。|

**示例**

```c
#define  BUFFER_SIZE_T 11
void FindNearestOneExample()
{
    uint16_t table[BUFFER_SIZE_T] = {32, 545, 766, 876, 1222, 1334, 1687, 3452, 4556, 32452, 45422};
    uint16_t inVal = 4559;
    uint16_t outVal;
    int32_t outIndex;
    HmppResult result = HMPPS_FindNearestOne_16u(inVal, &outVal, &outIndex, table, BUFFER_SIZE_T );
    if (result == HMPP_STS_NO_ERR) {
        printf("outVal = %d\n", outVal);
        printf("outIndex = %d\n", outIndex);
    }
}
```

运行结果：

```text
outVal = 4556
outIndex = 8
```

## Max

获取向量中的最大值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Max\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*max\);

    HmppResult HMPPS\_Max\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*max\);

- **浮点数的操作：**

    HmppResult HMPPS\_Max\_32f\(const float \*src, int32\_t len, float \*max\);

    HmppResult HMPPS\_Max\_64f\(const double \*src, int32\_t len, double \*max\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|max|指向最大值的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MaxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int16_t max;
    HmppResult result = HMPPS_Max_16s(src, BUFFER_SIZE_T, &max);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("max = %d\n", max);
}
```

运行结果：

```text
result = 0  max = 56
```

## MaxAbs

获取向量中的最大绝对值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MaxAbs\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*maxAbs\);

    HmppResult HMPPS\_MaxAbs\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*maxAbs\);

- **浮点数的操作：**

    HmppResult HMPPS\_MaxAbs\_32f\(const float \*src, int32\_t len, float \*maxAbs\);

    HmppResult HMPPS\_MaxAbs\_64f\(const double \*src, int32\_t len, double \*maxAbs\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|maxAbs|指向最大绝对值的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、maxAbs这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MaxAbsExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t maxAbs;

    HmppResult result = HMPPS_MaxAbs_16s(src, BUFFER_SIZE_T, &maxAbs);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("maxAbs = %d\n", maxAbs);
}
```

运行结果：

```text
result = 0  maxAbs = 31
```

## MaxAbsIndx

获取向量中的最大绝对值及其位置索引。

函数接口声明如下：

HmppResult HMPPS\_MaxAbsIndx\_16s\(const int16\_t \*src int32\_t len, int16\_t \*maxAbs, int32\_t \*indx\);

HmppResult HMPPS\_MaxAbsIndx\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*maxAbs, int32\_t \*indx\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|maxAbs|指向最大绝对值的指针。|非空|输出|
|indx|指向位置索引的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、maxAbs、indx这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MaxAbsIndxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t maxAbs;
    int32_t maxIndex;

    HmppResult result = HMPPS_MaxAbsIndx_16s(src, BUFFER_SIZE_T, &maxAbs, &maxIndex);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("maxAbs = %d, index of maxAbs is %d\n", maxAbs, maxIndex);
}
```

运行结果：

```text
result = 0
maxAbs = 31, index of maxAbs is 7
```

## MaxEvery

获取两向量中每一对元素的最大值。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_MaxEvery\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_MaxEvery\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_MaxEvery\_32f\_I\(const float \*src, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MaxEvery\_64f\_I\(const double \*src, double \*srcDst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_MaxEvery\_16s\_I\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MaxEvery\_32s\_I\(const int32\_t \*src, int32\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|src|指向原地操作中源向量的指针。|非空|输入|
|srcDst|指向原地操作中目的向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、src、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**注意**

入参len是无符号类型的数据，传入负数会导致len变为一个大数，会发生未知错误。

**示例**

```c
#define BUFFER_SIZE_T 10

void MaxEveryExample(void)
{
    float src1[BUFFER_SIZE_T] = {3.25, 0.45, 2.23, -8.11, 3.10, 15.56, 26.53, -31.13, 1.44, 23.18};
    float src2[BUFFER_SIZE_T] = {0.32, 0.56, -12.45, 45.67, 12.10, -2.11, -7.60, 6.78, 8.88, 1.24};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_MaxEvery_32f(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("The max vector is:");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf("  %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
The max vector is: 3.25  0.56  2.23  45.67  12.10  15.56  26.53  6.78  8.88  23.18
```

## MaxIndx

获取向量中的最大值及其位置索引。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MaxIndx\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*max, int32\_t \*indx\);

    HmppResult HMPPS\_MaxIndx\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*max, int32\_t \*indx\);

- **浮点数的操作：**

    HmppResult HMPPS\_MaxIndx\_32f\(const float \*src, int32\_t len, float \*max, int32\_t \*indx\);

    HmppResult HMPPS\_MaxIndx\_64f\(const double \*src, int32\_t len, double \*max, int32\_t \*indx\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|max|指向最大值的指针。|非空|输出|
|indx|指向索引的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max、indx这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MaxIndxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t max;
    int32_t maxIndex;

    HmppResult result = HMPPS_MaxIndx_16s(src, BUFFER_SIZE_T, &max, &maxIndex);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("max = %d, index of max is %d\n", max, maxIndex);
}
```

运行结果：

```text
result = 0
max = 26, index of max is 6
```

## Mean

计算向量的平均值。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Mean\_32f\(const float \*src, int32\_t len, float \*mean, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_Mean\_64f\(const double \*src, int32\_t len, double \*mean\);

    HmppResult HMPPS\_Mean\_32fc\(const Hmpp32fc \*src, int32\_t len, Hmpp32fc \*mean, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_Mean\_64fc\(const Hmpp64fc \*src, int32\_t len, Hmpp64fc \*mean\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Mean\_16s\_S\(const int16\_t \*src, int32\_t len, int16\_t \*mean, double scale\);

    HmppResult HMPPS\_Mean\_32s\_S\(const int32\_t \*src, int32\_t len, int32\_t \*mean, double scale\);

    HmppResult HMPPS\_Mean\_16sc\_S\(const Hmpp16sc \*src, int32\_t len, Hmpp16sc \*mean, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|mean|指向平均值的指针。|非空|输出|
|hint|算法模式。定义在枚举类型HmppHintAlgorithm中，请参见枚举类型。|枚举体HmppHintAlgorithm元素：HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|
|scale|缩放因数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、mean这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MeanExample(void)
{
    float src[BUFFER_SIZE_T] = {3.45, 0.12, 7.77, 4.45, 2.44, 3.67, 2.78, 8.88, 1.83, 5.57};
    float mean;
    HmppHintAlgorithm hint = HMPP_ALGHINT_FAST;

    HmppResult result = HMPPS_Mean_32f(src, BUFFER_SIZE_T, &mean, hint);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("mean = %.3f\n", mean);
}
```

运行结果：

```text
result = 0  mean = 4.096
```

## MeanStdDev

计算向量的平均值以及标准差。计算公式如下：

- 平均值

    ![](../../figures/zh-cn_formulaimage_0000002549921525.png)

- 标准差

    ![](../../figures/zh-cn_formulaimage_0000002518441676.png)

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_MeanStdDev\_32f\(const float \*src, int32\_t len, float \*mean, float \*stdDev, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_MeanStdDev\_64f\(const double \*src, int32\_t len, double \*mean, double \*stdDev\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_MeanStdDev\_16s\_S\(const int16\_t \*src, int32\_t len, int16\_t \*mean, int16\_t \*stdDev, double scale\);

    HmppResult HMPPS\_MeanStdDev\_16s32s\_S\(const int16\_t \*src, int32\_t len, int32\_t \*mean, int32\_t \*stdDev, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|[2,INT_MAX]|输入|
|mean|指向平均值的指针。|非空|输出|
|stdDev|指向标准差的指针。|非空|输出|
|hint|算法模式。定义在枚举类型HmppHintAlgorithm中，请参见枚举类型。|枚举体HmppHintAlgorithm元素：HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、mean、stdDev这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MeanStdDevExample(void)
{
    float src[BUFFER_SIZE_T] = {3.45, 0.12, 7.77, 4.45, 2.44, 3.67, 2.78, 8.88, 1.83, 5.57};
    float mean;
    float stdDev;
    HmppHintAlgorithm hint = HMPP_ALGHINT_FAST;

    HmppResult result = HMPPS_MeanStdDev_32f(src, BUFFER_SIZE_T, &mean, &stdDev, hint);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("mean = %.3f, stdDev = %.3f\n", mean, stdDev);
}
```

运行结果：

```text
result = 0
mean = 4.096, stdDev = 2.681
```

## Min

获取向量中的最小值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Min\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*min\);

    HmppResult HMPPS\_Min\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*min\);

- **浮点数的操作：**

    HmppResult HMPPS\_Min\_32f\(const float \*src, int32\_t len, float \*min\);

    HmppResult HMPPS\_Min\_64f\(const double \*src, int32\_t len, double \*min\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|min|指向最小值的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int16_t min;

    HmppResult result = HMPPS_Min_16s(src, BUFFER_SIZE_T, &min);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("min = %d\n", min);
}
```

运行结果：

```text
result = 0  min = 1
```

## MinAbs

获取向量中的最小绝对值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MinAbs\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*minAbs\);

    HmppResult HMPPS\_MinAbs\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*minAbs\);

- **浮点数的操作：**

    HmppResult HMPPS\_MinAbs\_32f\(const float \*src, int32\_t len, float \*minAbs\);

    HmppResult HMPPS\_MinAbs\_64f\(const double \*src, int32\_t len, double \*minAbs\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|minAbs|指向最小绝对值的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、minAbs这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinAbsExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t minAbs;

    HmppResult result = HMPPS_MinAbs_16s(src, BUFFER_SIZE_T, &minAbs);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("minAbs = %d\n", minAbs);
}
```

运行结果：

```text
result = 0  minAbs = 0
```

## MinAbsIndx

获取向量中的最小绝对值及其位置索引。

函数接口声明如下：

HmppResult HMPPS\_MinAbsIndx\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*min, int32\_t \*indx\);

HmppResult HMPPS\_MinAbsIndx\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*min, int32\_t \*indx\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|min|指向最小绝对值的指针。|非空|输出|
|indx|指向索引的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min、indx三个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinAbsIndxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t minAbs;
    int32_t minIndex;

    HmppResult result = HMPPS_MinAbsIndx_16s(src, BUFFER_SIZE_T, &minAbs, &minIndex);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("minAbs = %d, index of minAbs is %d\n", minAbs, minIndex);
}
```

运行结果：

```text
result = 0
minAbs = 0, index of minAbs is 1
```

## MinEvery

获取两向量中每一对元素的最小值。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_MinEvery\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_MinEvery\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_MinEvery\_16s\_I\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MinEvery\_32s\_I\(const int32\_t \*src, int32\_t \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_MinEvery\_32f\_I\(const float \*src, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MinEvery\_64f\_I\(const double \*src, double \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|src|指向原地操作中源向量的指针。|非空|输入|
|srcDst|指向原地操作中目的向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、src、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**注意**

入参len是无符号类型的数据，传入负数会导致len变为一个大数，会发生未知错误。

**示例**

```c
#define BUFFER_SIZE_T 10

void MinEveryExample(void)
{
    float src1[BUFFER_SIZE_T] = {3.25, 0.45, 2.23, -8.11, 3.10, 15.56, 26.53, -31.13, 1.44, 23.18};
    float src2[BUFFER_SIZE_T] = {0.32, 0.56, -12.45, 45.67, 12.10, -2.11, -7.60, 6.78, 8.88, 1.24};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0。
    HmppResult result = HMPPS_MinEvery_32f(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("The min vector is:");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf("  %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
The min vector is:  0.32  0.45  -12.45  -8.11  3.10  -2.11  -7.60  -31.13  1.44  1.24
```

## MinIndx

获取向量中的最小值及其索引。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MinIndx\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*min, int32\_t \*indx\);

    HmppResult HMPPS\_MinIndx\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*min, int32\_t \*indx\);

- **浮点数的操作：**

    HmppResult HMPPS\_MinIndx\_32f\(const float \*src, int32\_t len, float \*min, int32\_t \*indx\);

    HmppResult HMPPS\_MinIndx\_64f\(const double \*src, int32\_t len, double \*min, int32\_t \*indx\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|源向量。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|min|向量元素最小值。|非空|输出|
|indx|向量元素最小值的位置索引。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min、indx这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinIndxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t min;
    int32_t minIndex;

    HmppResult result = HMPPS_MinIndx_16s(src, BUFFER_SIZE_T, &min, &minIndex);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("min = %d, index of min is %d\n", min, minIndex);
}
```

运行结果：

```text
result = 0
min = -31, index of min is 7
```

## MinMax

获取向量的最大值和最小值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MinMax\_16s\(const int16\_t\* src, int32\_t len, int16\_t\* min, int16\_t\* max\);

    HmppResult HMPPS\_MinMax\_32s\(const int32\_t\* src, int32\_t len, int32\_t\* min, int32\_t\* max\);

    HmppResult HMPPS\_MinMax\_8u\(const uint8\_t\* src, int32\_t len, uint8\_t\* min, uint8\_t\* max\);

    HmppResult HMPPS\_MinMax\_16u\(const uint16\_t\* src, int32\_t len, uint16\_t\* min, uint16\_t\* max\);

    HmppResult HMPPS\_MinMax\_32u\(const uint32\_t\* src, int32\_t len, uint32\_t\* min, uint32\_t\* max\);

- **浮点数的操作：**

    HmppResult HMPPS\_MinMax\_32f\(const float\* src, int32\_t len, float\* min, float\* max\);

    HmppResult HMPPS\_MinMax\_64f\(const double\* src, int32\_t len, double\* min, double\* max\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|min|指向最小值的指针。|非空|输出|
|max|指向最大值的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min、max这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinMaxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t min;
    int16_t max;

    HmppResult result = HMPPS_MinMax_16s(src, BUFFER_SIZE_T, &min, &max);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("min = %d, max = %d\n", min, max);
}
```

运行结果：

```text
result = 0
min = -31, max = 26
```

## MinMaxIndx

获取向量中的最大值、最小值以及它们的索引。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_MinMaxIndx\_8u\(const uint8\_t \*src, int32\_t len, uint8\_t \*min, int32\_t \*minIndx, uint8\_t \*max,int32\_t \*maxIndx\);

    HmppResult HMPPS\_MinMaxIndx\_16u\(const uint16\_t \*src, int32\_t len, uint16\_t \*min, int32\_t \*minIndx,uint16\_t \*max, int32\_t \*maxIndx\);

    HmppResult HMPPS\_MinMaxIndx\_32u\(const uint32\_t \*src, int32\_t len, uint32\_t \*min, int32\_t \*minIndx,uint32\_t \*max, int32\_t \*maxIndx\);

    HmppResult HMPPS\_MinMaxIndx\_16s\(const int16\_t \*src, int32\_t len, int16\_t \*min, int32\_t \*minIndx, int16\_t \*max,int32\_t \*maxIndx\);

    HmppResult HMPPS\_MinMaxIndx\_32s\(const int32\_t \*src, int32\_t len, int32\_t \*min, int32\_t \*minIndx, int32\_t \*max,int32\_t \*maxIndx\);

- **浮点数的操作：**

    HmppResult HMPPS\_MinMaxIndx\_32f\(const float \*src, int32\_t len, float \*min, int32\_t \*minIndx, float \*max,int32\_t \*maxIndx\);

    HmppResult HMPPS\_MinMaxIndx\_64f\(const double \*src, int32\_t len, double \*min, int32\_t \*minIndx, double \*max,int32\_t\* maxIndx\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|min|指向最小值的指针。|非空|输出|
|minIndx|指向最小值的位置索引的指针。|非空|输出|
|max|指向最大值的指针。|非空|输出|
|maxIndx|指向最大值的位置索引的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min、minIndx、max、maxIndx这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MinMaxIndxExample(void)
{
    int16_t src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 15, 26, -31, 1, 23};
    int16_t min;
    int32_t minIndex;
    int16_t max;
    int32_t maxIndex;

    HmppResult result = HMPPS_MinMaxIndx_16s(src, BUFFER_SIZE_T, &min, &minIndex, &max, &maxIndex);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("min = %d, index of min is %d\n", min, minIndex);
    printf("max = %d, index of max is %d\n", max, maxIndex);
}
```

运行结果：

```text
result = 0
min = -31, index of min is 7
max = 26, index of max is 6
```

## Norm

计算向量的1范数（L1）、2范数（L2）或∞范数（Inf）。计算公式如下：

- L1范数

    ![](../../figures/zh-cn_formulaimage_0000002518281800.png)

- L2范数

    ![](../../figures/zh-cn_formulaimage_0000002549921551.png)

- ∞范数

    ![](../../figures/zh-cn_formulaimage_0000002549921561.png)

函数接口声明如下：

- **L1范数：**

    HmppResult HMPPS\_Norm\_L1\_32f\(const float\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_L1\_64f\(const double\* src, int32\_t len, double\* norm\);

    HmppResult HMPPS\_Norm\_L1\_16s32f\(const int16\_t\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_L1\_32fc64f\(const Hmpp32fc\* src, int32\_t len, double\* norm\);

    HmppResult HMPPS\_Norm\_L1\_64fc64f\(const Hmpp64fc\* src, int32\_t len, double\* norm\);

- **有缩放的L1范数：**

    HmppResult HMPPS\_Norm\_L1\_16s32s\_S\(const int16\_t\* src, int32\_t len, int32\_t\* norm, double scale\);

    HmppResult HMPPS\_Norm\_L1\_16s64s\_S\(const int16\_t\* src, int32\_t len, int64\_t\* norm, double scale\);

- **L2范数：**

    HmppResult HMPPS\_Norm\_L2\_32f\(const float\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_L2\_64f\(const double\* src, int32\_t len, double\* norm\);

    HmppResult HMPPS\_Norm\_L2\_16s32f\(const int16\_t\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_L2\_32fc64f\(const Hmpp32fc\* src, int32\_t len, double\* norm\);

    HmppResult HMPPS\_Norm\_L2\_64fc64f\(const Hmpp64fc\* src, int32\_t len, double\* norm\);

- **有缩放的L2范数：**

    HmppResult HMPPS\_Norm\_L2\_16s32s\_S\(const int16\_t\* src, int32\_t len, int32\_t\* norm, double scale\);

- **L2Sqr范数：**

    HmppResult HMPPS\_Norm\_L2Sqr\_16s64s\_S\(const int16\_t\* src, int32\_t len, int64\_t\* norm, double scale\);

- **无穷范数：**

    HmppResult HMPPS\_Norm\_Inf\_32f\(const float\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_Inf\_64f\(const double\* src, int32\_t len, double\* norm\);

    HmppResult HMPPS\_Norm\_Inf\_16s32f\(const int16\_t\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_Inf\_32fc32f\(const Hmpp32fc\* src, int32\_t len, float\* norm\);

    HmppResult HMPPS\_Norm\_Inf\_64fc64f\(const Hmpp64fc\* src, int32\_t len, double\* norm\);

- **有缩放的无穷范数：**

    HmppResult HMPPS\_Norm\_Inf\_16s32s\_S\(const int16\_t\* src, int32\_t len, int32\_t\* norm, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|norm|指向范数的指针。|非空|输出|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、norm这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

void NormExample(void)
{
    float[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 1, 7, 5, -2, 1};
    float norm1[1];
    float norm2[1];
    float norm3[1];
    HmppResult sign1, sign2, sign3;
    sign1 = HMPPS_Norm_Inf_32f(src, BUFFER_SIZE_T, norm1);
    printf("NormInf: result = %d\n", sign1);
    if (sign1 != HMPP_STS_NO_ERR) {
        return;
    }
    printf("Inf = %.2f\n", norm1[0]);
    sign2 = HMPPS_Norm_L1_32f(src, BUFFER_SIZE_T, norm2);
    printf("NormL1: result = %d\n", sign2);
    if (sign2 != HMPP_STS_NO_ERR) {
        return;
    }
    printf("L1 = %.2f\n", norm2[0]);
    sign3 = HMPPS_Norm_L2_32f(src, BUFFER_SIZE_T, norm3);
    printf("NormL2: result = %d\n", sign3);
    if (sign3 != HMPP_STS_NO_ERR) {
        return;
    }
    printf("L2 = %.2f\n", norm3[0]);
    printf("\n");
}
```

运行结果：

```text
NormInf: result = 0
Inf = 8.00
NormL1: result = 0
L1 = 32.00
NormL2: result = 0
L2 = 12.88
```

## NormDiff

两个向量差值的1范数（L1）、2范数（L2）或∞范数（Inf）。计算公式如下：

- L1范数

    ![](../../figures/zh-cn_formulaimage_0000002518281922.png)

- L2范数

    ![](../../figures/zh-cn_formulaimage_0000002518441828.png)

- ∞范数

    ![](../../figures/zh-cn_formulaimage_0000002518441830.png)

函数接口声明如下：

- **L1范数：**

    HmppResult HMPPS\_NormDiff\_L1\_32f\(const float\* src1, const float\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_L1\_64f\(const double\* src1, const double\* src2, int32\_t len, double\* norm\);

    HmppResult HMPPS\_NormDiff\_L1\_16s32f\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_L1\_32fc64f\(const Hmpp32fc\* src1, const Hmpp32fc\* src2, int32\_t len, double\* norm\);

    HmppResult HMPPS\_NormDiff\_L1\_64fc64f\(const Hmpp64fc\* src1, const Hmpp64fc\* src2, int32\_t len, double\* norm\);

- **有缩放的L1范数：**

    HmppResult HMPPS\_NormDiff\_L1\_16s32s\_S\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, int32\_t\* norm, double scale\);

    HmppResult HMPPS\_NormDiff\_L1\_16s64s\_S\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, int64\_t\* norm, double scale\);

- **L2范数：**

    HmppResult HMPPS\_NormDiff\_L2\_32f\(const float\* src1, const float\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_L2\_64f\(const double\* src1, const double\* src2, int32\_t len, double\* norm\);

    HmppResult HMPPS\_NormDiff\_L2\_16s32f\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_L2\_32fc64f\(const Hmpp32fc\* src1, const Hmpp32fc\* src2, int32\_t len, double\* norm\);

    HmppResult HMPPS\_NormDiff\_L2\_64fc64f\(const Hmpp64fc\* src1, const Hmpp64fc\* src2, int32\_t len, double\* norm\);

- **有缩放的L2范数：**

    HmppResult HMPPS\_NormDiff\_L2\_16s32s\_S\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, int32\_t\* norm, double scale\);

- **有缩放的L2Sqr范数**

    HmppResult HMPPS\_NormDiff\_L2Sqr\_16s64s\_S\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, int64\_t\* norm, double scale\);

- **无穷范数**

    HmppResult HMPPS\_NormDiff\_Inf\_32f\(const float\* src1, const float\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_Inf\_64f\(const double\* src1, const double\* src2, int32\_t len, double\* norm\);

    HmppResult HMPPS\_NormDiff\_Inf\_16s32f\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_Inf\_32fc32f\(const Hmpp32fc\* src1, const Hmpp32fc\* src2, int32\_t len, float\* norm\);

    HmppResult HMPPS\_NormDiff\_Inf\_64fc64f\(const Hmpp64fc\* src1, const Hmpp64fc\* src2, int32\_t len, double\* norm\);

- **有缩放的无穷范数**

    HmppResult HMPPS\_NormDiff\_Inf\_16s32s\_S\(const int16\_t\* src1, const int16\_t\* src2, int32\_t len, int32\_t\* norm, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向被减数的指针。|非空|输入|
|src2|指向减数的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|norm|指向范数的指针。|非空|输出|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、norm任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void NormDiffExample(void) {
  float src1[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 1, 7, 5, -2, 1};
  float src2[BUFFER_SIZE_T] = {1, -7, 3, 7, 8, 2, 4, -8, 5, 5};
  float norm1[1];
  float norm2[1];
  float norm3[1];
  HmppResult sign1, sign2, sign3;

  sign1 = HMPPS_NormDiff_Inf_32f(src1, src2, BUFFER_SIZE_T, norm1);
  printf("NormDiffInf: result = %d\n", sign1);
  if (sign1 != HMPP_STS_NO_ERR) {
    return;
  }
  printf("Inf = %.2f\n", norm1[0]);

  sign2 = HMPPS_NormDiff_L1_32f(src1, src2, BUFFER_SIZE_T, norm2);
  printf("NormDiffL1: result = %d\n", sign2);
  if (sign2 != HMPP_STS_NO_ERR) {
    return;
  }
  printf("L1 = %.2f\n", norm2[0]);

  sign3 = HMPPS_NormDiff_L2_32f(src1, src2, BUFFER_SIZE_T, norm3);
  printf("NormDiffL2: result = %d\n", sign3);
  if (sign3 != HMPP_STS_NO_ERR) {
    return;
  }
  printf("L2 = %.2f\n", norm3[0]);
  printf("\n");
}
```

运行结果：

```text
NormDiffInf: result = 0
Inf = 15.00
NormDiffL1: result = 0
L1 = 58.00
NormDiffL2: result = 0
L2 = 23.41
```

## Pwelch

Welch方法是一种修正周期图功率谱密度估计方法，它通过选取的窗口对数据进行加窗处理，先分段求功率谱之后再进行平均。

函数接口声明如下：

HmppResult HMPPS\_Pwelch\_32f\(const float\* src, const float\* window, float\* dst, int32\_t len, double overLap, int32\_t nfft, int32\_t windowLen\);

HmppResult HMPPS\_Pwelch\_64f\(const double\* src, const double\* window, double\* dst, int32\_t len, double overLap, int32\_t nfft, int32\_t windowLen\);

HmppResult HMPPS\_Pwelch\_32fc\(const Hmpp32fc\* src, const float\* window, float\* dst, int32\_t len, double overLap, int32\_t nfft, int32\_t windowLen\);

HmppResult HMPPS\_Pwelch\_64fc\(const Hmpp64fc\* src, const double\* window, double\* dst, int32\_t len, double overLap, int32\_t nfft, int32\_t windowLen\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向保存源信号向量的数组。|非空|输入|
|window|指向保存窗向量的数组。|可以为空。为空时默认为全为1，长度为windowLen。|输入|
|dst|功率谱密度的估计数组。|非空|输出|
|overlap|相邻两段数据之间的重叠部分占window长度的比例。|(0,0.95]|输入|
|nfft|FFT点数。|[INT_MIN,windowLen]，小于零时默认为windowLen。|输入|
|len|源信号向量长度。|(0,INT_MAX]|输入|
|windowLen|窗向量长度。|(0,len]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_SYS_MALLOC_FAILED|函数执行中出现malloc失败。|

**示例**

```c
#define  BUFFER_SIZE_T 5
#define  NFFT 2
#define  WINDOWLEN 2
#define  OVERLAP 0.5
void PwelchExample()
{
    float src[BUFFER_SIZE_T] = {1, 2.5, 3.3, 1, 5};
    float window[2] = {1,1};
    float dst[BUFFER_SIZE_T];
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppResult result = HMPPS_Pwelch_32f(src, window, dst, BUFFER_SIZE_T, OVERLAP, WINDOWLEN, NFFT);
    if (result == HMPP_STS_NO_ERR) {
        printf("dst = ");
        for (int32_t i = 0; i < (NFFT + 1) / 2; i++) {
            printf("%.2f ", dst[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
dst = 0.97 0.18
```

## StdDev

计算向量的标准差。

计算公式为：

![](../../figures/zh-cn_formulaimage_0000002549921541.png)

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_StdDev\_32f\(const float \*src, int32\_t len, float \*stdDev, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_StdDev\_64f\(const double \*src, int32\_t len, double \*stdDev\);

- **整型数的缩放操作：**

    HmppResult HMPPS\_StdDev\_16s\_S\(const int16\_t \*src, int32\_t len, int16\_t \*stdDev, double scale\);

    HmppResult HMPPS\_StdDev\_16s32s\_S\(const int16\_t \*src, int32\_t len, int32\_t \*stdDev, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|stdDev|指向标准差的指针。|非空|输出|
|hint|算法策略。定义在枚举类型HmppHintAlgorithm中，请参见枚举类型。|枚举体HmppHintAlgorithm元素：HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、stdDev这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10

void StdDevExample(void)
{
    float src[BUFFER_SIZE_T] = {4.58, 0.98, 4.30, 8.03, 11.19, 8.41, 4.55, 9.90, 0.14, 9.59};
    float dst;
    HmppHintAlgorithm hint = HMPP_ALGHINT_NONE;

    HmppResult result = HMPPS_StdDev_32f(src, BUFFER_SIZE_T, &dst, hint);
    printf("stdDev: result = %d ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("stdDev = %.2f\n", dst);
}
```

运行结果：

```text
stdDev: result = 0 stdDev = 3.82
```

## Sum

计算向量中所有元素之和。计算公式为：![](../../figures/zh-cn_formulaimage_0000002518281904.png)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Sum\_32f\(const float \*src, int32\_t len, float \*sum, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_Sum\_64f\(const double \*src, int32\_t len, double \*sum\);

    HmppResult HMPPS\_Sum\_32fc\(const Hmpp32fc \*src, int32\_t len, Hmpp32fc \*sum, HmppHintAlgorithm hint\);

    HmppResult HMPPS\_Sum\_64fc\(const Hmpp64fc \*src, int32\_t len, Hmpp64fc \*sum\);

- **整型数的缩放操作：**

    HmppResult HMPPS\_Sum\_16s\_S\(const int16\_t \*src, int32\_t len, int16\_t \*sum, double scale\);

    HmppResult HMPPS\_Sum\_32s\_S\(const int32\_t \*src, int32\_t len, int32\_t \*sum, double scale\);

    HmppResult HMPPS\_Sum\_16s32s\_S\(const int16\_t \*src, int32\_t len, int32\_t \*sum, double scale\);

    HmppResult HMPPS\_Sum\_16sc\_S\(const Hmpp16sc \*src, int32\_t len, Hmpp16sc \*sum, double scale\);

    HmppResult HMPPS\_Sum\_16sc32sc\_S\(const Hmpp16sc \*src, int32\_t len, Hmpp32sc \*sum, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|sum|指向向量和的指针。|非空|输出|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|
|hint|算法策略。定义在枚举类型HmppHintAlgorithm中，请参见枚举类型。|枚举体HmppHintAlgorithm元素：HMPP_ALGHINT_NONEHMPP_ALGHINT_FASTHMPP_ALGHINT_ACCURATE|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、sum这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10

void SumExample(void)
{
    float src[BUFFER_SIZE_T] = {9.99, 5.42, 10.58, 5.20, 9.12, 0.86, 10.13, 5.94, -0.62, 9.19};
    float sum;
    HmppHintAlgorithm hint = HMPP_ALGHINT_NONE;

    HmppResult result = HMPPS_Sum_32f(src, BUFFER_SIZE_T, &sum, hint);
    printf("sum: result = %d ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("sum = %.2f\n", sum);
}
```

运行结果：

```text
sum: result = 0 sum = 65.81
```

## SumLn

计算向量中所有元素的自然对数之和。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002549921641.png)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_SumLn\_32f\(const float\* src, int32\_t len, float\* sum\);

    HmppResult HMPPS\_SumLn\_64f\(const double\* src, int32\_t len, double\* sum\);

- **浮点数的变长操作：**

    HmppResult HMPPS\_SumLn\_32f64f\(const float\* src, int32\_t len, double\* sum\);

- **整型数转浮点数的操作：**

    HmppResult HMPPS\_SumLn\_16s32f\(const int16\_t\* src, int32\_t len, float\* sum\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|sum|指向结果的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、sum这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define  BUFFER_SIZE_T 10

void SumLnExample(void)
{
    float src[BUFFER_SIZE_T] = {11.44, 0.07, 10.23, 8.78, 0.30, 1.72, 1.37, 3.39, 7.37, 7.40};
    float dst = 0.00;  

    HmppResult result = HMPPS_SumLn_32f(src, BUFFER_SIZE_T, &dst);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst = %.2f \n", dst);
} 
```

运行结果：

```text
result = 0
dst = 5954088960.00
```

## TopK

输出源向量最大K个值及其索引。如果源向量中有相同值，则非HMPP\_TOPK\_RADIX算法计算这些相同值所对应的索引不排序，即非HMPP\_TOPK\_RADIX算法是不稳定的，TopK计算后的索引顺序与计算之前的顺序可能不同。

该函数调用流程如下：

1. 调用Init初始化缓冲区buffer内存。
2. 调用主函数。
3. 最后调用Release释放缓冲区buffer内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_TopKInit\_32s\(int32\_t srcLen, int32\_t \*dstValue, int32\_t \*dstIndex, int32\_t dstLen, HmppTopKMode hint, uint8\_t \*\*buffer\);

    HmppResult HMPPS\_TopKInit\_32f\(int32\_t srcLen, float \*dstValue, int32\_t \*dstIndex, int32\_t dstLen, HmppTopKMode hint, uint8\_t \*\*buffer\);

- **主函数操作：**

    HmppResult HMPPS\_TopK\_32s\(const int32\_t \*src, int32\_t srcIndex, int32\_t srcStride, int32\_t srcLen, int32\_t \*dstValue, int32\_t \*dstIndex, int32\_t dstLen, HmppTopKMode hint, uint8\_t \*buffer\);

    HmppResult HMPPS\_TopK\_32f\(const float \*src, int32\_t srcIndex, int32\_t srcStride, int32\_t srcLen, float \*dstValue, int32\_t \*dstIndex, int32\_t dstLen, HmppTopKMode hint, uint8\_t \*buffer\);

- **释放内存操作**：

    HmppResult HMPPS\_TopKRelease\(uint8\_t \*buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcIndex|起始索引（pSrc[0]的索引）。|[0, INT_MAX - srcLen)|输入|
|srcStride|源向量中元素之间的跨度|(0, INT_MAX]|输入|
|srcLen|源向量中的元素长度。|(0, INT_MAX]|输入|
|dstValue|指向值目标向量的指针。|非空|输出|
|dstIndex|指向值索引目标向量的指针。|非空|输出|
|dstLen|目标向量中的元素长度。|(0, INT_MAX]|输入|
|hint|计算使用的算法模型。|HMPP_TOPK_AUTO、HMPP_TOPK_DIRECT或HMPP_TOPK_RADIX|输入|
|buffer（init函数中）|指向计算所需缓冲区的指针的指针。|非空|输出|
|buffer（主函数中和release函数中）|指向计算所需缓冲区的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当srcLen或者dstLen小于或等于0时指示错误。|
|HMPP_STS_BAD_ARG_ERR|指示hint数值不支持时错误。|
|HMPP_STS_OVERFLOW|指示计算溢出时错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**注意**

调用该接口计算之前，必须调用Init接口初始化缓冲区buffer内存和值目标向量、值索引目标向量。

**示例**

```c
#define SRC_LEN_S 7
void TopKExample(void){
    int32_t srcLen = SRC_LEN_S;
    int32_t srcIndex = 0;
    int32_t srcStride = 4;
    int32_t dstLen = SRC_LEN_S;
    HmppTopKMode hint = HMPP_TOPK_AUTO;
    int32_t src[SRC_LEN_S] = { 7, 1, 11, 0, 2, 5, 16 };
    int32_t dstValue[SRC_LEN_S] = { 0 };
    int32_t dstIndex[SRC_LEN_S] = { 0 };
    uint8_t *buffer = NULL;
    HmppResult result;
    result = HMPPS_TopKInit_32s(srcLen, dstValue, dstIndex, dstLen, hint, &buffer);
    printf("HMPPS_TopKInit_32s result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_TopK_32s(src, srcIndex, srcStride, srcLen, dstValue, dstIndex, dstLen, hint, buffer);
    printf("HMPPS_TopK_32s result = %d\n", result);
    HMPPS_TopKRelease(buffer);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    int32_t i;
    printf("dstLen = %d\ndstValue =", dstLen);
    for(i = 0; i < dstLen; ++i) {
        printf(" %d", dstValue[i]);
    }
    printf("\ndstIndex =");
    for(i = 0; i < dstLen; ++i){
        printf(" %d", dstIndex[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_TopKInit_32s result = 0
HMPPS_TopK_32s result = 0
dstLen = 7
dstValue = 16 11 7 5 2 1 0
dstIndex = 6 2 0 5 4 1 3
```

## ZeroCrossing

计算向量跨零次数。

计算模式由参数zcType指定，计算结果保存在pValZC指向的地址中。计算模式包括以下三类：

- ZCType = HMPPZCR，计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441664.png)

- ZCType = HMPPZCXor，计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518281756.png)

- ZCType = HMPPZCC, 计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441652.png)

接口函数的声明如下：

HmppResult HMPPS\_ZeroCrossing\_16s32f\(const int16\_t \*src, uint32\_t len, float \*valZcr, HmppZCType zcType\);

HmppResult HMPPS\_ZeroCrossing\_32f\(const float \*src, uint32\_t len, float \*valZcr, HmppZCType zcType\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|valZcr|指向计算结果的指针。|非空|输出|
|zcType|跨零计算模式。定义在枚举类型HmppZCType中，请参见枚举类型。|枚举体HmppZCType元素：HMPPZCRHMPPZCXorHMPPZCC|输入|
|len|向量长度。|正数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、valZCR这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len为0。|
|HMPP_STS_RANGE_ERR|zcType为无效数值。|

**注意事项**

调用函数时，注意len的数值类型为无符号型，不能误传负数。

**示例**

```c
#define BUFFER_SIZE_T 10

void ZeroCrossingExample(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float valZCR[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(valZCR, BUFFER_SIZE_T); // 数组初始化，将valZCR所有元素初始化为0.
    HmppZCType HmppZCR = HMPP_ZCR;

    HmppResult result = HMPPS_ZeroCrossing_32f(src, 10, valZCR, HmppZCR);
    printf("result = %d.\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("valZCR =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", valZCR[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0.
valZCR = 7.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00
```
