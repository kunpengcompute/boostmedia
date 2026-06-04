# x265 FAQs

This document provides answers to frequently asked questions (FAQs) about using x265.

## x265 Compilation Parameters Do Not Match the Processor Architecture

**Symptom**

During x265 compilation, a message is displayed indicating that the compilation parameters do not match the processor architecture. For example, the processor uses the x86 architecture, but the compilation parameters of the AArch64 architecture are used.

**Key Process and Cause Analysis**

The compilation parameters do not match the processor architecture.

**Conclusion, Solution, and Effect**

Run **uname -a** to view the processor architecture information and select the correct compilation parameters based on the command output.

## NASM Cannot Be Found After GCC Is Installed

**Symptom**

After GCC is installed, a message is displayed indicating that NASM cannot be found.

**Key Process and Cause Analysis**

NASM is not installed.

**Conclusion, Solution, and Effect**

1. Download the NASM software package.

    Download address: [NASM official website](https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/)

2. Upload the software package to the server and decompress it.

    ```shell
    tar -xvf nasm.tar.gz
    ```

3. Start the installation.

    ```shell
    ./configure
    make
    sudo make install
    ```

## No Permission to Execute the Encoding Program Using x265

**Symptom**

"Permission denied" is displayed when using x265 to execute the encoding program.

**Key Process and Cause Analysis**

Insufficient permissions.

**Conclusion, Solution, and Effect**

Grant FFmpeg the permission to execute the encoding program.

```shell
chmod +x /home/ffmpeg/build/ffmpeg
```

## Dependency Library Loading Failed When x265 Is Used to Execute the Encoding Program

**Symptom**

When x265 is used to execute the encoding program, the message "ffmpeg: error while loading shared libraries: libavdevice.so.xx" is displayed.

**Key Process and Cause Analysis**

A dependency library fails to be loaded.

**Conclusion, Solution, and Effect**

1. Find the path to the dependency library specified in the error information.

    ```shell
    find / -name libavdevice.so.xx
    ```

2. Add the path to the environment variable, that is, add the found path next to the colon (:) in the following command:

    ```shell
    export LD_LIBRARY_PATH=/home/ffmpeg/install/lib/:Path_of_the_dependency_library
    ```
