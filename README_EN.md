# BoostMedia

BoostMedia provides application-layer acceleration based on the Kunpeng platform for video encoding and decoding and image processing.

## Architecture

![](figures/architecture.png)

## Feature Description

### Basic Media Operators

|  Feature| Description |
|--|--|
| KVCL (to be open-sourced)| Vector instructions are used to enhance common basic operators for video encoding. Vector instructions are accelerated based on hardware features to improve operator performance.|

### Video Encoders

|  Feature| Description |
|--|--|
| HW265Enc (to be open-sourced)| This H.265/HEVC-compliant video encoder uses the new Kunpeng 920 processor model to optimize encoding library affinity. It transforms YUV pixel files into standard H.265/HEVC bitstreams, with support for 8-bit depth and 420p video content.|
| x265 (to be open-sourced)| SVE vectorization deeply optimizes video encoding operators and makes full use of the Kunpeng platform to accelerate vector instructions, improving video encoding efficiency.|
| VVenC (to be open-sourced)| Kunpeng vector instructions accelerate underlying transcoding operators in VVenC, improving the overall performance.|

### Image Processing

|  Feature| Description |
|--|--|
| [OpenCV](https://gitcode.com/boostkit/opencv)| Operators in OpenCV are enhanced through methods such as algorithm optimization, vector instruction optimization, and parallel optimization, significantly improving OpenCV performance on Kunpeng servers.|
| [libavif](https://gitcode.com/boostkit/libavif) | The image encoding performance of libavif is improved through methods such as vector optimization, algorithm optimization, and workflow optimization.|
| [LibWebP](https://gitcode.com/boostkit/libwebp) | The image encoding performance of LibWebP is improved through methods such as vector optimization, algorithm optimization, and workflow optimization.|
| HMPP (to be open-sourced) | Hyper Media Performance Primitives (HMPP) is an acceleration library developed based on the Kunpeng processor hardware platform for image processing, color conversion, signal filtering and transforms, and geometry. It utilizes vector instruction sets to provide high-performance acceleration function interfaces for service functions such as computer vision operations, vector operations, statistics, signal filtering and transforms, and fixed-point arithmetic.|
| Vision (to be open-sourced) | The image processing performance of Vision is improved through methods such as vector optimization, algorithm optimization, and workflow optimization.|

### Media Framework

|  Feature| Description |
|--|--|
| [FFmpeg](https://gitcode.com/boostkit/ffmpeg) | The performance of color space conversion and video resolution scaling algorithms in FFmpeg is improved through methods such as vector optimization, algorithm optimization, and parallel optimization.|

## About the Community

This part provides the introduction and guidance to the public modules, such as the community governance architecture, SIG operations regulations, contribution, email subscription, and social media contact information.

## Contributions, Suggestions, and Discussions

We welcome your contributions to the community. If you have any questions/suggestions or want to provide feedback on feature requirements and bug reports, you can submit [issues](https://gitcode.com/boostkit/community/blob/master/docs/contributor/issue-submit.md). For details, see the [contribution guideline](https://gitcode.com/boostkit/community/blob/master/docs/contributor/contributing.md). You are also welcome to share insights in [Discussions](https://gitcode.com/boostkit/community/discussions). Thank you for your support.

## License

The documents of this project are licensed under CC-BY 4.0. For details, see [LICENSE](LICENSE-DOCS).
