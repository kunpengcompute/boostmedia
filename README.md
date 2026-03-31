# BoostMedia鲲鹏媒体套件

鲲鹏BoostKit媒体套件旨在为视频编解码、图像处理场景提供基于鲲鹏平台的应用层加速能力。

## 架构图

![arch.png](https://raw.gitcode.com/user-images/assets/9361804/8a6da15b-7a38-4e14-9bd9-82dfdf4f1d0d/arch.png 'arch.png')

## 特性介绍

### 媒体基础算子

|  特性| 特性介绍  |
|--|--|
| kvcl（待开源） | 采用向量化指令对视频编码通用基础算子进行优化，结合硬件特性充分利用向量化指令加速，提升算子性能 |

### 视频编码器

|  特性| 特性介绍  |
|--|--|
| HW265Enc  (待开源) | HW265Enc编码器是遵循H.265/HEVC（High Efficiency Video Coding）视频编解码标准开发的自研视频编码器，基于鲲鹏920新型号处理器实现编码库亲和优化，支持对YUV像素文件进行编码生成H.265/HEVC视频码流文件，支持8bit色彩深度、420p格式 |
| x265 (待开源) | 采用SVE向量化指令对视频编码算子进行深度优化，充分利用鲲鹏平台向量化指令加速，提升视频编码效率 |
| VVenC (待开源) | 针对开源VVenC编码库中的转码底层算子使用鲲鹏向量指令进行加速优化，提高整体性能 |

### 图像处理

|  特性| 特性介绍  |
|--|--|
| [opencv](https://gitcode.com/boostkit/opencv) | 通过算法优化、向量指令优化和并行优化等手段优化Opencv算子，显著提升OpenCV在鲲鹏服务器上的性能表现 |
| [libavif](https://gitcode.com/boostkit/libavif)  | 采用向量优化、算法优化、流程优化等方法，优化开源图像库libavif的图片编码性能 |
| [libwebp](https://gitcode.com/boostkit/libwebp)  | 使用向量优化，算法优化，流程优化等方法，优化开源图像库libwebp图片编码性能 |
| HMPP (待开源)  | HMPP（Hyper Media Performance Primitives，鲲鹏超媒体性能库）是在鲲鹏处理器硬件平台基础上开发的加速库，涉及图像处理、颜色转换、滤波、变换、几何，为计算机视觉运算、向量运算、统计、信号滤波、信号变换和固定精度运算等，通过向量指令集对业务功能提供高性能加速函数接口 |
| vision (待开源)  | 使用向量优化，算法优化，流程优化等方法，优化开源视觉库vision中图片处理性能 |

### 媒体框架

|  特性| 特性介绍  |
|--|--|
| [ffmpeg](https://gitcode.com/boostkit/ffmpeg)  | 通过向量优化、算法优化和并行优化等方法，提升开源媒体框架ffmpeg中的色彩空间转换、视频分辨率缩放算法性能 |

## 文档

## 讨论

如果发现问题，请进入[讨论](https://gitcode.com/boostkit/boostmedia/discussions)与我们联系。

## 许可协议

使用本领域源码及其附带软件，即视为您已阅读、理解并同意相关软件许可协议条款与条件的约束。
