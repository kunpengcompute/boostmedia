# KVCL：Kunpeng Video Codec Library

## 最新消息

- [2026.03.30]: 发布KVCL1.0.0版本。新增KvclDct16x16、KvclDct32x32、KvclIDct16x16、KvclLumaVPP32x32、KvclLumaVPP64x64、KvclLumaHPP32x32、KvclLumaHPP64x64、KvclChromaHPP32x32、KvclChromaHPP16x16、KvclScanPosLast、KvclQuantRdoq、KvclSad64x32、KvclSad64x64、KvclSad4x32x32、KvclSad4x32x64、KvclSad4x64x16、KvclSad4x64x32、KvclSad4x64x64、KvclSaoCuStatsE0、KvclSaoCuStatsE1、KvclSaoCuStatsE2、KvclSaoCuStatsE3、KvclSatd8x8、KvclSatd16x16、KvclSa8d8x8、KvclSa8d16x16算子。

## 简介

在不同的视频处理框架（如FFmpeg、Gstreamer等）和编码器中，存在大量功能相同的算子，如SATD/SAD、DCT、SAO。

KVCL（Kunpeng Video Codec Library）包含了视频编码中的TOP通用算子，且KVCL针对鲲鹏硬件对算子进行了优化。


## 版本说明

| KVCL版本   | 特性变更    |
| ------------ | ------------ |
|   1.0.0    |   新增KvclDct16x16、KvclDct32x32、KvclIDct16x16、KvclLumaVPP32x32、KvclLumaVPP64x64、KvclLumaHPP32x32、KvclLumaHPP64x64、KvclChromaHPP32x32、KvclChromaHPP16x16、KvclScanPosLast、KvclQuantRdoq、KvclSad64x32、KvclSad64x64、KvclSad4x32x32、KvclSad4x32x64、KvclSad4x64x16、KvclSad4x64x32、KvclSad4x64x64、KvclSaoCuStatsE0、KvclSaoCuStatsE1、KvclSaoCuStatsE2、KvclSaoCuStatsE3、KvclSatd8x8、KvclSatd16x16、KvclSa8d8x8、KvclSa8d16x16 算子   |

## 环境部署

kvcl的环境部署步骤详见《[环境部署](./docs/zh/install_guide_bin.md)》。

## 快速入门

kvcl的快速入门步骤详见《[快速入门](./docs/zh/quick_start.md)》。

## 功能介绍&特性介绍

### 功能

|  接口 |  接口职责 |
| ------------ | ------------ |
| KvclDct16x16 | 计算16x16像素块的DCT变换  |
| KvclDct32x32 | 计算32x32像素块的DCT变换  |
| KvclIDct16x16 | 计算16x16像素块的逆离散余弦变换（DCT）  |
| KvclLumaVPP32x32 | 计算32x32像素块的垂直8抽头插值  |
| KvclLumaVPP64x64 | 计算64x64像素块的垂直8抽头插值  |
| KvclLumaHPP32x32  | 计算32x32像素块的水平8抽头插值  |
| KvclLumaHPP64x64  | 计算64x64像素块的水平8抽头插值  |
| KvclChromaHPP32x32  | 计算32x32像素块的水平4抽头插值  |
| KvclChromaHPP16x16  | 计算16x16像素块的水平4抽头插值  |
| KvclScanPosLast  | 按照扫描顺序表提供的顺序扫描系数，直到找到最后一个非零系数为止  |
| KvclQuantRdoq  |  rdoq阶段的量化 |
| KvclSad64x32  |  计算64x32像素块的绝对差值和（SAD） |
|KvclSad64x64 |计算64x64像素块的绝对差值和（SAD） |
|KvclSad4x32x32 |计算4x32x32像素块的绝对差值和（SAD） |
|KvclSad4x32x64 |计算4x32x64像素块的绝对差值和（SAD） |
|KvclSad4x64x16 | 计算4x64x16像素块的绝对差值和（SAD）|
|KvclSad4x64x32 | 计算4x64x32像素块的绝对差值和（SAD）|
| KvclSad4x64x64| 计算4x64x64像素块的绝对差值和（SAD）|
|KvclSaoCuStatsE0 |计算SAO相关统计信息：SAO边缘偏移类型0 ，水平方向（左右邻域） |
|KvclSaoCuStatsE1 | 计算SAO相关统计信息：SAO边缘偏移类型1，垂直方向（上下邻域）|
|KvclSaoCuStatsE2 | 计算SAO相关的统计信息：SAO边缘偏移类型2 ，对角线方向（左上到右下45度邻域）|
|KvclSaoCuStatsE3 | 计算SAO相关统计信息：SAO边缘偏移类型3 ，反对角线方向（135度邻域，从右上到左下）|
|KvclSatd8x8 | 计算8x8像素块的SATD（Hadamard变换差值的绝对值之和）|
|KvclSatd16x16 |计算16x16像素块的SATD（Hadamard变换差值的绝对值之和） |
|KvclSa8d8x8 |计算8x8像素块的SA8D（Hadamard变换绝对差值之和） |
| KvclSa8d16x16 |计算16x16像素块的SA8D（Hadamard变换绝对差值之和） |

### 特性

kvcl使用多种手段优化算子，例如：

1. SIMD向量化：kvcl使用NEON、SVE、SVE2等SIMD指令实现算子，实现数据级并行。
2. 指令合并：kvcl识别特定的指令序列，将其替换为功能等效但更高效的指令序列，减少指令数量、降低延迟。
3. 针对硬件特性优化：kvcl针对鲲鹏硬件特性进行指令调度和寄存器分配，有效减少算子开销。
4. 数学表达式优化：通过深入理解计算模式的数学本质，结合硬件特性，实现显著性能提升。
5. 流水线利用率优化：鲲鹏芯片指令执行被划分为多个阶段，流水线利用率优化通过减少流水线停顿和优化数据依赖关系，使硬件流水线始终保持高吞吐状态。

## License

本项目的文档适用CC-BY 4.0许可证，具体请参见[LICENSE](./docs/zh/LICENSE)文件。
