# HMPP FAQ

本文提供使用HMPP过程的常见问题以及解决办法。

## 接口调用时提示头文件和引用动态库错误

**问题现象描述**

头文件包含错误：

```shell
fatal error: hmpp.h: No such file or directory
```

引用动态库错误：

```shell
./test: error while loading shared libraries: libHMPP_core.so.**xxxx**: cannot open shared object file: No such file or directory
```

上述现象结果中涉及的**xxxx**代表版本号。

**关键过程、根本原因分析**

安装包可能未正确安装，HMPP库的头文件和动态库并未存储在指定目录。

安装包安装后，环境变量未正常写入系统并生效。

**结论、解决方案及结果**

 1. 检查HMPP是否已安装。

    通过以下命令查找是否存在HMPP头文件和动态库，如果不存在则需要重新安装HMPP。

    ```bash
    ll /usr/local/include/HMPP
    ll /usr/local/lib/HMPP
    ```
    
 2. 添加环境变量。

     1. 打开“/etc/profile“文件。
        
        ```bash
        vi /etc/profile
        ```
        
     2. 在文件中添加如下环境变量。

        ```bash
        export C_INCLUDE_PATH=/usr/local/include/HMPP:$C_INCLUDE_PATH
        export CPLUS_INCLUDE_PATH=/usr/local/include/HMPP:$CPLUS_INCLUDE_PATH
        export LD_LIBRARY_PATH=/usr/local/lib/HMPP:$LD_LIBRARY_PATH
        ```

     3. 按`Esc`键，输入 **:wq!**，按`Enter`保存并退出编辑。

     4. 使配置文件生效。

        ```bash
        source /etc/profile
        ```
