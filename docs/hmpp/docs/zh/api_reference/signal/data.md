# 数据处理与信号生成

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Convert

此类函数转换源向量中的每个元素的数据类型，结果保存在目的向量中。

带\_S后缀的函数根据scale值对结果值进行缩放。如果转换后的结果超出输出数据范围，则将达到饱和。

对float16\_t型数据进行转换时函数不支持HMPP\_RND\_FINANCIAL舍入模式。

函数接口声明如下：

- **整型转整型的操作：**

    HmppResult HMPPS\_Convert\_24u32u\(const uint8\_t \*src, uint32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_8s8u\(const int8\_t \*src, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_8s16s\(const int8\_t \*src, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_16s32s\(const int16\_t \*src, int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_24s32s\(const uint8\_t \*src, int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_32s16s\(const int32\_t \*src, int16\_t \*dst, int32\_t len\);

- **整型转浮点的操作：**

    HmppResult HMPPS\_Convert\_24u32f\(const uint8\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_16s16f\(const int16\_t \*src, float16\_t \*dst, int32\_t len, HmppRoundMode roundMode\);

    HmppResult HMPPS\_Convert\_8u32f\(const uint8\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_8s32f\(const int8\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_16u32f\(const uint16\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_16s32f\(const int16\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_24s32f\(const uint8\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_32s32f\(const int32\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_32s64f\(const int32\_t \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_64s64f\(const int64\_t \*src, double \*dst, int32\_t len\);

- **浮点转浮点的操作：**

    HmppResult HMPPS\_Convert\_32f16f\(const float \*src, float16\_t \*dst, int32\_t len, HmppRoundMode rndMode\);

    HmppResult HMPPS\_Convert\_32f64f\(const float \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_16f32f\(const float16\_t \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Convert\_64f32f\(const double \*src, float \*dst, int32\_t len\);

- **有缩放的整型转整型操作：**

    HmppResult HMPPS\_Convert\_8u8s\_S\(const uint8\_t \*src, int8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\)

    HmppResult HMPPS\_Convert\_16s8s\_S\(const int16\_t \*src, int8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32u24u\_S\(const uint32\_t \*src, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_32s16s\_S\(const int32\_t \*src, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_32s24s\_S\(const int32\_t \*src, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_64s32s\_S\(const int64\_t \*src, int32\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

- **有缩放的整型转浮点操作：**

    HmppResult HMPPS\_Convert\_16s32f\_S\(const int16\_t \*src, float \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_16s64f\_S\(const int16\_t \*src, double \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_32s32f\_S\(const int32\_t \*src, float \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_32s64f\_S\(const int32\_t \*src, double \*dst, int32\_t len, double scale\);

- **有缩放的浮点转整型操作：**

    HmppResult HMPPS\_Convert\_32f8u\_S\(const float \*src, uint8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32f8s\_S\(const float \*src, int8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32f16u\_S\(const float \*src, uint16\_t \*dst, int32\_t len,HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32f16s\_S\(const float \*src, int16\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32f32s\_S\(const float \*src, int32\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f8u\_S\(const double \*src, uint8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f8s\_S\(const double \*src, int8\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f16u\_S\(const double \*src, uint16\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f16s\_S\(const double \*src, int16\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f32s\_S\(const double \*src, int32\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_64f64s\_S\(const double \*src, int64\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_16f16s\_S\(const float16\_t \*src, int16\_t \*dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Convert\_32f24u\_S\(const float \*src, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Convert\_32f24s\_S\(const float \*src, uint8\_t \*dst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|源向量长度。|(0,INT_MAX]或[3, INT_MAX]|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|
|rndMode|舍入模式。定义在枚举类型HmppRoundMode中，请参见枚举类型。|枚举体HmppRoundMode元素：HMPP_RND_ZERO、HMPP_RND_NEAR、HMPP_RND_FINANCIAL|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0或len小于3。|
|HMPP_STS_NOT_SUPPORT|当前数据类型转换不支持参数传入的round mode。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
void ConvertExample()
{
    const int8_t src[BUFFER_SIZE_S] = { 123, 32, 0, -123, 3, -128, 32, -127, 64 };
    uint8_t dst[BUFFER_SIZE_S] = {0};
    int32_t i;
    HmppResult result = HMPPS_Convert_8s8u(src, dst, BUFFER_SIZE_S);
    printf("result = %d \ndst =", result);
    for(i = 0; i < BUFFER_SIZE_S; i++){
        printf(" %u ", dst[i]);
    }
    printf("\n");
}

```

运行结果：

```text
result = 0 
dst = 123  32  0  0  3  0  32  0  64
```

## Copy

源地址数据拷贝到目的地址。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Copy\_8u\(const uint8\_t\* src, uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_32s\(const int32\_t \*src, int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_64s\(const int64\_t\* src, int64\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_32sc\(const Hmpp32sc\* src, Hmpp32sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_64sc\(const Hmpp64sc\* src, Hmpp64sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Copy\_32f\(const float \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_64f\(const double \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Copy\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

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

int main()
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    int32_t i;

    HmppResult result = HMPPS_Copy_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%.2f    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 1.64      1.63      -1.09      0.71      -3.20      -0.43      0.41      -4.83      5.36       -4.40
```

## Flip

向量反转。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Flip\_8u\(const uint8\_t\* src, uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Flip\_16u\(const uint16\_t\* src, uint16\_t\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Flip\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Flip\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_Flip\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_Flip\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Flip\_8u\_I\(uint8\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Flip\_16u\_I\(uint16\_t\* srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Flip\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Flip\_64f\_I\(double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Flip\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Flip\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

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
|HMPP_STS_NULL_PTR_ERR|dst、src、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9
void FlipExample()
{
    uint8_t src[BUFFER_SIZE_T] = {0, 1, 2, 3, 4, 5, 6, 7, 255};
    uint8_t dst[BUFFER_SIZE_T] = {};
    int32_t i;
    HmppResult result = HMPPS_Flip_8u(src, dst, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", dst[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
dst = 255 7 6 5 4 3 2 1 0
```

## GetLibVersion

函数接口声明如下。

**获取HMPP当前版本信息：**

const HmppLibraryVersion\* HMPPS\_GetLibVersion \(\);

**返回值**

返回指向存有版本信息的HmppLibraryVersion类型变量的首地址。

**错误码**

无

**注意**

不要释放指针指向的内存。

**示例**

```c
 void  GetLibVersionExample()
{
    const HmppLibraryVersion *libVersion = HMPPS_GetLibVersion();
    printf("HMPP_VERSION_MAJOR = %d\n", libVersion->major);
    printf("HMPP_VERSION_MINOR = %d\n", libVersion->minor);
    printf("HMPP_VERSION_PATCH = %d\n", libVersion->patch);
    printf("HMPP_VERSION_BUILDDATE = %s\n", libVersion->buildDate);
}
```

运行结果：

```text
HMPP_VERSION_MAJOR = 1
HMPP_VERSION_MINOR = 0
HMPP_VERSION_PATCH = 0
HMPP_VERSION_BUILDDATE = 2020.04.27
```

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>具体值参见hmpp.h头文件中定义。

## GetStatusString

**获取状态码描述：**

const char\* HMPPS\_GetStatusString\(HmppResult result\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|result|状态码。|HMPPResult中出现的枚举类型。|输入|

**返回值**

- 成功：返回状态码的对应描述。
- 失败：返回"Not Found This Error Description"。

**错误码**

无

**注意**

不要释放返回指针指向的内存。

**示例**

```c
void  GetStatusStringExample()
{
    printf("%s\n", HMPPS_GetStatusString(HMPP_STS_NO_ERR));
    printf("%s\n", HMPPS_GetStatusString(HMPP_STS_NULL_PTR_ERR));
    printf("%s\n", HMPPS_GetStatusString(HMPP_STS_SIZE_ERR));
    printf("%s\n", HMPPS_GetStatusString(HMPP_STS_NOT_SUPPORT));
}
```

运行结果：

```text
No Error
Null Pointer Error
Vector size <= 0 Error
This system does not support this function
```

## Malloc和Free

函数接口声明如下：

- **申请指定类型长度数组所需内存：**

    uint8\_t \*HMPPS\_Malloc\_8u\(int32\_t len\);

    uint16\_t \*HMPPS\_Malloc\_16u\(int32\_t len\);

    uint32\_t \*HMPPS\_Malloc\_32u\(int32\_t len\);

    uint64\_t \*HMPPS\_Malloc\_64u\(int32\_t len\);

    int8\_t \*HMPPS\_Malloc\_8s\(int32\_t len\);

    int16\_t \*HMPPS\_Malloc\_16s\(int32\_t len\);

    int32\_t \*HMPPS\_Malloc\_32s\(int32\_t len\);

    int64\_t \*HMPPS\_Malloc\_64s\(int32\_t len\);

    float \*HMPPS\_Malloc\_32f\(int32\_t len\);

    double \*HMPPS\_Malloc\_64f\(int32\_t len\);

    Hmpp8sc \*HMPPS\_Malloc\_8sc\(int32\_t len\);

    Hmpp16sc \*HMPPS\_Malloc\_16sc\(int32\_t len\);

    Hmpp32sc \*HMPPS\_Malloc\_32sc\(int32\_t len\);

    Hmpp64sc \*HMPPS\_Malloc\_64sc\(int32\_t len\);

    Hmpp32fc \*HMPPS\_Malloc\_32fc\(int32\_t len\);

    Hmpp64fc \*HMPPS\_Malloc\_64fc\(int32\_t len\);

- **释放内存：**

    void HMPPS\_Free\(void\* ptr\);

## Move

把源地址数据转移到目的地址。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Move\_8u\(const uint8\_t\* src, uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Move\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Move\_32s\(const int32\_t \*src, int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Move\_64s\(const int64\_t\* src, int64\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Move\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Move\_32sc\(const Hmpp32sc\* src, Hmpp32sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Move\_64sc\(const Hmpp64sc\* src, Hmpp64sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Move\_32f\(const float \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Move\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_Move\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Move\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

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

int main()
{
    uint8_t src[BUFFER_SIZE_T] = {1, 63, 9, 71, 3, 43, 41, 255, 0, 127};
    uint8_t dst[BUFFER_SIZE_T];
    int32_t i;

    HmppResult result = HMPPS_Move_8u(src, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 1    63    9    71    3    43    41    255    0    127
```

## RandGauss

产生给定均值、标准差的符合正态分布随机序列。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_RandGaussInit\_8u\(HmppsRandGaussPolicy\_8u \*\*policy, uint8\_t mean, uint8\_t stdDev, uint32\_t seed\);

    HmppResult HMPPS\_RandGaussInit\_16s\(HmppsRandGaussPolicy\_16s \*\*policy, int16\_t mean, int16\_t stdDev, uint32\_t seed\);

    HmppResult HMPPS\_RandGaussInit\_32f\(HmppsRandGaussPolicy\_32f \*\*policy, float mean, float stdDev, uint32\_t seed\);

    HmppResult HMPPS\_RandGaussInit\_64f\(HmppsRandGaussPolicy\_64f \*\*policy, double mean, double stdDev, uint32\_t seed\);

- **主函数操作：**

    HmppResult HMPPS\_RandGauss\_8u\(uint8\_t \*dst, int32\_t len, HmppsRandGaussPolicy\_8u \*policy\);

    HmppResult HMPPS\_RandGauss\_16s\(int16\_t \*dst, int32\_t len, HmppsRandGaussPolicy\_16s \*policy\);

    HmppResult HMPPS\_RandGauss\_32f\(float \*dst, int32\_t len, HmppsRandGaussPolicy\_32f \*policy\);

    HmppResult HMPPS\_RandGauss\_64f\(double \*dst, int32\_t len, HmppsRandGaussPolicy\_64f \*policy\);

- **释放内存操作：**

    HmppResult HMPPS\_RandGaussRelease\_8u\(HmppsRandGaussPolicy\_8u \*policy\);

    HmppResult HMPPS\_RandGaussRelease\_16s\(HmppsRandGaussPolicy\_16s \*policy\);

    HmppResult HMPPS\_RandGaussRelease\_32f\(HmppsRandGaussPolicy\_32f \*policy\);

    HmppResult HMPPS\_RandGaussRelease\_64f\(HmppsRandGaussPolicy\_64f \*policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|mean|均值。|视类型而定|输入|
|stdDev|标准差。|视类型而定|输入|
|seed|随机数种子。|视类型而定|输入|
|policy|产生随机序列参数结构体。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_MALLOC_FAILED|所需的额外内存申请失败。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    float dst[BUFFER_SIZE_T];
    int32_t i;
    float mean = 1;
    float stdDev = 1;
    float seed = 0;
    HmppsRandGaussPolicy_32f *policy = NULL;

    HmppResult result;
    result = HMPPS_RandGaussInit_32f(&policy, mean, stdDev, seed);
    if (result != HMPP_STS_NO_ERR)
    {
        return;
    }
    result = HMPPS_RandGauss_32f(dst, BUFFER_SIZE_T, policy);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f    ", dst[i]);
    }

    HMPPS_RandGaussRelease_32f(policy);

    return 0;
}
```

运行结果：

```text
result = 0
dst = 0.839658  0.516040  0.405604  0.981364  2.000319  -0.703950  0.667630  1.412678  1.724619  1.078169 
```

## RandUniform

产生给定均值、标准差的均匀分布随机序列。

函数接口声明如下：

- **初始化操作：**

    HmppResult HMPPS\_RandUniformInit\_8u\(HmppsRandUniformPolicy\_8u\*\* policy, uint8\_t low, uint8\_t high, uint32\_t seed\);

    HmppResult HMPPS\_RandUniformInit\_16s\(HmppsRandUniformPolicy\_16s\*\* policy, int16\_t low, int16\_t high, uint32\_t seed\);

    HmppResult HMPPS\_RandUniformInit\_32f\(HmppsRandUniformPolicy\_32f\*\* policy, float low, float high, uint32\_t seed\);

    HmppResult HMPPS\_RandUniformInit\_64f\(HmppsRandUniformPolicy\_64f\*\* policy, double low, double high, uint32\_t seed\);

- **主函数操作：**

    HmppResult HMPPS\_RandUniform\_8u\(uint8\_t\* dst, int32\_t len, HmppsRandUniformPolicy\_8u\* policy\);

    HmppResult HMPPS\_RandUniform\_16s\(int16\_t\* dst, int32\_t len, HmppsRandUniformPolicy\_16s\* policy\);

    HmppResult HMPPS\_RandUniform\_32f\(float\* dst, int32\_t len, HmppsRandUniformPolicy\_32f\* policy\);

    HmppResult HMPPS\_RandUniform\_64f\(double\* dst, int32\_t len, HmppsRandUniformPolicy\_64f\* policy\);

- **释放内存操作：**

    HmppResult HMPPS\_RandUniformRelease\_8u\(HmppsRandUniformPolicy\_8u\* policy\);

    HmppResult HMPPS\_RandUniformRelease\_16s\(HmppsRandUniformPolicy\_16s\* policy\);

    HmppResult HMPPS\_RandUniformRelease\_32f\(HmppsRandUniformPolicy\_32f\* policy\);

    HmppResult HMPPS\_RandUniformRelease\_64f\(HmppsRandUniformPolicy\_64f\* policy\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|low|最小值|视类型而定|输入|
|high|最大值|视类型而定|输入|
|seed|随机数种子。|视类型而定|输入|
|policy|产生随机序列参数结构体。|非空|输入/输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_MALLOC_FAILED|所需的额外内存申请失败。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    float dst[BUFFER_SIZE_T];
    int32_t i;
    float low = 1.0;
    float high = 1000.0;
    float seed = 0;
    HmppsRandUniformPolicy_32f *policy = NULL;

    HmppResult result;
    result = HMPPS_RandUniformInit_32f(&policy, low, high, seed);
    if (result != HMPP_STS_NO_ERR)
    {
        return;
    }
    result = HMPPS_RandUniform_32f(dst, BUFFER_SIZE_T, policy);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f  ", dst[i]);
    }

    HMPPS_RandUniformRelease_32f(policy);

    return 0;
}
```

运行结果：

```text
result = 0
dst = 171.657196   750.152100   97.275284   870.594727   577.726196   786.013489   692.501953   369.397522   874.030151   745.349976 
```

## ReplaceNAN

查找向量元素中的NaN值，并将NaN值替换成指定的值。

函数接口声明如下：

HmppResult HMPPS\_ReplaceNAN\_32f\_I\(float \*srcDst, int32\_t len, float value\);

HmppResult HMPPS\_ReplaceNAN\_64f\_I\(double \*srcDst, int32\_t len, double value\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|value|指定值。|不限，视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void ReplaceNANExample(void)
{
    const float nan = 0.0/0.0;
    float src[BUFFER_SIZE_T] = {3.28, nan, 2.05, -8.41, nan, 1.69, 7.91, nan, nan, 1.55};
    float repVal = 3.14;

    HmppResult result = HMPPS_ReplaceNAN_32f_I(src, BUFFER_SIZE_T, repVal);
    printf("ReplaceNAN: result = %d.\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf("   %.2f", src[i]);
    }
    printf("\n");
}
```

运行结果：

```text
ReplaceNAN: result = 0
dst =   3.28   3.14   2.05   -8.41   3.14   1.69   7.91   3.14   3.14   1.55
```

## SampleDown

信号的降采样，即使用采样因子（factor）降低采样率。

具体而言，降采样是将源采样序列src按序分为若干个块，每个块包含factor个采样点，丢弃其中的factor-1个采样点，并保存一个采样点至dst。参数phase是源采样序列的相位，它决定了每个块内要保留的采样点的位置。phase的取值范围应为\[0, factor-1\]。采样结果序列的长度保存在dstLen指向的位置。

处理方式可使用如下公式描述：

![](../../figures/zh-cn_formulaimage_0000002518441616.png)

![](../../figures/zh-cn_formulaimage_0000002550041455.png)

![](../../figures/zh-cn_formulaimage_0000002549921477.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_SampleDown\_16s\(const int16\_t \*src, int32\_t srcLen, int16\_t \*dst, int32\_t \*dstLen, int32\_t factor,int32\_t \*phase\);

    HmppResult HMPPS\_SampleDown\_16sc\(const Hmpp16sc \*src, int32\_t srcLen, Hmpp16sc \*dst, int32\_t \*dstLen, int32\_t factor,int32\_t \*phase\);

- **浮点数的操作：**

    HmppResult HMPPS\_SampleDown\_32f\(const float \*src, int32\_t srcLen, float \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleDown\_64f\(const double \*src, int32\_t srcLen, double \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleDown\_32fc\(const Hmpp32fc \*src, int32\_t srcLen, Hmpp32fc \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleDown\_64fc\(const Hmpp64fc \*src, int32\_t srcLen, Hmpp64fc \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcLen|源向量长度。|(0,INT_MAX]|输入|
|dst|指向目标向量的指针。|非空|输出|
|dstLen|指向目标向量长度的指针。|非空|输出|
|factor|采样因子。|(0, INT_MAX]|输入|
|phase|指向采样相位的指针。|非空且[0, factor)|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、dstLen、phase任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|srcLen小于或等于0。|
|HMPP_STS_SAMPLE_FACTOR_ERR|采样因子小于或等于0。|
|HMPP_STS_SAMPLE_PHASE_ERR|采样相位小于0或者大于或等于factor。|

**示例**

```c
#define BUFFER_SIZE_S 9
void SampleDownExample(void)
{
    Hmpp16sc src1[BUFFER_SIZE_S] = { 14761, -14761, -9981, 9381, 286, -7115, -15360, -7959, -26648, -13094,
                                    -29344, -999, -12922, 8793, -21146, 12262, 1568, -6382 };
    Hmpp16sc src2[BUFFER_SIZE_S] = { 30000, -30000, -9976, 9976, -848, -2080, -22268, -32406, 29451, 8620,
                                    19416, -30118, -31166, -28113, -11331, -8179, -30595, 14322 };
    Hmpp16sc dst[65] = { 0 };
    int32_t dstLen1 = 0;
    int32_t dstLen2 = 0;
    int32_t factor = 4;
    int32_t phase = 2;

    HmppResult result;
    result = HMPPS_SampleDown_16sc(src1, BUFFER_SIZE_S, dst, &dstLen1, factor, &phase);
    result |= HMPPS_SampleDown_16sc(src2, BUFFER_SIZE_S, dst + dstLen1, &dstLen2, factor, &phase);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i = 0;
    printf("dstLen1 = %d\ndst1 =", dstLen1);
    for(; i < dstLen1; ++i){
        printf(" %d %d   ", dst[i].re, dst[i].im);
    }
    printf("\ndstLen2 = %d\ndst2 =", dstLen1);
    for(; i < dstLen1 + dstLen2; ++i){
        printf(" %d %d   ", dst[i].re, dst[i].im);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dstLen1 = 2
dst1 = 286 -7115    -12922 8793
dstLen2 = 2
dst2 = -9976 9976    19416 -30118
```

## SampleUp

信号的上采样，即使用采样因子（factor）增加采样率。

具体而言，上采样是在源采样序列src的每个元素之间插入factor-1个0。因此，在采样结果序列中，factor个元素构成一个块。参数phase是采样相位，它决定了源序列元素在结果序列中的位置，它的取值范围应为\[0, factor-1\]。

处理方式可用如下公式描述：

![](../../figures/zh-cn_formulaimage_0000002550041625.png)

![](../../figures/zh-cn_formulaimage_0000002518281868.png)

![](../../figures/zh-cn_formulaimage_0000002550041613.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResultHMPPS\_SampleUp\_16s\(const int16\_t \*src, int32\_t srcLen, int16\_t \*dst, int32\_t \*dstLen, int32\_t factor,int32\_t \*phase\);

    HmppResult HMPPS\_SampleUp\_16sc\(const Hmpp16sc \*src, int32\_t srcLen, Hmpp16sc \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

- **浮点数的操作：**

    HmppResult HMPPS\_SampleUp\_32f\(const float \*src, int32\_t srcLen, float \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleUp\_64f\(const double \*src, int32\_t srcLen, double \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleUp\_32fc\(const Hmpp32fc \*src, int32\_t srcLen, Hmpp32fc \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

    HmppResult HMPPS\_SampleUp\_64fc\(const Hmpp64fc \*src, int32\_t srcLen, Hmpp64fc \*dst, int32\_t \*dstLen, int32\_t factor, int32\_t \*phase\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcLen|样本大小。|(0,INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstLen|采样宽度。|非空|输出|
|factor|采样因子。|(0,INT_MAX]|输入|
|phase|指向采样相位的指针。|非空且[0, factor)|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、dstLen、phase任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SAMPLE_FACTOR_ERR|采样因子小于或等于0。|
|HMPP_STS_SAMPLE_PHASE_ERR|采样相位小于0或者大于或等于factor。|

**示例**

```c
#define BUFFER_SIZE_S 9
void SampleUpExample(void)
{
    Hmpp16sc src1[BUFFER_SIZE_S] = { 14761, -14761, -9981, 9381, 286, -7115, -15360, -7959, -26648, -13094,
                                    -29344, -999, -12922, 8793, -21146, 12262, 1568, -6382 };
    Hmpp16sc src2[BUFFER_SIZE_S] = { 30000, -30000, -9976, 9976, -848, -2080, -22268, -32406, 29451, 8620,
                                    19416, -30118, -31166, -28113, -11331, -8179, -30595, 14322 };
    Hmpp16sc dst[65] = { 0 };
    int32_t dstLen1 = 0;
    int32_t dstLen2 = 0;
    int32_t factor = 4;
    int32_t phase = 2;

    HmppResult result;
    result = HMPPS_SampleUp_16sc(src1, BUFFER_SIZE_S, dst, &dstLen1, factor, &phase);
    result |= HMPPS_SampleUp_16sc(src2, BUFFER_SIZE_S, dst + dstLen1, &dstLen2, factor, &phase);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i = 0;
    printf("dstLen1 = %d\ndst1 =", dstLen1);
    for(; i < dstLen1; ++i){
        printf(" %d %d   ", dst[i].re, dst[i].im);
    }
    printf("\ndstLen2 = %d\ndst2 =", dstLen1);
    for(; i < dstLen1 + dstLen2; ++i){
        printf(" %d %d   ", dst[i].re, dst[i].im);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dstLen1 = 36
dst1 = 0 0    0 0    14761 -14761    0 0    0 0    0 0    -9981 9381    0 0    0 0    0 0    286 -7115    0 0    0 0    0 0    -15360 -7959    0 0    0 0    0 0    -26648 -13094    0 0    0 0    0 0    -29344 -999    0 0    0 0    0 0    -12922 8793    0 0    0 0    0 0    -21146 12262    0 0    0 0    0 0    1568 -6382    0 0
dstLen2 = 36
dst2 = 0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    0 0    -31166 -28113    0 0    0 0    0 0    -11331 -8179    0 0    0 0    0 0    -30595 14322    0 0
```

## Set

把常数设置到目的地址。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Set\_8u\(uint8\_t val,uint8\_t\* dst,int32\_t len\);

    HmppResult HMPPS\_Set\_16s\(int16\_t val, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Set\_32s\(int32\_t val, int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Set\_64s\(int64\_t val,int64\_t\* dst,int32\_t len\);

    HmppResult HMPPS\_Set\_16sc\(Hmpp16sc val,Hmpp16sc\* dst,int32\_t len\);

    HmppResult HMPPS\_Set\_32sc\(Hmpp32sc val, Hmpp32sc\* dst,int32\_t len\);

    HmppResult HMPPS\_Set\_64sc\(Hmpp64sc val, Hmpp64sc\* dst,int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Set\_32f\(float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Set\_64f\(double val, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Set\_32fc\(Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Set\_64fc\(Hmpp64fc val, Hmpp64fc\* dst,int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|val|固定值。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    uint8_t val = 127;
    uint8_t dst[BUFFER_SIZE_T];
    int32_t i;

    HmppResult result = HMPPS_Set_8u(val, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 127    127    127    127    127    127    127    127    127    127
```

## Sort

向量排序，包含升序和降序。

函数接口声明如下：

- **整型数的升序排序：**

    HmppResult HMPPS\_SortAscend\_8u\_I\(uint8\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortAscend\_16u\_I\(uint16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortAscend\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortAscend\_32s\_I\(int32\_t\* srcDst, int32\_t len\);

- **浮点数的升序排序：**

    HmppResult HMPPS\_SortAscend\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortAscend\_64f\_I\(double\* srcDst, int32\_t len\);

- **整型数的降序排序：**

    HmppResult HMPPS\_SortDescend\_8u\_I\(uint8\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortDescend\_16u\_I\(uint16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortDescend\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortDescend\_32s\_I\(int32\_t\* srcDst, int32\_t len\);

- **浮点数的降序排序：**

    HmppResult HMPPS\_SortDescend\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_SortDescend\_64f\_I\(double\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9
void Sort_Example() {
    uint8_t src[BUFFER_SIZE_T] = {255, 0, 254, 0, 253, 1, 252, 2, 251};
    int32_t i;
    HmppResult result = HMPPS_SortAscend_8u_I(src, BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("result = %d\n", result);
        printf("dst = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", src[i]);
        }
        printf("\n");
    }
}
```

运行结果：

```text
result = 0
dst = 0 0 1 2 251 252 253 254 255
```

## SortIndex

带索引的向量排序，包含升序和降序。如果参与排序的向量值有相同值，则这些相同值所对应的索引不排序，即排序算法是不稳定的，对相同值排序，排序后的索引顺序与排序之前的顺序不同。

函数接口声明如下：

- **整型数的升序排序：**

    HmppResult HMPPS\_SortIndexAscend\_8u\_I\(uint8\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexAscend\_16u\_I\(uint16\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexAscend\_16s\_I\(int16\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexAscend\_32s\_I\(int32\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

- **浮点数的升序排序：**

    HmppResult HMPPS\_SortIndexAscend\_32f\_I\(float\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexAscend\_64f\_I\(double\* srcDst, int32\_t dstIdx, int32\_t len\);

- **整型数的降序排序：**

    HmppResult HMPPS\_SortIndexDescend\_8u\_I\(uint8\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexDescend\_16u\_I\(uint16\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexDescend\_16s\_I\(int16\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexDescend\_32s\_I\(int32\_t\* srcDst, int32\_t dstIdx, int32\_t len\);

- **浮点数的降序排序：**

    HmppResult HMPPS\_SortIndexDescend\_32f\_I\(float\* srcDst, int32\_t dstIdx, int32\_t len\);

    HmppResult HMPPS\_SortIndexDescend\_64f\_I\(double\* srcDst, int32\_t dstIdx, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|dstIdx|指向排序后索引的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9
void Sort_Example() {
    uint8_t src[BUFFER_SIZE_T] = {255, 5, 254, 0, 253, 1, 252, 2, 251};
    int32_t *dstIdx = HMPPS_Malloc_32s(BUFFER_SIZE_T);
    int32_t i;
    HmppResult result = HMPPS_SortAscend_8u_I(src, dstIdx ,BUFFER_SIZE_T);
    if (result == HMPP_STS_NO_ERR) {
        printf("result = %d\n", result);
        printf("dst = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", src[i]);
        }
        printf("\n");
        printf("dstIdx = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", dstIdx[i]);
        }
        printf("\n");
    }
    HMPPS_Free(dstIdx);
}
```

运行结果：

```text
result = 0
dst = 0 1 2 5 251 252 253 254 255
dstIdx = 3 5 7 1 8 6 4 2 0
```

## SortRadix

向量基数排序，包含升序和降序。

函数接口声明如下：

- **辅助函数：**

    HmppResult HMPPS\_SortRadixInit\(int32\_t len, HmppDataType dataType, uint8\_t \*\*buffer\);

    HmppResult HMPPS\_SortRadixRelease\(uint8\_t\* buffer\);

- **整型数的升序排序：**

    HmppResult HMPPS\_SortRadixAscend\_8u\_I\(uint8\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_16u\_I\(uint16\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_16s\_I\(int16\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_32u\_I\(uint32\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_32s\_I\(int32\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_64u\_I\(uint64\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_64s\_I\(int64\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

- **浮点数的升序排序：**

    HmppResult HMPPS\_SortRadixAscend\_32f\_I\(float \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixAscend\_64f\_I\(double \*srcDst, int32\_t len, uint8\_t\* buffer\);

- **整型数的降序排序：**

    HmppResult HMPPS\_SortRadixDescend\_8u\_I\(uint8\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_16u\_I\(uint16\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_16s\_I\(int16\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_32u\_I\(uint32\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_32s\_I\(int32\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_64u\_I\(uint64\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_64s\_I\(int64\_t \*srcDst, int32\_t len, uint8\_t\* buffer\);

- **浮点数的降序排序：**

    HmppResult HMPPS\_SortRadixDescend\_32f\_I\(float \*srcDst, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixDescend\_64f\_I\(double \*srcDst, int32\_t len, uint8\_t\* buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|buffer|指向申请空间的指针。|非空|输入|
|dataType|数据类型。|枚举类型HmppDataType|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst、buffer中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DATA_TYPE_ERR|数据类型错误。|
|HMPP_STS_MALLOC_FAILED|内存申请失败。|

**示例**

```c
#define BUFFER_SIZE_T 9
void Sort_Example() {
    uint8_t src[BUFFER_SIZE_T] = {255, 0, 254, 0, 253, 1, 252, 2, 251};
    int32_t i;
    uint8_t *buffer;
    HMPPS_SortRadixInit(BUFFER_SIZE_T, HMPP8U, &buffer);
    HmppResult result = HMPPS_SortRadixAscend_8u_I(src, BUFFER_SIZE_T, buffer);
    if (result == HMPP_STS_NO_ERR) {
        printf("result = %d\n", result);
        printf("dst = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", src[i]);
        }
        printf("\n");
    }
    HMPPS_SortRadixRelease(buffer);
}
```

运行结果：

```text
result = 0
dst = 0 0 1 2 251 252 253 254 255
```

## SortRadixIndex

向量基数排序，包括升序和降序，输出排序后的索引。

函数接口声明如下：

- **辅助函数：**

    HmppResult HMPPS\_SortRadixIndexInit\(int32\_t len, HmppDataType dataType, uint8\_t \*\*buffer\);

    HmppResult HMPPS\_SortRadixIndexRelease\(uint8\_t\* buffer\);

- **整型数的升序排序：**

    HmppResult HMPPS\_SortRadixIndexAscend\_8u\(const uint8\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_16u\(const uint16\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_16s\(const int16\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_32u\(const uint32\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_32s\(const int32\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_64u\(const uint64\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_64s\(const int64\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

- **浮点数的升序排序：**

    HmppResult HMPPS\_SortRadixIndexAscend\_32f\(const float \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexAscend\_64f\(const double \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

- **整型数的降序排序：**

    HmppResult HMPPS\_SortRadixIndexDescend\_8u\(const uint8\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_16u\(const uint16\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_16s\(const int16\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_32u\(const uint32\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_32s\(const int32\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_64u\(const uint64\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_64s\(const int64\_t \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

- **浮点数的降序排序：**

    HmppResult HMPPS\_SortRadixIndexDescend\_32f\(const float \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

    HmppResult HMPPS\_SortRadixIndexDescend\_64f\(const double \*src, int32\_t srcStrideBytes, int32\_t \*dstIdx, int32\_t len, uint8\_t\* buffer\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|srcDst|指向原地操作向量的指针。|非空。|输入/输出|
|srcStrideBytes|两个源向量之间的距离，单位字节。|大于等于sizeof(T)，其中T为数据类型。|输入|
|len|向量长度。|(0,INT_MAX]。|输入|
|buffer|指向申请空间的指针。|非空。|输入|
|dataType|数据类型。|枚举类型HmppDataType。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|srcDst、buffer中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DATA_TYPE_ERR|数据类型错误。|
|HMPP_STS_MALLOC_FAILED|内存申请失败。|

**示例**

```c
#define BUFFER_SIZE_T 4
struct C{
     uint8_t key1;
     uint8_t key2;
     float data;
}C;
void Sort_Example() {
    struct C c_array[BUFFER_SIZE_T] = {{0,2,1.0f}, {1,3,2.0f}, {1,4,3.0f}, {8,2,10.0f}};
    uint8_t *buffer1,*buffer2;
    int32_t idx1[BUFFER_SIZE_T], idx2[BUFFER_SIZE_T];
    int32_t i;

    HMPPS_SortRadixIndexInit(BUFFER_SIZE_T, HMPP8U, &buffer1);
    HmppResult result1 = HMPPS_SortRadixIndexDescend_8u(&c_array[0].key1, sizeof(C), idx1, BUFFER_SIZE_T, buffer1);
    if (result1 == HMPP_STS_NO_ERR) {
        printf("result1 = %d\n", result1);
        printf("idx1 = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", idx1[i]);
        }
        printf("\n");
    }
    HMPPS_SortRadixIndexRelease(buffer1);
    HMPPS_SortRadixIndexInit(BUFFER_SIZE_T, HMPP32F, &buffer2);
    HmppResult result2 = HMPPS_SortRadixIndexAscend_32f(&c_array[0].data, sizeof(C), idx2, BUFFER_SIZE_T, buffer2);
    if (result2 == HMPP_STS_NO_ERR) {
        printf("result2 = %d\n", result2);
        printf("idx2 = ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%d ", idx2[i]);
        }
        printf("\n");
    }
    HMPPS_SortRadixIndexRelease(buffer2);
}
```

运行结果：

```text
result1 = 0
idx1 = 3 1 2 0
result2 = 0
idx2 = 0 1 2 3
```

## SwapBytes

反转数据字节顺序。可用于大小端数据的转换。

函数接口声明如下：

**整型数的操作：**

HmppResult HMPPS\_SwapBytes\_16u\(const uint16\_t \*src, uint16\_t \*dst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_24u\(const uint8\_t \*src, uint8\_t \*dst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_32u\(const uint32\_t \*src, uint32\_t \*dst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_64u\(const uint64\_t \*src, uint64\_t \*dst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_16u\_I\(uint16\_t \*srcDst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_24u\_I\(uint8\_t \*srcDst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_32u\_I\(uint32\_t \*srcDst, int32\_t len\);

HmppResult HMPPS\_SwapBytes\_64u\_I\(uint64\_t \*srcDst, int32\_t len\);

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
|HMPP_STS_NULL_PTR_ERR|当任何指定的指针为空时指示错误。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define SRC_LEN 8
void SwapBytesExample(void)
{
    int32_t len = SRC_LEN;
    uint32_t src[SRC_LEN] = { 3158544435, 4294967295, 1961184808, 1746231054, 1674436114, 0, 3016018637, 938407021 };
    uint32_t dst[SRC_LEN] = { 0 };

    HmppResult result = HMPPS_SwapBytes_32u(src, dst, len);
    printf("HMPPS_SwapBytes_16u result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    int32_t i;
    printf("len = %d\ndst =", len);
    for(i = 0; i < len; ++i){
        printf(" %u", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
HMPPS_SwapBytes_16u result = 0
len = 8
dst = 865092540 4294967295 676259188 241112424 316591459 0 3452617907 1844768311
```

## Tone

产生给定频率、相位和幅度的音调。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Tone\_16s\(int16\_t \*dst, int32\_t len, int16\_t magn, float freq, float \*phase\);

    HmppResult HMPPS\_Tone\_16sc\(Hmpp16sc \*dst, int32\_t len, int16\_t magn, float freq, float \*phase\);

- **浮点数的操作：**

    HmppResult HMPPS\_Tone\_32f\(float \*dst, int32\_t len, float magn, float freq, float \*phase\);

    HmppResult HMPPS\_Tone\_64f\(double \*dst, int32\_t len, double magn, double freq, double \*phase\);

    HmppResult HMPPS\_Tone\_32fc\(Hmpp32fc \*dst, int32\_t len, float magn, float freq, float \*phase\);

    HmppResult HMPPS\_Tone\_64fc\(Hmpp64fc \*dst, int32\_t len, double magn, double freq, double \*phase\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|phase|指向相位的指针。|[0.0, 2π)|输入|
|magn|振幅。|视类型而定|输入|
|freq|频率。|实数：[0.0, 0.5)复数：[0.0, 1.0)|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_PARAMETER_ERR|振幅小于0，频率，相位超出范围。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    int16_t magn = 25;
    float freq = 0.4;
    float phase = 5.1415901;

    HmppResult result = HMPPS_Tone_16s(dst, BUFFER_SIZE_T, magn, freq, &phase);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 10    4    -18    24    -21    10    4    -18    24    -21
```

## Triangle

产生三角波。

公式：

实数：x\[n\] = magn \* cth\(2π\* rFreq\*n + phase\), n = 0, 1, 2,...

复数：x\[n\] = magn \* \[cth\(2π\* rFreq\*n + phase\) + j \* sth\(2π\* rFreq\*n + phase\)\], n = 0, 1, 2,...

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Triangle\_16s\(int16\_t \*dst, int32\_t len, int16\_t magn, float freq, float sym, float \*phase\);

    HmppResult HMPPS\_Triangle\_16sc\(Hmpp16sc \*dst, int32\_t len, int16\_t magn, float freq, float sym, float \*phase\);

- **浮点数的操作：**

    HmppResult HMPPS\_Triangle\_32f\(float \*dst, int32\_t len, float magn, float freq, float sym, float \*phase\);

    HmppResult HMPPS\_Triangle\_64f\(double \*dst, int32\_t len, double magn, double freq, double sym, double \*phase\);

    HmppResult HMPPS\_Triangle\_32fc\(Hmpp32fc \*dst, int32\_t len, float magn, float freq, float sym, float \*phase\);

    HmppResult HMPPS\_Triangle\_64fc\(Hmpp64fc \*dst, int32\_t len, double magn, double freq, double sym, double \*phase\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|phase|指向相位的指针。|[0.0, 2π)|输入|
|magn|振幅。|视类型而定|输入|
|freq|频率。|[0.0, 0.5)|输入|
|sym|对称性。|（-π, π）|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_PARAMETER_ERR|振幅小于0，频率，相位超出范围。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    int16_t magn = 25;
    float freq = 0.4;
    float phase = 5.1415901;
    float sym = 1.3;

    HmppResult result = HMPPS_Triangle_16s(dst, BUFFER_SIZE_T, magn, freq, sym, &phase);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = -5     9     -18     23     -4     -5     9     -18     23     -4 
```

## VectorJaehne

生成特殊向量，可用作测试信号，以检查应用不同信号处理功能的效果。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_VectorJaehne\_8u\(uint8\_t \*dst, int32\_t len, uint8\_t magn\);

    HmppResult HMPPS\_VectorJaehne\_16u\(uint16\_t \*dst, int32\_t len, uint16\_t magn\);

    HmppResult HMPPS\_VectorJaehne\_16s\(int16\_t \*dst, int32\_t len, int16\_t magn\);

    HmppResult HMPPS\_VectorJaehne\_32s\(int32\_t \*dst, int32\_t len, int32\_t magn\);

- **浮点数的操作：**

    HmppResult HMPPS\_VectorJaehne\_32f\(float \*dst, int32\_t len, float magn\);

    HmppResult HMPPS\_VectorJaehne\_64f\(double \*dst, int32\_t len, double magn\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|magn|振幅。|视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_PARAMETER_ERR|振幅小于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    int16_t magn = 25;

    HmppResult result = HMPPS_Vectorjaehne_8u(dst, BUFFER_SIZE_T, magn);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 25     29     40     50     40     7     10     50     10     29     
```

## VectorSlope

该函数的功能是创建斜率向量化数组，其公式如下：

dst\[n\] = offset + slope \* n, n = 1, 2, 3, ...

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_VectorSlope\_8u\(uint8\_t \*dst, int32\_t len, float offset, float slope\);

    HmppResult HMPPS\_VectorSlope\_16u\(uint16\_t \*dst, int32\_t len, float offset, float slope\);

    HmppResult HMPPS\_VectorSlope\_16s\(int16\_t \*dst, int32\_t len, float offset, float slope\);

    HmppResult HMPPS\_VectorSlope\_32u\(uint32\_t \*dst, int32\_t len, double offset, double slope\);

    HmppResult HMPPS\_VectorSlope\_32s\(int32\_t \*dst, int32\_t len, double offset, double slope\);

- **浮点数的操作：**

    HmppResult HMPPS\_VectorSlope\_32f\(float \*dst, int32\_t len, float offset, float slope\);

    HmppResult HMPPS\_VectorSlope\_64f\(double \*dst, int32\_t len, double offset, double slope\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|斜率向量数组指针。|非空|输出|
|len|斜率向量数组元素个数。|(0,INT_MAX]|输入|
|offset|偏移值。|非空|输入|
|slope|斜率值。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|长度错误。|

**示例**

```c
void VectorSlopeExample()
{
    int16_t dst[BUFFER_SIZE_T];
    int32_t i;
    float offset =1.5;
    float slope =0.5;
    
    HmppResult result =HMPPS_VectorSlope_16s(dst, BUFFER_SIZE_T, offset, slope);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i =0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
2 2 2 3 4 4 4 5 6 6 6 7 8 8 8
```

## WinHamming

向量与汉明窗相乘。汉明窗公式定义为：![](../../figures/zh-cn_formulaimage_0000002518281658.png)_。_

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_WinHamming\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_16sc\(const Hmpp16sc \*src, Hmpp16sc \*dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_WinHamming\_32f\(const float \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_64f\(const double \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_WinHamming\_16s\_I\(int16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_16sc\_I\(Hmpp16sc \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_WinHamming\_32f\_I\(float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_64f\_I\(double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_32fc\_I\(Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHamming\_64fc\_I\(Hmpp64fc \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|[3,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于3。|

**示例**

```c
#define BUFFER_SIZE_T 10
void WinHammingExample(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_WinHamming_32f(src, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 0.131200 0.305820 -0.501533 0.546700 -3.111228 -0.418071 0.315700 -2.222389 1.005641 -0.352000
```

## WinBlackman

给指定向量加Blackman窗，其公式为：

![](../../figures/zh-cn_formulaimage_0000002518281608.png)

其中，

- WinBlackman类接口，其alpha值由接口参数传入。
- WinBlackmanStd类接口，其alpha=-0.16。
- WinBlackmanOpt类接口，其alpha计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441502.png)

函数接口声明如下：

- **数据的常规操作：**

    HmppResult HMPPS\_WinBlackman\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_32f\(const float\* src, float\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_64f\(const double\* src, double\* dst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinBlackman\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinBlackmanStd\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

- **数据的原地操作：**

    HmppResult HMPPS\_WinBlackman\_16s\_I\(int16\_t\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_32f\_I\(float\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_64f\_I\(double\* srcDst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinBlackman\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinBlackman\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinBlackmanStd\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_64f\_I\(double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanStd\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_64f\_I\(double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBlackmanOpt\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|src|指向源向量序列的指针。|非空|输入|
|srcDst|做原地操作时指向源和目的向量的指针。|非空|输入&输出|
|alpha|WinBlackman相关的调整参数。|视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst入参中存在空指针。|
|HMPP_STS_SIZE_ERR|WinBlackmanOpt类型接口len小于4，其他类型接口len小于3。|

**示例**

```c
#define BUFFER_SIZE_T 10

int WinBlackmanExample()
{
    int32_t i;
    float dst[BUFFER_SIZE_T] = {0.0};
    float alpha = 2.3399999;
    float src[BUFFER_SIZE_T] = {32.4324, 65.655998, -645.26532, 34534.34, 76547.547, 32.4324, -54353.234, -534.53448, 868.12323, 9.3542995};
    HmppResult result = HMPPS_WinBlackman_32f(src, dst, BUFFER_SIZE_T, alpha);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f  ", dst[i]);
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0.000000   71.158592   -1730.999023   86508.515625   95192.570312   40.332100   -136154.843750   -1433.950806   940.880188   0.000000
```

## WinBartlett

给指定向量加bartlett窗。其具体公式为：

![](../../figures/zh-cn_formulaimage_0000002518441724.png)

函数接口声明如下：

- **数据的常规操作：**

    HmppResult HMPPS\_WinBartlett\_16s\(const int16\_t \*src, int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

- **数据的原地操作：**

    HmppResult HMPPS\_WinBartlett\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_64f\_I\(double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinBartlett\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|
|src|指向源向量序列的指针。|非空|输入|
|srcDst|做原地操作时指向源和目的向量的指针。|非空|输入&输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于3。|

**示例**

```c
#define BUFFER_SIZE_T 10

int WinBartlettExample()
{
    int32_t i;
    int16_t dst[BUFFER_SIZE_T] = {0};
    int16_t src[BUFFER_SIZE_T] = {0, 25863, 20143, -32768, -4362, 10220, -22644, 13540, -29236, 187};
    HmppResult result = HMPPS_WinBartlett_16s(src, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d  ", dst[i]);
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0  5747  8952  -21845  -3877  9084  -15096  6018  -6497  0
```

## WinHann

给指定向量加Hann窗。其具体公式为：![](../../figures/zh-cn_formulaimage_0000002518441744.png)。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_WinHann\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_WinHann\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_64f\(const double\* src, double\* dst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_WinHann\_16s\_I\(int16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_WinHann\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_64f\_I\(double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_WinHann\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len\);

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
|HMPP_STS_SIZE_ERR|len小于3。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    int16_t src[BUFFER_SIZE_T] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int16_t dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_16s(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_WinHann_16s(src, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }

    printf("dst =");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d", dst[i]);   
    }

    return 0;
} 
```

运行结果：

```text
result = 0
dst = 0 0 1 2 4 5 4 3 1 0       
```

## WinKaiser

给指定向量加Kaiser窗。其具体公式为：

![](../../figures/zh-cn_formulaimage_0000002518441618.png)

I<sub>0</sub>\(\)表示第一类修正的0阶贝塞尔函数，计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002518281724.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_WinKaiser\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_16sc\(const Hmpp16sc\* src, Hmpp16sc\* dst, int32\_t len, float alpha\);

- **浮点数的操作：**

    HmppResult HMPPS\_WinKaiser\_32f\(const float\* src, float\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_64f\(const double\* src, double\* dst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinKaiser\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_64fc\(const Hmpp64fc\* src, Hmpp64fc\* dst, int32\_t len, double alpha\);

- **整型数的原地操作：**

    HmppResult HMPPS\_WinKaiser\_16s\_I\(int16\_t\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_16sc\_I\(Hmpp16sc\* srcDst, int32\_t len, float alpha\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_WinKaiser\_32f\_I\(float\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_64f\_I\(double\* srcDst, int32\_t len, double alpha\);

    HmppResult HMPPS\_WinKaiser\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float alpha\);

    HmppResult HMPPS\_WinKaiser\_64fc\_I\(Hmpp64fc\* srcDst, int32\_t len, double alpha\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|alpha|与Kaiser窗方程相关的可调参数。|alpha是double类型时，需要满足fabs(alpha)*(len-1)/2<=308。alpha是float类型时，需要满足fabs(alpha)*(len-1)/2<=38。|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于1。|
|HMPP_STS_HUGEWIN_ERR|Kaiser window的值太大。alpha是double类型时，fabs(alpha)*(len-1)/2>308。alpha是float类型时，fabs(alpha)*(len-1)/2>38。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    int16_t src[BUFFER_SIZE_T] = {10, 11, 12, 13, 14, 15, 16, 17, 18, 19};
    float alpha = 0.5;  
    int16_t dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_16s(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_WinKaiser_16s(src, dst, BUFFER_SIZE_T, alpha);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return -1;    
    }

    printf("dst =");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d", dst[i]);   
    }
    printf("\n");

    return 0;
} 
```

运行结果：

```text
result = 0
dst = 4 6 9 12 14 15 15 13 10 7    
```

## Zero

把目的地址内存数据清零。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Zero\_8u\(uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_16s\(int16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_32s\(int32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_64s\(int64\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_16sc\(Hmpp16sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_32sc\(Hmpp32sc\* dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_64sc\(Hmpp64sc\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Zero\_32f\(float \*dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_64f\(double \*dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_32fc\(Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Zero\_64fc\(Hmpp64fc\* dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|dst这个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

int main()
{
    uint8_t dst[BUFFER_SIZE_T];
    int32_t i;

    HmppResult result = HMPPS_Zero_8u(dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d    ", dst[i]);
    }

    return 0;
}
```

运行结果：

```text
result = 0
dst = 0    0    0    0    0    0    0    0    0    0
```
