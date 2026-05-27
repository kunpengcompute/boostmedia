# 环境部署

## 环境要求

**硬件环境**

| 项目  | 说明    |
| ------------ | ------------ |
| CPU | 鲲鹏950处理器 |

**软件环境**

| 软件  | 版本    |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| 编译器 | Clang 19.0或之后的版本 |
| make |  4.3或之后的版本 |
| cmake | 3.10或之后的版本 |

**其他要求**

SVE长度需设置为128，在openEuler 22.03 LTS SP4中可通过以下方式设置。

```shell
echo 16 > /proc/sys/abi/sve_default_vector_length
```

## 部署

从GitCode仓获取KVCL二进制包[BoostKit-boostmedia-kvcl_1.0.0.zip](https://gitcode.com/boostkit/boostmedia/releases/download/v1.0.0/BoostKit-boostmedia-kvcl_1.0.0.zip
)，运行以下命令解压。

```shell
unzip BoostKit-boostmedia-kvcl_1.0.0.zip
```

解压后，包含以下两个目录即为部署成功。

```shell
├──include  #KVCL头文件目录
└──lib      #动态库和静态库路径
```

其中包含KVCL头文件、动态库和静态库。建议优先使用静态库。
