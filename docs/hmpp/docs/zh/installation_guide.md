# HMPP安装指南

本文提供HMPP的安装指南，请按照以下步骤进行编译安装。

## 环境要求

硬件环境：

| 项目  | 说明    |
| ------------ | ------------ |
| CPU | 鲲鹏920/鲲鹏920新型号 |

软件环境：

| 软件  | 版本    |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| 编译器 | GCC 10.3.1或以上版本 |
| make |  4.3或以上版本 |
| cmake | 3.10或以上版本 |

## 安装依赖库

- 安装安全函数库，通过以下命令安装[libboundscheck](https://atomgit.com/openeuler/libboundscheck)。

    ```shell
    git clone https://atomgit.com/openeuler/libboundscheck.git
    cd libboundscheck
    make CC=gcc
    mkdir /usr/local/include/libboundscheck
    mkdir /usr/local/lib/libboundscheck
    cp include/* /usr/local/include/libboundscheck
    cp lib/* /usr/local/lib/libboundscheck
    ```

- 安装鲲鹏数学库KML，KML的安装与使用请参见《[鲲鹏数学库 开发指南](https://www.hikunpeng.com/document/detail/zh/kunpengaccel/math-lib/devg-kml/kunpengaccel_kml_0001.html)》安装到默认目录`/usr/local/kml`。

## 软件包安装HMPP

用户获取RPM包或deb包后（选其一即可），按照如下步骤进行安装。

- 安装HMPP。

    ```shell
    # rpm
    rpm -ivh boostmedia-hmpp-xxxx-1.aarch64.rpm

    # deb
    dpkg -i boostmedia-hmpp-xxxx.aarch64.deb
    ```

    xxx表示版本号。

- 查看HMPP是否安装成功。

    ```shell
    ll /usr/local/lib/HMPP 
    ll /usr/local/include/HMPP
    ```

    若有HMPP相关的动态库和头文件，则表示HMPP安装成功。

- 卸载HMPP。

    ```shell
    # RPM
    rpm -e boostmedia-hmpp-xxxx-1
    ```

    ```shell
    # deb
    dpkg -r boostmedia-hmpp
    ```
