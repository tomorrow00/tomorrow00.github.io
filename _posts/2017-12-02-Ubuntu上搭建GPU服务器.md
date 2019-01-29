---
layout: post
title: Ubuntu上搭建GPU服务器
date: 2017-12-02 20:05:00 +0800
categories: 配置文档
tag: 环境搭建
---

* content
{:toc}

> 1.安装显卡驱动
====================================

 - 下载：

    根据显卡类型以及操作系统，选定CUDA版本和语言设置，下载对应的显卡驱动。
    ![在这里插入图片描述]({{ '/styles/images/blogs/20190109002143528.jpeg' | prepend: site.baseurl }})
[    驱动下载地址](https://www.nvidia.cn/Download/index.aspx?lang=cn)

 - 安装

    `$ sudo ./NVIDIA-Linux-xxxxxx.run –no-opengl-files –no-x-check`

    注：

    1）选用下载的驱动名替代上述驱动名称；
    2）–no-opengl-files：表示只安装驱动文件，不安装OpenGL文件。这个参数不可省略，否则会导致登陆界面死循环；

    3）–no-x-check：表示安装驱动时不检查X服务，建议使用。

 - 验证

    `$ nvidia-smi`

    ![在这里插入图片描述]({{ '/styles/images/blogs/20190109002903182.jpeg' | prepend: site.baseurl }})

    出现此图表示安装成功。

> 2.安装CUDA
====================================

 - 下载：

    选定CUDA版本，根据操作系统及相应的版本配置安装CUDA。官网附带具体安装说明。
    ![在这里插入图片描述]({{ '/styles/images/blogs/201901090030455.jpeg' | prepend: site.baseurl }})
    [CUDA下载地址](https://developer.nvidia.com/cuda-toolkit-archive)

 - 安装

    `$ sudo sh cuda_10.0.130_410.48_linux.run`

 - CUDA Sample测试：

    ```
    #编译并测试设备 deviceQuery：
    $ cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
    $ sudo make
    $ ./deviceQuery

    #编译并测试带宽 bandwidthTest：
    $ cd ../bandwidthTest
    $ sudo make
    $./bandwidthTest
    ```

    如果这两个测试的最后结果都是Result = PASS，说明CUDA安装成功。

> 3.安装cuDNN
====================================

 - 下载：

    根据CUDA版本，选择相应的cuDNN下载并安装（需要NVIDIA账号登陆）。

    ![在这里插入图片描述]({{ '/styles/images/blogs/20190109003902252.jpeg' | prepend: site.baseurl }})

    [cuDNN下载地址](https://developer.nvidia.com/cudnn)

 - 安装：

    cuDNN安装包为Tar文件：
    ```
    $ tar -xzvf cudnn-9.0-linux-x64-v7.tgz
    $ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
    $ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
    $ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
    ```
    cuDNN安装包为Debian文件：
    ```
    安装运行库：
    $ sudo dpkg -i libcudnn7_7.0.3.11-1+cuda9.0_amd64.deb`
    安装开发者库：`
    $ sudo dpkg -i libcudnn7-dev_7.0.3.11-1+cuda9.0_amd64.deb`
    安装样例以及用户指南：
    $ sudo dpkg -i libcudnn7-doc_7.0.3.11-1+cuda9.0_amd64.deb
    ```

    [安装文档地址](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#install-linux)

 - cuDNN样例测试：
    ```
    将cuDNN样例拷贝到一个可操作目录下：
    $ cp -r /usr/src/cudnn_samples_v7/ $HOME
    进入该目录：
    $ cd $HOME/cudnn_samples_v7/mnistCUDNN
    编译mnistCUDNN样例：
    $ make clean && make
    运行mnistCUDNN样例：
    $ ./mnistCUDNN
    若出现以下情况，则说明cuDNN已经安装完成。
    Test passed!
    ```
