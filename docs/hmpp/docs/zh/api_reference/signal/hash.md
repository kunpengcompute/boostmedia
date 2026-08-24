# 聚合与哈希

[返回模块目录](index.md) | [返回HMPP API参考](../../api_reference.md)

## 函数说明

该模块包含聚合类和哈希类的函数。涉及到的数据类型包含HMPP基本的数据类型，以及两种新定义的数据类型：

- varchar

    ```c
    typedef uint8_t varchar;
    ```

    即字符类型，varchar\*表示字符串。

    也可用varchar\*表示字符串数组，这种字符串数组数据类型是紧凑型存储的数据类型，使用时会配套使用offset\*向量表示每个子字符串的起始地址。例如：varchar\*指向"wearegoodfriend"字符串，offset\[4\]=\{0, 2, 5, 9\};，因此实际想要表示的字符串数组是\{"we", "are", "good", "friend"\}。

- HmppDecimal128

    ```c
    typedef struct {
        uint64_t low;
        int64_t high;
    } HmppDecimal128;
    ```

    即128位整型数，用两个64位整型数分别表示其低位和高位。high的最高位是符号位，其余127位构成绝对值。

## CombineHash

函数接口声明如下：

HmppResult HMPPS\_CombineHash\(const int64\_t \*src1, const int64\_t \*src2, int32\_t len, int64\_t \*dst\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src1|指向源向量的指针。|非空|输入|
|src2|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|dst|指向结果的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src1、src2、dst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main() {
    int64_t src1[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int64_t dst[BUFFER_SIZE_T] = {0};
    int64_t src2[BUFFER_SIZE_T] = {33, 612, 224, 80, 3234, 150, 562, 311, 11, 232};
    HmppResult result = HMPPS_CombineHash(src1, src2, BUFFER_SIZE_T, dst);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
 printf("%d, ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
126, 798, 286, 328, 3327, 615, 2298, 1272, 42, 945,
```

## Hash

函数接口声明如下：

HmppResult HMPPS\_Hash\_16s\(const int16\_t \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_32s\(const int32\_t \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_64s\(const int64\_t \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_64f\(const double \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_bool\(const bool \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_decimal64\(const int64\_t \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_decimal128\(const HmppDecimal128 \*src, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

HmppResult HMPPS\_Hash\_varchar\(const varchar \*src, const int32\_t \*offset, int32\_t len, const int8\_t \*nullAddr, int64\_t \*dst\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|offset|指向子字符串偏移地址的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|nullAddr|指向空地址的指针。若nullAddr为空指针，表示src向量中所有元素都参与计算。否则，当nullAddr[i]=0时，src[i]参与计算（i表示索引）。|无要求，可以为空|输入|
|dst|指向结果的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、offset、dst这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main() {
    int64_t src[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int64_t dst[BUFFER_SIZE_T] = {0};
 int8_t nullAddr[BUFFER_SIZE_T] = {0, 0, 1, 1, 0, 1, 0, 0, 0, 0};
    HmppResult result = HMPPS_Hash_64s(src, BUFFER_SIZE_T, nullAddr, dst);
    printf("result = %d\n", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    for (int i = 0; i < BUFFER_SIZE_T; i++) {
 printf("%d, ", dst[i]);
    }
    printf("\n");
}
```

运行结果：

```text
result = 0
806985981, -434172799, 0, 0, 806985981, 0, 2071699152, -304648091, -479945518, -1157406791,
```

## Max

函数接口声明如下：

HmppResult HMPPS\_AggMax\_bool\(const bool \*src, int32\_t len, int8\_t \*nullAddr, bool \*max\);

HmppResult HMPPS\_AggMax\_varchar\(const varchar \*src, const int32\_t \*offset, int32\_t len, varchar \*max, int32\_t \*maxLen\);

HmppResult HMPPS\_AggMax\_16s\(const int16\_t \*src, int32\_t len, int8\_t \*nullAddr, int16\_t \*max\);

HmppResult HMPPS\_AggMax\_32s\(const int32\_t \*src, int32\_t len, int8\_t \*nullAddr, int32\_t \*max\);

HmppResult HMPPS\_AggMax\_64s\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, int64\_t \*max\);

HmppResult HMPPS\_AggMax\_64f\(const double \*src, int32\_t len, int8\_t \*nullAddr, double \*max\);

HmppResult HMPPS\_AggMax\_decimal128\(const HmppDecimal128 \*src, int32\_t len, int8\_t \*nullAddr, HmppDecimal128 \*max\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|nullAddr|指向空地址的指针。若nullAddr为空指针，表示src向量中所有元素都参与计算。否则，当nullAddr[i]=0时，src[i]参与计算（i表示索引）。|无要求，可以为空|输入|
|offset|指向子字符串偏移地址的指针。|非空|输入|
|max|指向结果的指针。|非空|输出|
|maxlen|指向结果字符串长度的指针。|非空|输出|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>使用HMPPS\_AggMax\_varchar函数时，用户需要给max分配足够大的内存，否则可能导致段错误。建议分配与src同样大的内存大小。

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、max、maxlen、offset这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main() {
    int64_t src[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int8_t nullAddr[BUFFER_SIZE_T] = {0, 1, 0, 0, 0, 0, 1, 0, 0, 0};
    int64_t max;
    HmppResult result = HMPPS_AggMax_64s(src, BUFFER_SIZE_T, nullAddr, &max);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    printf("max = %ld\n", max);

```

运行结果：

```text
result = 0  max = 31
```

## Mean

函数接口声明如下：

HmppResult HMPPS\_AggMean\_16s\(const int16\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, double \*sum, int64\_t \*count\);

HmppResult HMPPS\_AggMean\_32s\(const int32\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, double \*sum, int64\_t \*count\);

HmppResult HMPPS\_AggMean\_64s\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, double \*sum, int64\_t \*count\);

HmppResult HMPPS\_AggMean\_64f\(const double \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, double \*sum, int64\_t \*count\);

HmppResult HMPPS\_AggMean\_decimal64\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, HmppDecimal128 \*sum, int64\_t \*count\);

HmppResult HMPPS\_AggMean\_decimal128\(const HmppDecimal128 \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, HmppDecimal128 \*sum, int64\_t \*count\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|nullAddr|指向空地址的指针。若nullAddr为空指针，表示src向量中所有元素都参与计算。否则，当nullAddr[i]=0时，src[i]参与计算（i表示索引）。|无要求，可以为空|输入|
|overflow|指向溢出标志位的指针。|非空|输出|
|sum|指向求和结果的指针。|非空|输出|
|count|指向个数结果的指针。统计有效计算元素个数，最终的平均值为sum/count。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、nullAddr、sum、count、overflow这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include <stdint.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main()
{
    int64_t src[BUFFER_SIZE_T] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int8_t nullAddr[BUFFER_SIZE_T] = {0, 0, 1, 1, 0, 1, 0, 0, 0, 0};
    double sum;
    int64_t count;
    bool overflow;
    HmppResult result = HMPPS_AggMean_64s(src, BUFFER_SIZE_T, nullAddr, &overflow, &sum, &count);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    printf("sum = %lf, count = %d, overflow = %d\n", sum, count, overflow);
}

```

运行结果：

```text
result = 0  sum = 42.000000, count = 7, overflow = 0
```

## Min

函数接口声明如下：

HmppResult HMPPS\_AggMin\_bool\(const bool \*src, int32\_t len, int8\_t \*nullAddr, bool \*min\);

HmppResult HMPPS\_AggMin\_varchar\(const varchar \*src, const int32\_t \*offset, int32\_t len, varchar \*min, int32\_t \*minLen\);

HmppResult HMPPS\_AggMin\_16s\(const int16\_t \*src, int32\_t len, int8\_t \*nullAddr, int16\_t \*min\);

HmppResult HMPPS\_AggMin\_32s\(const int32\_t \*src, int32\_t len, int8\_t \*nullAddr, int32\_t \*min\);

HmppResult HMPPS\_AggMin\_64s\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, int64\_t \*min\);

HmppResult HMPPS\_AggMin\_64f\(const double \*src, int32\_t len, int8\_t \*nullAddr, double \*min\);

HmppResult HMPPS\_AggMin\_decimal128\(const HmppDecimal128 \*src, int32\_t len, int8\_t \*nullAddr, HmppDecimal128 \*min\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|nullAddr|指向空地址的指针。若nullAddr为空指针，表示src向量中所有元素都参与计算。否则，当nullAddr[i]=0时，src[i]参与计算（i表示索引）。|无要求，可以为空|输入|
|offset|指向子字符串偏移地址的指针。|非空|输入|
|min|指向结果的指针。|非空|输出|
|minlen|指向结果字符串长度的指针。|非空|输出|

>![](../../public_sys-resources/icon-note.gif) **说明：** 
>使用HMPPS\_AggMin\_varchar函数时，用户需要给min分配足够大的内存，否则可能导致段错误。建议分配与src同样大的内存大小。

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、min、minlen、offset这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main() {
    int64_t src[BUFFER_SIZE_T] = {3, 6, 2, 8, 3, 15, 56, 31, 1, 23};
    int8_t nullAddr[BUFFER_SIZE_T] = {0, 0, 0, 0, 0, 0, 0, 0, 1, 23};
    int64_t min;
    HmppResult result = HMPPS_AggMin_64s(src, BUFFER_SIZE_T, nullAddr, &min);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    printf("min = %ld\n", min);
}
```

运行结果：

```text
result = 0  min = 2
```

## Sum

函数接口声明如下：

HmppResult HMPPS\_AggSum\_16s\(const int16\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, int64\_t \*sum\);

HmppResult HMPPS\_AggSum\_32s\(const int32\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, int64\_t \*sum\);

HmppResult HMPPS\_AggSum\_64s\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, int64\_t \*sum\);

HmppResult HMPPS\_AggSum\_64f\(const double \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, double \*sum\);

HmppResult HMPPS\_AggSum\_decimal64\(const int64\_t \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, HmppDecimal128 \*sum\);

HmppResult HMPPS\_AggSum\_decimal128\(const HmppDecimal128 \*src, int32\_t len, int8\_t \*nullAddr, bool \*overflow, HmppDecimal128 \*sum\);

**参数**

|参数名|描述|取值范围|输入/输出|
|--|--|--|--|
|src|指向源向量的指针。|非空|输入|
|len|向量长度。|(0,INT_MAX]|输入|
|nullAddr|指向空地址的指针。若nullAddr为空指针，表示src向量中所有元素都参与计算。否则，当nullAddr[i]=0时，src[i]参与计算（i表示索引）。|无要求，可以为空|输入|
|overflow|指向溢出标志位的指针。|非空|输出|
|sum|指向求和结果的指针。|非空|输出|

**返回值**

- 成功：返回HMPP\_STS\_NO\_ERR。
- 失败：返回错误码。

**错误码**

|错误码|描述|
|--|--|
|HMPP_STS_NULL_PTR_ERR|src、sum、overflow这几个入参中存在空指针。|
|HMPP_STS_SIZE_ERR|len小于或等于0。|

**示例**

```c
#include <stdio.h>
#include "hmpp.h"
#define BUFFER_SIZE_T 10

int main()
{
    int64_t src[BUFFER_SIZE_T] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int8_t nullAddr[BUFFER_SIZE_T] = {0, 0, 1, 1, 0, 1, 0, 0, 0, 0};
    int64_t sum;
    bool overflow;
    HmppResult result = HMPPS_AggSum_64s(src, BUFFER_SIZE_T, nullAddr, &overflow, &sum);
    printf("result = %d  ", result);
    if (result != HMPP_STS_NO_ERR) {
        return 0;
    }
    printf("sum = %ld, overflow = %d\n", sum, overflow);
}
```

运行结果：

```text
result = 0  sum = 42, overflow = 0
```
