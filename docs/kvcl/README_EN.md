# KVCL: Kunpeng Video Codec Library

## Latest Updates

- [2026-03-30]: Released KVCL 1.0.0. The KvclDct16x16, KvclDct32x32, KvclIDct16x16, KvclLumaVPP32x32, KvclLumaVPP64x64, KvclLumaHPP32x32, KvclLumaHPP64x64, KvclChromaHPP32x32, KvclChromaHPP16x16, KvclScanPosLast, KvclQuantRdoq, KvclSad64x32, KvclSad64x64, KvclSad4x32x32, KvclSad4x32x64, KvclSad4x64x16, KvclSad4x64x32, KvclSad4x64x64, KvclSaoCuStatsE0, KvclSaoCuStatsE1, KvclSaoCuStatsE2, KvclSaoCuStatsE3, KvclSatd8x8, KvclSatd16x16, KvclSa8d8x8, and KvclSa8d16x16 operators were added.

## Overview

In different video processing frameworks (such as FFmpeg and GStreamer) and encoders, there are a large number of operators with the same functions, such as SATD/SAD, DCT, and SAO.

Kunpeng Video Codec Library (KVCL) contains top common operators in video encoding and optimizes operators for Kunpeng hardware.


## Release Notes

| KVCL Version  | Feature Change   |
| ------------ | ------------ |
|   1.0.0    |   The KvclDct16x16, KvclDct32x32, KvclIDct16x16, KvclLumaVPP32x32, KvclLumaVPP64x64, KvclLumaHPP32x32, KvclLumaHPP64x64, KvclChromaHPP32x32, KvclChromaHPP16x16, KvclScanPosLast, KvclQuantRdoq, KvclSad64x32, KvclSad64x64, KvclSad4x32x32, KvclSad4x32x64, KvclSad4x64x16, KvclSad4x64x32, KvclSad4x64x64, KvclSaoCuStatsE0, KvclSaoCuStatsE1, KvclSaoCuStatsE2, KvclSaoCuStatsE3, KvclSatd8x8, KvclSatd16x16, KvclSa8d8x8, and KvclSa8d16x16 operators were added.  |

## Environment Deployment

For details about how to deploy the KVCL environment, see [Environment Deployment](./docs/en/install_guide_bin.md).

## Quick Start

For details about how to quickly start KVCL, see [Quick Start](./docs/en/quick_start.md).

## Features

### Functions

|  Interface|  Function|
| ------------ | ------------ |
| KvclDct16x16 | Computes DCT of a 16x16 pixel block. |
| KvclDct32x32 | Computes DCT of a 32x32 pixel block. |
| KvclIDct16x16 | Computes inverse DCT of a 16x16 pixel block. |
| KvclLumaVPP32x32 | Computes vertical 8-tap interpolation of a 32x32 pixel block. |
| KvclLumaVPP64x64 | Computes vertical 8-tap interpolation of a 64x64 pixel block. |
| KvclLumaHPP32x32  | Computes horizontal 8-tap interpolation of a 32x32 pixel block. |
| KvclLumaHPP64x64  | Computes horizontal 8-tap interpolation of a 64x64 pixel block. |
| KvclChromaHPP32x32  | Computes horizontal 4-tap interpolation of a 32x32 pixel block. |
| KvclChromaHPP16x16  | Computes horizontal 4-tap interpolation of a 16x16 pixel block. |
| KvclScanPosLast  | Scans coefficients in the order provided by the scan order table until the last non-zero coefficient is found. |
| KvclQuantRdoq  |  RDOQ-based quantization|
| KvclSad64x32  |  Computes the SAD of a 64x32 pixel block.|
|KvclSad64x64 |Computes the SAD of a 64x64 pixel block.|
|KvclSad4x32x32 |Computes the SAD of a 4x32x32 pixel block.|
|KvclSad4x32x64 |Computes the SAD of a 4x32x64 pixel block.|
|KvclSad4x64x16 | Computes the SAD of a 4x64x16 pixel block.|
|KvclSad4x64x32 | Computes the SAD of a 4x64x32 pixel block.|
| KvclSad4x64x64| Computes the SAD of a 4x64x64 pixel block.|
|KvclSaoCuStatsE0 |SAO statistics: SAO edge offset type 0, horizontal direction (left and right neighborhoods)|
|KvclSaoCuStatsE1 | SAO statistics: SAO edge offset type 1, vertical direction (top and bottom neighborhoods)|
|KvclSaoCuStatsE2 | SAO statistics: SAO edge offset type 2, diagonal direction (45-degree neighborhood from the upper left to the lower right)|
|KvclSaoCuStatsE3 | SAO statistics: SAO edge offset type 3, anti-diagonal direction (135-degree neighborhood from the upper right to the lower left)|
|KvclSatd8x8 | Computes the SATD of an 8x8 pixel block (sum of absolute Hadamard transform differences).|
|KvclSatd16x16 |Computes the SATD of a 16x16 pixel block (sum of absolute Hadamard transform differences).|
|KvclSa8d8x8 |Computes the SA8D of an 8x8 pixel block (sum of absolute Hadamard transform differences).|
| KvclSa8d16x16 |Computes the SA8D of a 16x16 pixel block (sum of absolute Hadamard transform differences).|

### Features

KVCL uses multiple methods to optimize operators, for example:

1. SIMD vectorization: KVCL uses SIMD instructions such as NEON, SVE, and SVE2 to implement operators and achieve data-level parallelism.
2. Instruction combination: KVCL identifies specific instruction sequences and replaces them with more efficient instruction sequences that provide equivalent functionality, reducing the number of instructions and latency.
3. Hardware feature optimization: KVCL performs instruction scheduling and register allocation based on Kunpeng hardware features to effectively reduce operator overhead.
4. Mathematical expression optimization: By deeply understanding the mathematical essence of the computing mode and combining it with hardware features, significant performance improvement is achieved.
5. Pipeline utilization optimization: Kunpeng chip instruction execution is divided into multiple phases. Pipeline utilization optimization reduces pipeline pauses and optimizes data dependencies to ensure high throughput of hardware pipelines.

## License

The documents of this project are licensed under CC-BY 4.0. For details, see [LICENSE](./docs/en/LICENSE).
