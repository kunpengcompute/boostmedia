# Environment Deployment

## Environment Requirements

**Hardware**

| Project | Description   |
| ------------ | ------------ |
| CPU | Kunpeng 950|

**Software Environment**

| Software | Version   |
| ------------ | ------------ |
| OS | openEuler 22.03 LTS SP4 |
| Compiler| Clang 19.0 or later|
| make |  4.3 or later|
| cmake | 3.10 or later|

**Other Requirements**

The SVE length must be set to 128. In openEuler 22.03 LTS SP4, you can set the SVE length as follows:

```shell
echo 16 > /proc/sys/abi/sve_default_vector_length
```

## Deployment

Obtain the KVCL binary package and run the following command to decompress it:

```shell
unzip BoostKit-boostmedia-kvcl_1.0.0.zip
```

After the decompression, if the following two directories are displayed, the deployment is successful:

```shell
├──include # KVCL header file directory
└──lib # Paths of the dynamic and static libraries
```

The directory contains the KVCL header file, dynamic library, and static library. The static library is recommended.
