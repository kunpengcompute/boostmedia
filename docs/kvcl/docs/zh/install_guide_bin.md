# 环境部署

## 环境

硬件环境：

| 项目  | 说明    |
| ------------ | ------------ |
| CPU | 鲲鹏950 |

软件环境：

| 软件  | 版本    |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| 编译器 | clang ≥ 19.0 |
| make |  ≥ 4.3 |
| cmake | ≥ 3.10 |

其他：

sve长度需设置为128，在openEuler 22.03 LTS SP4中可通过以下方式设置。

```shell
echo 16 > /proc/sys/abi/sve_default_vector_length
```

## 部署

获取kvcl二进制包，运行以下命令解压。

```shell
unzip BoostKit-boostmedia-kvcl_1.0.0.zip
```

解压后，包含以下两个目录即为部署成功。

```shell
├──include  #kvcl头文件目录
└──lib      #动态库和静态库路径
```

其中包含kvcl头文件、动态库和静态库。

建议优先使用静态库。