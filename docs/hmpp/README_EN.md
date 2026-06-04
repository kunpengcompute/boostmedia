# Introduction to HMPP

Hyper Media Performance Primitives (HMPP) is an acceleration library developed by Huawei based on the Kunpeng processor hardware platform. It provides high-performance acceleration functional interfaces for signal and image processing through NEON instructions supported by the Kunpeng processor. Covering image processing, color conversion, filtering, transformation, and geometry, HMPP provides comprehensive functional APIs and ultimate performance optimization for computer vision computing, vector operations, statistics, signal filtering, signal transformation, and fixed-precision computing. It is applicable to fields such as digital media, data communications, biomedicine, and aerospace.

## Feature Description

HMPP consists of the basic function library, signal library, and image library. The following table describes their features.

|Library Name|Description|
|--|--|
|Basic function library|This module enables 63 basic functions, including functions for performing byte alignment, memory allocation, memory release, multi-thread setting, obtaining status code description, thread information, CPU cache, clock frequency, CPU timestamp, and HMPP version, setting instruction information, and toggling the FlushToZero mode.|
|Signal library (HMPPS)|This module implements the following functions. Basic vector operations: logical shift operations, vector conversion, vector statistics, sampling functions, initialization functions, etc. Signal transformation: Fast Fourier transform (FFT), chirp Z-transform (CZT), power spectrum, Hilbert transform, and wavelet transform (WT), etc. Filtering: convolution, Finite Impulse Response (FIR) filtering, Infinite Impulse Response (IIR) filtering, resampling, median filtering, autocorrelation, etc. Window functions: Blackman, Hann, Kaiser, Hamming, Bartlett, etc. Mathematical operations: arithmetic operation, triangle operation, and power, root, and exponential operations, etc.|
|Image library (HMPPI)|This module implements functions such as image color model conversion, thresholding, arithmetic and logical operations, and image geometric transformations.|

## Release Notes

The release notes describe the HMPP software version compatibility, software package downloads, and feature updates for each version.

| Version| Change Description|
| -- | -- |
| v2.6.1.beta1  | Added HMPPS_Exp_32fc_A24, HMPPS_ConjPack_32fc_I, HMPPI_Transpose_16s_C1R, HMPPI_Transpose_32s_C1R, HMPPI_Transpose_32f_C1R, HMPPI_Set_8u_C1R, HMPPI_Set_32f_C1R, HMPPI_Not_8u_C1IR, and HMPPI_Or_8u_C1IR operators.  |
| v2.6.0.beta1  | Added HMPPS_Sin_64f_A50, HMPPS_Tan_64f_A50, HMPPS_Asin_32f_A24, HMPPI_Or_8u_C1R, HMPPI_FilterMinBorder_8u_C1R, HMPPI_WarpAffineNearest_8u_C1R, HMPPI_Conv_8u_C1R, HMPPI_Conv_32f_C1R, HMPPI_ResizeLinearInit_16s, HMPPI_ResizeLinear_16s_C1R, HMPPI_ResizeNearestInit_8u, and HMPPI_ResizeNearest_8u_C1R operators.  |

For details about historical versions, see [HMPP Release Notes](./docs/en/release_notes.md).

## Compatibility Information

Currently, HMPP can be used only on the Kunpeng platform.

## API Reference

Describes the functions, signatures, and return values of HMPP functions. For details, see [HMPP API Reference](./docs/en/api_reference.md).

## Environment Deployment

Follow the instructions in [HMPP Installation Guide](./docs/en/installation_guide.md) to set up the environment for compilation and installation.

## Quick Start

Describes how to call HMPP APIs. For details, see [HMPP Quick Start](./docs/en/quick_start.md).

**Precautions for using HMPP are as follows:**

1. To achieve optimal performance, HMPP APIs do not perform complete validation on input parameters. The validity and rationality of input parameters must be guaranteed by the invoking service.

2. HMPP is a low-level primitive library. The computing process involves memory read/write and allocation. You must install the OS by yourself and perform OS security hardening. Install only necessary applications or uninstall unnecessary applications.

3. To prevent buffer overflow attacks, you are advised to use the address space layout randomization (ASLR) technology to randomize the layout of linear areas such as the heap, stack, and shared library mapping to make it more difficult for attackers to predict target addresses and locate code. This technology can be applied to heaps, stacks, and memory mapping areas (mmap base addresses, shared libraries, and vDSO pages). Enabling method: echo 2 >/proc/sys/kernel/randomize_va_space

## FAQ

See [HMPP FAQs](./docs/en/faq.md) for solutions to FAQs.

## License

This project is released under the Apache License Version 2.0. For details, see [LICENSE](./LICENSE).

The documents of this project are licensed under CC-BY 4.0. For details, see [LICENSE](./docs/en/LICENSE).
