# HMPP FAQ

This document provides solutions to frequently asked questions (FAQs) about using HMPP.

## Header File and Shared Library Reference Errors Reported After Calling an Interface

**Symptom**

Header file inclusion error:

```
fatal error: hmpp.h: No such file or directory
```

Shared library reference error:

```
./test: error while loading shared libraries: libHMPP_core.so.**xxxx**: cannot open shared object file: No such file or directory
```

_xxxx_ in the preceding error message indicates the version number.

**Key Process and Root Cause Analysis**

The installation package may not be correctly installed, and the header files and shared libraries of the HMPP library are not stored in the specified directories.

After the installation package is installed, the environment variables are not properly written into the system and thus do not take effect.

**Conclusion and Solution**

 1. Check whether HMPP has been installed.

    Run the following commands to check whether the HMPP header files and shared libraries exist. If not, reinstall HMPP.

    ```bash
    ll /usr/local/include/HMPP
    ll /usr/local/lib/HMPP
    ```

 2. Add environment variables.

     1. Open the `/etc/profile` file.

        ```bash
        vi /etc/profile
        ```

     2. Add the following environment variables to the file:

        ```bash
        export C_INCLUDE_PATH=$C_INCLUDE_PATH:/usr/local/include/HMPP
        export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/include/HMPP
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/HMPP
        ```

     3. Press `Esc`, type `:wq!`, and press `Enter` to save the file and exit.

     4. Make the settings take effect.

        ```bash
        source /etc/profile
        ```
