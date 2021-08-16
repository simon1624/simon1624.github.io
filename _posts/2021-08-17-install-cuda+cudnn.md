---
layout: post
title: install cuda and cudnn
tags: [ubuntu, Nvidia]
author: simon
---

___08/01/2021___

# How to install cuda + cudnn in Ubuntu

## cuda安装

第一步，关闭**nouveau**驱动，不再赘述。接着去官网下载runfile格式的cuda安装包，注意事项：

1. 安装前务必关闭图形化界面，使用指令

```bash
$ sudo apt-get install lightdm
```

2. 安装lightdm，为了防止黑屏，安装gdm3

```bash
$ sudo apt-get install gdm3
```

3. 通过以下指令,关闭图形化界面，

```bash
$ sudo dpkg-reconfigure lightdm
$ sudo service lightdm stop
```
4. 若关闭失败，重启后黑屏且卡在logo界面，先按<kbd>F1</kbd>查看启动情况，如果是__lightdm__启动失败，可以在开机时按住<kbd>Esc</kbd>键进入__grub__模式，选择__anvaced boot__，进入__recovery__模式，用如下指令将图形化界面关闭，再启动__gdm3__来恢复图形化界面。

```bash
$ sudo systemctl set-default multi-user.target
$ sudo service gdm3 start
```


### __！！！注意！！！！__

<kbd>ctl</kbd>+<kbd>alt</kbd>+<kbd>F2</kbd>可以切换控制台，进入__tty2__模式，一定要注意自己所在的控制台，__tty2__~__tty6__都可以通过指令

```bash
$ sudo systemctl set-default graphical.target  
```
进入图形化界面，记得退出来，切记需要关闭的是通过<kbd>ctl</kbd>+<kbd>alt</kbd>+<kbd>F1</kbd>（或<kbd>F7</kbd>）进入的主页面的图形化界面。

5. 关闭后按照官网指令执行安装，按<kbd>ctl</kbd>+<kbd>c</kbd>跳过安装须知，填入`accept`，接着问是否安装_cuda driver_，切记，如果已经安装了_NVIDIA_驱动,里面包含了_cuda driver_了，填`n`，询问是否安装_opengl_，填`n`，其他选项都填`y`。

    

## cudnn安装

1. 下载 3 个deb,然后执行:

```bash
sudo dpkg -i libcudnn7_7.6.5.32-1+cuda10.1_amd64.deb
sudo dpkg -i libcudnn7-dev_7.6.5.32-1+cuda10.1_amd64.deb
sudo dpkg -i libcudnn7-doc_7.6.5.32-1+cuda10.1_amd64.deb
```
2. 接着移动 3 个文件

```bash
sudo cp /usr/local/cuda-10.0/lib64/libcudart.so.10.0 /usr/local/lib/libcudart.so.10.0 && sudo ldconfig
sudo cp /usr/local/cuda-10.0/lib64/libcublas.so.10.0 /usr/local/lib/libcublas.so.10.0 && sudo ldconfig
sudo cp /usr/local/cuda-10.0/lib64/libcurand.so.10.0 /usr/local/lib/libcurand.so.10.0 && sudo ldconfig
```
3. 测试cudnn是否成功安装
    1)  复制__cuDNN samples__到__home__目录下

```bash
$ cp -r /usr/src/cudnn_samples_v7 /$HOME
```
​       2)  进入home目录

```bash
$ cd $HOME/cudnn_samples_v7/mnistCUDNN/
```
​	3) 编译mnistCUDNN

```bash 
$ sudo make clean 
$ sudo make
```
​	4）运行mnistCUDNN 
```bash
$ sudo ./mnistCUDNN
```

