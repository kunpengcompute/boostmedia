# 信号变换

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## CZT

适合于计算当取样频率间隔（sampling frequency interval）与取样时间间隔（sampling time interval）乘积的倒数不等于信号的时频分布面积时的算法。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002518441608.png)，![](../../figures/zh-cn_formulaimage_0000002518281712.png)

上式等同于FFT。

函数接口声明如下：

**主函数操作：**

HmppResult HMPPS\_CZT\_32f\(const float \*src, int32\_t srcLen, Hmpp32fc \*dst, int32\_t dstLen, Hmpp32fc w, Hmpp32fc a\);

HmppResult HMPPS\_CZT\_64f\(const double \*src, int32\_t srcLen, Hmpp64fc \*dst, int32\_t dstLen, Hmpp64fc w, Hmpp64fc a\);

HmppResult HMPPS\_CZT\_32fc\(const Hmpp32fc \*src, int32\_t srcLen, Hmpp32fc \*dst, int32\_t dstLen, Hmpp32fc w, Hmpp32fc a\);

HmppResult HMPPS\_CZT\_64fc\(const Hmpp64fc \*src, int32\_t srcLen, Hmpp64fc \*dst, int32\_t dstLen, Hmpp64fc w, Hmpp64fc a\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量指针。|非空|输入|
|srcLen|源向量元素个数。|(0, INT_MAX]|输入|
|dst|指向目标向量指针。|非空|输入|
|dstLen|目标向量元素个数。|(0, INT_MAX]|输出|
|w|z平面螺旋轮各点之间的比值。|通常模长为1|输入|
|a|z平面螺旋轮的起点。|通常模长为1|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当指定指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当srcLen或dstLen小于或等于0时指示错误。|
|HMPP_STS_OVERFLOW_ERR|数据规模(srcLen, dstLen)过大。|
|HMPP_STS_MALLOC_FAILED|运算过程中所需的内存分配失败。|

**示例**

```c
void Convolve_Example()
{
    const int srcLen = 10;
    const int dstLen = 20;
    float src[srcLen] = {9.244539, 0.686178, 4.528434, 7.181965, 6.123716, 5.890331, 2.779223, 1.576141, 3.751002, 8.829503};
    Hmpp32fc dst[dstLen];
    Hmpp32fc a = {0.306101, 0.951999};
    Hmpp32fc w = {-0.562033, -0.827115};
    HMPPS_CZT_32f(src, srcLen, dst, dstLen, w, a);
    for (int i = 0; i < dstLen; ++i) {
        printf("%.5f + %.5fi ", dst[i].re, dst[i].im);
    }
}
```

运行结果：

```text
7.35142 + 13.7323i, 14.6336 + 4.04731i, 6.05243 + 4.48011i, 17.1773 + -1.37873i, 10.6023 + -5.12952i, -3.68937 + 11.8929i, 2.38356 + -4.45827i, 5.83655 + 2.65852i, 17.3432 + 32.6026i, 5.83083 + 9.56965i, 16.495 + 1.73368i, 49.9786 + 6.41107i, 17.4045 + 2.00529i, 10.1817 + -10.0219i, 28.4027 + -31.2852i, 8.1634 + -4.72801i, 0.304818 + 0.486745i, -2.72213 + -18.6895i, 7.71386 + 4.53396i, 14.2174 + 5.30225i,
```

## DFT

计算任意长度实数序列、复数序列的正向/逆向傅里叶变换。

正向变换：![](../../figures/zh-cn_formulaimage_0000002550041523.png)

逆向变换：![](../../figures/zh-cn_formulaimage_0000002549921537.png)

![](../../figures/zh-cn_formulaimage_0000002550041535.png)

DFT（Discrete Fourier Transform）函数调用流程如下：

1. 调用Init初始化HmppsDFTPolicy结构体。
2. 调用CToC、RToC、CToR等主函数。
3. 最后调用Release释放。HmppsDFTPolicy函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_DFTCToCInit\_32f\(int32\_t len, int32\_t direction, int32\_t flag, HmppsDFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_DFTCToCInit\_64f\(int32\_t len, int32\_t direction, int32\_t flag, HmppsDFTPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_DFTCToCInit\_32fc\(int32\_t len, int32\_t direction, int32\_t flag, HmppsDFTPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_DFTCToCInit\_64fc\(int32\_t len, int32\_t direction, int32\_t flag, HmppsDFTPolicy\_64fc \*\*policy\);

    HmppResult HMPPS\_DFTRToCInit\_32f\(int32\_t len, int32\_t flag, HmppsDFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_DFTRToCInit\_64f\(int32\_t len, int32\_t flag, HmppsDFTPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_DFTCToRInit\_32f\(int32\_t len, int32\_t flag, HmppsDFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_DFTCToRInit\_64f\(int32\_t len, int32\_t flag, HmppsDFTPolicy\_64f \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_DFTCToC\_32f\(float \*srcRe, float \*srcIm, float \*dstRe, float \*dstIm, HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTCToC\_64f\(double \*srcRe, double \*srcIm, double \*dstRe, double \*dstIm, HmppsFFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_DFTCToC\_32fc\(Hmpp32fc \*src, Hmpp32fc \*dst, HmppsFFTPolicy\_32fc \*policy\);

    HmppResult HMPPS\_DFTCToC\_64fc\(Hmpp64fc \*src, Hmpp64fc \*dst, HmppsFFTPolicy\_64fc \*policy\);

    HmppResult HMPPS\_DFTRToC\_32f\(float \*src, Hmpp32fc \*dst, HmppsDFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTRToC\_64f\(double \*src, Hmpp64fc \*dst, HmppsDFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_DFTCToR\_32f\(Hmpp32fc \*src, float \*dst, HmppsDFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTCToR\_64f\(Hmpp64fc \*src, double \*dst, HmppsDFTPolicy\_64f \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_DFTCToCRelease\_32f\(HmppsDFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTCToCRelease\_64f\(HmppsDFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_DFTCToCRelease\_32fc\(HmppsDFTPolicy\_32fc \*policy\);

    HmppResult HMPPS\_DFTCToCRelease\_64fc\(HmppsDFTPolicy\_64fc \*policy\);

    HmppResult HMPPS\_DFTRToCRelease\_32f\(HmppsDFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTRToCRelease\_64f\(HmppsDFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_DFTCToRRelease\_32f\(HmppsDFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_DFTCToRRelease\_64f\(HmppsDFTPolicy\_64f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|len|FFT序列输入信号长度。|(0, 2<sup>27</sup>]|输入|
|direction|direction=1表示FFT正变换。direction=-1表示FFT逆变换。用于CToC模式。|±1|输入|
|flag|结果正规化模式。|HMPP_FFT_DIV_FWD_BY_N、HMPP_FFT_DIV_BWD_BY_N、HMPP_FFT_DIV_BY_SQRTN、HMPP_FFT_NODIV_BY_ANY|输入|
|policy（init函数中）|双重指针，指向HmppsDFTPolicy结构体，结构体内包含DFT计算需要的一些信息和缓存块的首地址。|非空|输出|
|policy（主函数和release函数）|指针，指向HmppsDFTPolicy结构体。|非空|输入|
|src|指向源序列的指针。|非空|输入|
|dst|指向输出序列的指针。|非空|输出|
|srcDst|指向原地操作序列的指针。|非空|输入/输出|

参数flag取值的说明

|取值|描述|
|--|--|
|HMPP_FFT_DIV_FWD_BY_N|正向变换，1/N正规化模式。|
|HMPP_FFT_DIV_BWD_BY_N|反向变换，1/N正规化模式。|
|HMPP_FFT_DIV_BY_SQRTN|正向或反向变换，1/N1/2正规化模式。|
|HMPP_FFT_NODIV_BY_ANY|正向或反向变换，不做特殊处理。|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|指针参数为空。|
|HMPP_STS_SIZE_ERR|len小于0。|
|HMPP_STS_MALLOC_FAILED|所需的额外内存申请失败。|
|HMPP_STS_FFT_FLAG_ERR|flag不是HMPP_FFT_DIV_FWD_BY_N、HMPP_FFT_DIV_BWD_BY_N、HMPP_FFT_DIV_BY_SQRTN或HMPP_FFT_NODIV_BY_ANY。|

**注意**

- 调用该接口计算DFT之前，必须调用HMPPS\_DFTInit接口初始化HmppsDFTPolicy规范结构。
- HmppsDFTPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。

**示例**

- DFTCToC调用示例：

    ```c
    #define PI 3.14159265358979323846
    
    void DFTCToC_Example()
    {
        Hmpp32fc src[8], dst[8];
        for (int32_t i = 0; i < 8; i++) {
            src[i].re = cos(2 * PI * i * 16 / 64);
            src[i].im = 1;
        }
        HmppResult result;
        HmppsDFTPolicy_32fc *policy = NULL;
        
        result = HMPPS_DFTCToCInit_32fc(8, 1, HMPP_FFT_NODIV_BY_ANY, &policy);// 正向FFT
        if (result != HMPP_STS_NO_ERR) {
            printf("Create Policy Error!\n");
            return;
        }
        result = HMPPS_DFTCToC_32fc(src, dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("DFT Error!\n");
            return;
        }
        HMPPS_DFTCToCRelease_32fc(policy);
        printf("dstRe =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", dst[i].re);
        }
        printf("\ndstIm =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", dst[i].im);
        }
        printf("\n");
    }
    ```

    运行结果：

    ```text
    dstRe =    -0.00    -0.00    4.00    0.00    0.00    0.00    4.00    -0.00
    dstIm =    8.00    -0.00    -0.00    -0.00    0.00    0.00    0.00    0.00
    ```

- DFTRToC/CToR调用示例：

    ```c
    void DFT_R_Example()
    {
        float src[8];
        Hmpp32fc rtoc_dst[5];
        float ctor_dst[8] = {0};
        for (int32_t i = 0; i < 8; i++) {
            src[i] = i + 1;
        }
        HmppResult result;
        HmppsDFTPolicy_32f *policy = NULL;
    
        result = HMPPS_DFTRToCInit_32f(8, HMPP_FFT_NODIV_BY_ANY, &policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("DFTRToC Create Policy Error!\n");
            return;
        }
        result = HMPPS_DFTRToC_32f(src, rtoc_dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("DFTRToC Error!\n");
            return;
        }
        HMPPS_DFTRToCRelease_32f(policy);
        printf("rtoc_dstRe =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", rtoc_dst[i].re);
        }
        printf("\nrtoc_dstIm =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", rtoc_dst[i].im);
        }
        printf("\n");
    
        result = HMPPS_DFTCToRInit_32f(8, HMPP_FFT_NODIV_BY_ANY, &policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("DFTCToR Create Policy Error!\n");
            return;
        }
        result = HMPPS_DFTCToR_32f(rtoc_dst, ctor_dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("DFTCToR Error!\n");
            return;
        }
        HMPPS_DFTCToRRelease_32f(policy);
        printf("ctor_dst =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", ctor_dst[i]);
        }
        printf("\n");
    }
    ```

    运行结果：

    ```text
    rtoc_dstRe =    36.00    -4.00    -4.00    -4.00    -4.00    1.00    3.00    5.00
    rtoc_dstIm =    0.00    9.66    4.00    1.66    0.00    2.00    4.00    6.00
    ctor_dst =    8.00    16.00    24.00    32.00    40.00    48.00    56.00    64.00
    ```

## FFT

计算2的幂次长度的实数序列、复数序列的正向/逆向快速傅里叶变换。

正向变换：![](../../figures/zh-cn_formulaimage_0000002550041555.png)

逆向变换：![](../../figures/zh-cn_formulaimage_0000002518441700.png)

![](../../figures/zh-cn_formulaimage_0000002550041543.png)

FFT函数调用流程如下：

1. 调用Init初始化HmppsFFTPolicy结构体。
2. 调用CToC、RToC、CToR等主函数。
3. 最后调用Release释放HmppsFFTPolicy函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_FFTCToCInit\_32f\(int32\_t power, int32\_t direction, int32\_t flag, HmppsFFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_FFTCToCInit\_64f\(int32\_t power, int32\_t direction, int32\_t flag, HmppsFFTPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_FFTCToCInit\_32fc\(int32\_t power, int32\_t direction, int32\_t flag, HmppsFFTPolicy\_32fc \*\*policy\);

    HmppResult HMPPS\_FFTCToCInit\_64fc\(int32\_t power, int32\_t direction, int32\_t flag, HmppsFFTPolicy\_64fc \*\*policy\);

    HmppResult HMPPS\_FFTRToCInit\_32f\(int32\_t power, int32\_t flag, HmppsFFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_FFTRToCInit\_64f\(int32\_t power, int32\_t flag, HmppsFFTPolicy\_64f \*\*policy\);

    HmppResult HMPPS\_FFTCToRInit\_32f\(int32\_t power, int32\_t flag, HmppsFFTPolicy\_32f \*\*policy\);

    HmppResult HMPPS\_FFTCToRInit\_64f\(int32\_t power, int32\_t flag, HmppsFFTPolicy\_64f \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_FFTCToC\_32f\(float \*srcRe, float \*srcIm, float \*dstRe, float \*dstIm, HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_FFTCToC\_64f\(double \*srcRe, double \*srcIm, double \*dstRe, double \*dstIm, HmppsFFTPolicy\_64f \*policy\)

    HmppResult HMPPS\_FFTCToC\_32fc\(Hmpp32fc \*src, Hmpp32fc \*dst, HmppsFFTPolicy\_32fc \*policy\);

    HmppResult HMPPS\_FFTCToC\_64fc\(Hmpp64fc \*src, Hmpp64fc \*dst, HmppsFFTPolicy\_64fc \*policy\)

    HmppResult HMPPS\_FFTCToC\_32fc\_I\(Hmpp32fc \*srcDst, HmppsFFTPolicy\_32fc \*policy\);

    HmppResult HMPPS\_FFTCToC\_64fc\_I\(Hmpp64fc \*srcDst, HmppsFFTPolicy\_64fc \*policy\);

    HmppResult HMPPS\_FFTRToC\_32f\(float \*src, Hmpp32fc \*dst, HmppsFFTPolicy\_32f \*policy\)

    HmppResult HMPPS\_FFTRToC\_64f\(double \*src, Hmpp64fc \*dst, HmppsFFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_FFTCToR\_32f\(Hmpp32fc \*src, float \*dst, HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_FFTCToR\_64f\(Hmpp64fc \*src, double \*dst, HmppsFFTPolicy\_64f \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_FFTCToCRelease\_32f\(HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_FFTCToCRelease\_64f\(HmppsFFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_FFTCToCRelease\_32fc\(HmppsFFTPolicy\_32fc \*policy\);

    HmppResult HMPPS\_FFTCToCRelease\_64fc\(HmppsFFTPolicy\_64fc \*policy\);

    HmppResult HMPPS\_FFTRToCRelease\_32f\(HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_FFTRToCRelease\_64f\(HmppsFFTPolicy\_64f \*policy\);

    HmppResult HMPPS\_FFTCToRRelease\_32f\(HmppsFFTPolicy\_32f \*policy\);

    HmppResult HMPPS\_FFTCToRRelease\_64f\(HmppsFFTPolicy\_64f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|power|FFT序列输入信号长度为![](../../figures/zh-cn_formulaimage_0000002518281788.png)。|[0, 27]|输入|
|direction|direction=1表示FFT正变换。direction=-1表示FFT逆变换。用于CToC模式。|±1|输入|
|flag|结果正规化模式。|HMPP_FFT_DIV_FWD_BY_N、HMPP_FFT_DIV_BWD_BY_N、HMPP_FFT_DIV_BY_SQRTN、HMPP_FFT_NODIV_BY_ANY|输入|
|policy（init函数中）|双重指针，指向HmppsFFTPolicy结构体，结构体内包含FFT计算需要的一些信息和缓存块的首地址。|非空|输出|
|policy（主函数和release函数）|指针，指向HmppsFFTPolicy结构体。|非空|输入|
|src|指向源序列的指针。|非空|输入|
|dst|指向输出序列的指针。|非空|输出|
|srcDst|指向原地操作序列的指针。|非空|输入/输出|

参数flag取值的说明

|取值|描述|
|--|--|
|HMPP_FFT_DIV_FWD_BY_N|正向变换，1/N正规化模式。|
|HMPP_FFT_DIV_BWD_BY_N|反向变换，1/N正规化模式。|
|HMPP_FFT_DIV_BY_SQRTN|正向或反向变换，1/N1/2正规化模式。|
|HMPP_FFT_NODIV_BY_ANY|正向或反向变换，不做特殊处理。|

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

**注意**

- 调用该接口计算FFT之前，必须调用HMPPS\_FFTCToCInit接口初始化HmppsFFTPolicy规范结构。
- HmppsFFTPolicy结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。

**示例**

- FFTCToC调用示例：

    ```c
    #define PI 3.14159265358979323846
    
    void FFTCToC_Example()
    {
        Hmpp32fc src[8], dst[8];
        for (int32_t i = 0; i < 8; i++) {
            src[i].re = cos(2 * PI * i * 16 / 64);
            src[i].im = 1;
        }
        HmppResult result;
        HmppsFFTPolicy_32fc *policy = NULL;
        
        result = HMPPS_FFTCToCInit_32fc(3, 1, HMPP_FFT_NODIV_BY_ANY, &policy);// 正向FFT
        if (result != HMPP_STS_NO_ERR) {
            printf("Create Policy Error!\n");
            return;
        }
        result = HMPPS_FFTCToC_32fc(src, dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("FFT Error!\n");
            return;
        }
        HMPPS_FFTCToCRelease_32fc(policy);
        printf("dstRe =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", dst[i].re);
        }
        printf("\ndstIm =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", dst[i].im);
        }
        printf("\n");
    }
    ```

    运行结果：

    ```c
    dstRe =    -0.00    -0.00    4.00    0.00    0.00    0.00    4.00    -0.00
    dstIm =    8.00    -0.00    -0.00    -0.00    0.00    0.00    0.00    0.00
    ```

- FFTRToC/CToR调用示例：

    ```c
    void FFT_R_Example()
    {
        float src[8];
        Hmpp32fc rtoc_dst[5];
        float ctor_dst[8] = {0};
        for (int32_t i = 0; i < 8; i++) {
            src[i] = i + 1;
        }
        HmppResult result;
        HmppsFFTPolicy_32f *policy = NULL;
    
        result = HMPPS_FFTRToCInit_32f(3, HMPP_FFT_NODIV_BY_ANY, &policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("RToC Create Policy Error!\n");
            return;
        }
        result = HMPPS_FFTRToC_32f(src, rtoc_dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("FFTRToC Error!\n");
            return;
        }
        HMPPS_FFTRToCRelease_32f(policy);
        printf("rtoc_dstRe =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", rtoc_dst[i].re);
        }
        printf("\nrtoc_dstIm =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", rtoc_dst[i].im);
        }
        printf("\n");
    
        result = HMPPS_FFTCToRInit_32f(3, HMPP_FFT_NODIV_BY_ANY, &policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("CToR Create Policy Error!\n");
            return;
        }
        result = HMPPS_FFTCToR_32f(rtoc_dst, ctor_dst, policy);
        if (result != HMPP_STS_NO_ERR) {
            printf("FFTCToR Error!\n");
            return;
        }
        HMPPS_FFTCToRRelease_32f(policy);
        printf("ctor_dst =");
        for (int32_t i = 0; i < 8; i++) {
            printf("    %.2f", ctor_dst[i]);
        }
        printf("\n");
    }
    ```

    运行结果：

    ```c
    rtoc_dstRe =    36.00    -4.00    -4.00    -4.00    -4.00    1.00    3.00    5.00
    rtoc_dstIm =    0.00    9.66    4.00    1.66    0.00    2.00    4.00    6.00
    ctor_dst =    8.00    16.00    24.00    32.00    40.00    48.00    56.00    64.00
    ```

## FFTShift

将零频点移到频谱的中间，对于一维信号，其实就是入参数组左半部和右半部对换。

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_FFTShift\_32f\(const float \*src, float \*dst, int32\_t len\);

HmppResult HMPPS\_FFTShift\_64f\(const double \*src, double \*dst, int32\_t len\);

HmppResult HMPPS\_FFTShift\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

HmppResult HMPPS\_FFTShift\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

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
|HMPP_STS_NULL_PTR_ERR|src、dst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10
void FFTShift_Example(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    int32_t i;

    HmppResult result = HMPPS_FFTShift_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%.2f    ", dst[i]);
    }

}
```

运行结果：

```text
result = 0
dst = -0.43      0.41      -4.83      5.36       -4.40      1.64      1.63      -1.09      0.71      -3.20
```

## FFTThread

- **设置多线程数上限：**

    HmppResult HMPPS\_SetFFTNumberThreads\(int32\_t fftNumberThreads\);

- **获取当前的线程数：**

    HmppResult HMPPS\_GetFFTNumberThreads\(int32\_t\* fftNumberThreads\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|fftNumberThreads|要限定的线程数上限（SetFFTNumberThreads）。|大于0|输入|
|fftNumberThreads|目标地址，指向内存存放当前线程数（GetFFTNumberThreads）。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|传入指针是空指针。|
|HMPP_STS_BAD_ARG_ERR|入参fftNumberThreads不合法。|

**示例**

```c
#define NUMBER_THREAD_FFT 4
void FFT_Thread_Example()
{
    int curNum = 0;
    HMPP_GetNumberThreads(&curNum);
    printf("curNum = %d\n", curNum);

    HMPP_SetFFTNumberThreads(NUMBER_THREAD_FFT);
    int num = 0;
    HMPP_GetNumberThreads(&num);
    printf("num = %d\n", num);
}
```

运行结果：

```text
curNum = 8
num = 4
```

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>
>- HMPP默认设置FFT多线程数为8，可通过本节提供的HMPP\_SetFFTNumberThreads函数设置本次任务执行时使用的FFT线程数，非永久有效。
>- 同时可通过环境变量HMPP\_FFT\_THREAD\_NUM设置FFT线程数。

## Goertz

为单个信号计算给定频率的离散傅里叶变换。

![](../../figures/zh-cn_formulaimage_0000002518281740.png)

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_Goertz\_32f\(const float \*src, int32\_t len, Hmpp32fc \*res, float freq\)

HmppResult HMPPS\_Goertz\_64f\(const double \*src, int32\_t len, Hmpp64fc \*res, double freq\);

HmppResult HMPPS\_Goertz\_32fc\(const Hmpp32fc \*src, int32\_t len, Hmpp32fc \*res, float freq\);

HmppResult HMPPS\_Goertz\_64fc\(const Hmpp64fc \*src, int32\_t len, Hmpp64fc \*res, double freq\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|源向量长度。|(0,INT_MAX]|输入|
|res|指向结果值的指针。|非空|输出|
|freq|傅里叶变换频率|[0.0, 1.0)|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、res这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_REL_FREQ_ERR|freq不在[0.0, 1.0)范围内。|

**示例**

```c
void Goertz_Example(void)
{
    float src[7] = {1, 2, 3, 4, 5, 6, 7};
    Hmpp32fc res;
    int32_t i;

    HmppResult result = HMPPS_Goertz_32f(src, 7, &res, 1.0 / 7);
    printf("re = %f, im = %f\n", res.re, res.im);
}
```

运行结果：

```text
re = -3.499998, im = 7.267825
```

## Hilbert

该函数计算复解析信号dst，该解析信号dst包含原始实信号src作为实部，计算希尔伯特变换作为虚部。Hilbert变换是根据spec规范参数执行的：样本数len和hint。输入数据将补零或截断为len的大小。

Hilbert函数调用流程如下：

1. 调用Init初始化HmppsHilbertPolicy\_32f结构体。
2. 调用主函数。
3. 最后调用Release释放HmppsHilbertPolicy\_32f函数所包含内存。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_HilbertInit\_32f32fc\(int32\_t len, HmppsHilbertPolicy\_32f32fc \*\*policy\);

- **主函数操作：**

    HmppResult HMPPS\_Hilbert\_32f32fc\(const float \*src, Hmpp32fc \*dst, int32\_t len, HmppsHilbertPolicy\_32f32fc \*policy\);

- **释放内存操作**：

    HmppResult HMPPS\_HilbertRelease\_32f32fc\(HmppsHilbertPolicy\_32f32fc \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|源向量。|非空|输入|
|dst|目标向量。|非空|输出|
|len|向量长度。|(0, INT_MAX]|输入|
|policy（init函数中）|双重指针，指向HmppsHilbertPolicy。|非空|输出|
|policy（主函数中和release函数中）|指针，指向HmppsHilbertPolicy结构体的指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NO_ERR|表示没有错误。|
|HMPP_STS_NULL_PTR_ERR|当指定的任一指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|当len小于或等于0时指示错误。|
|HMPP_STS_MALLOC_FAILED|Init函数中进行算法模型所需内存申请失败。|
|HMPP_STS_MISMATCH|Init函数申请内存的问题规模和主函数中实际计算的问题规模不匹配。|

**注意**

- 调用该接口计算之前，必须调用Init接口初始化HmppsHilbertPolicy\_32f规范结构。
- HmppsHilbertPolicy\_32f结构体初始化需在Init函数中进行申请的，用户无法自己进行该结构体申请定义。

**示例**

```c
void Hilbert_Example()
{
    const int len = 10;
    float src[len];
    Hmpp32fc dst[len];
    HmppsHilbertPolicy_32f32fc *policy = NULL;

    for (int i = 0; i < 10; ++i){
        src[i] = i / 10.0;
    }
    HmppResult result = HMPPS_HilbertInit_32f32fc(len, &policy);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_Hilbert_32f32fc(src, dst, len, policy);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    result = HMPPS_HilbertRelease_32f32fc(policy);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (int i = 0; i < len; ++i) {
        printf("%.2f + %.6fi ", dst[i].re, dst[i].im);
    }

}
```

运行结果：

```text
0 + 0.550553i, 0.1 + -0.0649839i, 0.2 + -0.0649839i, 0.3 + -0.210292i, 0.4 + -0.210292i, 0.5 + -0.210292i, 0.6 + -0.210292i, 0.7 + -0.0649839i, 0.8 + -0.0649839i, 0.9 + 0.550553i,
```

## WT

小波变换：小波前向初始化函数、小波反向初始化函数、小波前向设置延迟线函数、小波反向设置延迟线函数、小波前向获取延迟线函数、小波反向获取延迟线函数、小波前向变换、小波反向变换。

函数调用流程如下：

1. 调用Init初始化小波变换状态结构。
2. 调用主函数。
3. 最后调用Release释放小波变换状态结构。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_WTFwdInit\_32f\(HmppsWTFwdState\_32f\*\* state, const float\* tapsLow, int32\_t lenLow, int32\_t offsLow, const float\* tapsHigh, int32\_t lenHigh, int32\_t offsHigh\);

    HmppResult HMPPS\_WTInvInit\_32f \(HmppsWTInvState\_32f\*\* state, const float\* tapsLow, int32\_t lenLow, int32\_t offsLow, const float\* tapsHigh, int32\_t lenHigh, int32\_t offsHigh\);

- **主函数操作：**

    HmppResult HMPPS\_WTFwdSetDlyLine\_32f\(HmppsWTFwdState\_32f\* state, const float\* dlyLow, const float\* dlyHigh\);

    HmppResult HMPPS\_WTInvSetDlyLine\_32f\(HmppsWTInvState\_32f\* state, const float\* dlyLow, const float\* dlyHigh\);

    HmppResult  HMPPS\_WTFwdGetDlyLine\_32f \(HmppsWTFwdState\_32f\* state, float\* dlyLow, float\* dlyHigh\);

    HmppResult  HMPPS\_WTInvGetDlyLine\_32f\(HmppsWTInvState\_32f\* state, float\* dlyLow, float\* dlyHigh\);

    HmppResult  HMPPS\_WTFwd\_32f\(const float\* src, float\* dstLow, float\* dstHigh, int32\_t dstLen, HmppsWTFwdState\_32f\* state\);

    HmppResult HMPPS\_WTInv\_32f\(const float\* srcLow, const float\* srcHigh, int32\_t srcLen, float\* dst, HmppsWTInvState\_32f\* state\);

- **释放内存操作**：

    void HMPPS\_WTFwdRelease\_32f\(HmppsWTFwdState\_32f \*state\);

    void HMPPS\_WTInvRelease\_32f\(HmppsWTInvState\_32f \*state\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|state|Init函数：指向初始化的前向小波变换状态结构指针的指针。主函数中：指向状态结构的指针。|非空指针|输入，输出|
|tapsLow|指向低通滤波器抽头向量的指针。|非空指针|输入|
|lenLow|低通滤波器的抽头数。|正整数|输入|
|offsLow|低通滤波器的输入延迟（偏移量）。|大于等于-1|输入|
|tapsHigh|指向高通滤波器抽头向量的指针。|非空指针|输入|
|lenHigh|高通滤波器的抽头数。|正整数|输入|
|offsHigh|高通滤波器的输入延迟（偏移量）。|大于等于-1|输入|
|dlyLow|指向持有“低频”分量延迟线的向量的指针。|非空指针|输入|
|dlyHigh|指向持有“高频”分量延迟线的向量的指针。|非空指针|输入|
|src|指向保存用于分解的输入信号的向量的指针。|非空指针|输入|
|dstLow|指向包含输出粗略“低频”分量的向量的指针。|非空指针|输入，输出|
|dstHigh|指向包含输出详细“高频”分量的向量的指针。|非空指针|输入，输出|
|dstLen|向量dstHigh和dstLow中的元素数。|正整数|输入|
|srcLow|指向存放输入粗略“低频”分量的向量的指针。|非空指针|输入|
|srcHigh|指向存放详细“高频”分量的向量的指针。|非空指针|输入|
|srcLen|向量srcHigh和srcLow中的元素数。|正整数|输入|
|dst|指向保存输出重构信号的向量的指针。|非空指针|输入，输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_SIZE_ERR|lenHigh和lenLow中的任何一个小于等于0/dstLen小于等于0/srcLen小于等于0。|
|HMPP_STS_WT_OFFSET_ERR|offsLow和offsHigh中任意一个小于-1。|
|HMPP_STS_NULL_PTR_ERR|出现空指针。|
|HMPP_STS_CONTEXT_MATCH_ERR|状态结构体state的元素为空或不符合要求。|

**HMPPS\_WTFwd\_32f示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"

#define LEN 12

int main()
{
    float src[LEN] = {1,2,3,4,5,6,7,8,9,10,11,12};
    float tapsLow[4] = {1,2,3,4};
    float tapsHigh[4] = {1,2,3,4};
    float dstLow[LEN/2];
    float dstHigh[LEN/2];
    int32_t offsetLow = -1;
    int32_t offsetHigh = -1;
    int32_t lenLow = 4;
    int32_t lenHigh = 4;
    HmppsWTFwdState_32f* state;
    HMPPS_WTFwdInit_32f(&state,tapsLow,lenLow,offsetLow,tapsHigh,lenHigh,offsetHigh);
    HMPPS_WTFwdSetDlyLine_32f(state,&src[0],&src[0]);
    HmppResult result = HMPPS_WTFwd_32f(src,dstLow,dstHigh,6,state);
    printf("result = %d\n",result);
    if (result != HMPP_STS_NO_ERR){
       return 0;
    }
    printf("dstLow =");
    for(int i = 0;i<LEN/2;i++){
       printf("%.2f",dstLow[i]);
    }
    printf("\n");
    for(int i = 0;i<LEN/2;i++){
       printf("%.2f",dstHigh[i]);
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
result = 0
dstLow =14.00 20.00 40.00 60.00 80.00 100.00 
dstHigh = 14.00 20.00 40.00 60.00 80.00 100.00
```

**HMPPS\_WTInv\_32f示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"

#define LEN 12

int main()
{
    float srcLow[LEN/2] = {80,20,40,60,80,100};
    float srcHigh[LEN/2] = {84,20,40,60,80,100};
    float tapsLow[4] = {1,2,3,4};
    float tapsHigh[4] = {1,2,3,4};
    float dst[LEN];
    int32_t offsetLow = 0;
    int32_t offsetHigh = 0;
    int32_t lenLow = 4;
    int32_t lenHigh = 4;
    HmppsWTInvState_32f* state;
    HMPPS_WTInvInit_32f(&state,tapsLow,lenLow,offsetLow,tapsHigh,lenHigh,offsetHigh);
    HMPPS_WTInvSetDlyLine_32f(state,srcLow,srcHigh);
    HmppResult result = HMPPS_WTInv_32f(srcLow,srcHigh,6,dst,state);
    printf("result = %d\n",result);
    if (result != HMPP_STS_NO_ERR){
       return 0;
    }
    printf("dst =");
    for(int i =0;i<LEN;i++){
       printf(" %.2f",dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst =656.00 984.00 532.00 736.00 200.00 320.00 360.00 560.00 520.00 800.00 680.00 1040.00
```

## WTHaar

实现小波前向哈尔变换和小波反向哈尔变换。

**小波前向哈尔变换：**

该函数对len长度信号src进行前向单级离散Haar变换，并将分解后的粗低频分量存储在dstLow中，将详细的高频分量存储在dstHigh中。

HmppResult HMPPS\_WTHaarFwd\_32f\(const float\* src, int32\_t len, float\* dstLow, float\* dstHigh\);

HmppResult HMPPS\_WTHaarFwd\_64f\(const double\* src, int32\_t len, double\* dstLow, double\* dstHigh\);

**小波反向哈尔变换：**

该函数对粗“低频”分量srcLow和细“高频”分量srcHigh进行反向单级离散Haar变换，并将重构信号存储在len长度矢量dst中。

HmppResult HMPPS\_WTHaarInv\_32f\(const float\* srcLow, const float\* srcHigh, float\* dst, int32\_t len\);

HmppResult HMPPS\_WTHaarInv\_64f\(const double\* srcLow, const double\* srcHigh, double\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向变换源向量的指针。|非空指针|输入|
|srcLow|指向具有反向变换输入的粗略“低频”分量的数组的指针。|非空指针|输入|
|srcHigh|指向具有反向变换输入的详细“高频”分量的数组的指针。|非空指针|输入|
|len|前向：源向量中的元素个数。反向：目标向量中的元素个数。|正整数|输入|
|dstLow|指向具有用于前向变换的输出的粗略“低频”分量的数组的指针。|非空指针|输出|
|dstHigh|指向具有用于前向变换的输出的详细“高频”分量的数组的指针。|非空指针|输出|
|dst|指向带有逆变换输出信号的数组的指针。|非空指针|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_SIZE_ERR|变换源向量的长度小于0。|
|HMPP_STS_NULL_PTR_ERR|出现空指针。|

**HMPPS\_WTHaarFwd\_32f示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"

#define LEN 10

int main()
{
    float src[LEN] = {0.43,1.56,2.34,-4.56,0.76,1.89,-3.41,0.58,0.61,1.92};
    float dstLow[LEN / 2];
    float dstHigh[LEN / 2];
    HmppResult result = HMPPS_WTHaarFwd_32f(src, LEN, dstLow, dstHigh);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR){
        return 0;
    }
    printf("dstLow =");
    for(int i = 0;i < LEN / 2;i++){
        printf("%.2f ", dstLow[i]);
    }
    printf("\n");
    printf("dstHigh =");
    for(int i = 0; i < LEN / 2; i++){
        printf("%.2f ", dstHigh[i]);
    }
    printf("\n");


    return 0;
}
```

运行结果：

```text
result = 0
dstLow =0.99 -1.11 1.33 -1.42 1.26 
dstHigh =0.56 -3.45 0.56 2.00 0.65
```

**HMPPS\_WTHaarInv\_32f示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpps.h"

#define LEN 10

int main()
{
    float srcHigh[LEN/2]={1.64,1.63,-1.09,0.71,-3.20};
    float srcLow[LEN/2]={-0.43,0.41,-4.83,5.36,-4.40};
    float dst[LEN];
    HmppResult result = HMPPS_WTHaarInv_32f(srcLow, srcHigh, dst, LEN);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR){
       return 0;
    }
    printf("dst =");
    for (int i = 0; i<LEN; i++) {
       printf(" %.2f", dst[i]);
    }
    printf("\n");


    return 0;
}
```

运行结果：

```text
result = 0
dst = -2.07 1.21 -1.22 2.04 -3.74 -5.92 4.65 6.07 -1.20 -7.60
```
