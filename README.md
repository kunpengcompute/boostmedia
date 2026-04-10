# BoostMedia鲲鹏媒体套件

鲲鹏BoostKit媒体套件旨在为视频编解码、图像处理场景提供基于鲲鹏平台的应用层加速能力。

## 架构图

![arch.png](https://raw.gitcode.com/user-images/assets/9361804/8a6da15b-7a38-4e14-9bd9-82dfdf4f1d0d/arch.png 'arch.png')

## 特性介绍

### 媒体基础算子

|  特性| 特性介绍  | 仓库路径  |
|--|--|--|
| kvcl | 采用向量化指令对视频编码通用基础算子进行优化，结合硬件特性充分利用向量化指令加速，提升算子性能 | [kvcl文档](./docs/kvcl/README.md) <br>**仅提供文档仓路径，代码仓待开源。** |

### 视频编码器

|  特性| 特性介绍  | 仓库路径  |
|--|--|--|
| HW265Enc | HW265Enc编码器是遵循H.265/HEVC（High Efficiency Video Coding）视频编解码标准开发的自研视频编码器，基于鲲鹏920新型号处理器实现编码库亲和优化，支持对YUV像素文件进行编码生成H.265/HEVC视频码流文件，支持8bit色彩深度、420p格式 | **代码仓待开源** |
| x265 | 针对开源x265编码库中的转码底层算子使用鲲鹏向量指令进行加速优化，提高整体性能 | [x265文档](./docs/x265/README.md) <br>**仅提供文档仓路径，代码仓待开源。** |
| VVenC | 针对开源VVenC编码库中的转码底层算子使用鲲鹏向量指令进行加速优化，提高整体性能 | **代码仓待开源** |

### 图像处理

|  特性| 特性介绍  | 仓库路径  |
|--|--|--|
| opencv | 通过算法优化、向量指令优化和并行优化等手段优化Opencv算子，显著提升OpenCV在鲲鹏服务器上的性能表现 | [代码仓](https://gitcode.com/boostkit/opencv) |
| libavif  | 采用向量优化、算法优化、流程优化等方法，优化开源图像库libavif的图片编码性能 | [代码仓](https://gitcode.com/boostkit/libavif) |
| libwebp  | 使用向量优化，算法优化，流程优化等方法，优化开源图像库libwebp图片编码性能 | [代码仓](https://gitcode.com/boostkit/libwebp) |
| HMPP | HMPP（Hyper Media Performance Primitives，鲲鹏超媒体性能库）是在鲲鹏处理器硬件平台基础上开发的加速库，涉及图像处理、颜色转换、滤波、变换、几何，为计算机视觉运算、向量运算、统计、信号滤波、信号变换和固定精度运算等，通过向量指令集对业务功能提供高性能加速函数接口 | [HMPP文档](./docs/hmpp/README.md) <br>**仅提供文档仓路径，代码仓待开源。** |
| vision | 使用向量优化，算法优化，流程优化等方法，优化开源视觉库vision中图片处理性能 | **代码仓待开源** |

### 媒体框架

|  特性| 特性介绍  | 仓库路径  |
|--|--|--|
| ffmpeg  | 通过向量优化、算法优化和并行优化等方法，提升开源媒体框架ffmpeg中的色彩空间转换、视频分辨率缩放算法性能 | [代码仓](https://gitcode.com/boostkit/ffmpeg) |

## 关于社区

提供社区治理架构、SIG组织运作章程、参与贡献、邮件订阅、社媒联系方式等公共模块内容简介和指引。

## 贡献、建议与交流

欢迎大家为社区做贡献，如果使用过程中有任何问题/建议，或者需要反馈特性需求和bug报告，可以提交[Issues](https://gitcode.com/boostkit/community/blob/master/docs/contributor/issue-submit.md)联系我们，具体贡献方法可参考[这里](https://gitcode.com/boostkit/community/blob/master/docs/contributor/contributing.md)。同时也欢迎大家在[讨论专区](https://gitcode.com/boostkit/community/discussions)展开讨论交流。感谢您的支持。

## LICENSE

本项目的文档适用于CC-BY 4.0许可证，具体请参见[LICENSE文件](LICENSE-DOCS)。
