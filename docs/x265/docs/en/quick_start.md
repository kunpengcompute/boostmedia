# Getting Started with x265

This document provides an example of using x265 and FFmpeg for video encoding.

## Using FFmpeg to Invoke x265 in Video Encoding Operations

1. Add the environment variables of the dependency libraries.

    ```shell
    export LD_LIBRARY_PATH=/home/ffmpeg/install/lib:/home/x265/x265_install/lib
    export PKG_CONFIG_PATH=/home/x265/x265_install/lib/pkgconfig/
    ```

2. Run the encoding program. The following table describes the related parameters.

    ```shell
    /home/ffmpeg/install/bin/ffmpeg -s 1920x1080 -framerate 60 -i input.yuv -preset medium -c:v libx265 -x265-params "bitrate=2000:vbv-maxrate=2000:vbv-bufsize=2000" -f mp4 output.mp4
    ```

    |Parameter|Description|
    |--|--|
    |/home/ffmpeg/install/bin/ffmpeg|Specifies the path of the FFmpeg executable file. The path can be customized.|
    |-s 1920x1080|Sets the resolution of the output video to 1920 x 1080.|
    |-framerate 60|Sets the frame rate of the output video to 60 fps.|
    |-i input.yuv|Specifies the name and path of the input file.|
    |-preset medium|Sets the preset option of video encoding to **medium**. This parameter affects the encoding speed and compression efficiency.|
    |-c:v libx265|Uses the libx265 encoder to encode videos.|
    |-x265-params "bitrate=2000:vbv-maxrate=2000:vbv-bufsize=2000"|Sets x265 encoder parameters, including the bit rate, maximum bit rate, and buffer size.|
    |-f mp4 output.mp4|Sets the output format to MP4. You can also specify other formats.|

    If the number of encoded frames and the encoding time are displayed in the command output, the encoding is successful. For example:

    ```shell
    encoded 600 frames in 7.50s (80.01 fps), 1992.12 kb/s, Avg QP:38.30
    ```
