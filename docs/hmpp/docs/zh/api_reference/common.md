# HMPP公共说明

[返回HMPP API参考](../api_reference.md)

## HMPP函数库结构

### HMPP头文件说明

HMPP各个模块头文件说明如下表所示：

|文件名|说明|
|--|--|
|hmpp.h|定义函数库版本号。|
|hmpp_core.h|定义函数库公用的字节对齐、内存分配和内存释放函数。|
|hmpps.h|信号库的声明文件。|
|hmppi.h|图像库的声明文件。|
|hmpp_type.h|定义函数库使用的结构体、枚举类型和错误码。|
|hmpp_typebase.h|定义函数库的基本数据类型。|

### 命名规则

#### 函数命名规则

HMPP函数库的函数命名需遵循通用格式：**[HMPPS|HMPPI|HMPPA]_<name\>\_<datatype\>[\_<descriptor\>]\(<parameters\>)**

例如：HmppResult HMPPS_MulC_64f64s_IS(double val, int64_t *srcDst, uint32_t len, int32_t scale);

在上述例子中：

1. 前缀为HMPP（基础函数）、HMPPI（图像库）或HMPPS（信号库）。

2. MulC是函数名，表明该函数实现的功能是向量与常数相乘。

3. 64f64s表示该函数两个入参的数据类型，分别为64f（double）和64s（long long）。

4. 扩充描述_IS：

    I：表明该函数是原地操作函数，函数从输入向量依次取值，经过一系列运算以后将结果保存在源向量中。

    S：表明该函数使用入参的度量因子对输出作了缩放处理，实际结果可通过输出向量与缩放因子经过计算以后复原，保留计算精度。

5. 圆括号里面为该接口传入的参数。

**函数名<name\>**

函数名表明该函数的主要功能，格式为：<name\>=<operation\>\[\_modifier\]。其中：

- **operation**通常由一个或多个单词、首字母缩略词、缩写组成，描述该函数的基本功能。
- **modifier**在部分函数中使用，由单词或者缩写组成，描述该函数的扩展功能。例如计算向量范数的norm、normdiff函数，会有一个标识符表明该函数计算的是1-范数（\_L1）、2-范数（\_L2）还是无穷范数（\_Inf），类似的函数还有threshold、fft。

**数据类型<datatype\>**

数据类型表明该函数处理的数据类型，通常为函数参数的数据类型。HMPP中所使用的数据类型请参见[基本数据类型](#基本数据类型)。

数据类型的格式为\<bit depth\>\<bit interpretation\>。其中：

- **bit depth**表示位宽度，常用的位宽度有8位、16位、32位、64位。
- **bit interpretation**表示数据类型，通常有无符号整型u（unsigned integer）、有符号整型s（signed integer）、浮点类型f（float point）、复数c（complex）。

对于只处理一种类型数据的函数，<datatype\>域只包含上表中列出的一种值。如果函数处理的源向量和目标向量的数据类型不同，源向量和目标向量各自的数据类型都会体现在函数命名中，并且遵循固定的顺序：<datatype\>=<src1Datatype\>\[src2Datatype\]\[dstDatatype\]。

例如，HMPPS\_DotProd\_32f32fc计算两个数据类型分别为32位浮点型和32位浮点型复数的源向量的点积，并将计算结果存储在32位浮点型复数的目标向量中。

**描述符<descriptor\>**

描述符由一个或多个字母组成，显示更多关于函数的细节，进一步描述函数的功能。

主要描述符如下表所示：

|取值|说明|举例|
|--|--|--|
|I|函数执行的是原地操作，即源向量和目标向量是同一个向量（默认是非原地操作）。例如，原地加法操作计算公式为：srcDst = srcDst + src。|HMPPS_Add_16s_I|
|S|函数结果饱和并且缩放模式固定（默认饱和且无缩放）。|HMPPS_Add_16s_S|
|A*xx*|保证*xx*位二进制有效位的舍入。|HMPPS_Powx_32f_A11|

>![](../public_sys-resources/icon-note.gif) **说明：** 
>
>- 如果函数有两个或两个以上描述符，描述符在函数名中以字母表顺序依次展现（例如：HMPPS\_Add\_16s\_IS）。
>- 对于无描述符的函数，函数名中不包含该字段。

**函数参数<parameters\>**

<parameters\>元素指定该函数的所有参数。

参数的排列顺序按如下规则：

1. 源操作数，通常是一定长度的向量或常量。
2. 目标操作数。
3. 其他包含特定操作的参数。

参数命名遵守如下约定：

1. 每个参数名指定其功能。
2. 输入参数以src命名，特定情形中加上数字或单词进一步说明其含义（例如src2、srcLen）。
3. 输出参数以dst命名，与输入参数一样，特定情形中加上数字或单词进一步说明其含义（例如dst2、dstLen）。
4. 对于原地操作函数，输入/输出参数以srcDst命名。

#### 函数返回值

返回值：即错误码，定义在枚举类型HmppResult中。反映库函数执行状态。

程序不提供缓冲区来存储最终错误状态，调用者需要自行决定在函数返回时是否检查错误码。

错误码及其说明如下表所示：

|值|说明|
|--|--|
|0|HMPP_STS_NO_ERR，无错误。|
|1~199|图像库和信号库通用错误，例如空指针，大小错误等。|
|200~299|图像库和信号库通用告警，例如不支持的模式，操作长度溢出等。|
|300~399|图像库和信号库通用告警，但不改变代码流程，例如除零操作、负数开根号操作等。|
|400~599|信号库错误。|
|600~799|图像库错误。|

>![](../public_sys-resources/icon-notice.gif) **须知：** 
>返回非0的错误码并不表示函数没有执行完成。具体依函数处理逻辑而定。
>
>- 以HMPPS\_Div\_32f为例：
> 将常数0作为除数进行计算时，函数执行不会被中断，本次除法运算的结果被置为源向量的数据类型的最大值，即FLT\_MAX，函数返回状态码HMPP\_STS\_DIV\_BY\_ZERO。
>- 以HMPPS\_DivC\_64fc为例：
> 将常数0作为除数进行计算时，函数执行被中断并立即返回错误码HMPP\_STS\_DIV\_BY\_ZERO\_ERR。

#### 函数前缀

函数前缀命名需要遵循的规则：全大写。

函数前缀命名说明如下表所示：

|前缀|说明|
|--|--|
|HMPP|基础函数|
|HMPPI|图像库相关函数|
|HMPPS|信号库相关函数|

#### 函数功能

函数功能命名规则：大驼峰+首字母缩写。

函数功能的说明与命名规则如下表所示：

|函数功能|说明|命名规则|
|--|--|--|
|Add|加计算功能|大驼峰。|
|AddC|加一个常数|大驼峰+Constant首字母缩写，整体遵守大驼峰。|
|LShiftC|左移一个常数|L、C缩写，整体遵守大驼峰。|
|FFTInv|FFT反变换|FFT为缩写，整体遵守大驼峰。|

#### 特殊处理标记

特殊处理标记命名规则：大写，多个标记可共存。

特殊处理标记说明如下表所示：

|特殊处理标记|说明|
|--|--|
|I|In_Place操作，操作结果写回到数据源。|
|S|Scale操作；表征该函数会附加一个浮点数缩放因子scale作为函数参数。缩放因子必须为2<sup>n</sup>，且不能等于正无穷，不能为NaN。带缩放因子的函数在函数返回之前，将输出向量除以scale来实现对计算结果的精度调整，这种操作有助于保留输出数据的范围或其精度。|
|P|操作只在指定数量的vectors上进行。|
|A|图像数据包含alpha信道作为最后一个信道，需要C4，alpha信道不会被处理。|
|A0|图像数据包含alpha信道作为第一个信道，需要C4，alpha信道不会被处理。|
|C|操作使用Channel of interest（COI）。|
|R|函数对每个源图像定义感兴趣区域（ROI）进行操作。|
|V|函数对每个源图像定义的感兴趣体积（VOI）进行操作。|
|C1|图像数据按像素排序并且由1个分离交错的信道组成。|
|C2|图像数据按像素排序并且由2个分离交错的信道组成。|
|C3|图像数据按像素排序并且由3个分离交错的信道组成。|
|C4|图像数据按像素排序并且由4个分离交错的信道组成。|
|M|操作使用掩码来确定要处理的像素。|
|P2|图像数据是平面顺序，并且由2个（无交错）信道组成，每个平面都有一个单独的指针。|
|P3|图像数据是平面顺序，并且由3个分离的平面（无交错）信道组成，每个平面都有一个单独的指针。|
|P4|图像数据是平面顺序，并且由4个分离的平面（无交错）信道组成，每个平面都有一个单独的指针。|

#### 函数入参

函数入参格式为：<参数类型\> <形参变量名\>。

其中，<参数类型\>包括基本数据类型（请参见[基本数据类型](#基本数据类型)）、结构体和枚举类型（请参见[结构体与枚举](#结构体与枚举类型)）。

<形参变量名\>以小驼峰形式命名，如src、value、srcStep。

### 基本数据类型

#### 标准库数据类型和范围

下表为aarch64系统中C标准库定义的数据类型阈值的宏定义和范围。

|类型|数据类型|最小值--宏|最小值--值|最大值--宏|最大值--值|
|--|--|--|--|--|--|
|8u|uint8_t|-|0|UINT8_MAX|255，即2<sup>8</sup>-1|
|8s|int8_t|INT8_MIN|-INT8_MAX - 1，即-2<sup>7</sup>|INT8_MAX|127，即2<sup>7</sup>-1|
|16u|uint16_t|-|0|UINT16_MAX|65535，即2<sup>16</sup>-1|
|16s|int16_t|INT16_MIN|-INT16_MAX - 1，即-2<sup>15</sup>|INT16_MAX|32767，即2<sup>15</sup>-1|
|16f|float16_t|FLT16_MIN|6.10351562500000000000000000000000000e-5F16|FLT16_MAX|6.55040000000000000000000000000000000e+4F16|
|32u|uint32_t|-|0|UINT32_MAX|4294967295U，即2<sup>32</sup>-1|
|32s|int32_t|INT32_MIN|-INT32_MAX - 1，即-2<sup>31</sup>|INT32_MAX|2147483647，即2<sup>31</sup>-1|
|32f|float|FLT_MIN|1.17549435082228750796873653722224568e-38F|FLT_MAX|3.40282346638528859811704183484516925e+38F|
|64s|int64_t|INT64_MIN|-INT64_MAX - 1，即-2<sup>63</sup>|INT64_MAX|9223372036854775807L，即2<sup>63</sup>-1|
|64f|double|DBL_MIN|2.22507385850720138309023271733240406e-308L|DBL_MAX|1.79769313486231570814527423731704357e+308L|

#### 自定义数据类型和范围

**复数类型**

HMPP的复数类型由两个基本数据类型的成员组成的结构体来描述，分别表示复数的实部和虚部。

定义格式如下：

```c
typedef struct {
    <数据类型>  re;
    <数据类型>  im;
} Hmpp<类型>c;
```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>格式中的_<类型\>_和_<数据类型\>_的对应请参见[标准库数据类型和范围](#标准库数据类型和范围)。

HMPP中使用的复数类型定义在hmpp\_typebase.h中，包括：Hmpp16sc、Hmpp16uc、Hmpp32sc、Hmpp32fc、Hmpp64sc、Hmpp64fc、Hmpp8sc。

**Bool类型**

以枚举形式定义（定义在hmpp\_typebase.h中），如下：

```c
typedef enum {
    HMPP_FALSE = 0,
    HMPP_TRUE = 1
} HmppBool;
```

**特殊数据类型**

部分数据类型（如24s）不被HMPP库支持，可以通过HMPP中Convert函数转换成HMPP库支持的数据类型，以便后续处理。

对于24位有符号数据，每个向量元素由三个连续的8位无符号字符型（uint8\_t）字节组成，以小端字节序的形式存储（向量元素的低8位存储在低地址中），并用高8位的第一位作为符号位。这些数据通过HMPP中Convert函数可以转换为32位有符号整型（32s）或32位浮点型（32f）。

下表为HMPP自定义数据范围。

|宏定义|最大值|说明|
|--|--|--|
|UINT24_MAX|16777215|未定义对应的数据类型，存储在uint32_t类型变量中。|
|INT24_MAX|8388607|未定义对应的数据类型，存储在int32_t类型变量中。|
|INT24_MIN|-8388608|未定义对应的数据类型，存储在int32_t类型变量中。|

### 结构体与枚举类型

#### 结构体与枚举分类说明

结构体包含：复数型结构体和函数上下文结构体。

枚举包含：函数返回值和Bool类型等。

结构体命名规则及形式如下表所示：

|结构体类型|命名规则|形式|
|--|--|--|
|类型名|以Hmpp为前缀的大驼峰。|例如：HmppLibraryVersion|
|成员变量名|小驼峰。|例如：major，buildDate|

枚举类型命名规则及形式如下表所示：

|枚举类型|命名规则|形式|
|--|--|--|
|类型名|以Hmpp为前缀的大驼峰。|例如：HmppResult|
|成员变量名|全大写，下划线分割。|例如：HMPP_STS_NO_ERR|

#### 结构体

**复数型结构体**

请参见[自定义数据类型和范围](#自定义数据类型和范围)中的复数类型。

**函数上下文结构体**

HMPP函数库中定义了一些特殊的结构体，用来存储特定函数的上下文信息。例如，结构体HmppFFTSpec存储了快速傅里叶变换所需的旋转因子和位反转索引。按照结构体被函数引用以后的状态可分为两种不同的上下文结构体：

- 函数引用结构体的过程中没有修改其成员变量，这种结构体的名称前缀含有_Spec_。
- 函数引用结构体的过程中修改了它的成员变量，这种结构体的名称前缀含有_State_。

函数上下文解释依赖于处理器。因此，这些上下文相关的结构没有在公共头文件中定义，它们的字段也不可访问。HMPP函数库没有提供修改这些结构体或创建函数上下文作为自动变量的功能。

#### 枚举类型

常量HmppResult枚举了HMPP函数返回的状态值，表明操作是否有错误。

详细的信号处理函数的有效状态值以及错误信息请参见[函数返回值](#函数返回值)。

- 枚举类型HmppAlgMode定义了一些函数使用的算法类型：

    ```c
    typedef enum {
        HMPP_ALG_AUTO,      // Automatic algorithm selection based on the data scale.
        HMPP_ALG_DEFAULT,   // Direct calculation based on definition.
        HMPP_ALG_FFT,       // Use FFT to accelerate computing.
        HMPP_ALG_MASK = 0x000000FF
    } HmppAlgMode;
    ```

- 枚举类型HmppNormMode定义了一些函数使用的算法类型：

    ```c
    typedef enum {
        HMPP_NORM_NORMAL,
        HMPP_NORM_BIASED,
        HMPP_NORM_UNBIASED,
        HMPP_NORM_MASK
    } HmppNormMode;
    ```

- 枚举类型HmppCmpOp定义了临界值函数（threshold）中的关系操作符类型：

    ```c
    typedef enum {
        HMPP_CMP_LESS,    //当src[i]<level时，将level的值赋给dst[i]，否则将src[i]的值赋给dst[i].
        HMPP_CMP_GREATER  //当src[i]>level时，将level的值赋给dst[i]，否则将src[i]的值赋给dst[i].
    } HmppCmpOp;
    ```

- 枚举类型HmppRoundMode定义了转换函数中使用的舍入模式：

    ```c
    typedef enum {
        HMPP_RND_ZERO,        //取整舍入，对于浮点数输入，强制转换为整型输出
        HMPP_RND_NEAR,        //最近偶数舍入，四舍六入五取偶
        HMPP_RND_FINANCIAL,    //四舍五入
        HMPP_RND_HINT_ACCURATE
    } HmppRoundMode;
    ```

- 枚举类型HmppHintAlgorithm定义了一些函数中使用的计算方式类型，具体表现为计算速度快但有精度损失，或者保证精度但计算速度慢：

    ```c
    typedef enum {
        HMPP_ALGHINT_NONE,        //与HMPP_ALGHINT_FAST一致
        HMPP_ALGHINT_FAST,        //计算速度快，有精度损失
        HMPP_ALGHINT_ACCURATE     //保证结果精度，计算速度慢
    } HmppHintAlgorithm;
    ```

- 枚举类型HmppZCType定义了跨0次数计算函数中使用的计算方法类型：

    ```c
    typedef enum {
        HMPP_ZCR,
        HMPP_ZCX_OR,
        HMPP_ZCC
    } HmppZCType;
    ```

- 枚举类型HmppiBorderType定义了图像边界填充的方法类型：

    ```c
    typedef enum {
        HMPPI_BORDER_REPL = 1,
        HMPPI_BORDER_WRAP = 2,
        HMPPI_BORDER_MIRROR = 3,
        HMPPI_BORDER_MIRROR_R = 4,
        HMPPI_BORDER_DEFAULT = 5,
        HMPPI_BORDER_CONST = 6,
        HMPPI_BORDER_TRANSP = 7,
        HMPPI_BORDER_IN_MEM_TOP = 0x0010,
        HMPPI_BORDER_IN_MEM_BOTTOM = 0x0020,
        HMPPI_BORDER_IN_MEM_LEFT = 0x0040,
        HMPPI_BORDER_IN_MEM_RIGHT = 0x0080,
        HMPPI_BORDER_IN_MEM = HMPPI_BORDER_IN_MEM_LEFT | HMPPI_BORDER_IN_MEM_TOP | \
                              HMPPI_BORDER_IN_MEM_RIGHT | HMPPI_BORDER_IN_MEM_BOTTOM, // 0xF0
    } HmppiBorderType;
    ```

    边界填充说明如下：

    - **HMPP\_BORDER\_CONST：**

        所有边框像素的值都设置为常数。使用恒定边框时，所有边框像素的值都将设置为borderValue参数中指定的恒定值。在下图中，此常数值标记为V。红色标记的正方形对应于从源图像ROI复制的像素。

        ![](../figures/zh-cn_image_0000002518441648.png)

    - **HMPP\_BORDER\_DEFAULT：**

        边框设置为HMPP\_BORDER\_CONST，填充值依据基础操作选用填充的固定值。比如膨胀功能接口其borderValue=  **MIN\_VALUE**（源数据类型的最小值），腐蚀功能接口其borderValue=  **MAX\_VALUE**（源数据类型的最大值），下图用m表示源图像数据类型固定值。

        ![](../figures/zh-cn_image_0000002518441644.png)

    - **HMPP\_BORDER\_REPL：**

        边框从边缘像素复制而来。当使用复制的border时，从源图像边界像素获得边界像素的值，如下图所示。

        ![](../figures/zh-cn_image_0000002550041491.png)

    - **HMPP\_BORDER\_IN\_MEM：**

        边框是从内存中的源图像像素获得的。如果ROI不能覆盖源图像的内部边框像素，请使用此边框类型。在这种情况下，从内存中的源图像像素获得边界像素的值。在下图中，标记为红色的正方形对应于从源图像ROI复制的像素。黑色值的正方形对应于内存中的源图像像素。

        ![](../figures/zh-cn_image_0000002549921493.png)

    - **HMPP\_BORDER\_MIRROR：**

        边界像素从源图像边界像素镜像而来。当使用镜像边框时，边框像素的值是从源图像边界像素获得的，如下图所示。用红色标记的正方形对应于从源图像ROI复制的像素。绿色值的正方形对应于边框像素，该边框像素从源图像像素镜像获得。

        ![](../figures/zh-cn_image_0000002550041483.png)

- 枚举类型HmppAmrnbMode定义了AMRNB协议的编码速率：

    ```c
    typedef enum {
        HMPP_AMRNB_MR475,   //码率4.75kbit/s
        HMPP_AMRNB_MR515,   //码率5.15kbit/s
        HMPP_AMRNB_MR59,    //码率5.9kbit/s
        HMPP_AMRNB_MR67,    //码率6.7kbit/s
        HMPP_AMRNB_MR74,    //码率7.4kbit/s
        HMPP_AMRNB_MR795,   //码率7.95kbit/s
        HMPP_AMRNB_MR102,   //码率10.2kbit/s
        HMPP_AMRNB_MR122    //码率12.2kbit/s
    } HmppAmrnbMode;
    ```

- 枚举类型HmppAmrwbMode定义了AMRWB协议的编码速率：

    ```c
    typedef enum {
        HMPP_AMRWB_6600,    //码率6.6kbit/s
        HMPP_AMRWB_8850,    //码率8.85kbit/s
        HMPP_AMRWB_12650,   //码率12.65kbit/s
        HMPP_AMRWB_14250,   //码率14.25kbit/s
        HMPP_AMRWB_15850,   //码率15.85kbit/s
        HMPP_AMRWB_18250,   //码率18.25kbit/s
        HMPP_AMRWB_19850,   //码率19.85kbit/s
        HMPP_AMRWB_23050,   //码率23.05kbit/s
        HMPP_AMRWB_23850,   //码率23.85kbit/s
    } HmppAmrwbMode;
    ```

### 整数缩放

由于一些信号处理函数在进行内部计算时提高了数据精度，在调用这些函数时会附加一个缩放因子作为函数参数，被调用函数使用整数缩放因子对内部计算的结果进行缩放。缩放因子必须为2<sup>n</sup>，且不能等于正无穷，不能为NaN，这些信号处理函数的函数名带有S描述符。

带缩放因子的函数在函数返回之前，将输出向量乘以scale来实现对计算结果的精度调整，这种操作有助于保留输出数据的范围或精度。

例如，对16位有符号整数300进行乘方运算时，由于运算结果溢出，实际能存储的结果为32767而不是90000。当缩放因子scale被设为0.25（即2<sup>-2</sup>）时，存储的计算结果为22500，没有溢出，并且实际的计算结果可以通过22500\*4来复原。

对于需要部分保留精度的情况，例如整数3的开方运算，HMPPS\_Sqrt函数会调用库函数sqrt进行计算。在不传递scale的情况下，程序输出结果是2而不是1.732；假如给程序传递缩放参数8（即2<sup>3</sup>），程序会对计算结果缩放并输出结果14，程序调用者可使用输出结果14和缩放参数0.125（即2<sup>-3</sup>）经计算得到更精确的结果：14\*2<sup>-3</sup>=1.75。
