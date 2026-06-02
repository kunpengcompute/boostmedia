# HMPP Installation Guide

This document describes how to install HMPP.

## Environment Requirements

Hardware environment:

| Item | Description   |
| ------------ | ------------ |
| CPU | Kunpeng 920 and new Kunpeng 920 model|

Software environment:

| Software | Version   |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| Compiler| GCC 10.3.1 or later|
| make |  4.3 or later|
| cmake | 3.10 or later|

## Installing Dependencies

- Run the following commands to install the secure function library [libboundscheck](https://atomgit.com/openeuler/libboundscheck) library:

    ```shell
    git clone https://atomgit.com/openeuler/libboundscheck.git
    cd libboundscheck
    make CC=gcc
    mkdir /usr/local/include/libboundscheck
    mkdir /usr/local/lib/libboundscheck
    cp include/* /usr/local/include/libboundscheck
    cp lib/* /usr/local/lib/libboundscheck
    ```

- Install Kunpeng Math Library (KML) to the default directory `/usr/local/kml`. For details about how to install and use KML, see [Kunpeng Math Library Developer Guide](https://www.hikunpeng.com/document/detail/en/kunpengaccel/math-lib/devg-kml/kunpengaccel_kml_16_0001.html)

## Installing HMPP Using a Software Package

After obtaining the RPM or DEB package, perform the following steps for installation.

- Install HMPP.

    ```shell
    # rpm
    rpm -ivh boostmedia-hmpp-xxxx-1.aarch64.rpm

    # deb
    dpkg -i boostmedia-hmpp-xxxx.aarch64.deb
    ```

    _xxx_ indicates the version number.

- Check whether HMPP is successfully installed.

    ```shell
    ll /usr/local/lib/HMPP 
    ll /usr/local/include/HMPP
    ```

    If the shared libraries and header files related to HMPP are displayed, HMPP has been successfully installed.

- Uninstall HMPP.

    ```shell
    # RPM
    rpm -e boostmedia-hmpp-xxxx-1
    ```

    ```shell
    # deb
    dpkg -r boostmedia-hmpp
    ```
