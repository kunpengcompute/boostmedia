# HMPP版本说明书

本文提供HMPP的版本信息和特性更新情况。

## 版本配套说明

### 产品版本信息

<table><tbody><tr id="row41561572"><th class="firstcol" valign="top" width="42.17%" id="mcps1.1.3.1.1"><p id="p11044137"><a name="p11044137"></a><a name="p11044137"></a>产品名称</p>
</th>
<td class="cellrowborder" valign="top" width="57.830000000000005%" headers="mcps1.1.3.1.1 "><p id="p48427257"><a name="p48427257"></a><a name="p48427257"></a>Kunpeng BoostKit</p>
</td>
</tr>
<tr id="row24726251"><th class="firstcol" valign="top" width="42.17%" id="mcps1.1.3.2.1"><p id="p56669300"><a name="p56669300"></a><a name="p56669300"></a>产品版本</p>
</th>
<td class="cellrowborder" valign="top" width="57.830000000000005%" headers="mcps1.1.3.2.1 "><p id="p16166112734513"><a name="p16166112734513"></a><a name="p16166112734513"></a><span id="text11668124852817"><a name="text11668124852817"></a><a name="text11668124852817"></a>26.0.0</span></p>
</td>
</tr>
<tr id="row5497143514612"><th class="firstcol" valign="top" width="42.17%" id="mcps1.1.3.3.1"><p id="p162251517551"><a name="p162251517551"></a><a name="p162251517551"></a>软件名称</p>
</th>
<td class="cellrowborder" valign="top" width="57.830000000000005%" headers="mcps1.1.3.3.1 "><p id="p51757141375"><a name="p51757141375"></a><a name="p51757141375"></a>鲲鹏超媒体性能库HMPP（Hyper Media Performance Primitives）</p>
</td>
</tr>
<tr id="row14686327258"><th class="firstcol" valign="top" width="42.17%" id="mcps1.1.3.4.1"><p id="p15758185814610"><a name="p15758185814610"></a><a name="p15758185814610"></a>软件包版本</p>
</th>
<td class="cellrowborder" valign="top" width="57.830000000000005%" headers="mcps1.1.3.4.1 "><p id="p875815585616"><a name="p875815585616"></a><a name="p875815585616"></a>2.6.1.beta1</p>
</td>
</tr>
</tbody>
</table>

## v2.6.1.beta1

### 更新说明

| 函数库 | HMPPS（信号库） | HMPPI（图像库）|
| ---- | ---- | ---- |
|  新增特性 | <li>HMPPS_Exp_32fc_A24</li> <li>HMPPS_ConjPack_32fc_I</li>   | <li>HMPPI_Transpose_16s_C1R</li> <li>HMPPI_Transpose_32s_C1R</li>  <li>HMPPI_Transpose_32f_C1R</li> <li>HMPPI_Set_8u_C1R</li> <li>HMPPI_Set_32f_C1R</li> <li>HMPPI_Not_8u_C1IR</li> <li>HMPPI_Or_8u_C1IR</li> |

## v2.6.0.beta1

### 更新说明

| 函数库 | HMPPS（信号库） | HMPPI（图像库）|
| ---- | ---- | ---- |
|  新增特性 | <li>HMPPS_Asin_32f_A24</li> <li>HMPPS_Sin_64f_A50</li> <li>HMPPS_Tan_64f_A50</li>  | <li>HMPPI_Conv_8u_C1R</li> <li>HMPPI_Conv_32f_C1R</li>  <li>HMPPI_FilterMinBorder_8u_C1R</li> <li>HMPPI_Or_8u_C1R</li> <li>HMPPI_ResizeLinearInit_16s</li> <li>HMPPI_ResizeLinear_16s_C1R</li> <li>HMPPI_ResizeNearestInit_8u</li> <li>HMPPI_ResizeNearest_8u_C1R</li> <li>HMPPI_WarpAffineNearest_8u_C1R</li>|

## v2.5.2

### 更新说明

| 函数库 | HMPPS（信号库） | HMPPI（图像库）|
| ---- | ---- | ---- |
|  新增特性 | <li>HMPPS_Arg_32fc</li> <li>HMPPS_Inv_32f</li> <li>HMPPS_Threshold_LTVal_32s</li><li>HMPPS_Threshold_GTVal_32s</li> |   <li>HMPPI_Add_32f_C1R</li><li>HMPPI_Add_32f_C3R</li><li>HMPPI_CompareC_8u_C1R</li><li>HMPPI_CopyConstBorder_32f_C1R</li><li>HMPPI_CopyConstBorder_8u_C1R</li><li>HMPPI_CopyReplicateBorder_32f_C1R</li><li>HMPPI_CopyReplicateBorder_16s_C1R</li><li>HMPPI_CopyReplicateBorder_8u_C1R</li><li>HMPPI_FloodFill_4Con_8u_C1IR</li><li>HMPPI_FloodFill_8Con_32f_C1IR</li><li>HMPPI_FloodFill_8Con_8u_C1IR</li><li>HMPPI_Sub_32f_C1R</li><li>HMPPI_Sub_32f_C3R</li><li>HMPPI_DistanceTransform_3x3_8u32f_C1R</li><li>HMPPI_LabelMarkers_8u_C1IR</li><li>HMPPI_LabelMarkers_16u_C1IR</li><li>HMPPI_LabelMarkers_16u8u_C1R</li><li>HMPPI_MinEvery_32f_C1R</li><li>HMPPI_Mean_StdDev_32f_C1R</li><li>HMPPI_TrueDistanceTransform_8u32f_C1R</li><li>HMPPI_FFTCToCInit_32fc</li><li>HMPPI_FFTCToC_32fc_C1R</li><li>HMPPI_FFTCToCRelease_32fc</li><li>HMPPI_RResize_8u_C1V</li><li>HMPPI_RResize_16u_C1V</li><li>HMPPI_RResize_32f_C1V</li><li>HMPPI_ResizeNearestInit_32f</li><li>HMPPI_ResizeNearest_32f_C1R</li><li>HMPPI_ResizeNearestRelease_32f</li><li>HMPPI_ResizeCubicInit_32f</li><li>HMPPI_ResizeCubic_32f_C1R</li><li>HMPPI_ResizeCubicRelease_32f</li><li>HMPPI_ResizeLinearInit_32f</li><li>HMPPI_ResizeLinear_32f_C1R</li><li>HMPPI_ResizeLinearRelease_32f</li><li>HMPPI_WarpAffineNearestInit</li><li>HMPPI_WarpAffineNearest_64f_C1R</li><li>HMPPI_WarpAffineNearestRelease_32f</li><li>HMPPI_Set_64f_C1R</li><li>HMPPI_CreateKernel3D_32f</li><li>HMPPI_FilterBorderGetSize</li><li>HMPPI_FilterBorderInit_32f</li><li>HMPPI_FilterBorder_32f_C1V</li><li>HMPPI_FilterBorderRelease_32f</li><li>HMPPI_FilterMedianBorder_16s_C1R</li><li>HMPPI_FilterSobelHorizBorder_32f_C1R</li><li>HMPPI_FilterSobelNegVertBorder_32f_C1R</li>  |

## v2.5.1

### 更新说明

| 函数库 | HMPPA（音频库）|
| ---- | ---- |
|  新增特性 | <li>HMPPA_Evs_GetEncodeDstBufLen_16s8u</li><li>HMPPA_Evs_EncodeInit_16s8u</li><li>HMPPA_Evs_Encode_16s8u</li><li>HMPPA_Evs_EncodeRelease_16s8u</li> |

## v2.5.0

### 更新说明

| 函数库 | HMPPA（音频库）|
| ---- | ---- |
|  新增特性 | <li>HMPPA_Evs_GetDecodeDstBufLen_8u16s</li><li>HMPPA_Evs_DecodeInit_8u16s</li><li>HMPPA_Evs_Decode_8u16s</li><li>HMPPA_Evs_DecodeRelease_8u16s</li> |

## v2.4.0

### 更新说明

| 函数库 | HMPPS（信号库）|
| ---- | ---- |
|  新增特性 | <li>14个小波变换库接口</li> <li>IIR滤波类函数</li> |

## v2.3.0

### 更新说明

- 增加HMPP函数库编译方法说明。
- 修改FFT线程数设置接口。

## v2.2.1

### 更新说明

| 函数库 | HMPPA（音频库）|
| ---- | ---- |
|  新增特性 | 基础功能FFT线程数设置接口 |

## v2.2.0

### 更新说明

| 函数库 | HMPPA（音频库）|
| ---- | ---- |
|  新增特性 | 4个gsmhr编码接口|

## v2.0.0

### 更新说明

| 函数库 | HMPPS（信号库）|
| ---- | ---- |
|  新增特性 |22个聚合接口|

## v1.7.0

### 更新说明

| 函数库 | HMPPS（信号库）| HMPPI（图像库） |
| ---- | ---- | ---- |
|  新增特性 | 20个hash接口和聚合接口 | 100个图像处理接口 |

## v1.6.0

### 更新说明

| 函数库 | HMPPA（音频库）| HMPPI（图像库） |
| ---- | ---- | ---- |
|  新增特性 | 3类音频协议编码接口 | 121个图像处理接口 |

## v1.5.0

### 更新说明

| 函数库 | HMPPA（音频库）| HMPPI（图像库） |
| ---- | ---- | ---- |
|  新增特性 | 3类音频协议编码接口 | 100个图像处理接口 |

## v1.4.0

### 更新说明

| 函数库 | HMPPA（音频库）| HMPPI（图像库） |
| ---- | ---- | ---- |
|  新增特性 | 优化3类音频协议解码接口性能 | 100个图像处理接口 |

## v1.1.0

### 更新说明

| 函数库 | HMPPS（信号库）| HMPPI（图像库） |
| ---- | ---- | ---- |
|  新增特性 | 102个信号处理接口 | 2个图像处理接口 |
