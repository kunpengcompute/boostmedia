# x265快速入门

本文提供使用x265+FFmpeg进行视频编码的示例。

## 通过FFmpeg调用x265进行视频编码操作

- 添加依赖库环境变量。

    ```shell
    export LD_LIBRARY_PATH=/home/ffmpeg/install/lib:/home/x265/x265_install/lib
    export PKG_CONFIG_PATH=/home/x265/x265_install/lib/pkgconfig/
    ```

- 开始执行编码程序。相关参数的解释说明如下表所示。

    ```shell
    /home/ffmpeg/install/bin/ffmpeg -s 1920x1080 -framerate 60 -i input.yuv -preset medium -c:v libx265 -x265-params "bitrate=2000:vbv-maxrate=2000:vbv-bufsize=2000" -f mp4 output.mp4
    ```

    |参数|说明|
    |--|--|
    |/home/ffmpeg/install/bin/ffmpeg|指定FFmpeg可执行文件的路径，可自定义。|
    |-s 1920x1080|设置输出视频的分辨率为1920x1080。|
    |-framerate 60|设置输出视频的帧率为60帧每秒。|
    |-i input.yuv|指定输入文件的路径和文件名。|
    |-preset medium|设置视频编码的预设选项为medium，该参数设置会影响编码速度和压缩效率。|
    |-c:v libx265|指定使用libx265编码器进行视频编码。|
    |-x265-params "bitrate=2000:vbv-maxrate=2000:vbv-bufsize=2000"|设置x265编码器的参数，包括码率、最大码率、缓冲区大小。|
    |-f mp4 output.mp4|指定输出格式为mp4，可指定其他类型。|
    
    回显编码了多少帧则表示编码成功，如：

    ```shell
    encoded 600 frames in 7.50s (80.01 fps), 1992.12 kb/s, Avg QP:38.30
    ```
