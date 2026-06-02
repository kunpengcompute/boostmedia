# Project Introduction

x265 is an open-source free software and function library for encoding HEVC/H.265 videos. To improve the video encoding performance on Arm devices, this repository uses SVE, a vectorized instruction set architecture, to optimize standard x265. SVE allows the same instruction to be executed on hardware devices with different vector lengths. With this SVE feature, x265 can leverage the vectorized computing capability of the Arm architecture to accelerate video encoding and improve the encoding quality.

An x265 version optimized based on SVE works only on devices that support the Arm SVE instruction set. If your device does not support SVE, you cannot use an optimized version. If your device uses the x86 architecture, consider using the standard x265 version for video encoding.

## Feature Overview

SVE vectorization deeply optimizes video encoding operators and makes full use of the Kunpeng platform to accelerate vectorized instructions, improving video encoding efficiency.

## Release Notes

| Open-Source x265| Optimized Feature|
| ---- |----|
| 3.5 | SVE vectorization optimization for x265 encoding|

## Compatibility Information

An x265 version optimized based on SVE works only on devices that support the Arm SVE instruction set. If your device does not support SVE, you cannot use an optimized version. In this case, you can use the standard x265 version for video encoding.

## Environment Deployment

Environment deployment includes the configuration, compilation, and installation of the x265 environment. For details, see [Installation Guide](./docs/en/installation_guide.md).

## Quick Start

For details about how to use FFmpeg to call x265 for video encoding, see [Quick Start](./docs/en/quick_start.md).

## FAQs

For details, see [FAQs](./docs/en/faq.md).

## Reference

[x265 official documentation](http://x265.readthedocs.org/en/master/)

[x265 developer wiki](http://bitbucket.org/multicoreware/x265_git/wiki/)

## LICENSE

This project uses the GPL v2 license. For details, see [COPYING](./COPYING).

The documents of this project are licensed under CC-BY 4.0. For details, see [LICENSE](docs/en/LICENSE).
