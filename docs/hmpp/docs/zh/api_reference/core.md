# HMPP基础函数API参考

[返回HMPP API参考](../api_reference.md)

## 基础函数

### 函数说明

该模块实现了63个基础函数，包含字节对齐、内存分配、内存释放、获取状态码描述、多线程设置及线程信息获取、获取CPU的缓存、时钟频率和CPU时间戳，以及HMPP版本信息、指令信息获取及设置和FlushToZero模式开闭等函数。

>![](../public_sys-resources/icon-note.gif) **说明：** 
>以下所提供的所有接口示例代码引用的HMPP头文件都为hmpp.h。

### AlignPtr

**将传入地址按指定字节进行对齐：**

void\* HMPP\_AlignPtr\(void \*ptr, int32\_t align\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|ptr|源地址。|非空|输入|
|align|对齐字节。|2的整数次幂|输入|

**返回值**

- 成功：对齐的字节是2的整数次幂，返回地址是按align对齐的。
- 失败：对齐的字节不是2的整数次幂，返回的地址不保证是按align对齐。

**错误码**

无

**注意**

- 此函数的入参正确性需要用户自己保证，不会有错误状态码，调用函数时，注意判断返回值，否则有可能操作结果和预期不一致。
- 接口不作越界保护，上层调用代码需确保内存不越界。

**示例**

```c
#define BUFFER_SIZE 100
#define ALIGN_BYTE 64
void AlignPtr_Example() {
    void *ptr = malloc(BUFFER_SIZE);
    if (ptr != NULL) {
        void *alignPtr = HMPP_AlignPtr(ptr, ALIGN_BYTE);
        if ((uint64_t)alignPtr % ALIGN_BYTE == 0) {
            printf("%d byte align\n", ALIGN_BYTE);
        } else {
            printf("not byte align\n");
        }
    }
}
```

运行结果：

```text
64 byte align
```

### Malloc和Free

- **申请指定字节的内存大小：**

    void\* HMPP\_Malloc\(int32\_t len\);

- **释放内存：**

    void HMPP\_Free\(void\* ptr\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|len|字节长度（HMPP_Malloc函数）。数组长度（HMPPS_Malloc_xxx函数）。|大于0|输入|
|ptr|要释放内存的地址（Free函数）。|非空|输入|

**返回值**

HMPP\_Malloc函数：

- 成功：返回申请内存的首地址。
- 失败：返回NULL。

**注意**

Free函数入参一定是Malloc函数返回值。

**示例**

```c
#define BUFFER_SIZE 100
void Malloc_Free_Example() {
    void *ptr = HMPP_Malloc(BUFFER_SIZE);
    int suc = (ptr != NULL);
    HMPP_Free(ptr);
    uint8_t *ptrs = HMPPS_Malloc_8u(BUFFER_SIZE);
    suc = suc & (ptrs != NULL);
    HMPPS_Free(ptrs);
    printf("%d", suc);
}
```

运行结果：

```text
1
```

### GetStatusString

**获取状态码描述：**

const char\* HMPP\_GetStatusString\(HmppResult result\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|result|状态码。|HMPPResult中出现的枚举类型。|输入|

**返回值**

- 成功：返回状态码的对应描述。
- 失败：返回“Not Found This Error Description”。

**错误码**

无

**注意**

不要释放返回指针指向的内存。

**示例**

```c
void  HMPP_GetStatusString_Example()
{
    printf("%s\n", HMPP_GetStatusString(HMPP_STS_NO_ERR));
    printf("%s\n", HMPP_GetStatusString(HMPP_STS_NULL_PTR_ERR));
    printf("%s\n", HMPP_GetStatusString(HMPP_STS_SIZE_ERR));
    printf("%s\n", HMPP_GetStatusString(HMPP_STS_NOT_SUPPORT));
}
```

运行结果：

```text
No Error
Null Pointer Error
Vector size <= 0 Error
This system does not support this function
```

### Thread

- **设置多线程数上限：**

    HmppResult HMPP\_SetNumberThreads \(int32\_t numberThreads\);

- **获取当前的线程数：**

    HmppResult HMPP\_GetNumberThreads \(int32\_t\* numberThreads\);

- **获取当前线程ID：**

    HmppResult HMPP\_GetThreadIdx \(int32\_t\* threadIdx\);

- **获取当前多线程模式：**

    HmppResult HMPP\_GetThreadType \(HmppThreadType\* threadType\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|numberThreads|要限定的线程数上限（SetNumberThreads）。|大于0|输入|
|numberThreads|目标地址，指向内存存放当前线程数（GetNumberThreads）。|非空|输出|
|threadIdx|目标地址，指向内存存放当前线程的ID。|非空|输出|
|threadType|目标地址，指向内存存放当前线程模式。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|传入指针是空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#define NUMBER_THREAD_LIM 4
void  Thread_Example()
{
    HmppResult result = HMPP_SetNumberThreads(NUMBER_THREAD_LIM);
    printf("%s\n", HMPP_GetStatusString(result));

    HmppThreadType testThrType;
    result = HMPP_GetThreadType(&testThrType);
    printf("%s\n", HMPP_GetStatusString(result));

    if (testThrType == OMP) {
        printf("thread mode is omp\n");
        #pragma omp parallel for
        for (int32_t i = 0; i < NUMBER_THREAD_LIM; ++i) {
            int32_t testNumThr, testThrIdx;
            HMPP_GetNumberThreads(&testNumThr);
            HMPP_GetThreadIdx(&testThrIdx);
            printf("testNumThr = %d, testThrIdx = %d\n", testNumThr, testThrIdx);
        }
    } else {
        printf("no thread mode\n");
        int32_t testNumThr, testThrIdx;
        HMPP_GetNumberThreads(&testNumThr);
        HMPP_GetThreadIdx(&testThrIdx);
        printf("testNumThr = %d, testThrIdx = %d\n", testNumThr, testThrIdx);
    }
}
```

运行结果：

- 第一种：HMPP库没有开启多线程模式。

    ```text
    No Error
    No Error
    no thread mode
    testNumThr = 1, testThrIdx = 0
    ```

- 第二种：HMPP库开启多线程模式。

    ```text
    No Error
    No Error
    thread mode is omp
    testNumThr = 4, testThrIdx = 1
    testNumThr = 4, testThrIdx = 3
    testNumThr = 4, testThrIdx = 2
    testNumThr = 4, testThrIdx = 0
    ```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>
>- 目前HMPP仅发布二进制包，且未开启OMP编译选项，因此暂不支持多线程的功能。
>- 若支持多线程功能，几个线程之间的打印顺序可能与上述示例回显结果不同。

### GetCacheInfo

- **获取L2 Cache大小：**

    HmppResult HMPP\_GetL2CacheSize \(int32\_t \*size\);

- **获取L2 Cache和L3 Cache中的较大值：**

    HmppResult HMPP\_GetMaxCacheSizeB \(int32\_t \*size\);

- **获取各级缓存的参数，如类型、等级、大小等信息：**

    HmppResult HMPP\_GetCacheParams \(HmppCache \*\*cacheInfo\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|size|目标地址，指向地址存放缓存的大小。|非空|输出|
|cacheInfo|目标地址，指向地址存放指向HmppCache数组的指针值。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|size、cacheInfo这些指针入参中存在空指针。|
|HMPP_STS_NOT_SUPPORT|获取缓存信息失败。|

**示例**

```c
void  GetCacheInfo_Example()
{
    int32_t l2CacheSize = 0;
    HmppResult result = HMPP_GetL2CacheSize(&l2CacheSize);
    printf("%s\n", HMPP_GetStatusString(result));
    printf("l2CacheSize = %d Byte\n\n", l2CacheSize);

    int32_t maxCacheSize = 0;
    result = HMPP_GetMaxCacheSizeB(&maxCacheSize);
    printf("%s\n", HMPP_GetStatusString(result));
    printf("maxCacheSize = %d Byte\n\n", maxCacheSize);

    HmppCache *pCacheSize;
    result = HMPP_GetCacheParams(&pCacheSize);
    printf("%s\n", HMPP_GetStatusString(result));
    int32_t i = 0;
    while (pCacheSize[i].type) {
        printf("type = %d\n", pCacheSize[i].type);
        printf("type = %d\n", pCacheSize[i].level);
        printf("type = %d Byte\n\n", pCacheSize[i].size);
        ++i;
    }
}
```

运行结果：

```text
No Error
l2CacheSize = 524288 Byte

No Error
maxCacheSize = 33554432 Byte

No Error
type = 1
type = 1
type = 65536 Byte

type = 2
type = 1
type = 65536 Byte

type = 3
type = 2
type = 524288 Byte

type = 3
type = 3
type = 33554432 Byte
```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>实际输出可能与上述结果不同，可调用lscpu对比输出值。

### GetCpuFreq

**获取CPU频率：**

HmppResult HMPP\_GetCpuFreqMhz \(int32\_t \*mhz\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|mhz|目标地址，指向地址存放CPU频率值。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|mhz指针为空指针。|
|HMPP_STS_NOT_SUPPORT|获取信息失败。|

**示例**

```c
void  GetCpuFreq_Example()
{
    int32_t mhz;
    HmppResult result = HMPP_GetCpuFreqMhz(&mhz);
    printf("%s\n", HMPP_GetStatusString(result));
    printf("cpu frequency = %d Mhz\n", mhz);
}
```

运行结果：

```text
No Error
cpu frequency = 2600 Mhz
```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>
>- 此接口需要在root用户下，才能有正确返回值。
>- 实际输出可能与上述结果不同，可调用**dmidecode -t processor | grep "Current"**对比输出值。

### GetCpuClock

**获取时间戳，即机器复位后经过了多少个时钟周期：**

uint64\_t HMPP\_GetCpuClock \(\);

**返回值**

返回当前时间戳。

**错误码**

无

**示例**

```c
void  GetCpuClock()
{
    printf("clock = %llu\n", HMPP_GetCpuClock());
}
```

运行结果：

```text
clock = 259338746414838
```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>clock具体数值会变化。

### GetLibVersion

**获取HMPP当前版本信息：**

const HmppLibraryVersion\* HMPP\_GetLibVersion \(\);

**返回值**

返回指向存有版本信息的HmppLibraryVersion类型变量的首地址。

**错误码**

无

>![](../public_sys-resources/icon-note.gif) **说明：** 
>
>1. 不要释放指针指向的内存。
>2. 此API后续不再进行维护，不建议使用，如需使用相关功能，建议使用GetProductVersion接口来替代使用此接口。

**示例**

```c
void  HMPP_GetLibVersion_Example()
{
    const HmppLibraryVersion *libVersion = HMPP_GetLibVersion();
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

### GetProductVersion

获取安装版本HMPP产品信息，包含软件名称、软件版本、产品版本、产品构建时间四个方面信息。

函数接口声明如下：

HmppResult HMPP\_GetProductVersion\(HmppProVersion \*packageInfo\);

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|packageInfo|目标结构体，存放输出的HMPP产品信息。|非空|输出|

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|HmppProVersion指针为空指针。|

**示例**

```c
void  HMPP_GetProductVersion_Example()
{
    HmppProVersion versionGet;
    HmppResult result = HMPP_GetProductVersion(&versionGet);
    HmppProVersion *version = &versionGet;
    if (result == HMPP_STS_NO_ERR) {
        printf("Product Name: %s\n", version->productName);
        printf("Product Version: %s\n", version->productVersion);
        printf("Component Name: %s\n", version->componentName);
        printf("Component Version: %s\n", version->componentVersion);
        printf("Component AppendInfo: %s\n", version->componentAppendInfo);
        printf("Software Name: %s\n", version->softwareName);
        printf("Software Version: %s\n", version->softwareVersion);
    }
}
```

运行结果：

```text
Product Name: Kunpeng BoostKit
Product Version: 26.0.0
Component Name: BoostMedia-HMPP
Component Version: 2.6.2.beta1
Component AppendInfo: gcc
Software Name: boostmedia-hmpp
Software Version: 2.6.2.beta1
```

>![](../public_sys-resources/icon-note.gif) **说明：** 
>以上版本号和编译时间以实际运行结果为准，上述结果仅供参考。

### CpuFeature

- **设置HMPP支持的指令集：**

    HmppResult HMPP\_SetCpuFeatures \(uint64\_t cpuFeatures\);

- **获取CPU支持的指令集：**

    HmppResult HMPP\_GetCpuFeatures \(uint64\_t\* cpuFeatures\);

- **获取HMPP当前支持的指令集：**

    uint64\_t HMPP\_GetEnabledCpuFeatures\(\)

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|cpuFeatures|要设置的HMPP库支持的指令集（SetCpuFeatures）。|hmpp_core.h头文件中提供的几种后缀为_FM的宏。|输入|
|cpuFeatures|目标地址，指向地址存放CPU支持指令集标记数（GetCpuFeatures）。|非空。|输出|

**返回值**

HMPP\_SetCpuFeatures

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码HMPP\_STS\_UNKNOWN\_FEATURE。

HMPP\_GetCpuFeatures

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码HMPP\_STS\_NULL\_PTR\_ERR。

HMPP\_GetEnabledCpuFeatures

- 返回当前HMPP库支持指令集标记数。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|cpuFeatures指针为空指针。|
|HMPP_STS_UNKNOWN_FEATURE|要设置的指令集不在支持的几种指令集中。|

**注意**

目前只支持NEON\_FM（在hmppcore.h中定义）一种模式。

**示例**

```c
void  CpuFeature()
{
    uint64_t cpuFeatures;
    HmppResult result = HMPP_GetCpuFeatures(&cpuFeatures);
    printf("%s\n", HMPP_GetStatusString(result));
    printf("cpuFeatures = %016x\n", cpuFeatures);

    result = HMPP_SetCpuFeatures(NEON_FM);
    printf("%s\n", HMPP_GetStatusString(result));

    printf("enabledCpuFeatures = %016x\n", HMPP_GetEnabledCpuFeatures());
}
```

运行结果：

```text
No Error
cpuFeatures = 0000000000000001
No Error
enabledCpuFeatures = 0000000000000001
```

### SetDenormAreZeros

**启用或禁用刷零模式：**

HmppResult HMPP\_SetDenormAreZeros \(int32\_t value\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|value|0表示禁用非0表示启用|int32_t值范围|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NOT_SUPPORT|当前系统不是aarch64架构，不支持此函数。|

**示例**

```c
#define LEN 4
void  SetDenormAreZeros_Example()
{
    HmppResult result = HMPP_SetDenormAreZeros(1);
    cout << HMPP_GetStatusString(result) << endl;
    const float a[LEN] = {0.99 * pow(2, -126), 1.0 * pow(2, -126),
                          1.5 * pow(2, -126), 1.5 * pow(2, -126)};
    const float b[LEN] = {0.99 * pow(2, -126), 0.99 * pow(2, -126),
                          1.4 * pow(2, -126), -1.4 * pow(2, -126)};

    for (int32_t i = 0; i < LEN; ++i) {
        cout << a[i] + b[i] << endl;
    }

    result = HMPP_SetDenormAreZeros(0);
    cout << HMPP_GetStatusString(result) << endl;

    for (int32_t i = 0; i < LEN; ++i) {
        cout << a[i] + b[i] << endl;
    }
}
```

运行结果：

```text
No Error
0
1.17549e-38
3.40893e-38
0
No Error
2.32748e-38
2.33923e-38
3.40893e-38
1.17549e-39
```

### Init

**初始化函数：**

HmppResult HMPP\_Init\(\);

>![](../public_sys-resources/icon-note.gif) **说明：** 
>在此函数内会调用HMPP\_GetL2CacheSize、HMPP\_GetMaxCacheSizeB、HMPP\_GetCacheParams。

**返回值**

返回HMPP\_STS\_NO\_ERR。

**错误码**

无

### ParallelFor

**并行化函数：**

HmppResult HMPP\_ParallelFor \(int32\_t numTasks, void \*arg, function func\);

>![](../public_sys-resources/icon-note.gif) **说明：** 
>可将要执行的多个任务封装在HmppResult\(\*function\)\(int32\_t i, void \*arg\)内传入，使用方式见示例。

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|numTasks|任务数。|大于0（小于0不会报异常，可以认为就是一个任务都不创建）。|输入|
|arg|地址，指向func函数的参数值。|非空|输入|
|func|要执行函数（即任务）的函数指针。|非空|输入|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|arg或func指针为空指针。|

>![](../public_sys-resources/icon-note.gif) **说明：** 
>开启的多线程中也会返回错误码，该错误码在自定义函数中设定，由调用者决定。

**示例**

```c
#define LEN 20
 
void  ParallelFor_Example()
{
    typedef struct {
        int32_t val[LEN];
        int32_t rangeLen;
    }Arg;
    // 此函数实现的功能：对数组某段区间的数取绝对值
    auto func = [] (int32_t i, void *arg) -> HmppResult {
        Arg *args = (Arg *)arg;
        int32_t st = i * args->rangeLen;
        int32_t ed = st + args->rangeLen;
        for (int32_t k = st; k < ed; ++k) {
            args->val[k] = abs(args->val[k]);
        }
        return HMPP_STS_NO_ERR;
    };

    Arg arg;
    arg.rangeLen = 5;
    for (int32_t i = 0; i < LEN; ++i) {
        arg.val[i] = -(i + 1);
    }

    HmppResult result = HMPP_ParallelFor(LEN / arg.rangeLen, (void*)&arg, func);

    for (int32_t i = 0; i < LEN; ++i) {
        printf("%d\n", arg.val[i]);
    }
}
```

运行结果：

```text
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
```
