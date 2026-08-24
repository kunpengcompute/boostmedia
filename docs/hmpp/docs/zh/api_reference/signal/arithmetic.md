# 算术与逻辑运算

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## Abs

计算向量元素的绝对值。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Abs\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_Abs\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Abs\_32f\(const float\* src, float\* dst, int32\_t len\);

    HmppResult HMPPS\_Abs\_64f\(const double\* src, double\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Abs\_16s\_I\(int16\_s\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Abs\_32s\_I\(int32\_s\* srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Abs\_32f\_I\(float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Abs\_64f\_I\(double\* srcDst, int32\_t len\);

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
void AbsExample(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_Abs_32f(src, dst, BUFFER_SIZE_T);
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
dst = 1.64 1.63 1.09 0.71 3.20 0.43 0.41 4.83 5.36 4.40
```

## Add

向量与向量相加。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Add\_8u16u\(const uint8\_t \*src1, const uint8\_t \*src2, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_16u\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_32u\(const uint32\_t \*src1, const uint32\_t \*src2, uint32\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_16s\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len\);

- **整型与浮点数的操作：**

    HmppResult HMPPS\_Add\_16s32f\(const int16\_t \*src1, const int16\_t \*src2, float \*dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Add\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_32fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Add\_64fc\(const Hmpp64fc \*src1, const Hmpp64fc \*src2, Hmpp64fc \*dst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Add\_8u\_S\(const uint8\_t \*src1, const uint8\_t \*src2, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16u\_S\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16s\_S\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_32s\_S\(const int32\_t \*src1, const int32\_t \*src2, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_64s\_S\(const int64\_t \*src1, const int64\_t \*src2, int64\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16sc\_S\(const Hmpp16sc \*src1, const Hmpp16sc \*src2, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_32sc\_S\(const Hmpp32sc \*src1, const Hmpp32sc \*src2, Hmpp32sc \*dst, int32\_t len, double scale\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Add\_32u\_I\(const uint32\_t \*src, uint32\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Add\_16s\_I\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Add\_16s32s\_I\(const int16\_t \*src, int32\_t \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Add\_32f\_I\(const float \*src, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Add\_64f\_I\(const double \*src, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Add\_32fc\_I\(const Hmpp32fc \*src, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Add\_64fc\_I\(const Hmpp64fc \*src, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Add\_8u\_IS\(const uint8\_t \*src, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16u\_IS\(const uint16\_t \*src, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16s\_IS\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_32s\_IS\(const int32\_t \*src, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_16sc\_IS\(const Hmpp16sc \*src, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Add\_32sc\_IS\(const Hmpp32sc \*src, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放系数。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、src、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NAN。|

**示例**

```c
#define BUFFER_SIZE_T 9
void AddExample(void)
{
    uint32_t src1[BUFFER_SIZE_T] = {1598181665, 1446829146, 2752624014, 2171200733, 2676378769, 1078554841, 1318511000, 2592925506, 2518880388};
    uint32_t src2[BUFFER_SIZE_T] = {422526272, 1563791282, 1664517688, 1278844750, 1984585164, 1554125489, 1115993496, 1182866132, 2965039412};
    uint32_t dst[BUFFER_SIZE_T] = {0};
 
    HmppResult result = HMPPS_Add_32u(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("\ndst = ");
    for (int32_t i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 2020707937 -1284346868 -1 -844921813 -1 -1662286966 -1860462800 -519175658 -1
```

## AddC

常量与向量中的每个元素相加。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_AddC\_32f\(const float \*src, float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_AddC\_64f\(const double \*src, double val, double \*dst, int32\_t len\);

    HmppResult HMPPS\_AddC\_32fc\(const Hmpp32fc \*src, Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_AddC\_64fc\(const Hmpp64fc \*src, Hmpp64fc val, Hmpp64fc \*dst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_AddC\_8u\_S\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_16u\_S\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_16s\_S\(const int16\_t \*src, int16\_t val, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_32s\_S\(const int32\_t \*src, int32\_t val, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_64s\_S\(const int64\_t \*src, int64\_t val, int64\_t \*dst, int32\_t len, double scale, HmppRoundMode rndMode\);

    HmppResult HMPPS\_AddC\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc val, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_32sc\_S\(const Hmpp32sc \*src, Hmpp32sc val, Hmpp32sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_64u\_S\(const uint64\_t \*src, uint64\_t val, uint64\_t \*dst, int32\_t len, double scale, HmppRoundMode rndMode\);

- **整型数的原地操作：**

    HmppResult HMPPS\_AddC\_16s\_I\(int16\_t val, int16\_t \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_AddC\_32f\_I\(float val, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddC\_64f\_I\(double val, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddC\_32fc\_I\(Hmpp32fc val, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddC\_64fc\_I\(Hmpp64fc val, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_AddC\_8u\_IS\(uint8\_t val, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_16u\_IS\(uint16\_t val, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_16s\_IS\(int16\_t val, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_32s\_IS\(int32\_t val, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_16sc\_IS\(Hmpp16sc val, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddC\_32sc\_IS\(Hmpp32sc val, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值。|不限，视数据类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|
|rndMode|舍入模式。定义在枚举类型HmppRoundMode中，请参见枚举类型。|枚举体HmppRoundMode元素：HMPP_RND_ZEROHMPP_RND_NEARHMPP_RND_FINANCIAL|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void AddCExample(void)
{
    uint8_t src[BUFFER_SIZE_T] = {0, 1, 3, 5, 7, 10, 254, 255, 20, 50};
    uint8_t dst[BUFFER_SIZE_T] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
    uint8_t val = 1;
    double scale = 2.0;
    int32_t i;

    HmppResult result = HMPPS_AddC_8u_S(src, val, dst, BUFFER_SIZE_T, scale);
    printf("result = %d \n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 0  1  2  3  4  6  128  128  10  26
```

## AddProduct

两向量的乘积与目的向量相加。

计算公式如下：_srcDst\[n\] = srcDst\[n\] + src1\[n\] \* src2\[n\]_，n的范围为\[0，len\)。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_AddProduct\_32f\(const float \*src1, const float \*src2, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddProduct\_64f\(const double \*src1, const double \*src2, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddProduct\_32fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_AddProduct\_64fc\(const Hmpp64fc \*src1, const Hmpp64fc \*src2, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_AddProduct\_16s\_S\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddProduct\_16s32s\_S\(const int16\_t \*src1, const int16\_t \*src2, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_AddProduct\_32s\_S\(const int32\_t \*src1, const int32\_t \*src2, int32\_t \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|srcDst|指向目的向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void AddProductExample(void)
{
    Hmpp64fc src1[BUFFER_SIZE_T] = {
        12130, -6322,  19151, -18246, 26700,  -6189, 10090, -5686, -17433, -29849,
        -1064, -12397, 9846,  -18348, -27613, 9270,  12561, 24201, -8793,  -3989
    };
    Hmpp64fc src2[BUFFER_SIZE_T] = {
        -26409, -11074, -16480, -2962, 8889,   15810, -25568, -16638, 15040,  13349,
        23212,  -27356, 6691,   26146, -17678, 15252, -20052, 25113,  -16404, 24897
    };
    Hmpp64fc srcDst[BUFFER_SIZE_T] = {
        -26409, -11074, -16480, -2962, 8889,   15810, -25568, -16638, 15040,  13349,
        23212,  -27356, 6691,   26146, -17678, 15252, -20052, 25113,  -16404, 24897
    };
    int32_t i;

    HmppResult result = HMPPS_AddProduct_64fc(src1, src2, srcDst, BUFFER_SIZE_T);
    printf("result = %d \n dst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f ", srcDst[i]);
    }
}
```

运行结果：

```text
result = 0
 dst = -390377407.000000  -369669612.000000  335193279.000000  -352610356.000000  136277021.000000  -363806688.000000  545613085.000000  346738896.000000  -859652937.000000  243538101.000000
```

## AddProductC

源向量和常量的乘积与目的向量相加。

计算公式为：![](../../figures/zh-cn_formulaimage_0000002550041395.png)。

函数接口声明如下：

**浮点数的操作：**

HmppResult HMPPS\_AddProductC\_32f\(const float \*src, float val, float \*srcDst, int32\_t len\);

HmppResult HMPPS\_AddProductC\_64f\(const double \*src, const double val, double \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值。|不限，视类型而定|输入|
|srcDst|指向目的向量的指针。|非空|输入/输出|
|len|向量长度。|(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|输入参数中出现空指针，src、srcDst不允许为空。|
|HMPP_STS_SIZE_ERR|参数len不允许小于或者等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void AddProductCExample(void)
{
    double src[BUFFER_SIZE_T] = {2.2672e-08, -2.2672e-08, -18246, 26700, -6189, 10090, -5686, -17433, -29849, -1064};
    double dst[BUFFER_SIZE_T] = {3.2672e-08, -6.2672e-08, 18246, 26700, -7189, 20090, -6686, -16433, -23849, -1864};
    double val = -3;
    int32_t i;
    
    HmppResult result = HMPPS_AddProductC_64f(src, val, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %f ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst =  -0.000000  0.000000  72984.000000  -53400.000000  11378.000000  -10180.000000  10372.000000  35866.000000  65698.000000  1328.000000 
```

## And

向量与向量每个元素按位与。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_And\_8u\(const uint8\_t\* src1, const uint8\_t\* src2,uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_And\_16u\(const uint16\_t\* src1, const uint16\_t\* src2, uint16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_And\_32u\(const uint32\_t\* src1, const uint32\_t\* src2,uint32\_t\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_And\_8u\_I\(const uint8\_t\* src, uint8\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_And\_16u\_I\(const uint16\_t\* src, uint16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_And\_32u\_I\(const uint32\_t\* src, uint32\_t\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9

void AndExample(void)
{
    uint8_t src1[BUFFER_SIZE_T] = {1, 0,  45, 255,  214, 22, 11 ,112, 45};
    uint8_t src2[BUFFER_SIZE_T] = {0, 45,  55,  214, 22, 11 ,112, 45, 66};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    int32_t i, result;

    result = HMPPS_And_8u(src1, src2, dst, BUFFER_SIZE_T);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("result = %d \n dst = ", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 0 0 45 214 22 2 0 32 0
```

## AndC

常量与向量中的每个元素按位与。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_AndC\_8u\(const uint8\_t\* src, uint8\_t val, uint8\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_AndC\_16u\(const uint16\_t\* src, uint16\_t val, uint16\_t\* dst, int32\_t len\);

    HmppResult HMPPS\_AndC\_32u\(const uint32\_t\* src, uint32\_t val, uint32\_t\* dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_AndC\_8u\_I\(uint8\_t val, uint8\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_AndC\_16u\_I\(uint16\_t val, uint16\_t\* srcDst, int32\_t len\);

    HmppResult HMPPS\_AndC\_32u\_I\(uint32\_t val, uint32\_t\* srcDst, int32\_t len\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10

void AndCExample(void)
{
    uint16_t src[BUFFER_SIZE_T] = {55102, 65510, 45878, 34567, 55102, 65510, 45878, 34567, 55102, 65510};
    uint16_t dst[BUFFER_SIZE_T] = {};
    uint16_t val = 32574;
    int32_t i, result;
    result = HMPPS_AndC_16u(src, val, dst, BUFFER_SIZE_T);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("result = %d \n dst =", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 22334 32550 13110 1798 22334 32550 13110 1798 22334 32550
```

## Div

两向量相除。

函数接口声明如下：

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Div\_8u\_S\(const uint8\_t\* src1, const uint8\_t\* src2, uint8\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16u\_S\(const uint16\* src1, const uint16\_t src2, uint16\_t dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16s\_S\(const int16\_t\* src1, const int16\_t\* src2, int16\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_32s\_S\(const int32\_t\* src1, const int32\_t\* src2, int32\_t\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16sc\_S\(const Hmpp16sc\* src1, const Hmpp16sc\* src2, Hmpp16sc\* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_32s16s\_S\(const int16\_t\* src1, const int32\_t\* src2, int16\_t\* dst, int32\_t len, double scale\);

- **浮点数的操作：**

    HmppResult HMPPS\_Div\_32f\(const float\* src1, const float\* src2,float\* dst, int32\_t len\);

    HmppResult HMPPS\_Div\_64f\(const double\* src1, const double\* src2,double\* dst, int32\_t len\);

    HmppResult HMPPS\_Div\_32fc\(const Hmpp32fc\* src1, const Hmpp32fc\* src2,Hmpp32fc\* dst, int32\_t len\);

    HmppResult HMPPS\_Div\_64fc\(const Hmpp64fc\* src1, const Hmpp64fc\* src2,Hmpp64fc\* dst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Div\_8u\_IS\(const uint8\_t\* src, uint8\_t\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16u\_IS\(const uint16\_t\* src, uint16\_t\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16s\_IS\(const int16\_t\* src, int16\_t\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_16sc\_IS\(const Hmpp16sc\* src, Hmpp16sc\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Div\_32s\_IS\(const int32\_t\* src, int32\_t\* srcDst, int32\_t len, double scale\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Div\_32f\_I\(const float\* src, float\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Div\_64f\_I\(const double\* src, double\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Div\_32fc\_I\(const Hmpp32fc\* src, Hmpp32fc\* srcDst, int32\_t len\);

    HmppResult HMPPS\_Div\_64fc\_I\(const Hmpp64fc\* src, Hmpp64fc\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向除数向量的指针。|非空|输入|
|src2|指向被除数向量的指针。|非空|输入|
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
|HMPP_STS_NULL_PTR_ERR|输入参数中出现空指针，src1、src2、dst、src、srcDst不允许为空。|
|HMPP_STS_SIZE_ERR|参数len必须大于0。|
|HMPP_STS_DIV_BY_ZERO_ERR|除0错误。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为nan。|

**示例**

```c
#define BUFFER_SIZE_T 10
void DivExample(void)
{
    float src1[BUFFER_SIZE_T] = { -1.64, -1.63, -1.09, 0.71, 3.20, -0.43, -0.41, 4.83, 5.36, 4.40};
    float src2[BUFFER_SIZE_T] = { 5.20, 9.12, 0.86, 10.13, 5.94, -0.62, 9.19, 11.44, -0.07, 10.23};
    float dst[BUFFER_SIZE_T] = {0.00};
    int32_t i, result;
    result = HMPPS_Div_32f(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0.
dst = -3.17  -5.60  -0.79  14.27  1.86  1.44  -22.41  2.37  -0.01  2.32
```

## DivC

将一个向量中每个元素除以一个常数。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_DivC\_32f\(const float \*src, float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_DivC\_64f\(const double \*src, double val, double \*dst, int32\_t len\);

    HmppResult HMPPS\_DivC\_32fc\(const Hmpp32fc \*src, Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_DivC\_64fc\(const Hmpp64fc\* src, Hmpp64fc val, Hmpp64fc\* dst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_DivC\_8u\_S\(const uint8\_t \*src, const uint8\_t val, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16u\_S\(const uint16\_t \*src, const uint16\_t val, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16s\_S\(const int16\_t \*src, int16\_t val, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc val, Hmpp16sc \*dst, int32\_t len, double scale\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_DivC\_32f\_I\(float val, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_DivC\_64f\_I\(double val, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_DivC\_32fc\_I\(Hmpp32fc val, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_DivC\_64fc\_I\(Hmpp64fc val, Hmpp64fc\* srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_DivC\_8u\_IS\(const uint8\_t val, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16u\_IS\(const uint16\_t val, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16sc\_IS\(const Hmpp16sc val, Hmpp16sc\* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_16s\_IS\(int16\_t val, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_DivC\_64s\_IS\(int64\_t val, int64\_t \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值，除数。|不为0|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|直接在向量上进行运算，充当源向量和目标向量。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放系数。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|输入参数中出现空指针，src、dst、srcDst不允许为空。|
|HMPP_STS_SIZE_ERR|参数len不允许小于或者等于0。|
|HMPP_STS_DIV_BY_ZERO_ERR|除数为0错误。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void DivCExample(void)
{
    float src[BUFFER_SIZE_T] = {10.42, 8.64, 3.22, 10.90, 10.11, 4.27, 0.53, 4.00, 4.73, -1.23};
    float dst[BUFFER_SIZE_T] = {0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00};
    float val = 8.51;
    int32_t i, result;
    result = HMPPS_DivC_32f(src, val, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1.22  1.02  0.38  1.28  1.19  0.50  0.06  0.47  0.56 -0.14
```

## DivCRev

常量除以向量中的每个元素。

函数接口声明如下：

- **无符号整型数的操作：**

    HmppResult HMPPS\_DivCRev\_16u\(const uint16\_t\* src, uint16\_t val, uint16\_t\* dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_DivCRev\_32f\(const float\* src, float val, float\* dst, int32\_t len\);

- **无符号整型数的原地操作：**

    HmppResult HMPPS\_DivCRev\_16u\_I\(uint16\_t val, uint16\_t\* srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_DivCRev\_32f\_I\(float val, float\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|被除数。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|输入参数中出现空指针，src、dst、srcDst不允许为空。|
|HMPP_STS_SIZE_ERR|参数len必须大于0。|
|HMPP_STS_DIV_BY_ZERO|除数为0。|

**示例**

```c
#define BUFFER_SIZE_T 10
void DivCRevExample(void)
{
    float src[BUFFER_SIZE_T] = {6.44, 8.88, 0.78, 5.33, 10.29, 2.46, 10.88, 3.51, 10.72, 10.46};
    float dst[BUFFER_SIZE_T] = {0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00};
    float val = 8.51;
    int32_t i, result;
    result = HMPPS_DivCRev_32f(src, val, dst, BUFFER_SIZE_T);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1.32 0.96 10.91 1.60 0.83 3.46 0.78 2.42 0.79 0.81
```

## DivRound

带舍入的向量除法。

函数接口声明如下：

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Div\_Round\_8u\_S\(const uint8\_t\* src1, const uint8\_t\* src2, uint8\_t\* dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Div\_Round\_16u\_S\(const uint16\_t\* src1, const uint16\_t\* src2, uint16\_t\* dst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Div\_Round\_16s\_S\(const int16\_t\* src1, const int16\_t\* src2, int16\_t\* dst, int32\_t len, HmppRoundMode rndMode, double scale\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Div\_Round\_8u\_IS\(const uint8\_t\* src, uint8\_t\* srcDst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Div\_Round\_16u\_IS\(const uint16\_t\* src, uint16\_t\* srcDst, int32\_t len, HmppRoundMode rndMode, double scale\);

    HmppResult HMPPS\_Div\_Round\_16s\_IS\(const int16\_t\* src, int16\_t\* srcDst, int32\_t len, HmppRoundMode rndMode, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向除数向量的指针。|非空|输入|
|src2|指向被除数向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|src|指向原地操作中除数向量的指针。|非空|输入|
|srcDst|指向原地操作中被除数向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|rndMode|舍入模式。定义在枚举类型HmppRoundMode中，请参见枚举类型。|枚举体HmppRoundMode元素：HMPP_RND_ZEROHMPP_RND_NEARHMPP_RND_FINANCIAL|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、src、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_DIV_BY_ZERO|除0错误。|
|HMPP_STS_ROUND_MODEL_NOT_SUPPORTED_ERR|不支持的舍入模式。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void DivRoundExample(void)
{
    int16_t src1[BUFFER_SIZE_T] = {5, 8, 1, 8, 4, 1, 4, 8, 11, 8};
    int16_t src2[BUFFER_SIZE_T] = {4, 9, 1, 9, -1, 10, 7, 8, 1, 6};
    int16_t dst[BUFFER_SIZE_T] = {0, 0, 0, 0, 0, 0, 0, 0, 0, 0};
    int16_t i, result;
    result = HMPPS_Div_Round_16s_S(src1, src2, dst, BUFFER_SIZE_T, HMPP_RND_ZERO, 0.5);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst =");
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = 1 2 2 2 0 20 3 2 0 1
```

## Inv

将向量的元素反转。

函数接口声明如下：

HmppResult HMPPS\_Inv\_32f\(const float \*src, float \*dst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|len|向量长度。|(0, INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 10
void InvExample(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.
    HmppResult result = HMPPS_Inv_32f(src, dst, BUFFER_SIZE_T);
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
    InvExample();
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0.61 0.61 -0.92 1.41 -0.31 -2.33 2.44 -0.21 0.19 -0.23
```

## LShiftC

向量不饱和左移，包含8u，16u，16s，32s四种数据类型。不饱和左移不会对左移后的数据进行饱和处理，也不会保留有符号数的符号位。

函数接口声明如下：

**整型数的左移：**

HmppResult HMPPS\_LShiftC\_8u\(const uint8\_t\* src, int32\_t val, uint8\_t\* dst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_16u\(const uint16\_t\* src, int32\_t val, uint16\_t\* dst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_16s\(const int16\_t\* src, int32\_t val, int16\_t\* dst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_32s\(const int32\_t\* src, int32\_t val, int32\_t\* dst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_8u\_I\(int32\_t val, uint8\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_16u\_I\(int32\_t val, uint16\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_16s\_I\(int32\_t val, int16\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_LShiftC\_32s\_I\(int32\_t val, int32\_t\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向输入向量的指针。|非空|输入|
|dst|指向左移后输出向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|val|左移位数。|非负|输入|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src，dst或srcDst为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SHIFT_ERR|val小于0。|

**示例**

```c
#define BUFFER_SIZE_T 9

void LShiftCExample()
{
    uint8_t src[BUFFER_SIZE_T] = {0, 1, 2, 3, 4, 5, 6, 7, 255};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    int32_t i;

    HmppResult result = HMPPS_LShiftC_8u(src, 1, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 0 2 4 6 8 10 12 14 254
```

## MMul

矩阵乘法。

![](../../figures/zh-cn_formulaimage_0000002518281586.png)

op\(A\)为m\*k矩阵，op\(B\)为k\*n矩阵，C为m\*n矩阵。

函数接口声明如下：

HmppResult HMPPS\_MMul\_32f\(const float\* src1, const float \*src2, float dst, int32\_t mlen, int32\_t nlen, int32\_t klen\);

HmppResult HMPPS\_MMul\_64f\(const double\* src1, const double \*src2, double dst, int32\_t mlen, int32\_t nlen, int32\_t klen\);

HmppResult HMPPS\_MMul\_32fc\(const Hmpp32fc \* src1, const Hmpp32fc \*src2, Hmpp32fc dst, int32\_t mlen, int32\_t nlen, int32\_t klen\);

HmppResult HMPPS\_MMul\_64fc\(const Hmpp64fc \* src1, const Hmpp64fc \*src2, Hmpp64fc dst, int32\_t mlen, int32\_t nlen, int32\_t klen\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|矩阵A。|非空|输入|
|src2|矩阵B。|非空|输入|
|dst|目标矩阵C。|非空|输出|
|mlen|矩阵A的行，矩阵C的行。|大于0|输入|
|nlen|矩阵B的列，矩阵C的列。|大于0|输入|
|klen|矩阵B的行，矩阵A的列。|大于0|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|表明src1、src2或dst指针至少有一个为空。|
|HMPP_STS_SIZE_ERR|表明入参mlen，nlen或klen小于或等于0。|
|HMPP_STS_NO_ERR|表明没有错误。|

**注意**

该接口的矩阵是行主序。

**示例**

```c
#define BUFFER_SIZE1_T 12
#define BUFFER_SIZE2_T 16
void MMulExample(void)
{
    float src1[BUFFER_SIZE1_T] = {0.340188, -0.105617, 0.283099, 
                                    0.298440, 0.411647, -0.302449, 
                                   -0.164777, 0.268230, -0.222225, 
                                    0.053970, -0.022603, 0.128871};
    float src2[BUFFER_SIZE1_T] = {-0.135216, 0.013401, 0.452230, 0.416195, 
                                     0.135712, 0.217297, -0.358397, 0.106969, 
                                    -0.483699, -0.257113, -0.362768, 0.304177};
    float  dst[BUFFER_SIZE2_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE2_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_MMul_32f(src1, src2, dst, 4, 4, 3);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    printf("dst =");
    for (int i = 0; i < BUFFER_SIZE2_T; i++) {
        printf(" %f", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
dst = -0.197267 -0.091180 0.088997 0.216399 0.161806 0.171213 0.097149 0.076245 0.166173 0.113214 -0.090034 -0.107483 -0.072700 -0.037323 -0.014243 0.059244
```

## Mul

向量与向量相乘。

函数接口声明如下：

- **没有缩放的整型数的操作：**

    HmppResult HMPPS\_Mul\_8u16u\(const uint8\_t \*src1, const uint8\_t \*src2, uint16\_t \*dst, int32\_t len\)；

    HmppResult HMPPS\_Mul\_16s\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len\);

- **没有缩放的浮点数的操作：**

    HmppResult HMPPS\_Mul\_16s32f\(const int16\_t \*src1, const int16\_t \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Mul\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Mul\_32f32fc\(const float \*src1, const Hmpp32fc \*src2, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Mul\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Mul\_32fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Mul\_64fc\(const Hmpp64fc \*src1, const Hmpp64fc \*src2, Hmpp64fc \*dst, int32\_t len\);

- **有缩放的整型数的操作：**

    HmppResult HMPPS\_Mul\_8u\_S\(const uint8\_t \*src1, const uint8\_t \*src2, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16u\_S\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16s\_S\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16s32s\_S\(const int16\_t \*src1, const int16\_t \*src2, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_32s\_S\(const int32\_t \*src1, const int32\_t \*src2, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16sc\_S\(const Hmpp16sc \*src1, const Hmpp16sc \*src2, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_32sc\_S\(const Hmpp32sc \*src1, const Hmpp32sc \*src2, Hmpp32sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16u16s\_S\(const uint16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len, double scale\);

- **没有缩放的整型数的原地操作：**

    HmppResult HMPPS\_Mul\_16s\_I\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len\);

- **没有缩放的浮点数的原地操作：**

    HmppResult HMPPS\_Mul\_32f\_I\(const float \*src, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Mul\_64f\_I\(const double \*src, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Mul\_32fc\_I\(const Hmpp32fc \*src, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Mul\_32f32fc\_I\(const float \*src, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Mul\_64fc\_I\(const Hmpp64fc \*src, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Mul\_8u\_IS\(const uint8\_t \*src, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16u\_IS\(const uint16\_t \*src, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16s\_IS\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_32s\_IS\(const int32\_t \*src, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_16sc\_IS\(const Hmpp16sc \*src, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Mul\_32sc\_IS\(const Hmpp32sc \*src, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、src、dst、srcDst这几个输入参数中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void MulExample(void)
{
    uint8_t src1[BUFFER_SIZE_T] = {255, 0, 254, 0, 253, 1, 252, 2, 251, 3};
    uint8_t src2[BUFFER_SIZE_T] = {2, 3, 1, 2, 6, 2, 2, 12, 2, 8};
    uint8_t dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_8u(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0.

    HmppResult result = HMPPS_Mul_8u_S(src1, src2, dst, BUFFER_SIZE_T, 8.0);
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
```

运行结果：

```text
result = 0
dst = 64 0 32 0 190 0 63 3 63 3
```

## MulC

向量与常量相乘。

函数接口声明如下：

- **有缩放的浮点数的操作：**

    HmppResult HMPPS\_MulC\_32f\(const float \*src, float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_MulC\_64f\(const double \*src, double val, double\*dst, int32\_t len\);

    HmppResult HMPPS\_MulC\_32fc\(const Hmpp32fc \*src, Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_MulC\_64fc\(const Hmpp64fc \*src, Hmpp64fc val, Hmpp64fc \*dst, int32\_t len\);

    HmppResult HMPPS\_MulC\_Low\_32f16s\(const float \*src, float val, int16\_t \*dst, int32\_t len\);

- **有缩放的整型数的操作：**

    HmppResult HMPPS\_MulC\_8u\_S\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_16u\_S\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_16s\_S\(const int16\_t \*src, int16\_t val, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_32s\_S\(const int32\_t \*src, int32\_t val, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_32f16s\_S\(const float \*src, float val, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc val, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_32sc\_S\(const Hmpp32sc \*src, Hmpp32sc val, Hmpp32sc \*dst, int32\_t len, double scale\);

- **整型数的原地操作：**

    HmppResult HMPPS\_MulC\_8u\_IS\(uint8\_t val, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_16u\_IS\(uint16\_t val, uint16\_t \*srcDst, int32\_t len, double scale\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_MulC\_16s\_I\(int16\_t val, int16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MulC\_32f\_I\(float val, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MulC\_64f\_I\(double val, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MulC\_32fc\_I\(Hmpp32fc val, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_MulC\_64fc\_I\(Hmpp64fc val, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_MulC\_16s\_IS\(int16\_t val, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_32s\_IS\(int32\_t val, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_64s\_IS\(int64\_t val, int64\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_64f64s\_IS\(double val, int64\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_16sc\_IS\(Hmpp16sc val, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_MulC\_32sc\_IS\(Hmpp32sc val, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|常量。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|入参src、dst、srcDst中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10

void MulCExample(void)
{
    uint8_t srcDst[BUFFER_SIZE_T] = {255, 0, 254, 0, 253, 1, 252, 2, 251, 3};
    uint8_t val = 54;
    HmppResult result = HMPPS_MulC_8u_IS(val, srcDst, BUFFER_SIZE_T, 8.0);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("srcDst =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %d", srcDst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
srcDst = 255 0 255 0 255 7 255 14 255 20
```

## Not

向量中的每个元素按位取反。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Not\_8u\(const uint8\_t \*src, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Not\_16u\(const uint16\_t \*src, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Not\_32u\(const uint32\_t \*src, uint32\_t \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Not\_8u\_I\(uint8\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向第一个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9

void NotExample()
{
    uint8_t src[BUFFER_SIZE_T] = {198, 170, 22, 15, 13, 46, 221, 245, 156};
    uint8_t dst[BUFFER_SIZE_T] = {};
    int32_t i;

    HmppResult result = HMPPS_Not_8u(src, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 57 85 233 240 242 209 34 10 99
```

## Or

向量与向量每个元素按位或。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Or\_8u\(const uint8\_t \*src1, const uint8\_t \*src2, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Or\_16u\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Or\_32u\(const uint32\_t \*src1, const uint32\_t \*src2, uint32\_t \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Or\_8u\_I\(const uint8\_t \*src, uint8\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Or\_16u\_I\(const uint16\_t \*src, uint16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Or\_32u\_I\(const uint32\_t \*src, uint32\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9
void OrExample()
{
    uint8_t src1[BUFFER_SIZE_T] = {123, 145, 210, 99, 123, 145, 255, 132, 156};
    uint8_t src2[BUFFER_SIZE_T] = {101, 125, 33, 201, 101, 125, 214, 135, 120};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    int32_t i;
    
    HmppResult result = HMPPS_Or_8u(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 127 253 243 235 127 253 255 135 252
```

## Orc

常量与向量中的每个元素按位或。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_OrC\_8u\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_OrC\_16u\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_OrC\_32u\(const uint32\_t \*src, uint32\_t val, uint32\_t \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_OrC\_8u\_I\(const uint8\_t val, uint8\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_OrC\_16u\_I\(const uint16\_t val, uint16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_OrC\_32u\_I\(const uint32\_t val, uint32\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9
void OrCExample()
{
    uint8_t src[BUFFER_SIZE_T] = {123, 145, 210, 99, 123, 145, 255, 132, 156};
    uint8_t dst[BUFFER_SIZE_T] = {};
    uint8_t val = 255;
    int32_t i;
    
    HmppResult result = HMPPS_OrC_8u(src, val, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 255 255 255 255 255 255 255 255 255
```

## RShiftC

向量右移，包含8u，16u，16s，32s四种数据类型。右移会保留有符号整数的符号位，并从左侧填符号位。

函数接口声明如下：

**整型数的左移：**

HmppResult HMPPS\_RShiftC\_8u\(const uint8\_t\* src, int32\_t val, uint8\_t\* dst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_16u\(const uint16\_t\* src, int32\_t val, uint16\_t\* dst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_16s\(const int16\_t\* src, int32\_t val, int16\_t\* dst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_32s\(const int32\_t\* src, int32\_t val, int32\_t\* dst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_8u\_I\(int32\_t val, uint8\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_16u\_I\(int32\_t val, uint16\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_16s\_I\(int32\_t val, int16\_t\* srcDst, int32\_t len\);

HmppResult HMPPS\_RShiftC\_32s\_I\(int32\_t val, int32\_t\* srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向输入向量的指针。|非空|输入|
|dst|指向左移后输出向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|val|左移位数。|非负|输入|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src，dst或srcDst为空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SHIFT_ERR|val小于0。|

**示例**

```c
#define BUFFER_SIZE_T 9

void RShiftCExample()
{
    uint8_t src[BUFFER_SIZE_T] = {0, 1, 2, 3, 4, 5, 6, 7, 255};
    uint8_t dst[BUFFER_SIZE_T] = {};
    int32_t i;

    HmppResult result = HMPPS_LShiftC_8u(src, 1, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 0 2 4 6 8 10 12 14 254
```

## Sqr

这些函数计算src指向的每个元素的平方，然后把数据存放到dst指向的内存中，计算过程为：![](../../figures/zh-cn_formulaimage_0000002550041595.png)。

使用in-place方式进行计算时，函数内部从srcDst中取出数据进行计算，获得的计算结果又写入到srcDst指向的内存区域中，即可表示为：![](../../figures/zh-cn_formulaimage_0000002518281852.png)。

计算一个整数的平方时，输出结果可能会超出对应数据类型的取值范围而饱和，为了获取更精确的结果，需要使用比例因子。

函数接口声明如下：

- **整型数的缩放操作：**

    HmppResult HMPPS\_Sqr\_8u\_S\(const uint8\_t\* src, uint8\_t \* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_8u\_IS\(uint8\_t \* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16u\_S\(const uint16\_t\* src, uint16\_t \* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16u\_IS\(uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16sc\_S\(const Hmpp16sc \* src, Hmpp16sc \* dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16sc\_IS\(Hmpp16sc \* srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16s\_S\(const int16\_t \*src, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sqr\_16s\_IS\(int16\_t \*srcDst, int32\_t len, double scale\);

- **浮点数的缩放操作：**

    HmppResult HMPPS\_Sqr\_32f\(const float \*src, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_64f\(const double \*src, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_64fc\(const Hmpp64fc \*src, Hmpp64fc \*dst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Sqr\_32f\_I\(float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_32fc\_I\(Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_64f\_I\(double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sqr\_64fc\_I\(Hmpp64fc \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|dst|指向目的向量的指针。|非空|输入|
|src|指向源向量的指针。|非空|输入|
|srcDst|指向源数据目的地址的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|参数len不允许小于或者等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10

void SqrExample(void)
{
    float src[BUFFER_SIZE_T] = {1.64, 1.63, -1.09, 0.71, -3.20, -0.43, 0.41, -4.83, 5.36, -4.40};
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T); // 数组初始化，将dst所有元素初始化为0。

    HmppResult result = HMPPS_Sqr_32f(src, dst, BUFFER_SIZE_T);
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
dst = 2.689600 2.656900 1.188100 0.504100 10.240001 0.184900 0.168100 23.328899 28.729601 19.360001
```

## Sub

向量与向量相减。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Sub\_16s\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len\);

- **浮点数的操作：**

    HmppResult HMPPS\_Sub\_32f\(const float \*src1, const float \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Sub\_64f\(const double \*src1, const double \*src2, double \*dst, int32\_t len\);

    HmppResult HMPPS\_Sub\_32fc\(const Hmpp32fc \*src1, const Hmpp32fc \*src2, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_Sub\_16s32f\(const int16\_t \*src1, const int16\_t \*src2, float \*dst, int32\_t len\);

    HmppResult HMPPS\_Sub\_64fc\(const Hmpp64fc \*src1, const Hmpp64fc \*src2, Hmpp64fc \*dst, int32\_t len\);

- **有缩放的整型数操作：**

    HmppResult HMPPS\_Sub\_16s\_S\(const int16\_t \*src1, const int16\_t \*src2, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_32s\_S\(const int32\_t \*src1, const int32\_t \*src2, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_8u\_S\(const uint8\_t \*src1, const uint8\_t \*src2, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_16u\_S\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_16sc\_S\(const Hmpp16sc \*src1, const Hmpp16sc \*src2, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_32sc\_S\(const Hmpp32sc \*src1, const Hmpp32sc \*src2, Hmpp32sc \*dst, int32\_t len, int32\_t scale\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Sub\_16s\_I\(const int16\_t \*src, int16\_t \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Sub\_32f\_I\(const float \*src, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sub\_64f\_I\(const double \*src, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sub\_32fc\_I\(const Hmpp32fc \*src, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Sub\_64fc\_I\(const Hmpp64fc \*src, Hmpp64fc \*srcDst, int32\_t len\);

- **有缩放的整型数原地操作：**

    HmppResult HMPPS\_Sub\_16s\_IS\(const int16\_t \*src1, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_32s\_IS\(const int32\_t \*src1, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_8u\_IS\(const uint8\_t \*src, uint8\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_16u\_IS\(const uint16\_t \*src, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_16sc\_IS\(const Hmpp16sc \*src, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_Sub\_32sc\_IS\(const Hmpp32sc \*src, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向减数的指针。|非空|输入|
|src2|指向被减数的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|src|指向原地操作中减数的指针。|非空|输入|
|srcDst|指向原地操作向量中被减数的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0,INF)且输入为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、src、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void SubExample(void)
{
    float src1[BUFFER_SIZE_T] = { -1.40, 10.85, 7.26, 8.98, 0.26, 6.47, 10.42, 8.64, 3.22, 10.90};    
    float src2[BUFFER_SIZE_T] = { 10.11, 4.27, 0.53, 4.00, 4.73, -1.23, 6.44, 8.88, 0.78, 5.33};    
    float dst[BUFFER_SIZE_T];
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_Sub_32f(src1, src2, dst, BUFFER_SIZE_T);
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
sub: result = 0.
dst = 11.51    -6.58    -6.73    -4.98    4.47    -7.70    -3.98    0.24    -2.44    -5.57
```

## SubC

向量与常量相减。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_SubC\_32f\(const float \*src, float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_SubC\_64f\(const double \*src, double val, double \*dst, int32\_t len\);

    HmppResult HMPPS\_SubC\_32fc\(const Hmpp32fc \*src, Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_SubC\_64fc\(const Hmpp64fc \*src, Hmpp64fc val, Hmpp64fc \*dst, int32\_t len\);

- **整型数的缩放操作：**

    HmppResult HMPPS\_SubC\_16s\_S\(const int16\_t \*src, int16\_t val, int16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_32s\_S\(const int32\_t \*src, int32\_t val, int32\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_8u\_S\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_16u\_S\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc val, Hmpp16sc \*dst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_32sc\_S\(const Hmpp32sc \*src, Hmpp32sc val, Hmpp32sc \*dst, int32\_t len, double scale\);

- **整型数的原地操作：**

    HmppResult HMPPS\_SubC\_16s\_I\(int16\_t val, int16\_t \*srcDst, int32\_t len\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_SubC\_32f\_I\(float val, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubC\_64f\_I\(double val, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubC\_32fc\_I\(Hmpp32fc val, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubC\_64fc\_I\(Hmpp64fc val, Hmpp64fc \*srcDst, int32\_t len\);

- **整型数的原地缩放操作：**

    HmppResult HMPPS\_SubC\_16s\_IS\(int16\_t val, int16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_32s\_IS\(int32\_t val, int32\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_8u\_IS\(uint8\_t val, uint8\_t \*srcDst, int32\_t len, double scale\)

    HmppResult HMPPS\_SubC\_16u\_IS\(uint16\_t val, uint16\_t \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_16sc\_IS\(Hmpp16sc val, Hmpp16sc \*srcDst, int32\_t len, double scale\);

    HmppResult HMPPS\_SubC\_32sc\_IS\(Hmpp32sc val, Hmpp32sc \*srcDst, int32\_t len, double scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向被减数的指针。|非空|输入|
|val|给定常数。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|(0, inf)且为2<sup>n</sup>|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_SCALE_ERR|scale不在(0,INF)范围内或输入为NaN。|

**示例**

```c
#define BUFFER_SIZE_T 10
void SubCExample(void)
{
    float src[BUFFER_SIZE_T] = {10.29, 2.46, 10.88, 3.51, 10.72, 10.46, 2.85, 5.44, 7.68, 11.25};
    float valTest = 8.51;
    float dst[BUFFER_SIZE_T] = {0.00};
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    HmppResult result = HMPPS_SubC_32f(src, valTest, dst, BUFFER_SIZE_T);
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
subC: result = 0.
dst = 1.78    -6.05    2.37    -5.00    2.21    1.95    -5.66    -3.07    -0.83    2.74
```

## SubCRev

常量与向量相减。

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_SubCRev\_32f\(const float \*src, float val, float \*dst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_64f\(const double \*src, double val, double \*dst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_32fc\(const Hmpp32fc \*src, Hmpp32fc val, Hmpp32fc \*dst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_64fc\(const Hmpp64fc \*src, Hmpp64fc val, Hmpp64fc \*dst, int32\_t len\);

- **整型数的缩放操作：**

    HmppResult HMPPS\_SubCRev\_16s\_S\(const int16\_t \*src, int16\_t val, int16\_t \*dst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_32s\_S\(const int32\_t \*src, int32\_t val, int32\_t \*dst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_8u\_S\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_16u\_S\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_16sc\_S\(const Hmpp16sc \*src, Hmpp16sc val, Hmpp16sc \*dst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_32sc\_S\(const Hmpp32sc \*src, Hmpp32sc val, Hmpp32sc \*dst, int32\_t len, int32\_t scale\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_SubCRev\_32f\_I\(float val, float \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_64f\_I\(double val, double \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_32fc\_I\(Hmpp32fc val, Hmpp32fc \*srcDst, int32\_t len\);

    HmppResult HMPPS\_SubCRev\_64fc\_I\(Hmpp64fc val, Hmpp64fc \*srcDst, int32\_t len\);

- **整型数的原地缩放操作：**

    HmppResult HMPPS\_SubCRev\_16s\_IS\(int16\_t val, int16\_t \*srcDst, int32\_t len ,int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_32s\_IS\(int32\_t val, int32\_t \*srcDst, int32\_t len ,int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_8u\_IS\(uint8\_t val, uint8\_t \*srcDst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_16u\_IS\(uint16\_t val, uint16\_t \*srcDst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_16sc\_IS\(Hmpp16sc val, Hmpp16sc \*srcDst, int32\_t len, int32\_t scale\);

    HmppResult HMPPS\_SubCRev\_32sc\_IS\(Hmpp32sc val, Hmpp32sc \*srcDst, int32\_t len, int32\_t scale\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向减数的指针。|非空|输入|
|val|给定常数。|不限，视类型而定|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|scale|缩放因子。|[INT_MIN, INT_MAX]|输入|

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
#define  BUFFER_SIZE 10

void SubCRevExample(void)
{
    float src[BUFFER_SIZE_T] = {1.46,  9.40, 10.73, 5.90, 5.74, -0.96, 8.55, -1.39, 1.71, 2.32}; 
    float valTest = 8.51;
    float dst[BUFFER_SIZE_T] = {0.00};
    (void)HMPPS_Zero_32f(dst, BUFFER_SIZE_T);

    HmppResult result =  HMPPS_SubCRev_32f(src, valTest, dst, BUFFER_SIZE_T);

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
subCRev: result = 0.
dst = 7.05    -0.89    -2.22    2.61    2.77    9.47    -0.04    9.90    6.80    6.19
```

## Threshold

指定常数作为阈值，给向量中的元素做设阈操作。

接口中包含参数relOp，该参数指定了比较操作的类型，它的取值是HMPP\_CMP\_LT（小于）和HMPP\_CMP\_GT（大于）。

计算公式如下：

- 有实数序列src，且relOp = HMPP\_CMP\_LT，则

    ![](../../figures/zh-cn_formulaimage_0000002518441848.png)

- 有实数序列src，且relOp = HMPP\_CMP\_GT，则

    ![](../../figures/zh-cn_formulaimage_0000002518441844.png)

- 有复数序列src，且relOp = HMPP\_CMP\_LT，则

    ![](../../figures/zh-cn_formulaimage_0000002518441840.png)

- 有复数序列src，且relOp = HMPP\_CMP\_GT，则

    ![](../../figures/zh-cn_formulaimage_0000002518441836.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Threshold\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level, HmppCmpOp relOp\);

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_32f\(const float\* src, float\* dst, int32\_t len, float level, HmppCmpOp relOp\);

    HmppResult HMPPS\_Threshold\_64f\(const double\* src, double\* dst, int32\_t len, double level, HmppCmpOp relOp\);

    HmppResult HMPPS\_Threshold\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float level, HmppCmpOp relOp\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Threshold\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level, HmppCmpOp relOp\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_32f\_I\(float\* srcDst, int32\_t len, float level, HmppCmpOp relOp\);

    HmppResult HMPPS\_Threshold\_64f\_I\(double\* srcDst, int32\_t len, double level, HmppCmpOp relOp\);

    HmppResult HMPPS\_Threshold\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float level, HmppCmpOp relOp\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|不限，视类型而定|输入|
|relOp|运算模式。|HMPP_CMP_LT、HMPP_CMP_GT|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_BAD_ARG_ERR|运算模式错误。|

**示例**

```c
#define BUFFER_SIZE_T 10
void ThresholdExample(void) {
  float src[BUFFER_SIZE_T] = {-1.64, -1.63, -1.09, 0.71, 3.20,
                              -0.43, -0.41, 4.83,  5.36, 4.40};
  float dst1[BUFFER_SIZE_T] = {0.00};
  float dst2[BUFFER_SIZE_T] = {0.00};
  HmppResult result;

  result = HMPPS_Threshold_32f(src, dst1, BUFFER_SIZE_T, 3.14, HMPP_CMP_LT);
  printf("Threshold1: result = %d\n", result);
  if (result != HMPP_STS_NO_ERR) {
    return;
  }
  printf("dst1 =");
  for (int i = 0; i < BUFFER_SIZE_T; i++) {
    printf(" %.2f", dst1[i]);
  }

  result = HMPPS_Threshold_32f(src, dst2, BUFFER_SIZE_T, 3.14, HMPP_CMP_GT);
  printf("\nThreshold2: result = %d\n", result);
  if (result != HMPP_STS_NO_ERR) {
    return;
  }
  printf("dst2 =");
  for (int i = 0; i < BUFFER_SIZE_T; i++) {
    printf(" %.2f", dst2[i]);
  }
  printf("\n");
}
```

运行结果：

```text
Threshold1: result = 0
dst1 = 3.14 3.14 3.14 3.14 3.20 3.14 3.14 4.83 5.36 4.40
Threshold2: result = 0
dst2 = -1.64 -1.63 -1.09 0.71 3.14 -0.43 -0.41 3.14 3.14 3.14
```

## ThresholdNorm

指定常数作为阈值，给向量中的元素做设阈操作。与threshold接口不同的是，不包含relOp参数，比较模式由接口函数名直接指定。包含两类比较模式：

- HMPPS\_Threshold\_LT

    小于操作，即level是源向量的下边界。计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441624.png)

    若src\[n\]为复数序列，则

    ![](../../figures/zh-cn_formulaimage_0000002550041487.png)

- HMPPS\_Threshold\_GT

    大于操作，即level是源向量的上边界。计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441636.png)

    若src\[n\]为复数序列，则

    ![](../../figures/zh-cn_formulaimage_0000002518441632.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Threshold\_LT\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_LT\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t level\);

    HmppResult HMPPS\_Threshold\_GT\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_GT\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t level\);

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_LT\_32f\(const float\* src, float\* dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LT\_64f\(const double\* src, double\* dst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_LT\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GT\_32f\(const float\* src, float\* dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GT\_64f\(const double\* src, double\* dst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_GT\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float level\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Threshold\_LT\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_LT\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t level\);

    HmppResult HMPPS\_Threshold\_GT\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_GT\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t level\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_LT\_32f\_I\(float\* srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LT\_64f\_I\(double\* srcDst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_LT\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GT\_32f\_I\(float\* srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GT\_64f\_I\(double\* srcDst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_GT\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float level\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|不限，视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_THRESH_NEG_LEVEL_ERR|阈值小于0。|

**示例**

```c
#define BUFFER_SIZE_T 20

void ThresholdNormExample(void)
{
    float src[BUFFER_SIZE_T] = {4.52,  5.92, 6.15, 8.17,  9.93,  6.04, 11.17, 2.79, 3.58, 0.71,
                                -0.15, 9.68, 9.13, 11.04, 10.37, 0.21, 7.47,  0.05, 2.33, -1.58};
    float dst1[BUFFER_SIZE_T] = {0.00};
    float dst2[BUFFER_SIZE_T] = {0.00};
    HmppResult result;
    result = HMPPS_Threshold_LT_32f(src, dst1, BUFFER_SIZE_T, 3.14);
    printf("Threshold1: result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst1 =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst1[i]);
    }
    result = HMPPS_Threshold_GT_32f(src, dst2, BUFFER_SIZE_T, 3.14);
    printf("\nThreshold2: result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("dst2 =");
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f", dst2[i]);
    }
    printf("\n");
}
```

运行结果：

```text
Threshold1: result = 0
dst1 = 4.52 5.92 6.15 8.17 9.93 6.04 11.17 3.14 3.58 3.14 3.14 9.68 9.13 11.04 10.37 3.14 7.47 3.14 3.14 3.14
Threshold2: result = 0
dst2 = 3.14 3.14 3.14 3.14 3.14 3.14 3.14 2.79 3.14 0.71 -0.15 3.14 3.14 3.14 3.14 0.21 3.14 0.05 2.33 -1.58
```

## ThresholdAbs

对向量中所有元素的绝对值做设阈操作。比较操作由接口函数名指定，包括：

- HMPPS\_Threshold\_LTAbs

    小于操作，即level是向量的下边界。

    ![](../../figures/zh-cn_formulaimage_0000002518441716.png)

- HMPPS\_Threshold\_GTAbs

    大于操作，即level是向量的上边界。

    ![](../../figures/zh-cn_formulaimage_0000002518281816.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Threshold\_LTAbs\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_LTAbs\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t level\);

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_LTAbs\_32f\(const float\* src, float\* dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LTAbs\_64f\(const double\* src, double\* dst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_32f\(const float\* src, float\* dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_64f\(const double\* src, double\* dst, int32\_t len, double level\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTAbs\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_LTAbs\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t level\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTAbs\_32f\_I\(float\* srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LTAbs\_64f\_I\(double\* srcDst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_32f\_I\(float\* srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_GTAbs\_64f\_I\(double\* srcDst, int32\_t len, double level\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|不限，视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_THRESH_NEG_LEVEL_ERR|阈值小于0。|

**示例**

```c
#define  BUFFER_SIZE_T 20

void ThresholdAbsExample(void)
{
    float src[BUFFER_SIZE_T] = {1.46, 9.40, 10.73, 5.90, 5.74, -0.96, 8.55,  -1.39, 1.71,  2.32, 9.99, 5.42, 10.58, 5.20, 9.12, 0.86,  10.13, 5.94,  -0.62, 9.19};
    float dst[BUFFER_SIZE_T] = {0.00};
    HmppResult result;
    int32_t i;
    result = HMPPS_Threshold_LTAbs_32f(src, dst, BUFFER_SIZE_T, 3.14);
    printf("Threshold1: result = %d.\ndst1 = ", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    result = HMPPS_Threshold_GTAbs_32f(src, dst, BUFFER_SIZE_T, 3.14);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("\nThreshold2: result = %d.\ndst2 = ", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
}
```

运行结果：

```text
Threshold1: result = 0.
dst1 = 3.14    9.40    10.73    5.90    5.74    -3.14    8.55    -3.14    3.14    3.14    9.99    5.42    10.58    5.20    9.12    3.14    10.13    5.94    -3.14    9.19
Threshold2: result = 0.
dst2 = 1.46    3.14    3.14    3.14    3.14    -0.96    3.14    -1.39    1.71    2.32    3.14    3.14    3.14    3.14    3.14    0.86    3.14    3.14    -0.62    3.14
```

## ThresholdVal

指定常数作为阈值，给向量中的元素做设阈操作。与threshold接口不同的是，不在阈值范围内的向量元素将被设置成指定值value。

比较操作分为三类，由接口函数名指定，包括：

- HMPPS\_Threshold\_LTVal

    小于操作。计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002550041727.png)

    如果源向量src是复数序列，此时参数level必须是实数，计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441870.png)

- HMPPS\_Threshold\_GTVal

    大于操作。计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002518441874.png)

    如果源向量src是复数序列，此时参数level必须是实数，计算公式为：

    ![](../../figures/zh-cn_formulaimage_0000002549921729.png)

- HMPPS\_Threshold\_LTValGTVal

    该模式要求源向量的元素同时满足大于下边界和小于上边界的条件。参数levelLT是下边界，levelGT是上边界。小于levelLT的元素会被设为valueLT，大于levelGT的元素会被设为valueGT。要求levelLT必须小于或等于levelGT。计算公式如下：

    ![](../../figures/zh-cn_formulaimage_0000002549921733.png)

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Threshold\_LTVal\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_GTVal\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t levelLT, int16\_t valueLT, int16\_t levelGT, int16\_t valueGT\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t levelLT, int32\_t valueLT, int32\_t levelGT, int32\_t valueGT\);

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_LTVal\_32f\(const float\* src, float\* dst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_LTVal\_32s\(const int32\_t \*src, int32\_t \*dst, int32\_t len, int32\_t level, int32\_t value\);

    HmppResult HMPPS\_Threshold\_LTVal\_64f\(const double\* src, double\* dst, int32\_t len, double level, double value\);

    HmppResult HMPPS\_Threshold\_LTVal\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float level, Hmpp32fc value\);

    HmppResult HMPPS\_Threshold\_GTVal\_32f\(const float\* src, float\* dst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_GTVal\_32s\(const int32\_t \*src, int32\_t \*dst, int32\_t len, int32\_t level, int32\_t value\);

    HmppResult HMPPS\_Threshold\_GTVal\_64f\(const double\* src, double\* dst, int32\_t len, double level, double value\);

    HmppResult HMPPS\_Threshold\_GTVal\_32fc\(const Hmpp32fc\* src, Hmpp32fc\* dst, int32\_t len, float level, Hmpp32fc value\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_32f\(const float\* src, float\* dst, int32\_t len, float levelLT, float valueLT, float levelGT, float valueGT\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_64f\(const double\* src, double\* dst, int32\_t len, double levelLT, double valueLT, double levelGT, double valueGT\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTVal\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_GTVal\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t levelLT, int16\_t valueLT, int16\_t levelGT, int16\_t valueGT\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t levelLT, int32\_t valueLT, int32\_t levelGT, int32\_t valueGT\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTVal\_32f\_I\(float\* srcDst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_LTVal\_64f\_I\(double\* srcDst, int32\_t len, double level, double value\);

    HmppResult HMPPS\_Threshold\_LTVal\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float level, Hmpp32fc value\);

    HmppResult HMPPS\_Threshold\_GTVal\_32f\_I\(float\* srcDst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_GTVal\_64f\_I\(double\* srcDst, int32\_t len, double level, double value\);

    HmppResult HMPPS\_Threshold\_GTVal\_32fc\_I\(Hmpp32fc\* srcDst, int32\_t len, float level, Hmpp32fc value\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_32f\_I\(float\* srcDst, int32\_t len, float levelLT, float valueLT, float levelGT, float valueGT\);

    HmppResult HMPPS\_Threshold\_LTValGTVal\_64f\_I\(double\* srcDst, int32\_t len, double levelLT, double valueLT, double levelGT, double valueGT\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|不限，视类型而定|输入|
|value|设定值。|不限，视类型而定|输入|
|levelLT|阈值下界。|小于或等于levelGT|输入|
|levelGT|阈值上界。|大于levelLT|输入|
|valueLT|下界替换值。|不限，视类型而定|输入|
|valueGT|上界替换值。|不限，视类型而定|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst任一入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_THRESHOLD_ERR|阈值下界大于阈值上界。|
|HMPP_STS_THRESH_NEG_LEVEL_ERR|阈值小于0。|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>在所有的复数操作中，level必须是正数。

**示例**

```c
#define  BUFFER_SIZE_T 20

void ThresholdValExample(void)
{
    float src[BUFFER_SIZE_T] = {-1.63, 0.92, 6.15, 3.34, -1.28, 4.53, 8.79, 4.23, 2.18,  9.69,
                              5.34,  8.03, 1.90, 8.76, 4.58,  0.98, 4.30, 8.03, 11.19, 8.41};
    float dst[BUFFER_SIZE_T] = {0.00};

    int32_t i;
    HmppResult result = HMPPS_Threshold_LTVal_32f(src, dst, BUFFER_SIZE_T, 3.14, -2.71);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("Threshold1: result = %d.\ndst1 = ", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    result = HMPPS_Threshold_GTVal_32f(src, dst, BUFFER_SIZE_T, 8.51, 2.71);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("\nThreshold2: result = %d.\ndst2 = ", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
    HMPPS_Zero_32f(dst, BUFFER_SIZE_T);
    result = HMPPS_Threshold_LTValGTVal_32f(src, dst, BUFFER_SIZE_T, 3.14, 2.71, 8.51, -2.71);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("\nThreshold3: result = %d.\ndst3 = ", result);
    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
}
```

运行结果：

```text
Threshold1: result = 0.
dst1 = -2.71    -2.71    6.15    3.34    -2.71    4.53    8.79    4.23    -2.71    9.69    5.34    8.03    -2.71    8.76    4.58    -2.71    4.30    8.03    11.19    8.41
Threshold2: result = 0.
dst2 = -1.63    0.92    6.15    3.34    -1.28    4.53    2.71    4.23    2.18    2.71    5.34    8.03    1.90    2.71    4.58    0.98    4.30    8.03    2.71    8.41
Threshold3: result = 0.
dst3 = 2.71    2.71    6.15    3.34    2.71    4.53    -2.71    4.23    2.71    -2.71    5.34    8.03    2.71    -2.71    4.58    2.71    4.30    8.03    -2.71    8.41
```

## ThresholdAbsVal

指定常数作为阈值，给向量中的元素的绝对值做设阈操作。与ThresholdAbs接口不同的是，不在阈值范围内的向量元素将被设置成指定值value。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Threshold\_LTAbsVal\_16s\(const int16\_t\* src, int16\_t\* dst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_LTAbsVal\_32s\(const int32\_t\* src, int32\_t\* dst, int32\_t len, int32\_t level, int32\_t value\);

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_LTAbsVal\_32f\(const float\* src, float\* dst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_LTAbsVal\_64f\(const double\* src, double\* dst, int32\_t len, double level, double value\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTAbsVal\_16s\_I\(int16\_t\* srcDst, int32\_t len, int16\_t level, int16\_t value\);

    HmppResult HMPPS\_Threshold\_LTAbsVal\_32s\_I\(int32\_t\* srcDst, int32\_t len, int32\_t level, int32\_t value\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTAbsVal\_32f\_I\(float\* srcDst, int32\_t len, float level, float value\);

    HmppResult HMPPS\_Threshold\_LTAbsVal\_64f\_I\(double\* srcDst, int32\_t len, double level, double value\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|不限，视类型而定|输入|
|value|指定常数。|不限，视类型而定|输入|

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
#define  BUFFER_SIZE_T 20

void ThresholdAbsValExample(void)
{
    float src[BUFFER_SIZE_T] = {11.44, -0.07, 10.23, 8.78, -0.30, 1.72, -1.37, 3.39, 7.37, 7.40,
                              8.36,  -1.56, 6.65,  0.04, 6.56,  8.60, 0.84,  4.96, 5.13, 2.56};
    float dst[BUFFER_SIZE_T] = {0.00};
    HmppResult result;
    int32_t i = 0;
    result = HMPPS_Threshold_LTAbsVal_32f(src, dst, BUFFER_SIZE_T, 3.14, 8.51);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }
    printf("ThresholdAbsVal: result = %d.\ndst = ", result);
    for (; i < BUFFER_SIZE_T; i++) {
        printf(" %.2f   ", dst[i]);
    }
}
```

运行结果：

```text
ThresholdAbsVal: result = 0.
dst = 11.44    8.51    10.23    8.78    8.51    8.51    8.51    3.39    7.37    7.40    8.36    8.51    6.65    8.51    6.56    8.60     8.51    4.96    5.13    8.51
```

## ThresholdInv

给向量中元素的模指定下边界做设阈操作，并计算元素的倒数。

level表示元素的模，应该为一个正实数。计算公式为：

![](../../figures/zh-cn_formulaimage_0000002550041551.png)

如果向量元素中包含0并且level也为零处理方式为：

![](../../figures/zh-cn_formulaimage_0000002518281796.png)

函数接口声明如下：

- **浮点数的操作：**

    HmppResult HMPPS\_Threshold\_LTInv\_32f\(const float \*src, float \*dst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LTInv\_64f\(const double \*src, double \*dst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_LTInv\_32fc\(const Hmpp32fc \*src, Hmpp32fc \*dst, int32\_t len, float level\);

- **浮点数的原地操作：**

    HmppResult HMPPS\_Threshold\_LTInv\_32f\_I\(float \*srcDst, int32\_t len, float level\);

    HmppResult HMPPS\_Threshold\_LTInv\_64f\_I\(double \*srcDst, int32\_t len, double level\);

    HmppResult HMPPS\_Threshold\_LTInv\_32fc\_I\(Hmpp32fc \*srcDst, int32\_t len, float level\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|
|level|阈值。|非负数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|
|HMPP_STS_THRESH_NEG_LEVEL_ERR|阈值小于0。|
|HMPP_STS_INV_ZERO|level为0或向量元素为0时发出此警告，程序继续执行，对应的目标向量元素为Inf(∞)。|

**示例**

```c
#define  BUFFER_SIZE_T 20

void ThresholdInvExample(void)
{
    float src[BUFFER_SIZE_T] = { 7.68, 3.24, 10.00, 3.29, 5.34, 4.04, 5.57, 7.23, 0.63, 9.39, 9.80, 8.57, -0.94, 11.20, 2.80, -0.55, 1.95, -0.65, 7.34, 9.12};
    float dst[BUFFER_SIZE_T] = {0.00};
    HmppResult result;
    int32_t i = 0;
    result = HMPPS_Threshold_LTInv_32f(src, dst, BUFFER_SIZE_T, 3.14);
    if(result != HMPP_STS_NO_ERR){
        return;
    }
    printf("ThresholdInv: result = %d.\ndst = ", result);
    for(;i < BUFFER_SIZE_T;i++){
        printf(" %.2f   ", dst[i]);
    }
}
```

运行结果：

```text
ThresholdInv: result = 0.
dst = 0.13    0.31    0.10    0.30    0.19    0.25    0.18    0.14    0.32    0.11    0.10    0.12    -0.32    0.09    0.32    -0.32    0.32    -0.32    0.14    0.11
```

## Xor

向量与向量每个元素按位异或。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_Xor\_8u\(const uint8\_t \*src1, const uint8\_t \*src2, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Xor\_16u\(const uint16\_t \*src1, const uint16\_t \*src2, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_Xor\_32u\(const uint32\_t \*src1, const uint32\_t \*src2, uint32\_t \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_Xor\_8u\_I\(const uint8\_t \*src, uint8\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Xor\_16u\_I\(const uint16\_t \*src, uint16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_Xor\_32u\_I\(const uint32\_t \*src, uint32\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向第一个源向量的指针。|非空|输入|
|src2|指向第二个源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|srcDst|指向原地操作向量的指针。|非空|输入/输出|
|len|向量长度。|(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst、srcDst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define BUFFER_SIZE_T 9

void XorExample()
{
    uint8_t src1[BUFFER_SIZE_T] = {55, 63, 123, 145, 210, 99, 123, 145, 210};
    uint8_t src2[BUFFER_SIZE_T] = {123, 145, 210, 99, 123, 145, 210, 99, 123};
    uint8_t dst[BUFFER_SIZE_T] = {0};
    int32_t i;
    
    HmppResult result = HMPPS_Xor_8u(src1, src2, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 76 174 169 242 169 242 169 242 169
```

## XorC

常量与向量中的每个元素按位异或。

函数接口声明如下：

- **整型数的操作：**

    HmppResult HMPPS\_XorC\_8u\(const uint8\_t \*src, uint8\_t val, uint8\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_XorC\_16u\(const uint16\_t \*src, uint16\_t val, uint16\_t \*dst, int32\_t len\);

    HmppResult HMPPS\_XorC\_32u\(const uint32\_t \*src, uint32\_t val, uint32\_t \*dst, int32\_t len\);

- **整型数的原地操作：**

    HmppResult HMPPS\_XorC\_8u\_I\(const uint8\_t val, uint8\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_XorC\_16u\_I\(const uint16\_t val, uint16\_t \*srcDst, int32\_t len\);

    HmppResult HMPPS\_XorC\_32u\_I\(const uint32\_t val, uint32\_t \*srcDst, int32\_t len\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|val|固定值。|不限，视类型而定|输入|
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
#define BUFFER_SIZE_T 9

void XorCExample()
{
    uint16_t src[BUFFER_SIZE_T] = {40580, 58192, 61183, 49861, 23520, 27185, 25177, 65534, 40580};
    uint16_t dst[BUFFER_SIZE_T] = {};
    uint16_t val = 20328;
    int32_t i;
    
    HmppResult result = HMPPS_OrC_16u(src, val, dst, BUFFER_SIZE_T);
    printf("result = %d \ndst =", result);
    if (result != HMPP_STS_NO_ERR) {
        return;
    }

    for (i = 0; i < BUFFER_SIZE_T; i++) {
        printf("%d ", dst[i]);
    }
}
```

运行结果：

```text
result = 0
dst = 57324 61304 61439 53229 24552 28537 28537 65534 57324
```
