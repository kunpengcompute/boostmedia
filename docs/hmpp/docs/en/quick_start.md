# HMPP Quick Start

This document provides sample code for calling HMPP API functions to help you quickly get started with the library.

## Special Usage

Some complex functions, such as sampling, filtering, and audio, include paired APIs to implement auxiliary functions like initialization. Strictly follow the pairing relationships specified in the guide to ensure correct functionality. For details, see the description of each API.

- For signal library APIs, use the HMPPS_Malloc and HMPPS_Free series of functions to allocate and release memory space. Select the corresponding API function based on the data type of the allocated space to ensure the functional security and optimal performance of the HMPP library.
- Image library APIs do not restrict the combination of image width and height. The storage size of the input image (width x height x number of channels x data type size) must not exceed `INT32_MAX` bytes. Industry-standard image storage specifications, such as 1080P, are recommended.

## Compilation Methods

- (Mandatory) Set the HMPP environment variables and add the HMPP compilation options.
    Run the following command to ensure that the HMPP-related environment variables are correctly set:

    ```shell
    export LD_LIBRARY_PATH=/usr/local/lib/HMPP:$LD_LIBRARY_PATH
    ```

    Add compilation options related to HMPP header files and shared library paths:

    ```shell
    -I /usr/local/include/HMPP -L /usr/local/lib/HMPP
    ```

- (Mandatory) Run the following command to ensure that environment variables related to libboundscheck are correctly set:

    ```shell
    export LD_LIBRARY_PATH=/usr/local/lib/libboundscheck:$LD_LIBRARY_PATH
    ```

    Add the libboundscheck header files and shared library paths to the compilation options:

    ```shell
    -I /usr/local/include/libboundscheck -L /usr/local/lib/libboundscheck -lboundscheck
    ```

- (Optional) To use HMPP basic functions, add the following compilation options:

    ```shell
    -lHMPP_core
    ```

- (Optional) To use the HMPP signal library and HMPP image library, run the following commands to ensure that KML-related environment variables are correctly set:

    ```shell
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/nolocking:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/locking:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/omp:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kblas/pthread:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kspblas/single:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/neon/kspblas/multi:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch/kvml/single:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/local/kml/lib/noarch/kvml/multi:$LD_LIBRARY_PATH
    ```

    Add KML header files and shared library paths to the compilation options:

    ```shell
    -I /usr/local/kml/include -L /usr/local/kml/lib/neon -lkfft -lkfftf -L /usr/local/kml/lib/noarch/kvml/single -lkvml -L /usr/local/kml/lib/noarch/ -lkm -lm -L /usr/local/kml/lib/neon/kblas/locking -lkblas
    ```

- (Optional) To use the HMPP signal library, add the following compilation options:

    ```shell
    -lHMPP_signal -lHMPP_core -lpthread
    ```

- (Optional) To use the HMPP image library, add the following compilation options:

    ```shell
    -lHMPP_image -lHMPP_core -lm -lHMPP_signal -lpthread
    ```

## API Calling Example

The HMPP header file referenced in the provided sample code is `hmpp.h`.

- Create a `test.c` file and add the following code:

    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <stdint.h>
    #include "hmpp.h"
    #define BUFFER_SIZE_T 20
    void AddExample()
    {
        uint32_t *src1 = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        uint32_t *src2 = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        uint32_t *dst = HMPPS_Malloc_32u(BUFFER_SIZE_T);
        int32_t i, result;
    
        if (src1 == NULL || src2 == NULL || dst == NULL) {
            return;
        }
        for (i = 0; i < BUFFER_SIZE_T; ++i) {
             src1[i] = i;
             src2[i] = i + 1;
        }
        
        result = HMPPS_Add_32u(src1, src2, dst, BUFFER_SIZE_T);
        printf("result = %d \n", result);
        printf("src1: ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", src1[i]);
        }
        printf("\nsrc2: ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", src2[i]);
        }
        printf("\ndst : ");
        for (i = 0; i < BUFFER_SIZE_T; i++) {
            printf("%2d ", dst[i]);
        }
        printf("\n");

        HMPPS_Free(src1);
        HMPPS_Free(src2);
        HMPPS_Free(dst);
    }
    int main(void)
    {
        AddExample();
        return 0;
    }
    ```

- Compile the file.

    ```shell
    gcc test.c -o test -L /usr/local/lib/HMPP -I /usr/local/include/HMPP -lHMPP_signal  -lHMPP_core -lpthread -I /usr/local/include/libboundscheck -L /usr/local/lib/libboundscheck -lboundscheck -I /usr/local/kml/include -L /usr/local/kml/lib/neon -lkfft -lkfftf -L /usr/local/kml/lib/noarch/kvml/single -lkvml -L /usr/local/kml/lib/noarch/ -lkm -lm -L /usr/local/kml/lib/neon/kblas/locking -lkblas
    ```

- Execute the file.

    ```shell
    ./test
    ```

    The execution result is as follows:

    ```shell
    result = 0 
    src1:  0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 
    src2:  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
    dst :  1  3  5  7  9 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39 
    ```
