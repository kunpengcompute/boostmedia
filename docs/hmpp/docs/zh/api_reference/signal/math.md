# 数学与复数运算

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Arctan

计算向量中每个元素的反正切值。

计算公式为：_pDst\[n\]=tan<sup>-1</sup>\(pSrc\[n\]\)_，n的范围为\[0，len\)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Arctan\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Arctan\_64f\(const double\* src, double\* dst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Arctan\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Arctan\_64f\_I\(double\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void ArctanExample(void)
{
    float src[BUFFER_SIZE_T] = {4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04, 11.17, 2.79, 3.58};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppResult result = HMPPS_Arctan_32f(src, dst, BUFFER_SIZE_T);
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
```

运行结果：

```text
result = 0
dst = 1.35 1.40 1.38 1.41 1.45 1.47 1.41 1.48 1.23 1.30
```

## Arctan2

计算两个向量的在四个象限的反正切。

计算公式为：_Dst\[n\]=tan<sup>-1</sup>\(Src\[n\]\)_，n的范围为\[0，len\)。

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_Arctan2\_32f\(const float \*src1, const float \*src2, float \*dst, const int32\_tlen\);

HmppResult HMPPS\_Arctan2\_64f\(const double \*src1, const double \*src2, double \*dst, const int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len不大于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void Arctan2Example(void)
{
    float src1[BUFFER_SIZE_T] = {4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04, 11.17, 2.79, 3.58};
    float src2[BUFFER_SIZE_T] = {11.17, 2.79, 3.58, 4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_Arctan2_32f(src1, src2, dst, BUFFER_SIZE_T);
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
```

运行结果：

```text
result = 0
dst = 0.38 1.13 0.96 0.94 0.94 1.09 0.78 0.94 0.27 0.54
```

## Arg

计算两个向量的幅角。

函数接口声明如下。

**主函数操作**：

HmppResult HMPPS\_Arg\_32fc\(const Hmpp32fc\* pSrc, float\* pDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|pSrc|指向源向量指针。|非空|输入|
|pDst|指向目标向量指针。|非空|输出|
|len|源向量与目标向量长度。|(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当srcLen或dstLen小于或等于0时指示错误。|

**示例**

```c
#define BUFFER_SIZE_T 10
void ArgExample(void)
{
    Hmpp32fc src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppResult result = HMPPS_Arg_32fc(src, dst, BUFFER_SIZE_T / 2);
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
    ArgExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0.78 2.56 -3.01 -1.49 -0.69 0.00 0.00 0.00 0.00 0.00
```

## Asin

计算源向量中每个元素的反正弦值。

函数接口声明如下：

HmppResult HMPPS_Asin_32f_A24(const float*pSrc, float* pDst, int len);

**参数**

| 参数名 | 描述                 | 取值范围    | 输入/输出 |
| ------ | -------------------- | ----------- | --------- |
| src    | 指向源向量的指针。   | 非空        | 输入      |
| dst    | 指向目标向量的指针。 | 非空        | 输出      |
| len    | 向量长度。           | (0,INT_MAX] | 输入      |

**返回值**

- 成功：返回HMPP_STS_NO_ERR。
- 失败：返回错误码。

**错误码**

| 错误码                | 描述                 |
| --------------------- | -------------------- |
| HMPP_STS_NULL_PTR_ERR | src、dst存在空指针。 |
| HMPP_STS_SIZE_ERR     | len小于或等于0。     |

**示例**

```c
#include <stdio.h>
#include <hmpp.h>

#define BUFFER_SIZE_T 10

void AsinExample(void)
{
    float src[BUFFER_SIZE_T] = {0.52, 0.92, 0.16, 0.15, 0.17, 0.93, 0.04, 0.17, 0.79, 0.58};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_Asin_32f_A24(src, dst, BUFFER_SIZE_T);
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

int main()
{
    AsinExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0.55 1.17 0.16 0.15 0.17 1.19 0.04 0.17 0.91 0.62
```

## CartToPolar

直角坐标转极坐标。

极角计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281900.png)

极径计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002550041653.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_CartToPolar\_16sc\_S\(const Hmpp16sc \*src, int16\_t \*dstMagn, int16\_t \*dstPhase, int32\_t len, double magnScale, double phaseScale\);

- **浮点数的操作：**

    HmppResult HMPPS\_CartToPolar\_32f\(const float \*srcRe, const float \*srcIm, float \*dstMagn, float \*dstPhase, int32\_t len\);

    HmppResult HMPPS\_CartToPolar\_64f\(const double \*srcRe, const double \*srcIm, double \*dstMagn, double \*dstPhase, int32\_t len\);

    HmppResult HMPPS\_CartToPolar\_32fc\(const Hmpp32fc \*src, float \*dstMagn, float \*dstPhase, int32\_t len\);

    HmppResult HMPPS\_CartToPolar\_64fc\(const Hmpp64fc \*src, double \*dstMagn, double \*dstPhase, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向复数源向量的指针。|非空|输入|
|srcRe|指向复数实部源向量复数的指针。|非空|输入|
|srcIm|指向复数虚部源向量复数的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|dstMagn|指向极径目标向量的指针。|非空|输出|
|dstPhase|指向极角目标向量的指针。|非空，目标向量中的值范围为(-π, π]|输出|
|magnScale|极径的缩放系数。|(0, inf)且为2<sup>n</sup>|输入|
|phaseScale|极角的缩放系数。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|magnScale或者phaseScale不在(0,INF)范围内或输入为NaN。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**示例**

```c
#define SRC_LEN 8
void CartToPolarExample(void)
{
    int32_t len = SRC_LEN;
    float srcRe[SRC_LEN] = { 59.960567, 7.2509279, 1.7840941, 155.84264, 0.0020125117, 0.73378527, 4497.1704, 630.54828 };
    float srcIm[SRC_LEN] = { 2.0548565, 0.00067954202, 0.028709119, 0.0001744011, 0.0054633785, 0.00063873257, 2293.6162, 7.3549595 };
    float dstMagn[SRC_LEN] = { 0.0f };
    float dstPhase[SRC_LEN] = { 0.0f };

    HmppResult result = HMPPS_CartToPolar_32f(srcRe, srcIm, dstMagn, dstPhase, len);
    printf("HMPPS_CartToPolar_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i;
    printf("len = %d\ndstMagn =", len);
    for(i = 0; i < len; ++i){
        printf(" %f", dstMagn[i]);
    }
    printf("\ndstPhase =");
    for(i = 0; i < len; ++i){
        printf(" %f", dstPhase[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_CartToPolar_32f result = 0
len = 8
dstMagn = 59.995770 7.250928 1.784325 155.842636 0.005822 0.733786 5048.288574 630.591187
dstPhase = 0.034257 0.000094 0.016090 0.000001 1.217856 0.000870 0.471626 0.011664
```

## Conj

计算输入复数的共轭复数。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Conj\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Conj\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_Conj\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Conj\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Conj\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Conj\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 5
void ConjExample(void)
{
    Hmpp16sc src[BUFFER_SIZE_T] = {1, 63, 9, 71, 3, 43, 41, 255, 0, 127};
    Hmpp16sc dst[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_Conj_16sc(src, dst, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d   %d   ", dst[i].re, dst[i].im);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1      -63     9     -71      3      -43       41      -255      0     -127
```

## ConjPack

将Pack格式频谱原地还原为共轭对称复数频谱。

函数接口声明如下：

    HmppResult HMPPS\_ConjPack\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t lenDst\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|lenDst|ConjPack目标向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|lenDst小于或等于0。|

**示例**

```c
#include "hmpps.h"
#include <stdio.h>

int main(void)
{
    Hmpp32fc srcDst[6] = {
        {10.0f, 0.0f},
        {1.0f, 2.0f},
        {3.0f, 4.0f},
        {0.0f, 0.0f},
        {0.0f, 0.0f},
        {20.0f, 0.0f}
    };
    HmppResult ret = HMPPS_ConjPack_32fc_I(srcDst, 6);
    printf("ret=%d\n", ret);
    for (int i = 0; i < 6; ++i) {
        printf("(%.6f, %.6f) ", srcDst[i].re, srcDst[i].im);
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
ret=0
(10.000000, 0.000000) (0.000000, 1.000000) (2.000000, 3.000000) (4.000000, 0.000000) (2.000000, -3.000000) (0.000000, -1.000000)
```

## Cos

计算源向量中每个元素的余弦值。

函数接口声明如下：

HmppResult HMPPS\_Cos\_32f\(const float\* src, float\* dst, int32\_t len\);

HmppResult HMPPS\_Cos\_64f\(const double\* src, double\* dst, int32\_t len\);

HmppResult HMPPS\_Cos\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

HmppResult HMPPS\_Cos\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define  BUFFER_SIZE_T 5
void CosExample()
{
    float src[BUFFER_SIZE_T] = {1, 2.5, 3.3, 1, 5};
    float dst[BUFFER_SIZE_T];
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppResult result = HMPPS_Cos_32f(src, dst, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("dst = ");
        for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%.2f ", dst[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
dst = 0.54 -0.80 -0.99 0.54 0.28
```

## CplxToReal

输入复数，输出实部和虚部。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_CplxToReal\_16sc\(const Hmpp16sc\* src, int16\_t\* dstRe, int16\_t\* dstIm, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_CplxToReal\_32fc\(const Hmpp32fc\* src, float\* dstRe, float\* dstIm, int32\_t len\);

    HmppResult HMPPS\_CplxToReal\_64fc\(const Hmpp64fc\* src, double\* dstRe, double\* dstIm, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dstRe|指向复数实部目的向量复数的指针。|非空|输入|
|dstIm|指向复数虚部目的向量复数的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dstRe、dstIm这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 5
void CplxToRealExample(void)
{
    Hmpp16sc src[BUFFER_SIZE_T] = {1, 63, 9, 71, 3, 43, 41, 255, 0, 127};
    int16_t dstRe[BUFFER_SIZE_T];
    int16_t dstIm[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_CplxToReal_16sc(src, dstRe, dstIm, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d   %d   ", dstRe[i], dstIm[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1   63    9   71    3   43    41   255    0   127 
```

## Cubrt

计算每个元素的三次方根。公式为：![](../../figures/zh-cn_formulaimage_0000002549921769.png)。

对于含有scale参数的计算接口，公式为：![](../../figures/zh-cn_formulaimage_0000002550041765.png)。

函数接口声明如下：

HmppResult HMPPS\_Cubrt\_32f\(const float \*src, float \*dst, int32\_t len\);

HmppResult HMPPS\_Cubrt\_32s16s\_S\(const int32\_t \*src, int16\_t \*dst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向计算结果的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**注意**

scale参数取值应在-16到16之间，否则计算结果未定义。

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    float src[BUFFER_SIZE_T] = {1.28, 4.53, 8.79, 4.23, 2.18, 9.69, 5.34, 8.03, 1.90, 8.76};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_Cubrt_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;    
    }

    printf("dst =");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);    
    }
    printf("\n");

    return 0;
}
```

运行结果：

```text
result = 0
dst = 1.09 1.65 2.06 1.62 1.30 2.13 1.75 2.00 1.24 2.06
```

## Exp

以向量中每一个元素为指数，计算以e为底的幂次方。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Exp\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Exp\_64f\(const double\* src, double\* dst, int32\_t len\);

- **浮点复数的操作：**

    HmppResult HMPPS\_Exp\_32fc\_A24\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Exp\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Exp\_64f\_I\(double\* srcDst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Exp\_16s\_S\(const int16\_t\* src, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Exp\_32s\_S\(const int32\_t\* src, int32\_t\* dst, int32\_t len, double scale\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Exp\_16s\_IS\(int16\_t\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Exp\_32s\_IS\(int32\_t\* srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|
|HMPP_STS_OVERFLOW|HMPPS_Exp_32f、HMPPS_Exp_64f、HMPPS_Exp_32fc_A24的计算结果超过了正最大规范数。|
|HMPP_STS_UNDERFLOW|HMPPS_Exp_32f、HMPPS_Exp_64f、HMPPS_Exp_32fc_A24的计算结果小于正最小规范数。|

**示例**

```c
#define BUFFER_SIZE_T 10

void ExpExample(void)  
{
    float src[BUFFER_SIZE_T] = {1.30, 5.34, 4.93, 10.08, 8.64, -0.86, -0.05, 5.63, 2.90, 4.43};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0。
    HmppResult result = HMPPS_Exp_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
} 
```

运行结果：

```text
result = 0
dst = 3.67 208.51 138.38 23860.98 5653.33 0.42 0.95 278.66 18.17 83.93
```

## Imag

获取复数虚部。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Imag\_16sc\(const Hmpp16sc\* src, int16\_t\* dstIm, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Imag\_32fc\(const Hmpp32fc\* src, float\* dstIm, int32\_t len\);

    HmppResult HMPPS\_Imag\_64fc\(const Hmpp64fc\* src, double\* dstIm, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dstlm|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dstIm这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 5
void ImagExample(void)
{
    Hmpp16sc src[BUFFER_SIZE_T] = {1, 63, 9, 71, 3, 43, 41, 255, 0, 127};
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_Imag_16sc(src, dst, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d  ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 63     71      43      255     127
```

## Ln

计算向量中每个元素的自然对数。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Ln\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Ln\_64f\(const double\* src, double\* dst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Ln\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Ln\_64f\_I\(double\* srcDst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Ln\_16s\_S\(const int16\_t\* src, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Ln\_32s\_S\(const int32\_t\* src, int32\_t\* dst, int32\_t len, double scale\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Ln\_16s\_IS\(int16\_t\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Ln\_32s\_IS\(int32\_t\* srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_LN_NEG_ARG|真数为负数。|
|HMPP_STS_LN_ZERO_ARG|真数为0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|
|HMPP_STS_SINGULARITY|src中至少有一个元素等于0。|
|HMPP_STS_DOMAIN|src中至少有一个元素小于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void LnExample(void)   
{ 
    float src[BUFFER_SIZE_T] = {6.17, 6.13, 0.70, 9.23, 3.71, 6.13, 0.90, 10.21, 0.70, 1.12};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_Ln_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n"); 
} 
```

运行结果：

```text
result = 0
dst = 1.82 1.81 -0.36 2.22 1.31 1.81 -0.11 2.32 -0.36 0.11
```

## Log10

计算src中每个元素以10为底的对数，并将结果存储到dst中。

实数公式：_dst\[n\] = log10\(src\[n\]\)_，0 ≤ n < len。

复数公式：_dst\[n\] = log10\(src\[n\].re \* src\[n\].re + src\[n\].im \* src\[n\].im\) / 2 + arctan\(b / a\)i_，0 ≤ n < len。

函数接口声明如下：

**主函数操作：**

HmppResult HMPPS\_Log10\_32f\(const float\* src, float\* dst, int32\_t len\);

HmppResult HMPPS\_Log10\_64f\(const double\* src, double\* dst, int32\_t len\);

HmppResult HMPPS\_Log10\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

HmppResult HMPPS\_Log10\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 警告：返回HMPP\_STS\_DOMAIN或HMPP\_STS\_SINGULARITY。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len、overLap、window、nfft不在有效取值范围内。|
|HMPP_STS_DOMAIN|告警，向量中存在小于0的元素。|
|HMPP_STS_SINGULARITY|告警，向量中存在等于0的元素。|

**示例**

```c
#define  BUFFER_SIZE_T 5
void Log10Example()
{
    float src[BUFFER_SIZE_T] = {1, 2.5, 3.3, 1, 5};
    float dst[BUFFER_SIZE_T];
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0。
    HmppResult result = HMPPS_Log10_32f(src, dst, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("dst = ");
        for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%.2f ", dst[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
dst = 0.00 0.40 0.52 0.00 0.70
```

## Magnitude

计算复数向量的模。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002518281856.png)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Magnitude\_16s32f\(const int16\_t \*srcRe, const int16\_t \*srcIm, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Magnitude\_16sc32f\(const Hmpp16sc\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Magnitude\_32f\(const float\* srcRe, const float\* srcIm, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Magnitude\_64f\(const double\* srcRe, const double\* srcIm, double\* dst, int32\_t len\);

    HmppResult HMPPS\_Magnitude\_32fc\(const Hmpp32fc\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Magnitude\_64fc\(const Hmpp64fc\* src, double\* dst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Magnitude\_16sc\_S\(const Hmpp16sc\* src, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Magnitude\_32sc\_S\(const Hmpp32sc\* src, int32\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Magnitude\_16s\_S\(const int16\_t \*srcRe, const int16\_t \*srcIm, int16\_t \*dst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcRe|指向复数实部源向量的指针。|非空|输入|
|srcIm|指向复数虚部源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INF)且输入为2<sup>n</sup>|输入|
|scale|缩放系数。|[INT_MIN, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void MagnitudeExample(void) {
    float srcRe[BUFFER_SIZE_T] = {-0.10, 0.47, 11.54, 7.41, 9.14,
                                  6.89,  2.73, 8.15,  9.29, 7.94};
    float srcIm[BUFFER_SIZE_T] = {7.10, 3.12, 6.47,  3.87, 9.18,
                                  8.64, 2.00, -1.04, 6.34, 5.19};
    float dst[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_Magnitude_32f(srcRe, srcIm, dst, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 7.10      3.16     13.23      8.36     12.95     11.05      3.38      8.22     11.25      9.49
```

## Normalize

使用偏移和除法运算对实数或复数向量的元素进行归一化，本函数归一化采用将向量的数值平移和缩放到一个范围，即线性归一化。

向量归一化处理。计算公式为：![](../../figures/zh-cn_formulaimage_0000002518441562.png)。

对于原地操作函数，计算公式为：![](../../figures/zh-cn_formulaimage_0000002549921409.png)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Normalize\_32f\(const float \*src, float \*dst, int32\_t len, float sub, float div\);

    HmppResult HMPPS\_Normalize\_64f\(const double \*src, double \*dst, int32\_t len, double sub, double div\);

    HmppResult HMPPS\_Normalize\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len, Hmpp32fc sub, float div\);

    HmppResult HMPPS\_Normalize\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t len, Hmpp64fc sub, double div\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Normalize\_32f\_I\(float \*srcDst, int32\_t len, float sub, float div\);

    HmppResult HMPPS\_Normalize\_64f\_I\(double \*srcDst, int32\_t len, double sub, double div\);

    HmppResult HMPPS\_Normalize\_32fc\_I\(Hmpp32fc \*srcDst, int32\_t len, Hmpp32fc sub, float div\);

    HmppResult HMPPS\_Normalize\_64fc\_I\(Hmpp64fc \*srcDst, int32\_t len, Hmpp64fc sub, double div\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Normalize\_16s\_S\(const int16\_t \*src, int16\_t \*dst, int32\_t len, int16\_t sub, int32\_t div, double scale\);

    HmppResult HMPPS\_Normalize\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc \*dst, int32\_t len, Hmpp16sc sub, int32\_t div, double scale\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Normalize\_16s\_IS\(int16\_t \*srcDst, int32\_t len, int16\_t sub, int32\_t div, double scale\);

    HmppResult HMPPS\_Normalize\_16sc\_IS\(Hmpp16sc \*srcDst, int32\_t len, Hmpp16sc sub, int32\_t div, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|sub|减数（偏移）因子。|视类型而定|输入|
|div|分母因子。|非0|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DIV_BY_ZERO_ERR|div等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

void NormalizeExample(void)
{
    float src[BUFFER_SIZE_T] = {3, 0, 2, -8, 3, 1, 7, 5, -2, 1};
    float dst[BUFFER_SIZE_T];
    float sub = 3;
    float div = 5;
    HmppResult result;
    result = HMPPS_Normalize_32f(src, dst, BUFFER_SIZE_T, sub, div);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.1f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 0.0 -0.6 -0.2 -2.2 0.0 -0.4 0.8 0.4 -1.0 -0.4
```

## PolarToCart

极坐标转直角坐标。

公式如下：

![](../../figures/zh-cn_formulaimage_0000002549921393.png)

![](../../figures/zh-cn_formulaimage_0000002549921401.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResultHMPPS\_PolarToCart\_16sc\_S\(constint16\_t\*srcMagn, constint16\_t\*srcPhase, Hmpp16sc\*dst, int32\_tlen, doublemagnScale, double phaseScale\);

- **浮点数的操作：**

    HmppResultHMPPS\_PolarToCart\_32f\(constfloat\*srcMagn, constfloat\*srcPhase, float\*dstRe, float\*dstIm, int32\_tlen\);

    HmppResultHMPPS\_PolarToCart\_64f\(constdouble\*srcMagn, constdouble\*srcPhase, double\*dstRe, double\*dstIm, int32\_tlen\);

    HmppResultHMPPS\_PolarToCart\_32fc\(constfloat\*srcMagn, constfloat\*srcPhase, Hmpp32fc\*dst, int32\_tlen\);

    HmppResultHMPPS\_PolarToCart\_64fc\(constdouble\*srcMagn, constdouble\*srcPhase, Hmpp64fc\*dst, int32\_tlen\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcMagn|指向极径源向量的指针。|非空|输入|
|srcPhase|指向极角源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|dst|指向复数目标向量的指针。|非空|输出|
|dstRe|指向复数实部目标向量的指针。|非空|输出|
|dstIm|指向复数虚部目标向量的指针。|非空|输出|
|magnScale|极径的缩放系数。|(0, inf)且为2<sup>n</sup>|输入|
|phaseScale|极角的缩放系数。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|magnScale或者phaseScale不在(0,INF)范围内或输入为NaN。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|

**示例**

```c
#define SRC_LEN 8
void PolarToCartExample(void)
{
    int32_t len = SRC_LEN;
    float srcMagn[SRC_LEN] = { 4.94, -2.39, -6.89, 54602.84, 8.17, 9.61, -7.003, 8.9 };
    float srcPhase[SRC_LEN] = { 4.0, 2.67, -1.02, -1.23, -6.84, -5.73, 3.89, 9.54 };
    float dstRe[SRC_LEN] = { 0.0f };
    float dstIm[SRC_LEN] = { 0.0f };

    HmppResult result = HMPPS_PolarToCart_32f(srcMagn, srcPhase, dstRe, dstIm, len);
    printf("HMPPS_PolarToCart_32f result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i;
    printf("len = %d\ndstRe =", len);
    for(i = 0; i < len; ++i){
        printf(" %f", dstRe[i]);
    }
    printf("\ndstIm =");
    for(i = 0; i < len; ++i){
        printf(" %f", dstIm[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_PolarToCart_32f result = 0
len = 8
dstRe = -3.228999 2.129122 -3.605992 18250.328125 6.935862 8.176719 5.131613 -8.840986
dstIm = -3.738604 -1.085790 5.871024 -51462.562500 -4.317721 5.049095 4.765349 -1.023208
```

## Pow

此向量是将每个src1向量的src2次幂的计算结果存储到dst中。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002518441680.png)。

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_Pow\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

HmppResult HMPPS\_Pow\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DOMAIN|表明src向量中至少有一个元素小于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void PowExample(void)
{
    float src1[BUFFER_SIZE_T] = {4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04, 11.17, 2.79, 3.58};
    float src2[BUFFER_SIZE_T] = {11.17, 2.79, 3.58, 4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_Pow_32f(src1, src2, dst, BUFFER_SIZE_T);
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
```

运行结果：

```text
result = 0
dst = 20792004.00 142.82 355.87 3678.88 251393.86 139398.48 63588.30 365253632.00 26598.05 2215.41
```

## Powx

计算源向量中每个元素的恒定幂次方。

函数接口声明如下：

HmppResult HMPPS\_Powx\_32f\_A11\(const float \*src, const float constValue, float \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_32f\_A21\(const float \*src, const float constValue, float \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_32f\_A24\(const float \*src, const float constValue, float \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_64f\_A26\(const double \*src, const double constValue, double \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_64f\_A50\(const double \*src, const double constValue, double \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_64f\_A53\(const double \*src, const double constValue, double \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_32fc\_A11\(const Hmpp32fc \*src, const Hmpp32fc constValue, Hmpp32fc \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_32fc\_A21\(const Hmpp32fc \*src, const Hmpp32fc constValue, Hmpp32fc \*dst, int32\_t len\);

HmppResult HMPPS\_Powx\_32fc\_A24\(const Hmpp32fc \*src, const Hmpp32fc constValue, Hmpp32fc \*dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|constValue|幂运算的指数。|不限，视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 警告：返回HMPP\_STS\_DOMAIN或HMPP\_STS\_SINGULARITY。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DOMAIN|告警，向量中存在小于0的元素且指数constValue为小数。|
|HMPP_STS_SINGULARITY|告警，向量中存在等于0的元素且指数constValue为负数。|

**示例**

```c
#define  BUFFER_SIZE_T 5
void PowxExample()
{
    float src[BUFFER_SIZE_T] = {1, 2.5, 3.3, 1, 5};
    float constValue = 2;
    float dst[BUFFER_SIZE_T];
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0。
    HmppResult result = HMPPS_Powx_32f_A11(src, constValue, dst, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("dst = ");
        for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%.2f ", dst[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
dst = 1.00 6.25 10.89 1.00 25.00
```

## PowerSpectr

计算复数向量的功率谱，计算公式如下：![](../../figures/zh-cn_formulaimage_0000002518441558.png)。

函数接口声明如下：

- **整型数的计算：**

    HmppResult HMPPS\_PowerSpectr\_16s32f\(const int16\_t\* srcRe, const int16\_t\* srcIm, float\* dst, int32\_t len\);

    HmppResult HMPPS\_PowerSpectr\_16sc32f\(const Hmpp16sc\* src, float\* dst, int32\_t len\);

- **浮点数的运算：**

    HmppResult HMPPS\_PowerSpectr\_32f\(const float\* srcRe, const float\* srcIm, float\* dst, int32\_t len\);

    HmppResult HMPPS\_PowerSpectr\_64f\(const double\* srcRe, const double\* srcIm, double\* dst, int32\_t len\);

    HmppResult HMPPS\_PowerSpectr\_32fc\(const Hmpp32fc\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_PowerSpectr\_64fc\(const Hmpp64fc\* src, double\* dst, int32\_t len\);

- **带缩放的整型数计算：**

    HmppResult HMPPS\_PowerSpectr\_16s\_S\(const int16\_t\* srcRe, const int16\_t\* srcIm, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_PowerSpectr\_16sc\_S\(const Hmpp16sc\* src, int16\_t\* dst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcRe|指向输入实部向量的指针。|非空|输入|
|srcIm|指向输入虚部向量的指针。|非空|输入|
|src|指向输入复数向量的指针。|非空|输入|
|dst|指向输出功率谱密度向量的指针。|非空|输出|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcRe，srcIm或src为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 5
void PowerSpectrExample(void)
{
    float srcRe[BUFFER_SIZE_T] = {0.0, 1.0, 2.0, 4.5, 150.0};
    float srcIm[BUFFER_SIZE_T] = {0.5, 1.5, 2.5, 4.0, 150.0};
    float dst[BUFFER_SIZE_T];   
    int32_t i;
    HmppResult result = HMPPS_PowerSpectr_32f(srcRe, srcIm, dst, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f  ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 0.250000  3.250000  10.250000  36.250000  45000.000000
```

## Real

获取复数实部。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Real\_16sc\(const Hmpp16sc\* src, int16\_t\* dstRe, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Real\_32fc\(const Hmpp32fc\* src, float\* dstRe, int32\_t len\);

    HmppResult HMPPS\_Real\_64fc\(const Hmpp64fc\* src, double\* dstRe, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dstRe|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dstRe任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 5
void RealExample(void)
{
    Hmpp16sc src[BUFFER_SIZE_T] = {1, 63, 9, 71, 3, 43, 41, 255, 0, 127};
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_Real_16sc(src, dst, BUFFER_SIZE_T);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d  ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1   9    3    41     0
```

## RealToCplx

输入实部虚部，合成复数。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_RealToCplx\_16s\(const int16\_t\* srcRe, const int16\_t\* srcIm, Hmpp16sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_RealToCplx\_32f\(const float\* srcRe, const float\* srcIm, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_RealToCplx\_64f\(const double\* srcRe, const double\* srcIm, Hmpp64fc\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcRe|指向复数实部源向量复数的指针。|非空|输入|
|srcIm|指向复数虚部源向量复数的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcRe、srcIm、dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 5
void RealToCplxExample(void)
{
    int16_t srcRe[BUFFER_SIZE_T] = {1, 63, 9, 71, 3};
    int16_t srcIm[BUFFER_SIZE_T] = {43, 41, 255, 0, 127};
    Hmpp16sc dst[BUFFER_SIZE_T];
    int32_t i;
    HmppResult result = HMPPS_RealToCplx_16s(srcRe, srcIm, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d   %d   ", dst[i].re, dst[i].im);
    } 
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1       43       63     41      9        255       71       0       3       127
```

## Phase

求给定复数向量的相位角。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Phase\_16s32f\(const int16\_t\* srcRe, const int16\_t\* srcIm, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Phase\_16sc32f\(const Hmpp16sc\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Phase\_16s\_S\(const int16\_t\* srcRe, const int16\_t\* srcIm, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Phase\_16sc\_S\(const Hmpp16sc\* src, int16\_t\* dst, int32\_t len, double scale\);

- **浮点数的操作：**

    HmppResult HMPPS\_Phase\_32f\(const float\* srcRe, const float\* srcIm, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Phase\_64f\(const double\* srcRe, const double\* srcIm, double\* dst, int32\_t len\);

    HmppResult HMPPS\_Phase\_32fc\(const Hmpp32fc\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Phase\_64fc\(const Hmpp64fc\* src, double\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|src|指向复数向量序列的指针。|视类型而定|输入|
|srcRe|指向复数实部向量序列的指针。|视类型而定|输入|
|srcIm|指向复数虚部向量序列的指针。|视类型而定|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    double dst[BUFFER_SIZE_T];
    int32_t i;
    double src1[BUFFER_SIZE_T] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    double src2[BUFFER_SIZE_T] = {41.7918, 171.61, 55.8247, 85.8605, 93.5198, 275.385, 229.065, 302.278, 64.373, 309.137};

    HmppResult result = HMPPS_Phase_64f(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f    ", dst[i]);
    }

    return 0;
} 
```

运行结果：

```text
result = 0
dst = 1.570796     1.564969     1.534985     1.535870     1.528051     1.552642     1.544609     1.547643     1.447155     1.541691
```

## Sin

计算源向量中每个元素的正弦值。

函数接口声明如下：

HmppResult HMPPS\_Sin\_32f\(const float\* src, float\* dst, int32\_t len\);

HmppResult HMPPS\_Sin\_64f\(const double\* src, double\* dst, int32\_t len\);

HmppResult HMPPS_Sin_64f_A50(const double*pSrc, double* pDst, int len);

HmppResult HMPPS\_Sin\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

HmppResult HMPPS\_Sin\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include <hmpp.h>

#define BUFFER_SIZE_T 5
void SinExample()
{
    float src[BUFFER_SIZE_T] = {1, 2.5, 3.3, 1, 5};
    float dst[BUFFER_SIZE_T];
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T);  // 数组初始化，将dst所有元素初始化为0。
    HmppResult result = HMPPS_Sin_32f(src, dst, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("dst = ");
        for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%.2f ", dst[i]);
        }
        printf("\n");
    }
}

int main()
{
    SinExample();
    return 0;
}
```

运行结果：

```text
dst = 0.84 0.60 -0.16 0.84 -0.96
```

## Sqrt

计算向量每个元素的平方根。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002518441852.png)

对于原地操作函数，计算公式为：![](../../figures/zh-cn_formulaimage_0000002550041707.png)

复数开方的计算公式为：

![](../../figures/zh-cn_formulaimage_0000002549921709.png)

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Sqrt\_32f\(const float \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_64f\(const double \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t len\);

- **整型数的缩放操作：**

    HmppResult HMPPS\_Sqrt\_8u\_S\(const uint8\_t \*src, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16u\_S\(const uint16\_t \*src, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16s\_S\(const int16\_t \*src, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_32s16s\_S\(const int32\_t \*src, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc \*dst, int32\_t len, double scale\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Sqrt\_32f\_I\(float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_64f\_I\(double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_32fc\_I\(Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqrt\_64fc\_I\(Hmpp64fc \*srcDst, int32\_t len\);

- **整型数的原地缩放操作：**

    HmppResult HMPPS\_Sqrt\_8u\_IS\(uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16u\_IS\(uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16s\_IS\(int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqrt\_16sc\_IS\(Hmpp16sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SQRT_NEG_ARG|被开方的数为负数。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define  BUFFER_SIZE 10
void SqrtExample(void)
{
    float src[BUFFER_SIZE] = { 1.28, 4.53, 8.79, 4.23, 2.18, 9.69, 5.34, 8.03, 1.90, 8.76};
    float dst[BUFFER_SIZE] = { 0.00 };
    HmppResult result;
    int32_t i = 0;
    result = HMPPS_Sqrt_32f(src, dst, BUFFER_SIZE);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst = ");
    for(; i < BUFFER_SIZE; ++i){
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1.13 2.13 2.96 2.06 1.48 3.11 2.31 2.83 1.38 2.96
```

## Tan

计算向量的正切值。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002518281784.png)。

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_Tan\_32f\(const float \*src, float \*dst, int32\_t len\);

HmppResult HMPPS\_Tan\_64f\(const double \*src, double \*dst, int32\_t len\);

HmppResult HMPPS_Tan_64f_A50(const double *src, double*dst, int32_t len);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include <hmpp.h>

#define BUFFER_SIZE_T 10

void TanExample(void)
{
    float src[BUFFER_SIZE_T] = {4.52, 5.92, 5.16, 6.15, 8.17, 9.93, 6.04, 11.17, 2.79, 3.58};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_Tan_32f(src, dst, BUFFER_SIZE_T);
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

int main()
{
    TanExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 5.13 -0.38 -2.08 -0.13 -3.06 0.55 -0.25 -5.67 -0.37 0.47
```
