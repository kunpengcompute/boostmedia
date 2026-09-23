# HMPP安装指南

本文提供HMPP的安装指南，请按照以下步骤进行编译安装。

## 环境要求

硬件环境：

| 项目  | 说明    |
| ------------ | ------------ |
| CPU | 鲲鹏920处理器、鲲鹏920新型号处理器、鲲鹏950处理器 |

软件环境：

| 软件  | 版本    |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4/openEuler 24.03 LTS SP4 |
| 编译器 | GCC 10.3.1或以上版本 |
| make |  4.3或以上版本 |
| cmake | 3.10或以上版本 |

## 安装依赖库

  1. 安装安全函数库，通过以下命令安装[libboundscheck](https://atomgit.com/openeuler/libboundscheck)。

      ```shell
      git clone https://atomgit.com/openeuler/libboundscheck.git
      cd libboundscheck
      make CC=gcc
      mkdir /usr/local/include/libboundscheck
      mkdir /usr/local/lib/libboundscheck
      cp include/* /usr/local/include/libboundscheck
      cp lib/* /usr/local/lib/libboundscheck
      ```

  2. 安装鲲鹏数学库KML。

      ```shell
      wget https://mirrors.huaweicloud.com/kunpeng/archive/HPC/KML/KML_26.1.RC1.zip

      unzip KML_26.1.RC1.zip

      tar -zxvf kml-26.1.RC1.aarch64.tar.gz

      cp -r kml /usr/local
      ```

## 软件包安装HMPP

### 获取软件包

从GitCode仓获取HMPP软件包[BoostKit-boostmedia-hmpp_2.6.2.beta1.zip](https://gitcode.com/boostkit/boostmedia/releases/download/v1.0.3-beta1/BoostKit-boostmedia-hmpp_2.6.2.beta1.zip)。

### 软件包完整性校验

为了防止软件包在传递过程或存储期间被恶意篡改，需下载对应的SHA256文件用于完整性验证。

1. 从GitCode仓获取HMPP软件包对应的SHA256文件[BoostKit-boostmedia-hmpp_2.6.2.beta1.zip.sha256sum](https://gitcode.com/boostkit/boostmedia/releases/download/v1.0.3-beta1/BoostKit-boostmedia-hmpp_2.6.2.beta1.zip.sha256sum)。

2. 计算文件的sha256校验值。linux执行命令如下：

    ```bash
    sha256sum <package>
    ```

    windows执行命令如下：

    ```bash
    certutil -hashfile <package> SHA256
    ```

    命令执行完成后，输出校验值。

3. 对比计算的校验值和SHA文件中的校验值是否一致，如果校验值一致说明文件完整，如果校验值不一致则可以确认文件完整性已被破坏，需要重新获取。

>![](./public_sys-resources/icon-note.gif) **说明：**
>
>- 如果校验失败，请不要使用该软件包，通过提交ISSUE反馈。
>- 使用软件包安装/升级之前，也需要按上述过程先校验软件包的SHA256值，确保软件包未被篡改。

### 软件包安装

软件包解压后得到rpm包和deb包（选其一安装即可），按照以下步骤进行安装。

  1. 解压软件包

      ```shell
      unzip BoostKit-boostmedia-hmpp_xxxx.zip
      ```

      xxxx表示版本号。

  2. 安装HMPP。

      ```shell
      # rpm
      rpm -ivh boostmedia-hmpp-xxxx-1.aarch64.rpm

      # deb
      dpkg -i boostmedia-hmpp-xxxx.aarch64.deb
      ```

  3. 查看HMPP是否安装成功。

      ```shell
      ll /usr/local/lib/HMPP
      ll /usr/local/include/HMPP
      ```

      若有HMPP相关的动态库和头文件，则表示HMPP安装成功。

## 卸载HMPP

- 通过以下命令卸载rpm包或deb包安装的HMPP。

    ```shell
    # rpm
    rpm -e boostmedia-hmpp-xxxx-1

    # deb
    dpkg -r boostmedia-hmpp
    ```
