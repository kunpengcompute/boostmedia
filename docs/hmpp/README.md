# HMPP介绍

HMPP（Hyper Media Performance Primitives，鲲鹏超媒体性能库）是华为公司在自研鲲鹏处理器硬件平台基础上开发的加速库，通过鲲鹏处理器支持NEON加速指令集对信号处理和图像处理业务功能提供高性能加速函数接口。HMPP涉及图像处理、颜色转换、滤波、变换、几何，为计算机视觉运算、向量运算、统计、信号滤波、信号变换和固定精度运算等提供了丰富的功能接口和极致性能优化，可适用于数字媒体、数据通信、生物医学、航空航天等领域。

## 特性说明

HMPP包含基础函数库、信号库、图像库，其特性说明如下表所示：

|名称|特性说明|
|--|--|
|基础函数库|该模块实现了63个基础函数，包含字节对齐、内存分配、内存释放、获取状态码描述、多线程设置及线程信息获取、获取CPU（Central Processing Unit）的缓存、时钟频率和CPU时间戳，以及HMPP版本信息、指令信息获取及设置和FlushToZero模式开闭等函数。|
|信号库（HMPPS）|该模块实现了以下几种函数：基础向量运算：逻辑移位运算、向量转换、向量统计、采样函数、初始化函数等。信号变换：FFT（Fast Fourier Transform）、CZT（Chirp Z-Transform）、功率谱、希尔伯特、小波变换（Wavelet Transform）等。滤波：卷积、FI（Finite Impulse Response）滤波、IIR（Infinite Impulse Response）滤波、重采样、中值滤波、自相关等。窗口函数：Blackman、Hann、Kaiser、Hamming、Bartlett等。数学运算：算术运算、三角运算、幂、根、指数运算等。|
|图像库（HMPPI）|该模块实现了图像颜色模型转换、阈值、算术逻辑运算、图像几何变换等相关函数。|

## 版本说明

版本说明包含HMPP的软件版本配套关系和软件包下载以及每个版本的特性变更说明。

| 版本 | 变更 |
| -- | -- |
| v2.6.1.beta1  | 新增HMPPS_Exp_32fc_A24、HMPPS_ConjPack_32fc_I、HMPPI_Transpose_16s_C1R、HMPPI_Transpose_32s_C1R、HMPPI_Transpose_32f_C1R、HMPPI_Set_8u_C1R、HMPPI_Set_32f_C1R、HMPPI_Not_8u_C1IR、HMPPI_Or_8u_C1IR算子。   |
| v2.6.0.beta1  | 新增HMPPS_Sin_64f_A50、HMPPS_Tan_64f_A50、HMPPS_Asin_32f_A24、HMPPI_Or_8u_C1R、HMPPI_FilterMinBorder_8u_C1R、HMPPI_WarpAffineNearest_8u_C1R、HMPPI_Conv_8u_C1R、HMPPI_Conv_32f_C1R、HMPPI_ResizeLinearInit_16s、HMPPI_ResizeLinear_16s_C1R、HMPPI_ResizeNearestInit_8u、HMPPI_ResizeNearest_8u_C1R算子。   |

更多历史版本信息具体请参见[HMPP版本说明书](./docs/zh/release_notes.md)。

## 兼容性信息

HMPP目前仅支持鲲鹏平台下使用。

## API参考

API参考包括HMPP函数功能、签名、返回值等的说明，具体请参见[HMPP API参考](./docs/zh/api_reference.md)。

## 环境部署

环境部署包括HMPP的编译安装步骤，具体请参见[HMPP安装指南](./docs/zh/installation_guide.md)。

## 快速入门

快速入门包括通过API调用HMPP接口，具体请参见[HMPP快速入门](./docs/zh/quick_start.md)。

**HMPP使用注意事项如下：**

1、为获得最优性能，HMPP接口内部不做完整入参校验，入参合法性及合理性由调用方业务来保证。

2、HMPP为底层原语库，计算流程涉及内存读写、分配，不提供和发布操作系统，操作系统须用户自行安装，HMPP不承担操作系统的安全责任，用户需要结合自身应用对操作系统安全加固，包括不安装或者剔除不必要的应用等。

3、为阻止缓冲区溢出攻击，建议使用ASLR（Address Space Layout Randomization）技术，通过对堆、栈、共享库映射等线性区布局的随机化，增加攻击者预测目的地址的难度，防止攻击者直接定位攻击代码位置。该技术可作用于堆、栈、内存映射区（mmap基址、shared libraries、vdso页）。开启方式：echo 2 >/proc/sys/kernel/randomize_va_space

## 常见问题

具体请参见[HMPP FAQ](./docs/zh/faq.md)。

## License

本项目采用Apache License Version 2.0许可证，详见[LICENSE](./LICENSE)文件。

本项目的文档适用CC-BY 4.0许可证，具体请参见[LICENSE](./docs/zh/LICENSE)文件。
