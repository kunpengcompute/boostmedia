# KVCL: Kunpeng Video Codec Library

<!-- md-trans-meta sourceCommit=27d274ca52a2dd84968a3eba75037a1dc0b75d33 translatedAt=2026-09-23T05:03:56.436Z pushedAt=2026-09-28T02:52:02.049Z -->

## Latest News

- [2026.09.30]: Released KVCL 1.1.0. Added the KvclVvcDct16x16, KvclVvcDct32x32, KvclVvcDct64x64, and KvclVvcSa16d32x32 operators.
- [2026.03.30]: Released KVCL 1.0.0. Added the KvclDct16x16, KvclDct32x32, KvclSatd8x8, KvclSatd16x16, KvclSa8d8x8, KvclSa8d16x16, KvclScanPosLast, KvclLumaVPP32x32, KvclLumaVPP64x64, KvclChromaHPP32x32, KvclChromaHPP16x16, KvclSaoCuStatsE0, KvclSaoCuStatsE1, KvclSaoCuStatsE2, KvclSaoCuStatsE3, KvclSad64x32, KvclSad64x64, KvclChromaHPS32x32, KvclChromaHPS16x16, and KvclSa16d16x16 operators.

## Overview

Across different video processing frameworks (such as FFmpeg and GStreamer) and encoders, there are numerous operators with identical functionality, such as SATD/SAD, DCT, and SAO.

KVCL (Kunpeng Video Codec Library) includes the top common operators in video encoding, and these operators are optimized for Kunpeng hardware.

## Directory Structure

```shell
├── docs                     # Project documentation directory
│   └── zh                   # Chinese documentation directory
│       ├── LICENSE          # Document license
│       ├── feature_guide.md # Feature guide
│       ├── installation_guide.md # Installation guide
│       └── release_note.md  # Release notes
├── examples                 # Sample code directory
├── include                  # Operator header file directory
├── src                      # Source code directory
│   ├── aarch64              # Operator assembly implementation directory
│   ├── cmake                # CMake project directory
│   └── neon                 # NEON source code directory
├── test                     # Test code directory
│   └── unit_bench_test      # Unit test directory
├── build.sh                 # Build script
├── CMakeLists.txt           # CMake configuration file
├── LICENSE                  # Project license
└── README.md                # Project description
```

## Release Notes

| KVCL Version   | Feature Changes    |
| ------------ | ------------ |
|   1.0.0    |   Added the following operators: KvclDct16x16, KvclDct32x32, KvclSatd8x8, KvclSatd16x16, KvclSa8d8x8, KvclSa8d16x16, KvclScanPosLast, KvclLumaVPP32x32, KvclLumaVPP64x64, KvclChromaHPP32x32, KvclChromaHPP16x16, KvclSaoCuStatsE0, KvclSaoCuStatsE1, KvclSaoCuStatsE2, KvclSaoCuStatsE3, KvclSad64x32, KvclSad64x64, KvclChromaHPS32x32, KvclChromaHPS16x16, KvclSa16d16x16   |
|   1.1.0    |   Added the following operators: KvclVvcDct16x16, KvclVvcDct32x32, KvclVvcDct64x64, KvclVvcSa16d32x32   |

## Environment Deployment

For details about KVCL environment deployment, see [KVCL Environment Deployment](./docs/en/installation_guide.md).

## Quick Start

For details about KVCL features, see [KVCL Feature Guide](./docs/en/feature_guide.md).

## Feature Description

### Functions

| Interface | Responsibility |
| ------------ | ------------ |
| KvclDct16x16 | Computes the discrete cosine transform (DCT) of a 16x16 pixel block. |
| KvclDct32x32 | Computes the DCT of a 32x32 pixel block. |
| KvclIDct16x16 | Computes the inverse discrete cosine transform (IDCT) of a 16x16 pixel block. |
| KvclLumaVPP32x32 | Computes the vertical 8-tap interpolation of a 32x32 pixel block. |
| KvclLumaVPP64x64 | Computes the vertical 8-tap interpolation of a 64x64 pixel block. |
| KvclLumaHPP32x32 | Computes the horizontal 8-tap interpolation of a 32x32 pixel block. |
| KvclLumaHPP64x64 | Computes the horizontal 8-tap interpolation of a 64x64 pixel block. |
| KvclChromaHPP32x32 | Computes the horizontal 4-tap interpolation of a 32x32 pixel block. |
| KvclChromaHPP16x16 | Computes the horizontal 4-tap interpolation of a 16x16 pixel block. |
| KvclScanPosLast | Scans coefficients in the order provided by the scan order table until the last non-zero coefficient is found. |
| KvclQuantRdoq | Quantization in the rate-distortion optimized quantization (RDOQ) phase. |
| KvclSad64x32 | Computes the sum of absolute differences (SAD) of a 64x32 pixel block. |
| KvclSad64x64 | Computes the SAD of a 64x64 pixel block. |
| KvclSad4x32x32 | Computes the SAD of a 4x32x32 pixel block. |
| KvclSad4x32x64 | Computes the SAD of a 4x32x64 pixel block. |
| KvclSad4x64x16 | Computes the SAD of a 4x64x16 pixel block. |
| KvclSad4x64x32 | Computes the SAD of a 4x64x32 pixel block. |
| KvclSad4x64x64 | Computes the SAD of a 4x64x64 pixel block. |
| KvclSaoCuStatsE0 | Computes sample adaptive offset (SAO)-related statistics: SAO edge offset type 0, horizontal direction (left and right neighbors). |
| KvclSaoCuStatsE1 | Computes SAO-related statistics: SAO edge offset type 1, vertical direction (upper and lower neighbors). |
| KvclSaoCuStatsE2 | Computes SAO-related statistics: SAO edge offset type 2, diagonal direction (45° neighbors from top-left to bottom-right). |
| KvclSaoCuStatsE3 | Computes SAO-related statistics: SAO edge offset type 3, anti-diagonal direction (135° neighbors from top-right to bottom-left). |
| KvclSatd8x8 | Computes the sum of absolute transformed differences (SATD) of an 8x8 pixel block via Hadamard transform. |
| KvclSatd16x16 | Computes the SATD of a 16x16 pixel block via Hadamard transform. |
| KvclSa8d8x8 | Computes the SA8D of an 8x8 pixel block via Hadamard transform. |
| KvclSa8d16x16 | Computes the SA8D of a 16x16 pixel block via Hadamard transform. |
| KvclSa16d16x16 | Computes the SA16D of a 16x16 pixel block via Hadamard transform. |
| KvclVvcSa16d32x32 | Computes the SA16D of a 32x32 pixel block via Hadamard transform under VVC. |
| KvclVvcDct16x16 | Computes the DCT of a 16x16 pixel block under VVC. |
| KvclVvcDct32x32 | Computes the DCT of a 32x32 pixel block under VVC. |
| KvclVvcDct64x64 | Computes the DCT of a 64x64 pixel block under VVC. |

### Features

KVCL uses various methods to optimize operators, for example:

1. SIMD vectorization: KVCL uses SIMD instructions such as NEON, SVE, and SVE2 to implement operators, achieving data-level parallelism.
2. Instruction merging: KVCL identifies specific instruction sequences and replaces them with functionally equivalent but more efficient instruction sequences, reducing the number of instructions and lowering latency.
3. Optimization for hardware features: KVCL performs instruction scheduling and register allocation tailored to Kunpeng hardware features, effectively reducing operator overhead.
4. Mathematical expression optimization: KVCL understands the mathematical nature of computation patterns and combines them with hardware features, achieving significant performance improvements.
5. Pipeline utilization optimization: Instruction execution on Kunpeng chips is divided into multiple stages. Pipeline utilization optimization keeps the hardware pipeline in a high-throughput state by reducing pipeline stalls and optimizing data dependencies.

## License

For the license of the KVCL project, see [LICENSE](./LICENSE). The documentation of this project is licensed under CC-BY 4.0. For details, see [LICENSE](./docs/LICENSE).

## Contribution Statement

We appreciate and welcome contributions to the community! For specific contribution guidelines, refer to [this page](https://gitcode.com/boostkit/community/blob/master/docs/contributor/contributing.md). You are also welcome to join discussions in the [discussion area](https://gitcode.com/boostkit/community/discussions). Thank you for your support.
