# 颜色空间转换

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## BGRToYCbCr420

将图像模型由BGR格式转换为YCbCr格式，采样格式为4:2:0。依据采样方式，其中roiSize的宽和高需为2的倍数，否则，将roiSize的值修正到最近的2的倍数后，再进行计算，并返回告警信息。

计算公式如下：

Y = 0.257\*R + 0.504\*G + 0.098\*B + 16

Cb = -0.148\*R - 0.291\*G + 0.439\*B + 128

Cr = 0.439\*R - 0.368\*G - 0.071\*B + 128

函数接口声明如下：

HmppResult HMPPI\_BGRToYCbCr420\_8u\_C3P3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

HmppResult HMPPI\_BGRToYCbCr420\_8u\_AC4P3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|dst|指向目的向量的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|(0,INT_MAX]|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize.width、roiSize.height这两个入参中存在值小于2。|
|HMPP_STS_STEP_ERR|srcStep、dstStep小于等于0。|
|HMPP_STS_DOUBLE_SIZE|告警码，roiSize.width、roiSize.height这两个入参中存在值不是2的倍数。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 12

int CopyExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t a[DST_BUFFER_SIZE_T], b[DST_BUFFER_SIZE_T], c[DST_BUFFER_SIZE_T];
    uint8_t *dst[3] = {a, b, c};

    int32_t srcStep = 15 * sizeof(uint8_t);
    int32_t dstStep[3] = {12 * sizeof(uint8_t),12 * sizeof(uint8_t),12 * sizeof(uint8_t)};
    HmppResult result = HMPPI_BGRToYCbCr420_8u_C3P3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for(int32_t i = 0; i < 3; i++){
        int32_t dstWidth = dstStep[i] / sizeof(uint8_t);
        for (int32_t j = 0; j < DST_BUFFER_SIZE_T; j++) {
            if( j % dstWidth == 0 ){
                printf("\n");
            }
            printf("%3d ",dst[i][j]);       
        }
    }
    printf("\n");
    return 0;
}
```

运行结果：

```text
result = 0
dst =
 37  65  74  80   0   0   0   0   0   0   0   0
121 133   0   0   0   0   0   0   0   0   0   0
133 111   0   0   0   0   0   0   0   0   0   0
```

## BGRToYUV420

将BGR图像转换为YUV颜色模型；使用4:2:0采样。

此函数根据以下公式将伽马校正的R'G'B'图像转换为Y'U'V'图像，采样为4:2:0：

![](../../figures/zh-cn_formulaimage_0000002549921737.png)

![](../../figures/zh-cn_formulaimage_0000002550041737.png)

![](../../figures/zh-cn_formulaimage_0000002550041731.png)

对于范围\[0,255\]内的数字BGR值，Y'在范围\[0, 255\]内变化，U'在范围\[-112, +112\]内变化，V'在范围\[-157, +157\]内变化。为了适应\[0, 255\]的范围，将常量值128添加到计算的U和V值中，然后对V取饱和。

函数接口声明如下：

HmppResult HMPPI\_BGRToYUV420\_8u\_AC4P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向像素顺序数据的源缓冲区的指针。在平面数据的情况下，用于分隔源颜色平面的指针数组。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向像素顺序数据的目标缓冲区的指针。在平面数据的情况下，指向分隔目标颜色平面的指针数组。|非空|输出|
|dstStep|指向目标图像中连续行的起点之间的距离的数组（以字节为单位）。|非负整数|输出|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_DOUBLE_SIZE|roiSize的width和height不是2的倍数。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t a[DST_BUFFER_SIZE_T], b[DST_BUFFER_SIZE_T], c[DST_BUFFER_SIZE_T];
    uint8_t *dst[3] = {a, b, c};

    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep[3] = {12 * sizeof(uint8_t),12 * sizeof(uint8_t),12 * sizeof(uint8_t)};
    HmppResult result = HMPPI_BGRToYUV420_8u_AC4P3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for(int32_t i = 0; i < 3; i++){
        int32_t dstWidth = dstStep[i] / sizeof(uint8_t);
        for (int32_t j = 0; j < DST_BUFFER_SIZE_T; j++) {
            if( j % dstWidth == 0 ){
                printf("\n");
            }
            printf("%3d ",dst[i][j]);
        }
    }
    printf("\n");

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
 24  68  77  41   0   0   0   0 104  17 137 186
 79  74  27  46   0   0   0   0   0   0   0   0
 63  16  57  49   0   0   0   0 224  95 137 186
 24  68  77  41 224  30 227 233 255 255   0   0
 46  78  61 246   0   0   0   0  88 161 135 186
255 255   0   0  24   0   0   0   0   0   0   0
255 255 255 255   0   0   0   0   4   0   0   0
  0   0   0   0  56 245
124 123  69 186 255 255   0   0 136  58  69 186
128 126   0   0  80 162  88 186 255 255   0   0
 96 125  69 186 255 255   0   0  80  51 137 186
255 255   0   0   0   0   0   0   0   0   0   0
224  95 137 186 255 255   0   0  80  51 137 186
255 255   0   0 221   4  64   0   0   0   0   0
224   8  70 186 255 255   0   0 224   3  64   0
  0   0   0   0   0   0
132 124   0   0   1   0   0   0   1   0   0   0
131 136   0   0   0   0   0   0   0   0   0   0
  1   0   0   0 255 255   0   0 104  17 137 186
255 255   0   0  96  31 227 233 255 255   0   0
200 214 134 186 255 255   0   0 224 107 137 186
255 255   0   0   0   0   0   0   0   0   0   0
144  30 227 233 255 255   0   0  48 138  72 186
255 255   0   0  28 231
```

## RGBToYCbCr

将RGB图像转换为YCbCr颜色模型。

此函数根据以下公式将值在\[0,255\]范围内的伽马校正R'G'B'图像转换为Y'Cb'Cr'图像。

![](../../figures/zh-cn_formulaimage_0000002550041581.png)

![](../../figures/zh-cn_formulaimage_0000002518281824.png)

![](../../figures/zh-cn_formulaimage_0000002550041569.png)

在YCbCr模型中，Y运算后的取值范围是\[16, 235\]，Cb和Cr运算后的取值范围是\[16,240\]。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYCbCr\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYCbCr\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_RGBToYCbCr\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **从像素阶到平面数据的转换：**

    HmppResult HMPPI\_RGBToYCbCr\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYCbCr\_8u\_AC4P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_RGBToYCbCr_8u_C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

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
 33 134 122  61 134 122  82 101 128  84 122 134   0   0   0
 80 134 122  74 133 100  65 122 134  32 130 137   0   0   0
 74 122 134  45 117 134  43 134 122  71 134 122   0   0   0
 33 134 122  61 134 122  82 101 128  84 122 134   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## RGBToYCbCr422

将RGB图像转换为YCbCr图像，采样为4:2:2。

此函数将伽马校正的R'G'B'图像src转换为Y'Cb'Cr'图像dst，采样格式为4:2:2。其中转换公式与函数RGBToYCbCr相同。

像素阶图像的转换缓冲区的位深度降低为每像素16位，而源缓冲区的位深度为24位。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYCbCr422\_8u\_C3C2R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_RGBToYCbCr422\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

- **从像素阶到平面数据的转换：**

    HmppResult HMPPI\_RGBToYCbCr422\_8u\_P3C2R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_RGBToYCbCr422()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_RGBToYCbCr422_8u_C3C2R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_RGBToYCbCr422();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
 33 134  61 122  82 111  84 131   0   0   0   0   0   0   0
 80 134  74 111  65 126  32 135   0   0   0   0   0   0   0
 74 119  45 134  43 134  71 122   0   0   0   0   0   0   0
 33 134  61 122  82 111  84 131   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## RGBToYCrCb422

将24位每像素RGB图像转换为16位每像素YCrCb图像。该函数根据与函数RGBToYCbCr相同的公式将伽玛校正的R'G'B'图像转换为Y'Cb'Cr'图像。不同的是，HMPPI\_RGBToYCrCb422对转换图像使用4:2:2采样格式。

转换后的缓冲区的位深度减少为每像素16位，而源缓冲区的位深度为24位。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYCrCb422\_8u\_C3C2R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **从平面数据到像素阶数据的转换：**

    HmppResult HMPPI\_RGBToYCrCb422\_8u\_P3C2R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_RGBToYCrCb422()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_RGBToYCbCr422_8u_C3C2R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_RGBToYCrCb422();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
 33 134  61 122  82 111  84 131   0   0   0   0   0   0   0
 80 134  74 111  65 126  32 135   0   0   0   0   0   0   0
 74 119  45 134  43 134  71 122   0   0   0   0   0   0   0
 33 134  61 122  82 111  84 131   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## RGBToYUV

该接口将RGB图像转换为YUV颜色模型。

此函数根据以下公式将伽马校正的R'G'B'图像src转换为Y'U'V'图像dst：

Y' = 0.299\*R' + 0.587\*G' + 0.114\*B'

U' = -0.147\*R' - 0.289\*G' + 0.436\*B' = 0.492\*\(B'-Y'\)

V' = 0.615\*R' - 0.515\*G' - 0.100\*B' = 0.877\*\(R'-Y'\)

对于范围\[0,255\]内的数字RGB值，Y'的范围为\[0,255\]，U'在范围\[-112,112\]内变化，V'在范围\[-157,157\]内变化。为了适应\[0,255\]的范围，将常量值128添加到计算的U'和V'值中，并对V'取饱和运算。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYUV\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYUV\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_RGBToYUV\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **从像素阶到平面数据的转换：**

    HmppResult HMPPI\_RGBToYUV\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYUV\_8u\_AC4P4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[4\], int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

#include "hmpp.h"

void TestExample_RGBToYUV()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_RGBToYUV_8u_C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_RGBToYUV();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
 19 134 120  52 134 120  77 100 128  79 121 135   0   0   0
 74 134 120  67 132  88  57 121 135  18 129 140   0   0   0
 68 121 135  33 116 136  30 134 120  63 134 120   0   0   0
 19 134 120  52 134 120  77 100 128  79 121 135   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## RGBToYUV420

将RGB图像转换成采样为4:2:0的YUV图像，即每4个Y共用一组UV分量。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYUV420\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_RGBToYUV420\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYUV420\_8u\_P3\(const uint8\_t \* src\[3\], uint8\_t \* dst\[3\], HmppiSize imgSize\);

- **从像素阶到平面数据的转换：**

    HmppResult HMPPI\_RGBToYUV420\_8u\_C3P3\(const uint8\_t \* src, uint8\_t \* dst\[3\], HmppiSize imgSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向像素顺序数据的源缓冲区的指针。在平面数据的情况下，用于分隔源颜色平面的指针数组。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向像素顺序数据的目标缓冲区的指针。在平面数据的情况下，指向分隔目标颜色平面的指针数组。|非空|输出|
|dstStep|指向目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|imgSize|源和目标图像的大小（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize或者imgSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t a[DST_BUFFER_SIZE_T], b[DST_BUFFER_SIZE_T], c[DST_BUFFER_SIZE_T];
    uint8_t *dst[3] = {a, b, c};

    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep[3] = {12 * sizeof(uint8_t),12 * sizeof(uint8_t),12 * sizeof(uint8_t)};
    HmppResult result = HMPPI_RGBToYUV420_8u_C3P3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for(int32_t i = 0; i < 3; i++){
        int32_t dstWidth = dstStep[i] / sizeof(uint8_t);
        for (int32_t j = 0; j < DST_BUFFER_SIZE_T; j++) {
            if( j % dstWidth == 0 ){
                printf("\n");
            }
            printf("%3d ",dst[i][j]);
        }
    }
    printf("\n");

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
 19  52  77  79 251 255   0   0 112  17 254 204
 74  67  57  18 144  99  21 239 255 255   0   0
 68  33  30  63 251 255   0   0   0   0  66   0
 19  52  77  79  16 115 254 204 251 255   0   0
 16 123 254 204 251 255   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0
134 118  21 239 255 255   0   0  88  99  21 239
126 122   0   0 240   2  64   0   0   0   0   0
 65   5  64   0   0   0   0   0 120  98  21 239
255 255   0   0 116  98  21 239 255 255   0   0
 56 245 216   3   0   0   0   0   0 240 253 204
251 255   0   0 176  98  21 239   0   0   0   0
  0   0   0   0   0   0   0   0   1   0   0   0
251 255   0   0   0   0
112 134 254 204 251 255   0   0 176 101 254 204
128 125   0   0   1   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0  72  99  21 239
255 255   0   0  88  99  21 239 255 255   0   0
 46  16 138  52   0   0   0   0  14   0   0   0
  0   0   0   0  14   0   0   0   0   0   0   0
  5   0   0   0   0   0   0   0   8 101 254 204
251 255   0   0   0   0
```

## RGBToYUV422

此函数将RGB图像转换为YUV422颜色模型，使用4:2:2采样。因为YUV422格式图像采用4:2:2采样，即每2个像素块，采样2次Y，1次U和V。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_RGBToYUV422\_8u\_C3C2R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_RGBToYUV422\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

    HmppResult HMPPI\_RGBToYUV422\_8u\_C3P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep\[3\], HmppiSize roiSize\);

- **对平面数据的操作（无ROI）：**

    HmppResult HMPPI\_RGBToYUV422\_8u\_P3\(const uint8\_t \*src\[3\], uint8\_t \*dst\[3\], HmppiSize imgSize\);

- **从像素阶到平面数据的转换（无ROI）：**

    HmppResult HMPPI\_RGBToYUV422\_8u\_C3P3\(const uint8\_t \*src, uint8\_t \*dst\[3\], HmppiSize imgSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向像素顺序数据的源缓冲区的指针。在平面数据的情况下，用于分隔源颜色平面的指针数组。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向像素顺序数据的目标缓冲区的指针。在平面数据的情况下，指向分隔目标颜色平面的指针数组。|非空|输出|
|dstStep|指向目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|imgSize|源和目标图像的大小（以像素为单位）。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize或者imgSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 180
#define DST_BUFFER_SIZE_T 120
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t a[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t b[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t c[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t *dst[3] = {a, b, c};

    int32_t srcStep = 12 * sizeof(uint8_t);
    int32_t dstStep[3] = {20 * sizeof(uint8_t), 20 * sizeof(uint8_t), 20 * sizeof(uint8_t)};
    HmppResult result = HMPPI_RGBToYUV422_8u_C3P3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for(int32_t i = 0; i < 3; i++){
        int32_t dstWidth = dstStep[i] / sizeof(uint8_t);
        for (int32_t j = 0; j < DST_BUFFER_SIZE_T; j++) {
            if( j % dstWidth == 0 ){
                printf("\n");
            }
            printf("%3d ",dst[i][j]);
        }
    }
    printf("\n");
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
 19  52  77  79   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
 46  19  52  77   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
 79  46  19  52   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
 77  79  46  19   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
134 111   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
128 117   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
121 134   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
111 128   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
120 131   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
128 124   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
135 120   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
131 128   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## YCbCrI420ToRGB24

将图像模型由YCbCrI格式转换为RGB或者BGR格式。计算公式如下：

![](../../figures/zh-cn_formulaimage_0000002549921355.png)

![](../../figures/zh-cn_formulaimage_0000002518441508.png)

![](../../figures/zh-cn_formulaimage_0000002518281594.png)

![](../../figures/zh-cn_formulaimage_0000002550041375.png)

![](../../figures/zh-cn_formulaimage_0000002518281612.png)

![](../../figures/zh-cn_formulaimage_0000002550041369.png)，其中clamp表示限定值在\[0, 255\]。

函数接口声明如下：

- **YCbCrI420转换为RGB24：**

    HmppResult HMPPI\_YCbCrI420ToRGB24\_8u\(const uint8\_t \*src, uint8\_t \*dst, int32\_t width, int32\_t height\);

- **YCbCrI420转换为BGR24：**

    HmppResult HMPPI\_YCbCrI420ToBGR24\_8u\(const uint8\_t \*src, uint8\_t \*dst, int32\_t width, int32\_t height\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|dst|指向目的向量的指针。|非空|输出|
|width|图像宽度（按像素计算）。|(0,INT_MAX]，可被2整除|输入|
|height|图像高度（按像素计算）。|(0,INT_MAX]，可被2整除|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、dst这两个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|width、height这两个入参中存在值小于或等于0。|
|HMPP_STS_DOUBLE_SIZE|width、height这两个入参中存在值不是2的倍数。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 6
#define DST_BUFFER_SIZE_T 12

int YcbcrToRgbExample(){
    int32_t i;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    uint8_t src[SRC_BUFFER_SIZE_T] = {0, 255, 5, 34, 122, 78};
    HmppResult result = HMPPI_YCbCrI420ToRGB24_8u(src, dst, 2, 2);
    printf("result = %d \ndst =", result);
    for (i = 0; i < DST_BUFFER_SIZE_T; i++) {
        printf(" %d  ", dst[i]);
    }
    return 0;
}
```

运行结果：

```text
result = 0
dst = 0   24   0   199   255   255   0   30   0   0   64   9
```

## YCbCrToBGR

将YCbCr图像转换为BGR颜色模型。

此函数根据与函数YCbCrToRGB相同的公式，将Y'Cb'Cr'图像转换为24位伽马校正的B'G'R'图像。输出B'G'R'值饱和到范围\[0, 255\]。

第四个通道是通过将通道值设置为常量值aval来创建的。

函数接口声明如下：

HmppResult HMPPI\_YCbCrToBGR\_8u\_P3C3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

HmppResult HMPPI\_YCbCrToBGR\_8u\_P3C4R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|创建第四个通道的常量值。|非负整数|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YCbCrToBGR()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src1[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src2[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        21, 23, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        31, 24, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        41, 25, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        51, 26, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        61, 27, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src3[SRC_BUFFER_SIZE_T] = {
        11, 12, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        12, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        13, 32, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        14, 42, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        15, 52, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        16, 62, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    const uint8_t *src[3] = {src1, src2, src3};

    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 20 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    uint8_t aval = 25;
    HmppResult result = HMPPI_YCbCrToBGR_8u_P3C4R(src, srcStep, dst, dstStep, roi, aval);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_YCbCrToBGR();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
  0 135   0  25   0 143   0  25   0 134   0  25   0 134   0  25   0   0   0   0
  0 133   0  25   0 133   0  25   3 132  20  25   0 135   0  25   0   0   0   0
  0 133   0  25   0 133   0  25   0 133   0  25   0 134   0  25   0   0   0   0
  0 116   0  25   0 109   0  25   0 134   0  25   0 134   0  25   0   0   0   0
  0   0   0   0   0   0   0   0   0   0
```

## YCbCrToBGR\_709CSC

将YCbCr图像转换为BGR\(ITU-R BT.709 CSC\)颜色模型。

该函数将平面Y'Cb'Cr'图像转换为符合ITU-R BT.709建议【ITU709】的数字分量视频信号的三通道或四通道伽马校正B'G'R'图像用于计算机系统考虑\(CSC\)。根据以下公式【Jack01】进行转换：

![](../../figures/zh-cn_formulaimage_0000002549921601.png)

![](../../figures/zh-cn_formulaimage_0000002518441740.png)

![](../../figures/zh-cn_formulaimage_0000002518441752.png)

输出R'G'B'值饱和到范围\[0, 255\]。

第四个通道是通过将通道值设置为常量值aVal来创建的。

函数接口声明如下：

HmppResult HMPPI\_YCbCrToBGR\_709CSC\_8u\_P3C3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\);

HmppResult HMPPI\_YCbCrToBGR\_709CSC\_8u\_P3C4R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|创建第四个通道的常量值。|非负整数|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YCbCrToBGR_709CSC()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src1[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src2[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        21, 23, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        31, 24, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        41, 25, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        51, 26, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        61, 27, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src3[SRC_BUFFER_SIZE_T] = {
        11, 12, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        12, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        13, 32, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        14, 42, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        15, 52, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        16, 62, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    const uint8_t *src[3] = {src1, src2, src3};

    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 20 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    uint8_t aval = 254;
    HmppResult result = HMPPI_YCbCrToBGR_709CSC_8u_P3C4R(src, srcStep, dst, dstStep, roi, aval);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}
int main()
{
   TestExample_YCbCrToBGR_709CSC();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
  0  82   0 254   0  92   0 254   0  91   0 254   0  95   0 254   0   0   0   0
  0 105   0 254   0 109   0 254   0 114  12 254   0  86   0 254   0   0   0   0
  0 109   0 254   0 105   0 254   0 100   0 254   0  95   0 254   0   0   0   0
  0  71   0 254   0  69   0 254   0  91   0 254   0  95   0 254   0   0   0   0
  0   0   0   0   0   0   0   0   0   0
```

## YCbCrToRGB

将YCbCr图像转换为RGB颜色模型。

此函数将Y'Cb'Cr'图像src转换为24位伽马校正的R'G'B'图像dst。转换使用以下公式：

R' = 1.164\*\(Y' - 16\) + 1.596\*\(Cr' - 128\)

G' = 1.164\*\(Y' - 16\) - 0.813\*\(Cr' - 128\) - 0.392\*\(Cb' - 128\)

B' = 1.164\*\(Y' - 16\) + 2.017\*\(Cb' - 128\)

输出R'G'B'值饱和到范围\[0,255\]。

第四个通道是通过将通道值设置为常量值aval来创建的。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YCbCrToRGB\_8u\_C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YCbCrToRGB\_8u\_AC4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_YCbCrToRGB\_8u\_P3R\(const uint8\_t\* src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YCbCrToRGB\_8u\_P3C3R\(const uint8\_t\* src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **从平面到像素阶数据的转换：**

    HmppResult HMPPI\_YCbCrToRGB\_8u\_P3C4R\(const uint8\_t\* src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aval\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aval|创建第四个通道的常量值。|非负整数|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YCbCrToRGB()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_YCbCrToRGB_8u_C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_YCbCrToRGB();
}
```

运行结果：

```text
result = 0
dst =
  0 113   0   0 112   0   0 173   0   0 154   0   0   0   0
  0 111   0   0  64   0   0 155   0   0 152   0   0   0   0
  0 155   0   0 165   0   0 113   0   0 111   0   0   0   0
  0 113   0   0 112   0   0 173   0   0 154   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## YCbCr422ToRGB

此函数将YCbC422图像转换为RGB颜色模型，使用4:2:2采样。因为YCbC422格式图像采用4:2:2采样，即每2个像素块，采样2次Y，1次Cb和Cr。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YCbCr422ToRGB\_8u\_C2C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YCbCr422ToRGB\_8u\_C2C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aval\);

- **从像素阶到平面数据的转换：**

    HmppResult HMPPI\_YCbCr422ToRGB\_8u\_C2P3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **从平面数据到像素阶数据的转换：**

    HmppResult HMPPI\_YCbCr422ToRGB\_8u\_P3C3R\(const uint8\_t \*src\[3\], int32\_t srcStep\[3\], uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向像素顺序数据的源缓冲区的指针。在平面数据的情况下，用于分隔源颜色平面的指针数组。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向像素顺序数据的目标缓冲区的指针。在平面数据的情况下，指向分隔目标颜色平面的指针数组。|非空|输出|
|dstStep|指向目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aval|常量值，用来填充第四通道。|无|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值，P3C3R的roi宽需大于1。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 120
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    uint8_t a[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t b[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t c[DST_BUFFER_SIZE_T] = { 0 };
    uint8_t *dst[3] = {a, b, c};

    int32_t srcStep = 12 * sizeof(uint8_t);
    int32_t dstStep = 20 * sizeof(uint8_t);
    HmppResult result = HMPPI_YCbCr422ToRGB_8u_C2P3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    for(int32_t i = 0; i < 3; i++){
        int32_t dstWidth = dstStep / sizeof(uint8_t);
        for (int32_t j = 0; j < DST_BUFFER_SIZE_T; j++) {
            if( j % dstWidth == 0 ){
                printf("\n");
            }
            printf("%3d ",dst[i][j]);
        }
    }
    printf("\n");
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
  0   0   0   7   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  7   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
103 129 102 127   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
173 147 103 129   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
163 137 155 117   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
119  55 163 137   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## YUV420ToBGR

将采样格式为4:2:0的YUV图像转换为BGR颜色模型。

函数接口声明如下：

HmppResult HMPPI\_YUV420ToBGR\_8u\_P3C3R\(const uint8\_t\* src\[3\], int srcStep\[3\], uint8\_t \*dst, int dstStep,HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|imgSize|对于无ROI的操作，源图像和目标图像的大小。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YUV420ToBGR()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src1[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src2[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        21, 23, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        31, 24, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        41, 25, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        51, 26, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        61, 27, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src3[SRC_BUFFER_SIZE_T] = {
        11, 12, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        12, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        13, 32, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        14, 42, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        15, 52, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        16, 62, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    const uint8_t *src[3] = {src1, src2, src3};

    int32_t srcStep[3] = {20 * sizeof(uint8_t), 20 * sizeof(uint8_t), 20 * sizeof(uint8_t)};
    int32_t dstStep = 20 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    HmppResult result = HMPPI_YUV420ToBGR_8u_P3C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");
}
int main()
{
   TestExample_YUV420ToBGR();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
  0 124   0   0 135   0   0 141   0   0 152   0   0   0   0   0   0   0   0   0
  0 179   0   0 190   0   0 196   0   0 130   0   0   0   0   0   0   0   0   0
  0 137   6   0 126   0   0 104   0   0  93   0   0   0   0   0   0   0   0   0
  0  71   0   0  82   0   0  82   0   0  93   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0
```

## YUVToRGB

将YUV图像转换为RGB颜色模型。

此函数根据以下公式将Y'U'V'图像src转换为伽马校正的R'G'B'图像dst：

R' = Y' + 1.140\*V'

G' = Y' - 0.394\*U' - 0.581\*V'

B' = Y' + 2.032\*U'

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YUVToRGB\_8u\_C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YUVToRGB\_8u\_AC4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YUVToRGB\_8u\_C3C4R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aval\);

- **对平面数据的操作：**

    HmppResult HMPPI\_YUVToRGB\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **从平面到像素阶数据的转换：**

    HmppResult HMPPI\_YUVToRGB\_8u\_P3C3R\(const uint8\_t \*src\[3\], int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YUVToRGB()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    int32_t srcStep = 20 * sizeof(uint8_t);
    int32_t dstStep = 15 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};

    HmppResult result = HMPPI_YUVToRGB_8u_C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");

}

int main()
{
   TestExample_YUVToRGB();
}
```

运行结果：

```text
result = 0
dst =
  0 107   0   0 108   0   0 154   0  17 144   0   0   0   0
 20 109   0   0  67   0   0 143   0   0 140   0   0   0   0
  0 143   0   0 149   0   0 108   0   0 109   0   0   0   0
  0 107   0   0 108   0   0 154   0  17 144   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## YUV420ToRGB

将采样格式为4:2:0的YUV图像转换为RGB颜色模型。

此函数根据以下公式将Y'U'V'图像src转换为伽马校正的R'G'B'图像dst：

R' = Y' + 1.140\*V'

G' = Y' - 0.394\*U' - 0.581\*V'

B' = Y' + 2.032\*U'

函数接口声明如下：

HmppResult HMPPI\_YUV420ToRGB\_8u\_P3C3R\(const uint8\_t\* src\[3\], int srcStep\[3\], uint8\_t \*dst, int dstStep,HmppiSize roiSize\);

HmppResult HMPPI\_YUV420ToRGB\_8u\_P3AC4R\(const uint8\_t \* src\[3\], int srcStep\[3\], uint8\_t \*dst, int dstStep,HmppiSize roiSize\);

HmppResult HMPPI\_YUV420ToRGB\_8u\_P3R\(const uint8\_t \* src\[3\], int srcStep\[3\], uint8\_t \*dst\[3\], int dstStep,HmppiSize roiSize\);

HmppResult HMPPI\_YUV420ToRGB\_8u\_P3C3\(const uint8\_t \* src\[3\], uint8\_t \* dst, HmppiSize imgSize\);

HmppResult HMPPI\_YUV420ToRGB\_8u\_P3\(const uint8\_t \* src\[3\], uint8\_t \* dst\[3\], HmppiSize imgSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储指向源平面图像的颜色平面的指针。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储指向目标平面图像的颜色平面的指针。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|imgSize|对于无ROI的操作，源图像和目标图像的大小。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep中存在零或负值。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 90
#define DST_BUFFER_SIZE_T 90

void TestExample_YUV420ToRGB()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src1[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src2[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        21, 23, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        31, 24, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        41, 25, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        51, 26, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        61, 27, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };

    const uint8_t src3[SRC_BUFFER_SIZE_T] = {
        11, 12, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        12, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        13, 32, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        14, 42, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        15, 52, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        16, 62, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33
    };
    const uint8_t *src[3] = {src1, src2, src3};

    int32_t srcStep[3] = {20 * sizeof(uint8_t), 20 * sizeof(uint8_t), 20 * sizeof(uint8_t)};
    int32_t dstStep = 20 * sizeof(uint8_t);;
    uint8_t dst[DST_BUFFER_SIZE_T] = {0};
    HmppResult result = HMPPI_YUV420ToRGB_8u_P3C3R(src, srcStep, dst, dstStep, roi);

    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint8_t);
    for(int32_t i = 0; i < DST_BUFFER_SIZE_T; i++){
        if (i % dstWidth == 0) {
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");
}
int main()
{
   TestExample_YUV420ToRGB();
   return 0;
}
```

运行结果：

```text
result = 0
dst =
  0 124   0   0 135   0   0 141   0   0 152   0   0   0   0   0   0   0   0   0
  0 179   0   0 190   0   0 196   0   0 130   0   0   0   0   0   0   0   0   0
  6 137   0   0 126   0   0 104   0   0  93   0   0   0   0   0   0   0   0   0
  0  71   0   0  82   0   0  82   0   0  93   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0
```

## YUV422ToRGB

此函数将YUV422图像转换为RGB颜色模型，使用4:2:2采样。因为YUV422格式图像采用4:2:2采样，即每2个像素块，采样2次Y，1次U和V。

函数接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YUV422ToRGB\_8u\_C2C3R\(const uint8\_t \*src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作：**

    HmppResult HMPPI\_YUV422ToRGB\_8u\_P3R\(const uint8\_t \*src\[3\], int32\_t srcStep\[3\], uint8\_t \*dst\[3\], int32\_t dstStep, HmppiSize roiSize\);

- **对平面数据的操作（无ROI）：**

    HmppResult HMPPI\_YUV422ToRGB\_8u\_P3\(const uint8\_t \*src\[3\], uint8\_t \*dst\[3\], HmppiSize imgSize\);

- **从平面数据到像素阶数据的转换：**

    HmppResult HMPPI\_YUV422ToRGB\_8u\_P3AC4R\(const uint8\_t \*src\[3\], int32\_t srcStep\[3\], uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YUV422ToRGB\_8u\_P3C3R\(const uint8\_t \*src\[3\], int32\_t srcStep\[3\], uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

- **从平面数据到像素阶数据的转换（无ROI）：**

    HmppResult HMPPI\_YUV422ToRGB\_8u\_P3C3\(const uint8\_t \*src\[3\], uint8\_t \*dst, HmppiSize imgSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向像素顺序数据的源缓冲区的指针。在平面数据的情况下，用于分隔源颜色平面的指针数组。|非空|输入|
|srcStep|源图像中连续行起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向像素顺序数据的目标缓冲区的指针。在平面数据的情况下，指向分隔目标颜色平面的指针数组。|非空|输出|
|dstStep|指向目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|imgSize|源和目标图像的大小（以像素为单位）。|imgSize.width∈(0,INT_MAX]，imgSize.height∈(0,INT_MAX]|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零或负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 144
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };

    int32_t srcStep = 12 * sizeof(uint8_t);
    int32_t dstStep = 18 * sizeof(uint8_t);
    HmppResult result = HMPPI_YUV422ToRGB_8u_C2C3R(src, srcStep, dst, dstStep, roi);

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
  0 102   0   0 124   0   9 103   0  31 125   0   0   0   0   0   0   0
  0 156   0   0 134   0   0 102   0   0 124   0   0   0   0   0   0   0
  5 151   0   0 129   0   0 143   0   0 110   0   0   0   0   0   0   0
 31 116   0   0  61   0   0 150   0   0 128   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
```

## CbYCr422ToRGB

此类函数将采样格式为4:2:2的CbYCr图像转换为RGB图像。其中4:2:2的CbYCr采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518441592.png)

![](../../figures/zh-cn_formulaimage_0000002550041451.png)

![](../../figures/zh-cn_formulaimage_0000002518441600.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_CbYCr422ToRGB\_8u\_C2C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 96
#define DST_BUFFER_SIZE_T 144
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66,
        77, 88, 22, 88, 77, 66, 55, 44, 33, 11, 22, 33,
        44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };

    int32_t srcStep = 12 * sizeof(uint8_t);
    int32_t dstStep = 18 * sizeof(uint8_t);
    HmppResult result = HMPPI_CbYCr422ToRGB_8u_C2C3R(src, srcStep, dst, dstStep, roi);

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
0 130   0   0 156   0   0 128   0   2 154   0   0   0   0   0   0   0 
0 138   0   0 100   0   0 130   0   0 155   0   0   0   0   0   0   0 
0 137   0   0 111   0   0 148   0   0 135   0   0   0   0   0   0   0 
0 190   0   0 190   0   0 138   0   0 112   0   0   0   0   0   0   0 
0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0 
0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0 
0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0 
0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0 
```

## RGBToCbYCr422

此类函数将RGB图像转换为采样格式为4:2:2的CbYCr图像。其中4:2:2的CbYCr采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002549921533.png)

![](../../figures/zh-cn_formulaimage_0000002550041519.png)

![](../../figures/zh-cn_formulaimage_0000002518441668.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_RGBToCbYCr422\_8u\_C3C2R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToCbYCr422Gamma\_8u\_C3C2R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 128
#define DST_BUFFER_SIZE_T 60
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 0, 0, 0, 0,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22, 0, 0, 0, 0,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66, 0, 0, 0, 0,
        77, 88, 22, 88, 77, 66, 55, 44, 33, 11, 22, 33, 0, 0, 0, 0,
        44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 0, 0, 0, 0,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 0, 0, 0, 0,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22, 0, 0, 0, 0,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66, 0, 0, 0, 0
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };

    int32_t srcStep = 16 * sizeof(uint8_t);
    int32_t dstStep = 12 * sizeof(uint8_t);
    HmppResult result = HMPPI_RGBToCbYCr422_8u_C3C2R(src, srcStep, dst, dstStep, roi);

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
133  33 124  61 114  82 128  84   0   0   0   0 
126  56 129  33 126  61 124  82   0   0   0   0 
123  84 132  56 131  33 125  61   0   0   0   0 
113  82 129  84 125  56 131  33   0   0   0   0 
  0   0   0   0   0   0   0   0   0   0   0   0 
```

## YCbCr422ToGray

此类函数将采样格式为4:2:2的YCbCr图像转换为灰度图像。其中4:2:2的YCbCr采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002549921705.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_YCbCr422ToGray\_8u\_C2C1R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 128
#define DST_BUFFER_SIZE_T 60
void TestExample()
{
    HmppiSize roi = { 6, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 0, 0, 0, 0,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22, 0, 0, 0, 0,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66, 0, 0, 0, 0,
        77, 88, 22, 88, 77, 66, 55, 44, 33, 11, 22, 33, 0, 0, 0, 0,
        44, 55, 66, 77, 88, 22, 88, 77, 66, 55, 44, 33, 0, 0, 0, 0,
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66, 0, 0, 0, 0,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22, 0, 0, 0, 0,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66, 0, 0, 0, 0
    };

    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };

    int32_t srcStep = 16 * sizeof(uint8_t);
    int32_t dstStep = 8 * sizeof(uint8_t);
    HmppResult result = HMPPI_YCbCr422ToGray_8u_C2C1R(src, srcStep, dst, dstStep, roi);

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
 11  33  55  77  22  77   0   0 
 55  33  22  44  66  88   0   0 
 88  66  44  11  33  55   0   0 
 77  22  77  55  33  22   0   0 
  0   0   0   0   0   0   0   0 
  0   0   0   0   0   0   0   0 
  0   0   0   0   0   0   0   0 
  0   0   0   0
```

## YCbCr422ToBGR

此类函数将采样格式为4:2:2的YCbCr图像转换为BGR图像。其中4:2:2的YCbCr采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518281760.png)

![](../../figures/zh-cn_formulaimage_0000002518441656.png)

![](../../figures/zh-cn_formulaimage_0000002549921505.png)

函数的接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YCbCr422ToBGR\_8u\_C2C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\)

    HmppResult HMPPI\_YCbCr422ToBGR\_8u\_C2C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\)

- **从平面阶数据到像素阶数据的转换：**

    HmppResult HMPPI\_YCbCr422ToBGR\_8u\_P3C3R\(const uint8\_t\* src\[0x3\], int32\_t srcStep\[0x3\], uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈[2,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|常量值，用来填充第四通道。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零、负值，或roiSize.width的值小于2。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 48
#define DST_BUFFER_SIZE_T 60
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t srcY[SRC_BUFFER_SIZE_T] = {
        11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22,
        88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66,
        77, 88, 22, 88, 77, 66, 55, 44, 33, 11, 22, 33
    };

    const uint8_t srcCb[SRC_BUFFER_SIZE_T] = {
        177, 188, 122, 188, 177, 166, 155, 144,
        155, 144, 133, 111, 122, 133, 144, 155,
        188, 177, 166, 155, 144, 133, 111, 122,
        111, 122, 133, 144, 155, 166, 177, 188
    };

    const uint8_t srcCr[SRC_BUFFER_SIZE_T] = {
        255, 244, 233, 111, 212, 233, 244, 255,
        188, 177, 166, 255, 144, 233, 121, 122,
        177, 188, 222, 188, 177, 166, 155, 144,
        211, 222, 233, 244, 155, 166, 177, 188
    };

    const uint8_t *src[3] = {srcY, srcCb, srcCr};
    int32_t srcStep[3] = {12, 8, 8};
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 12 * sizeof(uint8_t);

    HmppResult result = HMPPI_YCbCr422ToBGR_8u_P3C3R(src, srcStep, dst, dstStep, roi);
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
 93   0 196 105   0 209 140   0 204 153   0 217 
 99   0 141  87   0 128  52   0  97  26   0  72 
204  20 162 192   7 149 157   0 153 144   0 141 
 36  10 203  49  22 216   0   0 157  71   9 233 
  0   0   0   0   0   0   0   0   0   0   0   0
```

## BGRToYCbCr422

此类函数将BGR图像转换为采样格式为4:2:2的YCbCr图像。其中4:2:2的YCbCr采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518281908.png)

![](../../figures/zh-cn_formulaimage_0000002518281914.png)

![](../../figures/zh-cn_formulaimage_0000002518281912.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_BGRToYCbCr422\_8u\_AC4C2R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_BGRToYCbCr422\_8u\_C3C2R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 64
#define DST_BUFFER_SIZE_T 60
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        121, 212, 131, 255, 11, 22, 33, 44, 55, 66, 77, 88, 22, 88, 77, 66,
        156, 93,  243, 200, 55, 44, 33, 11, 22, 33, 44, 55, 66, 77, 88, 22,
        4,   233, 109, 27,  88, 77, 66, 55, 44, 33, 11, 22, 33, 44, 55, 66,
        6,   7,   8,   0,   77, 88, 22, 88, 77, 66, 55, 44, 33, 11, 22, 33
    };

    int32_t srcStep = 16 * sizeof(uint8_t);
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 12 * sizeof(uint8_t);

    HmppResult result = HMPPI_BGRToYCbCr422_8u_AC4C2R(src, srcStep, dst, dstStep, roi);
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
168 111  37 116  74 111  82 131   0   0   0   0 
141 134  52 156  46 122  84 134   0   0   0   0 
162  90  80 106  40 129  56 126   0   0   0   0 
 22 130  74 114  71 135  30 127   0   0   0   0 
  0   0   0   0   0   0   0   0   0   0   0   0
```

## YCrCb422ToBGR

此类函数将采样格式为4:2:2的YCrCb图像转换为BGR图像。其中4:2:2的YCrCb采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518441866.png)

![](../../figures/zh-cn_formulaimage_0000002518281968.png)

![](../../figures/zh-cn_formulaimage_0000002549921721.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_YCrCb422ToBGR\_8u\_C2C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_YCrCb422ToBGR\_8u\_C2C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|常量值，用来填充第四通道。|非负整数|输入|

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
|HMPP_STS_DOUBLE_SIZE|roiSize.width值为奇数（此为警告码，函数仍可正常执行）。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 64
#define DST_BUFFER_SIZE_T 64
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        121, 212, 131, 255, 11, 22, 33, 44, 55, 66,
        156, 93,  243, 200, 55, 44, 33, 11, 22, 33,
        4,   233, 109, 27,  88, 77, 66, 55, 44, 33,
        6,   7,   8,   0,   77, 88, 22, 88, 77, 66,
        44,  33,  22,  11,  55, 66, 77, 88, 99, 10
    };

    int32_t srcStep = 10 * sizeof(uint8_t);
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 16 * sizeof(uint8_t);
    uint8_t aVal = 0x80;

    HmppResult result = HMPPI_YCrCb422ToBGR_8u_C2C4R(src, srcStep, dst, dstStep, roi, aVal);
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
255   4 255 128 255  16 255 128   0 113   0 128   0 139   0 128 
255 163 107 128 255 255 208 128   0 160   0 128   0 134   0 128 
  0   0 154 128   0  62 255 128   0 154   2 128   0 128   0 128 
  0 137   0 128   0 139   0 128   0 119   7 128   0  55   0 128
```

## YCrCb422ToRGB

此类函数将采样格式为4:2:2的YCrCb图像转换为RGB图像。其中4:2:2的YCrCb采样格式表示每2个像素块采样2次Y，1次Cb，1次Cr。

此类函数依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518441760.png)

![](../../figures/zh-cn_formulaimage_0000002518441764.png)

![](../../figures/zh-cn_formulaimage_0000002549921609.png)

函数的接口声明如下：

- **对像素阶数据的操作：**

    HmppResult HMPPI\_YCrCb422ToRGB\_8u\_C2C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize\);

    HmppResult HMPPI\_YCrCb422ToRGB\_8u\_C2C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\);

- **从像素阶数据到平面阶数据的转换：**

    HmppResult HMPPI\_YCrCb422ToRGB\_8u\_C2P3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t \*dst\[0x3\], int32\_t dstStep, HmppiSize roiSize\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|常量值，用来填充第四通道。|非负整数|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src或者dst中存在空指针。|
|HMPP_STS_SIZE_ERR|roiSize的字段为零、负值。|
|HMPP_STS_STEP_ERR|srcStep或者dstStep中存在零或负值。|
|HMPP_STS_ROI_ERR|roiSize.width *通道数* 数据类型所占字节数 > 步长。|
|HMPP_STS_DOUBLE_SIZE|roiSize.width值为奇数（此为警告码，函数仍可正常执行）。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 64
#define DST_BUFFER_SIZE_T 64
void TestExample()
{
    HmppiSize roi = { 4, 4 };
    const uint8_t src[SRC_BUFFER_SIZE_T] = {
        121, 212, 131, 255, 11, 22, 33, 44, 55, 66, 42, 10,
        156, 93,  243, 200, 55, 44, 33, 11, 22, 33, 30, 99,
        4,   233, 109, 27,  88, 77, 66, 55, 44, 33, 5,  22,
        6,   7,   8,   0,   77, 88, 22, 88, 77, 66, 1,   0,
        44,  33,  22,  11,  55, 66, 77, 88, 99, 10, 59, 32
    };

    int32_t srcStep = 12 * sizeof(uint8_t);
    uint8_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 16 * sizeof(uint8_t);
    uint8_t aVal = 0x40;

    HmppResult result = HMPPI_YCrCb422ToRGB_8u_C2C4R(src, srcStep, dst, dstStep, roi, aVal);
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
255   4 255  64 255  16 255  64   0 113   0  64   0 139   0  64 
107 163 255  64 208 255 255  64   0 160   0  64   0 134   0  64 
154   0   0  64 255  62   0  64   2 154   0  64   0 128   0  64 
  0 137   0  64   0 139   0  64   7 119   0  64   0  55   0  64
```

## RGBToGray

此类函数将RGB图像转换为灰度图像，并依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002549921369.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_RGBToGray\_8u\_C3C1R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_8u\_AC4C1R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_16u\_C3C1R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_16u\_AC4C1R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_16s\_C3C1R\(const int16\_t\* src, int32\_t srcStep, int16\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_16s\_AC4C1R\(const int16\_t\* src, int32\_t srcStep, int16\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_32f\_C3C1R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_RGBToGray\_32f\_AC4C1R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|

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
|HMPP_STS_NOT_EVEN_STEP_ERR|当数据类型不为uint8_t时，步长为奇数。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 64
#define DST_BUFFER_SIZE_T 32
void TestExample()
{
    HmppiSize roi = { 3, 5 };
    const int16_t src[SRC_BUFFER_SIZE_T] = {
        121, 212, 131, 255, 11, 22, 33, 44, 55, 66, 42, 10,
        156, 93,  243, 200, 55, 44, 33, 11, 22, 33, 30, 99,
        4,   233, 109, 27,  88, 77, 66, 55, 44, 33, 5,  22,
        6,   7,   8,   0,   77, 88, 22, 88, 77, 66, 1,   0,
        44,  33,  22,  11,  55, 66, 77, 88, 99, 10, 59, 32
    };

    int32_t srcStep = 12 * sizeof(int16_t);
    int16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 6 * sizeof(int16_t);

    HmppResult result = HMPPI_RGBToGray_16s_C3C1R(src, srcStep, dst, dstStep, roi);
    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(int16_t);
    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; i++) {
        if( i % dstWidth == 0 ){
            printf("\n");
        }
        printf("%3d ",dst[i]);
    }
    printf("\n");
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
176  85  42   0   0   0 
129  97  19   0   0   0 
150  69  57   0   0   0 
  7  55  67   0   0   0 
 35  43  86   0   0   0 
  0   0
```

## GrayToRGB

此类函数将灰度图像转换为RGB图像，并依据以下公式进行颜色转换：

![](../../figures/zh-cn_formulaimage_0000002518281636.png)

![](../../figures/zh-cn_formulaimage_0000002518441548.png)

![](../../figures/zh-cn_formulaimage_0000002518441532.png)

函数的接口声明如下：

**对像素阶数据的操作：**

HmppResult HMPPI\_GrayToRGB\_8u\_C1C3R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_GrayToRGB\_8u\_C1C4R\(const uint8\_t\* src, int32\_t srcStep, uint8\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint8\_t aVal\)

HmppResult HMPPI\_GrayToRGB\_16u\_C1C3R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_GrayToRGB\_16u\_C1C4R\(const uint16\_t\* src, int32\_t srcStep, uint16\_t\* dst, int32\_t dstStep, HmppiSize roiSize, uint16\_t aVal\)

HmppResult HMPPI\_GrayToRGB\_32f\_C1C3R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize\)

HmppResult HMPPI\_GrayToRGB\_32f\_C1C4R\(const float\* src, int32\_t srcStep, float\* dst, int32\_t dstStep, HmppiSize roiSize, float aVal\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源图像的指针。该数组存储源平面图像的颜色数据。|非空|输入|
|srcStep|源图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|dst|指向目的图像感兴趣区域的指针。该数组存储目标平面图像的颜色数据。|非空|输出|
|dstStep|目标图像中连续行的起点之间的距离（以字节为单位）。|非负整数|输入|
|roiSize|源和目标图像感兴趣区域的大小（以像素为单位）。|roiSize.width∈(0,INT_MAX]，roiSize.height∈(0,INT_MAX]|输入|
|aVal|常量值，用来填充第四通道。|非负整数|输入|

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
|HMPP_STS_NOT_EVEN_STEP_ERR|当数据类型不为uint8_t时，步长为奇数。|
|HMPP_STS_NO_ERR|返回值正确，任何其他值表示错误或警告。|

**示例**

```c
#define SRC_BUFFER_SIZE_T 32
#define DST_BUFFER_SIZE_T 84
void TestExample()
{
    HmppiSize roi = { 3, 4 };
    const uint16_t src[SRC_BUFFER_SIZE_T] = {
        12123, 21243, 13165, 255, 1, 2,
        10256, 11193, 30243, 200, 3, 4,
        4,     233,   109,    27, 5, 6,
        6,     7,     8,       0, 7, 8
    };

    int32_t srcStep = 6 * sizeof(uint16_t);
    uint16_t dst[DST_BUFFER_SIZE_T] = { 0 };
    int32_t dstStep = 14 * sizeof(uint16_t);
    uint16_t aVal = 0x8000;

    HmppResult result = HMPPI_GrayToRGB_16u_C1C4R(src, srcStep, dst, dstStep, roi, aVal);
    printf("result = %d \ndst = ", result);
    if (result != HMPP_STS_NO_ERR) {
        printf("result error: %d\n", result);
    }
    int32_t dstWidth = dstStep / sizeof(uint16_t);
    for (int32_t i = 0; i < DST_BUFFER_SIZE_T; i++) {
        if( i % dstWidth == 0 ){
            printf("\n");
        }
        printf("%5d ",dst[i]);
    }
    printf("\n");
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
12123 12123 12123 32768 21243 21243 21243 32768 13165 13165 13165 32768     0     0 
10256 10256 10256 32768 11193 11193 11193 32768 30243 30243 30243 32768     0     0 
    4     4     4 32768   233   233   233 32768   109   109   109 32768     0     0 
    6     6     6 32768     7     7     7 32768     8     8     8 32768     0     0 
    0     0     0     0     0     0     0     0     0     0     0     0     0     0 
    0     0     0     0     0     0     0     0     0     0     0     0     0     0
```
